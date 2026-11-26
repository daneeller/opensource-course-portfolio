# 维度四 · 综合项目报告与代码（满分 20）

**扣分规则**：未填项目仓扣 20；有仓但报告 / 代码缺一项扣 10；两者都无扣 20。
**本维自评**：不扣。三个公开项目仓，每仓均同时具备**报告**（README 或正式测试报告）与**可运行代码**。

---

## 项目仓对照表

| 仓库 | 链接 | 报告 | 代码 |
|---|---|---|---|
| ClassOrder（课序） | https://github.com/daneeller/course-order | `README.md`（8.2 KB：定位 / 功能 / 环境 / 构建 / 工程笔记 / 隐私） | `entry/`（ArkTS 应用源码）、`logic-test/`（113 条逻辑断言测试台） |
| Apache Commons CLI 许可扫描 | https://github.com/daneeller/f25015220-apache-commons-cli-license-scan | `report/f25015220-陈思宇-ApacheCommonsCLI-软件许可测试报告.docx` + `preview.pdf` | `scan-result/`（扫描结果数据），ScanCode Toolkit 32.5.0 实际运行 |
| Handover Defect Collaboration | https://github.com/daneeller/fde-handover-collab | `README.md`（10.5 KB）、`docs/`、`knowledge_base/` | `src/`（audit.py / locator.py / rules.py / models.py 等）、`app/`、`tests/`、`server.py` |

## 关键事实（可复核）

- 三仓均为 **public**，可直接打开查证；
- ClassOrder 的 `logic-test/` 内含可执行的断言测试台，`run.sh` 相对路径、自动探测 Node.js / TypeScript；
- 许可扫描仓是完整的一次 **软件许可合规实践**：141 个受控文件、SPDX License List 3.27、含误报与盲点分析；
- Handover Defect Collaboration 有真实可跑的 Python 后端（`server.py` + `src/`）与单元测试（`tests/`）。

## 其他公开项目（主页已列，供参考）

- 个人站点：https://daneeller.github.io
- GitHub Profile：https://github.com/daneeller

> 注：另有一个本地桌面应用项目（MemoLink）尚未公开，未列入本档案。
