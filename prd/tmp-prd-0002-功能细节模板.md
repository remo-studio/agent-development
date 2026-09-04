# [产品名称] - 功能细节

## 文档信息

| 字段     | 内容                  |
| -------- | --------------------- |
| 版本     | 1.0.0                 |
| 日期     | [YYYY-MM-DD]          |
| 作者     | [作者名称]            |
| 状态     | Draft / Review / Approved |
| 关联文档 | [产品概述](./prd-0001-[项目名]-product-overview.md) \| [模块设计](./prd-0002-[项目名]-module-design.md) \| [需求索引](./prd-0000-[项目名]-requirement-index.md) |

## 修订历史

| 版本  | 日期         | 作者     | 变更说明 |
| ----- | ------------ | -------- | -------- |
| 1.0.0 | [YYYY-MM-DD] | [作者名] | 初稿     |

---

## 使用说明

本文档按 **FE → FP → RP** 三层结构编写，模型定义见 [三层需求架构设计方案](./三层需求架构_FE_FP_RP_设计方案.md)。

- **一个 `##` 章节 = 一个模块（Feature）**，标题须与[模块设计](./prd-0002-[项目名]-module-design.md)一致
- **一个 `###` 小节 = 一个 Function Point**，FP 是唯一估算单位
- **每个 FP 下列出它的 Rule Point**，RP 是唯一验收单位，验收标准写在 RP 的 Given/When/Then 里，**不单独写"接受度标准"章节**
- 每条 RP 必须可判定"满足 / 不满足"；判定不了就拆分，或提升为独立 FP
- 所有需求 ID 先在[需求索引](./prd-0000-[项目名]-requirement-index.md)登记，再写进本文

> 本模板不使用「用户故事（Story）」——Story 已废弃，其职责由 FP 承担。

---

## 目录

1. [[模块1名称]](#模块1名称)
2. [[模块2名称]](#模块2名称)
3. [详细交互流程](#详细交互流程)
4. [错误处理与边界情况](#错误处理与边界情况)

---

## [模块1名称]

**Feature ID：** `MOD-[2位]-SUB-[2位]`

[一句话说明该模块交付什么业务能力]

### Function Points 一览

| FP | 需求 ID | 功能点 | 类型 | 复杂度 | 权重 | RP 数 | 估算 | 状态 |
|---|---|---|---|---|---:|---:|---:|---|
| FP-1 | `fun-[module]-[submodule]-0001` | [功能点名称] | Create | M | 3 | 6 | 16h | TODO |
| FP-2 | `fun-[module]-[submodule]-0002` | [功能点名称] | Query | S | 2 | 4 | 10h | TODO |
| FP-3 | `fun-[module]-[submodule]-0003` | [功能点名称] | Update | M | 3 | 5 | 14h | TODO |
| | | **合计** | | | **8** | **15** | **40h** | |

> FP 类型取值：`Create` `Read` `Update` `Delete` `Query` `Search` `Filter` `Import` `Export` `Batch` `Rule` `Workflow` `Integration` `Notification` `Report` `Permission` `AI` `Other`
> 复杂度与权重：`XS=1` `S=2` `M=3` `L=5` `XL=8` `XXL=13`
> 状态取值：`TODO` / `IN_PROGRESS` / `DONE` / `SKIP`

---

### FP-1 · [功能点名称]

| 字段 | 值 |
|---|---|
| 需求 ID | `fun-[module]-[submodule]-0001` |
| 类型 | Create |
| 复杂度 / 权重 | M / 3 |
| 估算 | 16h |
| 来源 | [PRD 段落或需求来源，对应 source_ref] |

**功能描述**

[这个功能点做什么，交付给谁，完成后用户能得到什么]

#### Rule Points

| RP ID | 规则点 | 类型 | 复杂度 |
|---|---|---|---:|
| RP-001 | [业务规则，必须可判定满足/不满足] | Validation | 1 |
| RP-002 | [业务规则] | Business Rule | 2 |
| RP-003 | [业务规则] | Permission | 2 |
| RP-004 | [业务规则] | Notification | 3 |
| RP-005 | [业务规则] | Audit | 1 |
| RP-006 | [业务规则] | Error Handling | 2 |

> RP 类型取值：`Input` `Output` `Validation` `Business Rule` `Permission` `State` `Workflow` `Error Handling` `Notification` `Audit` `Integration` `Performance` `Security` `Data` `UI Behavior` `Other`
> RP 复杂度：`Simple=1` `Normal=2` `Complex=3`（仅作需求复杂度参考，不用于推导工时）

#### 验收标准

每条需要验证的 RP 展开为 Given / When / Then：

```gherkin
场景: RP-002 [规则点简述]
  给定 [前置条件]
  当 [用户操作]
  那么 [预期结果]

场景: RP-003 [规则点简述]
  给定 [前置条件]
  当 [用户操作]
  那么 [预期结果]
```

#### 前端要点

- [关键组件 / 交互 / 状态]

#### 后端要点

- [关键业务逻辑 / 依赖]

---

### FP-2 · [功能点名称]

| 字段 | 值 |
|---|---|
| 需求 ID | `fun-[module]-[submodule]-0002` |
| 类型 | Query |
| 复杂度 / 权重 | S / 2 |
| 估算 | 10h |
| 来源 | [PRD 段落] |

**功能描述**

[…]

#### Rule Points

| RP ID | 规则点 | 类型 | 复杂度 |
|---|---|---|---:|
| RP-007 | [业务规则] | Validation | 1 |
| RP-008 | [业务规则] | Permission | 2 |

#### 验收标准

```gherkin
场景: RP-007 [规则点简述]
  给定 [前置条件]
  当 [用户操作]
  那么 [预期结果]
```

---

### FP-3 · [功能点名称]

[同上结构]

---

## [模块2名称]

**Feature ID：** `MOD-[2位]-SUB-[2位]`

[同上结构：Function Points 一览 → 各 FP → Rule Points → 验收标准]

---

## 详细交互流程

> 跨 FP 的流程画在这里；流程中的每一个判定分支，都应在对应 FP 下有一条 RP 承载。

### 流程 1: [流程名称]

```text
┌─────────────────────────────┐
│ [起始步骤]                  │
└─────────────────────────────┘
              │
              ▼
┌─────────────────────────────┐
│ [步骤 2]                    │
│ - [子步骤]                  │
└─────────────────────────────┘
              │
   ┌──────────┼──────────┐
   ▼          ▼          ▼
[分支A]    [分支B]    [分支C]
```

| 分支 | 触发条件 | 对应 RP |
|---|---|---|
| 分支A | [条件] | RP-002 |
| 分支B | [条件] | RP-003 |
| 分支C | [条件] | RP-006 |

### 流程 2: [流程名称]

[同上结构]

---

## 错误处理与边界情况

> 每一条错误处理与边界规则，都必须在对应 FP 下登记为一条 RP（类型 `Error Handling` 或 `Validation`），本节只做集中说明。

### 错误场景 1: [错误场景描述]

| 项 | 内容 |
|---|---|
| 对应 RP | RP-006 |
| 触发条件 | [具体情况] |
| 检测方式 | [如何检测] |
| 用户提示 | [用户看到什么] |
| 恢复方式 | [如何恢复] |

### 边界情况 1: [边界情况描述]

| 项 | 内容 |
|---|---|
| 对应 RP | RP-00X |
| 边界条件 | [具体边界] |
| 前端行为 | [前端如何处理] |
| 后端校验 | [后端如何验证] |
| 用户体验 | [用户看到的提示] |

---

## 统计

| 指标 | 数量 |
|---|---:|
| Function Points | [N] |
| Weighted FP | [N] |
| Rule Points | [N] |
| Estimated Effort | [N]h |

---

**文档状态：** [状态]
**最后更新：** [YYYY-MM-DD]
**负责人：** [负责人名称]
