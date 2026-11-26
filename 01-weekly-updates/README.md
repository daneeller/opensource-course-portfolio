# 维度一 · 本周更新（满分 25）

**扣分规则**：无提交扣 25；1–2 次扣 15；3–5 次扣 8；6 次及以上不扣。
**时间口径**：按字面日历日期，本周 = 2026-11-24（周一）～ 2026-11-30（周日）。
**本维自评**：不扣（本周 fork `cpython` 已有 3 个 author=csy 的 commit，另有 PR / issue 评论 / CLA / CI 验证等活动）。

---

## 提交记录（GitHub 可查）

仓库：`daneeller/cpython`，分支 `gh-162414-backup-timeout`，commit author = `csy`。
查证命令：`gh api "repos/daneeller/cpython/commits?sha=gh-162414-backup-timeout&per_page=8"`

| 时间（UTC） | Commit | 内容 |
|---|---|---|
| 11-25 03:36 | `15c0774` | Restore the one-line backup() signature |
| 11-25 03:28 | `62b47f8` | Fix cross-reference targets for sqlite_errorcode/sqlite_errorname |
| 11-25 02:36 | `9ed05cd` | Add optional bounded retry timeout to sqlite3.Connection.backup() |

> 说明：commit author 名为 `csy`（git 配置），GitHub 登录名为 `daneeller`，用 `author=daneeller` 过滤 API 会漏统计；按 sha 直查可见全部 3 条。

## 本周其他开源活动（同周，可交叉查证）

| 时间（UTC） | 事件 | 链接 |
|---|---|---|
| 11-25 12:22 | issue #161868 发布 Windows 验证结果（issuecomment-7162277816） | https://github.com/python/cpython/issues/161868 |
| 11-25 04:23 | issue #162414 评论（最终版，含 PR 链接与 CI 全绿结论） | https://github.com/python/cpython/issues/162414 |
| 11-25 03:15 | 创建 PR #162594（Read for review） | https://github.com/python/cpython/pull/162594 |
| 11-25 02:19 | Fork python/cpython | https://github.com/daneeller/cpython |
| 11-24～25 | 签署 CLA、CI 59/59 全绿（含 5 种 sanitizer 配置） | PR #162594 Checks 页 |

## 证据文件

- [`evidence/weekly_activity.json`](evidence/weekly_activity.json) — 从 GitHub API 抓取的本周公开活动与 commit 原始数据。
