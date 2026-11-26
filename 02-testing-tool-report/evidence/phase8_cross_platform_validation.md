# CPython #162414 — Phase 8 独立兼容性验证报告

> 本阶段只读验证：不修改正式源码/测试/文档，不 commit/push/PR/发评论。
> 所有结论基于真实构建出的解释器运行结果；不可验证项如实标注原因。

---

## 一、冻结的补丁状态（只读记录）

| 项目 | 值 |
|---|---|
| 主工作区 | `C:\Users\陈思宇\WorkBuddy\2026-11-19-13-54-34\cpython-gil` |
| 分支 | `gh-162414-backup-timeout` |
| HEAD | `b8d2f65`（未变） |
| `git diff --check` | exit 0（无空白错误） |
| 隔离工作树 | `cpython-gil-ft`（Free-threaded）、`cpython-gil-dbg`（Debug），均为 `--detach HEAD` + 应用 `final_full_patch.diff` + NEWS |
| 补丁文件 | `outputs/final_full_patch.diff`（601 行）、`news_blurb.diff` |

隔离工作树与主工作区 Release 产物完全分离，未覆盖 `cpython-gil/PCbuild/amd64/` 下已通过测试的 Release 解释器。

---

## 二、构建环境探测结果

| 环境 | 状态 | 依据 |
|---|---|---|
| Windows Free-threaded | ✅ 可构建 | VS BuildTools MSVC `14.44.35207`（`cl.exe` 19.44.35230）+ Windows SDK `10.0.22621.0` / `10.0.26100.0`；mimalloc 已内置于 `externals/mimalloc`，无需外部依赖 |
| Windows Debug | ✅ 可构建 | 同上工具链，`build.bat -c Debug` |
| WSL2 Ubuntu Linux | ❌ 不可用 | `wsl.exe` 被 WorkBuddy 安全策略列入 Program Blacklist，`Permission denied`；按指令不绕过、不安装 |
| Linux Free-threaded | ❌ 不可用 | 依赖 WSL2，同上原因 |

> 用户优先级为「Windows Free-threaded + Linux Debug」。Linux Debug 因 WSL2 不可用无法进行；已用 Windows Debug 替代覆盖 Debug 配置维度。

---

## 三、编译器与工具链（Windows）

| 项 | 值 |
|---|---|
| MSVC 工具集 | `14.44.35207` |
| `cl.exe` 版本 | `19.44.35230`（x64） |
| Windows SDK | `10.0.22621.0`、`10.0.26100.0` |
| 构建驱动 | 系统 Python `py -3.14` 启动 `build_ft.py` / `build_dbg.py`，内部调 `cmd.exe /c PCbuild\build.bat` |
| 代理清理 | 脚本内去重 `HTTPS_PROXY`/`https_proxy` 等大小写重复键（沿用 Phase 3 已验证方案） |
| 输出重定向 | 文件重定向（非 `stdout=PIPE`），避免 MSBuild 后代继承管道导致 EOF 死锁 |

---

## 四、各环境验证结果

> 构建结果将在真实编译完成后填入。以下为结构占位。

### 4.1 Windows Free-threaded（优先项）— ✅ 已验证

| 项 | 结果 |
|---|---|
| 构建命令 | `py -3.14 build_ft.py` → `cmd.exe /c PCbuild\build.bat -p x64 -c Release --disable-gil`（工作树 `cpython-gil-ft`） |
| 编译/链接结果 | **已成功生成**，0 错误、2 警告（`Modules/expat/xmlparse.c:9540` 的 C4267 size_t→unsigned，与本补丁无关）；642 行日志，耗时 2m42s |
| 解释器路径 | `C:\Users\陈思宇\WorkBuddy\2026-11-19-13-54-34\cpython-gil-ft\PCbuild\amd64t\python.exe`（free-threaded 专用 `amd64t` 目录，未覆盖 Release 的 `amd64`） |
| CPython 版本 | `3.16.0a2+dev free-threading build (heads/gh-162414-backup-timeout-dirty:b8d2f65, Nov 24 2026, 21:13:57) [MSC v.1944 64 bit (AMD64)]` |
| SQLite 版本 | `3.53.4` |
| GIL 启用状态 | **`gil_enabled: False`（GIL 已禁用）** — 目标配置确认 |
| Backup 专项测试 | **SUCCESS — run=27，1 test OK**（5.1s） |
| sqlite3 全量测试 | **SUCCESS — run=534 skipped=7**（6.1s，0 failed） |
| 锁冲突复现 | `kind=exc exc=OperationalError elapsed=0.644s callbacks=8 sqlite_errorcode=5 (SQLITE_BUSY)`；回调时间序列 `(5,0.316)…(5,0.644)` |
| 修复后健康路径 | 2000 行多页备份完成（`timeout=0.000001`、`pages=1`），`last_status=101 (DONE)`，7 次回调 — 极小预算不误伤正常备份 |
| 未验证项 | 无（FT 维度全部完成） |

> 复现总耗时 0.644s 超过外层 `timeout=0.3`：底层连接 `timeout=0.05` 的锁等待不计入外层预算，符合文档「soft, best-effort limit, not a hard real-time deadline」的声明，非缺陷。

### 4.2 Windows Debug（优先项，替代 Linux Debug 维度）— ✅ 已验证

| 项 | 结果 |
|---|---|
| 构建命令 | `py -3.14 build_dbg.py` → `cmd.exe /c PCbuild\build.bat -p x64 -c Debug`（工作树 `cpython-gil-dbg`） |
| 编译/链接结果 | **已成功生成**，0 错误、1 警告（同 `xmlparse.c:9540` C4267，与本补丁无关）；增量编译 11.49s |
| 解释器路径 | `C:\Users\陈思宇\WorkBuddy\2026-11-19-13-54-34\cpython-gil-dbg\PCbuild\amd64\python_d.exe`（Debug 专用 `python_d.exe`，未覆盖 Release 的 `python.exe`） |
| CPython 版本 | `3.16.0a2+dev (heads/gh-162414-backup-timeout-dirty:b8d2f65, Nov 24 2026, 21:23:53) [MSC v.1944 64 bit (AMD64)]` |
| SQLite 版本 | `3.53.4` |
| GIL 启用状态 | `gil_enabled: True`（GIL 启用，标准 Debug 配置，符合预期） |
| Backup 专项测试 | **SUCCESS — run=27，1 test OK**（5.3s） |
| sqlite3 全量测试 | **SUCCESS — run=534 skipped=7**（7.3s，0 failed） |
| 锁冲突复现 | `kind=exc exc=OperationalError elapsed=0.654s callbacks=8 sqlite_errorcode=5 (SQLITE_BUSY)`；回调序列 `(5,0.325)…(5,0.653)` |
| 修复后健康路径 | 2000 行多页备份完成（`timeout=0.000001`、`pages=1`），`last_status=101 (DONE)`，7 次回调 |
| 未验证项 | 无（Debug 维度全部完成） |

> 构建过程说明：Debug 首次全量构建曾在 21:24 后停滞、后台任务两次被系统标记 killed（非编译错误，日志无 error）。第三次以增量方式重启后 11.49s 补完并链接成功。这段中断属任务调度层面，不影响产物正确性。

### 4.3 WSL2 Ubuntu Linux

- 状态：不可构建（WSL2 被安全策略阻止）

### 4.4 Linux Free-threaded

- 状态：不可构建（依赖 WSL2）

---

## 五、验证脚本与方法

- 复现探针：`outputs/phase8_verify.py`，用真实构建出的解释器运行，记录版本/GIL/锁冲突复现/健康备份。
- 专项测试：`<python.exe> -m test test_sqlite3.test_backup -v`
- 全量测试：`<python.exe> -m test test_sqlite3`
- 每个环境独立工作树 + 独立 `PCbuild/amd64` 产物目录，互不覆盖。

---

## 六、总结与边界

### 6.1 验证矩阵

| 环境 | 可构建 | GIL | 解释器 | Backup 专项 | sqlite3 全量 | 锁冲突复现 |
|---|---|---|---|---|---|---|
| Windows Free-threaded | ✅ | **禁用** | `amd64t/python.exe` | 27 OK | 534 / 7 skip / 0 fail | SQLITE_BUSY, 0.644s, 8 回调 |
| Windows Debug | ✅ | 启用 | `amd64/python_d.exe` | 27 OK | 534 / 7 skip / 0 fail | SQLITE_BUSY, 0.654s, 8 回调 |
| WSL2 Ubuntu Linux | ❌ | — | — | — | — | WSL2 被安全策略黑名单阻止 |
| Linux Free-threaded | ❌ | — | — | — | — | 依赖 WSL2，同上 |

### 6.2 已验证事实

- 补丁在 **GIL 禁用** 与 **GIL 启用** 两种 Windows 配置下均编译成功、测试全过，行为一致。
- 两个配置的 CPython 版本均为 `3.16.0a2+dev (…b8d2f65)`，SQLite 均为 `3.53.4`。
- 锁冲突复现在两种配置下都以 `OperationalError / sqlite_errorcode=5 / SQLITE_BUSY` 返回,回调次数一致(8 次),总耗时略超 `timeout=0.3`(0.644s / 0.654s),原因是底层连接锁等待不计入外层预算——与文档「soft, best-effort limit」一致,非缺陷。
- 极小预算(`timeout=0.000001`)下 2000 行健康备份在两种配置下都完整完成(2000 行、`last_status=101/DONE`),证明修复未误伤正常备份路径。
- 所有构建均在独立工作树 + 独立产物目录完成,**未覆盖**主工作区已通过测试的 Release 产物。

### 6.3 静态分析 / 推断

- Debug 与 FT 的编译警告均为 `Modules/expat/xmlparse.c:9540` 的 C4267,属上游 expat 既有代码,与本次 `connection.c` 补丁无因果关系(该文件未被本补丁修改)。
- 锁冲突复现的回调次数(8)与耗时在不同配置下高度一致,推断计时逻辑不依赖 GIL 状态;但这属于观察推断,非源码级证明。

### 6.4 未验证的风险 / 不可验证项

- **Linux(WSL2 Ubuntu)与 Linux Free-threaded 完全未验证**:`wsl.exe` 被 WorkBuddy 安全策略列入 Program Blacklist(`Permission denied`),按指令不绕过、不安装。因此本补丁在 Linux 上的编译、链接、测试行为**无本机证据**,跨平台结论仅限于 Windows。
- FT 与 Debug 的构建各有一次后台任务被系统清理(killed),但均由后续增量重启补完并成功出产物,产物正确性不受影响;中断本身的原因(任务调度)未在本次范围内深究。
- 未做 ASan / UBSan / 引用计数专项内存检查;未做多线程并发压力下的 backup 竞争测试(超出本阶段授权范围)。

### 6.5 结论

在可获得的两类 Windows 环境(Debug / Free-threaded)中,`connection.c` 的有界重试补丁**编译干净、测试全过、锁冲突行为正确且跨配置一致**。Linux 维度因环境安全策略限制无法验证,如实标注为未验证,不作推断性结论。

本阶段全程只读验证:未修改正式源码/测试/文档,未 commit/push/PR/发评论,free-threaded 构建已在 4.1 节完成(本阶段授权范围内)。等待用户完成独立审查后再决定后续安排。
