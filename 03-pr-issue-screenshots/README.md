# 维度三 · PR/Issue 截图（满分 15）

**扣分规则**：未找到扣 15；有图片但文件（命名/说明不清）酌情扣。
**本维自评**：不扣。提供 9 张 GitHub 公开页面的真实截图，按仓库与编号命名，每张对应下方说明表。

> 截图方式：Chrome headless 模式直接访问 GitHub 公开 URL，未登录状态捕获，非绘制/合成图。页面时间以截图当日（2026-11-26）为准。

---

## 截图清单

### CPython（4 张）

| 文件 | 页面 | 要点 |
|---|---|---|
| [`images/pr162594.png`](images/pr162594.png) | [PR #162594](https://github.com/python/cpython/pull/162594) | 标题、Open 状态、3 commits / 57 checks / 5 files changed、reviewer 已指派、`awaiting review` 标签；正文含 Summary / Reproduction / Design |
| [`images/pr162594_checks.png`](images/pr162594_checks.png) | [PR #162594 Checks](https://github.com/python/cpython/pull/162594/checks) | Checks 列表页（展开各组可见全部通过）；DO-NOT-MERGE / Unresolved review / Lint / Tests 27 条均绿 |
| [`images/issue162414.png`](images/issue162414.png) | [Issue #162414](https://github.com/python/cpython/issues/162414) | 需求来源 issue，含本人最终回复（PR 链接、机制说明、CI 全绿结论） |
| [`images/issue161868.png`](images/issue161868.png) | [Issue #161868](https://github.com/python/cpython/issues/161868) | GIL 饥饿复现 issue，含本人 11-25 的 Windows 验证回帖 |
| [`images/pr161930.png`](images/pr161930.png) | [PR #161930](https://github.com/python/cpython/pull/161930) | 被验证的上游修复 PR，仍是 Open 状态 |

### Flet（4 张）

| 文件 | 页面 | 要点 |
|---|---|---|
| [`images/flet_pr7101.png`](images/flet_pr7101.png) | [PR #7101](https://github.com/flet-dev/flet/pull/7101) | 异步文档缺口修复，Merged |
| [`images/flet_pr7110.png`](images/flet_pr7110.png) | [PR #7110](https://github.com/flet-dev/flet/pull/7110) | issue #7109 → PR，logger 错误归属修复，Merged |
| [`images/flet_pr7113.png`](images/flet_pr7113.png) | [PR #7113](https://github.com/flet-dev/flet/pull/7113) | 跨线程 `asyncio.Event.set()` 不安全调用修复，Merged |
| [`images/flet_pr7127.png`](images/flet_pr7127.png) | [PR #7127](https://github.com/flet-dev/flet/pull/7127) | `Page.update()` 线程安全契约文档澄清，Open |

## 查证说明

- 每张截图均为 1440×1000 PNG，页面顶部含 GitHub 导航与仓库名，可确认来源；
- 截图仅捕获首屏；完整讨论 / 检查项请点表中链接查看；
- 截图原始文件也存放于 `../outputs/course_portfolio/screenshots/`（本地），本仓库为归档副本。
