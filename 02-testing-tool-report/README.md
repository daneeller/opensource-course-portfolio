# 维度二 · 测试工具使用报告（满分 20）

**扣分规则**：找到明确报告不扣；仅有测试材料扣 10；未找到扣 20。
**本维自评**：不扣。本目录提供多份**结构化测试报告**，每份含测试目标、环境、命令、原始数据与结论，不是零散的测试材料。

---

## 报告清单

| 报告 | 内容摘要 | 原始文件 |
|---|---|---|
| 最终测试报告 | backup 专项 27 项 + `test_sqlite3` 全套 534 run / 7 skip / 0 fail | [`evidence/final_test_report.md`](evidence/final_test_report.md) |
| 跨平台验证报告 | Windows Release / Debug / free-threaded 三配置对照；锁冲突复现数据 | [`evidence/phase8_cross_platform_validation.md`](evidence/phase8_cross_platform_validation.md) |
| GIL 验证（基线） | 未修复 3.14.5 / 分支基线上 `sleep(0.001)` 主线程卡顿实测 | [`evidence/gil_validation_baseline.json`](evidence/gil_validation_baseline.json) |
| GIL 验证（修复后） | PR #161930 修复版的回归测试 PASS + 卡顿实测 | [`evidence/gil_validation_pr161930.json`](evidence/gil_validation_pr161930.json) |

## 使用的测试工具 / 方法

- **CPython 测试框架**：`python -m test test_sqlite3 -v`（unittest 驱动，CPython 官方 `Lib/test/`）；
- **专项测试**：`Lib/test/test_sqlite3/test_backup.py` 中 12 个 timeout 相关新增用例 + 15 个原有 backup 用例；
- **多配置构建矩阵**：`PCbuild\build.bat -p x64 -c Release/Debug`，free-threaded 构建（`amd64t`）；
- **锁冲突探针**：独立 Python 脚本（`f01_probe_final.py` / `gil_repro.py`），采集回调时间序列、`OperationalError` / `sqlite_errorcode=5 (SQLITE_BUSY)`；
- **上游 CI**：GitHub Actions 57–59 项检查，含 ASan / MSan / TSan / TSan(free-threaded) / UBSan 五种 sanitizer 配置。

## 关键数据（可复核）

- `test_sqlite3`：run=534、skipped=7、fail=0（Release / Debug / free-threaded 三配置一致）；
- 锁冲突复现：`sqlite_errorcode=5 (SQLITE_BUSY)`，回调 8 次，耗时约 0.64–0.65s（与文档所述软性时限一致）；
- GIL 验证：未修复基线 531 / 706 / 455 ms → PR #161930 修复后 8 / 8 / 11 ms（同机 x64 Release 对照）。
