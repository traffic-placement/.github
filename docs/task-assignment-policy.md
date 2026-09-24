# 派活规范

适用：traffic-placement 全员。这条规范只解决一件事——**让"谁在什么时候交付什么"在 GitHub 上有一条能查的记录**。微信里说过的话不算派活。

## 结构现状

| 元素 | 现状 |
| --- | --- |
| 团队 | `traffic`（5 仓库）、`marriage-love`（3 仓库），**两个队都是全部 5 人** |
| 业务线标识 | 标签 `line:traffic` / `line:marriage-love`，8 个仓库都已建 |
| 组织原生字段 | `Priority`(Urgent/High/Medium/Low)、`Effort`(High/Medium/Low)、`Start date`、`Target date` |
| 建任务入口 | 各仓库 New issue → 选「任务（traffic 线）」或「任务（marriage-love 线）」表单 |

因为两队列成员相同，**`@traffic` / `@marriage-love` 会通知到同一批 5 个人，所以禁止用 @team 派活**。线的归属看 `line:` 标签，人的归属看 Assignee。

## 九条规则

**R1 唯一入口。** 新任务一律走 issue 表单创建（空白 issue 已关闭）。表单里「验收标准」是必填项——写不出验收标准，说明这活还没想清楚，不该派。

**R2 一人一件。** 可执行 issue 必须恰好 1 个 assignee。挂两个人 = 没有那个人。父需求可以不 assign，或挂我自己。

**R3 线的归属用标签，不用 @。** 表单会自动打 `line:traffic` 或 `line:marriage-love`。跨线需求拆成两条 issue，各自挂自己的线，别在一条里混。

**R4 issue 建在改代码的那个仓库。** 需求（父 issue）落在主战场仓库，跨仓库的子任务落在各自仓库，再用 sub-issue 挂到父需求上。不要建一个"任务集散仓库"。

**R5 PR 必须闭环。** 改代码的 PR 正文写 `Closes traffic-placement/<仓库>#<编号>`。注意默认分支不一致：`traffic_frontend` 是 `master`，其余是 `main`——只有合进默认分支才会自动关闭。没有关联 issue 的 PR 视为插单，要在描述里补上 issue 链接。

**R6 状态只有一个来源。** 进度看 issue 的 open/closed 与 Project 的 Status。标签只用来标不可变属性（`line:*`、模块），**禁止 `wip` `doing` `done` 这类表示状态的标签**。

**R7 排期看 `Target date` 与迭代，不用 Milestone。** Milestone 是仓库级的，管不了跨线的一期总量。

**R8 优先级只有四档。** `Urgent` 保留给线上事故；一周内出现 3 个以上 `Urgent`，说明排期本身有问题，不是大家在努力。

**R9 升级规则。**
- traffic 线当前只有 jipika 一人有产出，属于单点。该线 issue 若 3 天内既无 assignee 也无 comment，我直接处理：要么拆出能交给别人的子任务，要么承认这就是他一个人的活并写进排期。
- 谁手上同一期超过 5 件，或同时跨两条线各 3 件以上，由我调整，不需要你自己扛。
- 生产事故：先按 R9 找人，24 小时内补 issue 并标 `Urgent`。

## 每周节奏

- **周一**：我过一遍按状态分组的看板，`Todo` 里没 assignee 的当场指派或关掉。
- **周五**：我扫本周 `Done`，PR 关联写错的顺手修；顺手看有没有人在同一件事上卡超过 3 天。

成员自己要做的只有两件事：建 issue 时把验收标准写真话，交活时确认 PR 关联生效。
