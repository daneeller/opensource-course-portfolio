# 开源软件与技术 · 学期证据档案

**作者**：陈思宇（daneeller / csy） · **学号**：f25015220
**归档日期**：2026-11-26 · **评分依据**：课程「五维扣分规则」（表2，总分 100）

本仓库按评分规则的五个维度逐项归档可查证据。每个维度一个目录，包含：

- `README.md` — 该维度的结论、证据清单与查证路径；
- `evidence/` — 对应的原始报告 / 数据文件（真实产物，非事后编造）。

**原则**：所有截图为 GitHub 公开页面的真实 headless 截图；所有提交数、检查数、测试结果均可通过链接或仓库历史复核。文档中的时间口径按**字面日历日期**理解。

---

## 五维得分对照（自评）

| 维度 | 满分 | 扣分规则 | 本档案对应目录 | 自评 | 依据摘要 |
|---|---|---|---|---|---|
| 本周更新 | 25 | 无提交扣25；1-2次扣15；3-5次扣8；6次及以上不扣 | [01-weekly-updates](01-weekly-updates/README.md) | 不扣 | 本周（11/24–11/26）fork `cpython` 3 个 commit + PR/评论/CLA/CI 等活动 |
| 测试工具使用报告 | 20 | 明确报告不扣；仅测试材料扣10；未找到扣20 | [02-testing-tool-report](02-testing-tool-report/README.md) | 不扣 | 多份结构化测试报告（最终测试/跨平台/Sanitizer/GIL 验证），均有命令与数据 |
| PR/Issue 截图 | 15 | 未找到扣15；有图片但文件…扣… | [03-pr-issue-screenshots](03-pr-issue-screenshots/README.md) | 不扣 | 9 张真实 GitHub 页面截图（CPython 4 + Flet 4 + CI 1） |
| 综合项目报告与代码 | 20 | 未填项目仓扣20；有仓但报告/代码缺一项扣10；两者都无扣20 | [04-project-reports-code](04-project-reports-code/README.md) | 不扣 | 3 个公开项目仓，均含 README 报告与可运行代码 |
| 前半学期工作量 | 20 | 截至 11/5 无提交扣20；1-2次扣10；3次及以上不扣 | [05-first-half-workload](05-first-half-workload/README.md) | 不扣 | 11/5 前 5 个仓合计 16 次提交（作者 commit 历史可查） |

> 自评仅为对照说明，最终以任课教师判定为准。

---

## 查证入口速查

| 证据类型 | 直达链接 |
|---|---|
| CPython PR #162594（主补丁） | https://github.com/python/cpython/pull/162594 |
| CPython issue #162414（需求来源） | https://github.com/python/cpython/issues/162414 |
| CPython issue #161868（GIL 饥饿复现） | https://github.com/python/cpython/issues/161868 |
| CPython PR #161930（被验证的修复） | https://github.com/python/cpython/pull/161930 |
| GitHub 主页（个人介绍） | https://github.com/daneeller |
| 个人站点 | https://daneeller.github.io |

**PR 最新动态（2026-11-26 查证）**：PR #162594 已由 CPython bot 指派 reviewer
`erlend-aasland`，标签 `awaiting review`，仍为 Open、非 Draft。详见
[01-weekly-updates](01-weekly-updates/README.md) 与截图 `images/pr162594.png`。

---

## 目录结构

```
opensource-course-portfolio/
├── README.md                      本文件
├── 01-weekly-updates/             维度一：本周更新
├── 02-testing-tool-report/        维度二：测试工具使用报告
├── 03-pr-issue-screenshots/       维度三：PR/Issue 截图
├── 04-project-reports-code/       维度四：综合项目报告与代码
└── 05-first-half-workload/        维度五：前半学期工作量
```
