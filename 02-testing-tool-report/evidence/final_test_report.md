# CPython #162414 — 最终测试报告（修正后终版，真实运行 2026-11-24）

## 环境

- 解释器：`./PCbuild/amd64/python.exe` = `3.16.0a2+dev (heads/gh-162414-backup-timeout-dirty:b8d2f65, Nov 24 2026, ...) [MSC v.1944 64 bit (AMD64)]`
- SQLite：3.53.4
- 构建：增量编译，`py -3.14 build_direct.py` → `PCbuild\build.bat -p x64 -c Release`，文件重定向写 `build.log`
- 结果：**exit 0，0 警告 0 错误，用时 00:00:08.94**；日志中确认 `connection.c` 被重新编译、`_sqlite3.pyd` 重新链接
- 注意：必须用系统 Python（`py -3.14`）启动构建脚本；用构建出的 `python.exe` 启动会被 CPython 自带 "Killing any running python.exe instances" 步骤杀掉（首次尝试 exit 127，已记录并改用系统 Python 成功）

## 测试结果

### 1. backup 专项（15 个原有 + 12 个 timeout 测试）

```
./PCbuild/amd64/python.exe -m test test_sqlite3.test_backup -v
Ran 27 tests in 5.098s
OK
Result: SUCCESS  (27 tests, 0 failures)
```

12 个 `BackupTimeoutTests`（`test_backup.py:259-420`）全部 OK：

| # | 测试 | 行号 |
|---|---|---|
| 1 | test_timeout_is_keyword_only | :259 |
| 2 | test_timeout_rejects_invalid_values | :265 |
| 3 | test_timeout_minus_one_keeps_default_behaviour | :273 |
| 4 | test_does_not_time_out_without_lock_conflict | :279 |
| 5 | test_bounded_retry_reports_lock_error | :287 |
| 6 | test_timeout_zero_returns_on_first_conflict | :298 |
| 7 | test_sleep_is_clipped_to_remaining_budget | :305 |
| 8 | test_progress_exception_is_preserved | :316 |
| 9 | test_completed_backup_is_not_reported_as_timeout | :330 |
| 10 | test_connections_reusable_after_timeout | :344 |
| 11 | **test_deadline_counts_progress_callback_time**（新增 F-01 专项） | :353 |
| 12 | **test_lock_window_restarts_after_sqlite_ok**（新增 P-2 专项） | :381 |

无 `threading._dangling` 警告（线程清理加固后）。

### 2. test_sqlite3 全套

```
./PCbuild/amd64/python.exe -m test test_sqlite3
== Tests result: SUCCESS ==
Total duration: 5.6 sec
Total tests: run=534 skipped=7
Result: SUCCESS  (0 failures)
```

上一版为 532 run；本轮 +2 个新增测试 = 534。

### 3. F-01 最终探针（`cpython-gil/f01_probe_final.py`）

**Scenario A — 原始 1 ms 复现场景（修复后重跑）：**

```
timeout=0.001, sleep=5.0，源持续锁定：
  源 timeout=0.05 → 总 0.360s, 回调 [(5, 0.329), (5, 0.360)]
  源 timeout=0.5  → 总 1.124s, 回调 [(5, 1.092), (5, 1.124)]
  sqlite_errorcode=5 / SQLITE_BUSY
```

结论：仍为 2 次回调，但第二次 step 启动时 `remaining ≈ +1 ms`（预算**尚未耗尽**），顶部检查按设计放行；step 内部阻塞跑过预算后由 BUSY 分支检查点终止。不能把"1 ms 下两次回调"当作 F-01 未修复的证据——详见 `final_source_review.md` F-01 节。

**Scenario B — 确定性修复前后区分证据：**

```
timeout=0.2, sleep=0.001, 首个 BUSY 回调主动 sleep 0.4s：
  kind=exc / OperationalError
  elapsed=0.601s
  callbacks=[(5, 0.201), ('callback_slept', 0.4)]
  sqlite_errorcode=5 / SQLITE_BUSY
```

只有 1 次 BUSY 回调；回调耗时 0.4s > 预算 0.2s，被计入窗口，回调结束后直接超时，无第二次 step。修复前因 deadline 在回调后建立会得到 2 次回调。这是专项测试的实测依据。

**Scenario C — 结构化诊断与"可超过剩余预算"边界：**

```
timeout=0.3, sleep=0.01, 源 timeout=0.1：
  elapsed=0.768s
  回调 8 次：t=0.442（首次，step 阻塞）/ 0.490 / 0.537 / 0.584 / 0.630 / 0.676 / 0.723 / 0.768
  sqlite_errorcode=5 / sqlite_errorname='SQLITE_BUSY'
```

首次 step 阻塞 0.442s 不计入预算；最后一次 step 在预算内开始、在预算外返回，符合文档的 best-effort 边界。

### 4. F-06 边界实测

```
timeout=1e12      → OverflowError: timestamp out of range for C PyTime_t
timeout=1e300     → OverflowError: timestamp out of range for C PyTime_t
timeout=1e-12     → OK
timeout=0.0       → OK
```

有限超范围由 `_PyTime_FromSecondsObject` 原生转换抛出，未改动 P-1 决策。

## 未运行

- U-2：free-threaded 构建（按本轮指令暂缓）。

## 禁止事项遵守情况

未 commit、未 push、未创建 PR、未发布 Issue 评论；未修改 busy handler 或连接配置；未扩大功能范围。
