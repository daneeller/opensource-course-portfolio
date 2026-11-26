# 维度五 · 前半学期工作量（满分 20）

**扣分规则**：截至 11/5 无提交扣 20；1–2 次扣 10；3 次及以上不扣。
**时间口径**：按字面日历日期，截止 2026-11-05。
**本维自评**：不扣。11/5 前合计 **16 次提交**，分布于 4 个公开仓库。

---

## 提交统计（GitHub commit 历史可查）

| 仓库 | 11/5 前提交数 | 链接 |
|---|---|---|
| `daneeller/course-order` | 2 | https://github.com/daneeller/course-order/commits/main |
| `daneeller/f25015220-apache-commons-cli-license-scan` | 3 | https://github.com/daneeller/f25015220-apache-commons-cli-license-scan/commits/main |
| `daneeller/daneeller` | 8 | https://github.com/daneeller/daneeller/commits/master |
| `daneeller/daneeller.github.io` | 3 | https://github.com/daneeller/daneeller.github.io/commits/main |
| `daneeller/fde-handover-collab` | 0 | 该仓创建于 11/19，不计入 |
| **合计** | **16** | — |

> 查证方法：各仓 commits 页按日期筛选至 11/5；或用 API
> `gh api "repos/<repo>/commits?until=2026-11-05T23:59:59Z&per_page=100"`。
> 注意：commit author 名可能为 `csy` / 中文名，不必等于 GitHub 登录名 `daneeller`，
> 查证时不要按 `author=daneeller` 过滤，否则会漏统计。

## 代表性提交

- **10-14** `course-order` 初始提交 + 补充开源材料（许可证、README、逻辑验证台 113 条断言）；
- **10 月下旬** `daneeller` profile 仓 8 次迭代（个人主页搭建与内容完善）；
- **10 月下旬～11 月初** `daneeller.github.io` 3 次提交（个人站点搭建）；
- **11 月初** `license-scan` 3 次提交（ScanCode Toolkit 扫描与报告初稿）。

## 证据文件

- [`evidence/commits_before_1105.json`](evidence/commits_before_1105.json) — 从 GitHub API 抓取的 11/5 前全部 16 条提交明细（sha / 时间 / author / 消息首行）。
