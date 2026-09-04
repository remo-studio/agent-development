# [产品名称] - 需求索引

## 文档信息

| 字段     | 内容                  |
| -------- | --------------------- |
| 版本     | 1.0.0                 |
| 日期     | [YYYY-MM-DD]          |
| 作者     | [作者名称]            |
| 状态     | Draft / Review / Approved |
| 关联文档 | [产品概述](./prd-0001-[项目名]-product-overview.md) \| [模块设计](./prd-0002-[项目名]-module-design.md) \| [功能细节](./prd-0003-[项目名]-function-details.md) |

## 修订历史

| 版本  | 日期         | 作者     | 变更说明 |
| ----- | ------------ | -------- | -------- |
| 1.0.0 | [YYYY-MM-DD] | [作者名] | 初稿     |

---

## 使用说明

本文件是**全仓库需求 ID 的唯一权威来源**。

- 新增需求：先在本文登记 ID，再去功能细节文档写正文；顺序反了会出现重号
- ID 一经分配**永不复用、永不改写**；需求作废只改状态为 `SKIP`
- `module` / `submodule` 段必须与目录名、文件名中的对应段完全一致
- 本文只登记到 **FP（需求）层**；RP 明细写在功能细节文档里，本文只统计数量

ID 体系：

| 层级 | 格式 | 示例 |
|---|---|---|
| 模块 | `MOD-[2位]` | `MOD-01` |
| 子模块 | `MOD-[2位]-SUB-[2位]` | `MOD-01-SUB-03` |
| 需求（FP） | `fun-[module]-[submodule]-[4位]` | `fun-user-auth-0001` |

状态取值：`TODO` / `IN_PROGRESS` / `DONE` / `SKIP`

代码中引用：

```text
// @req        fun-user-auth-0001
// @submodule  MOD-01-SUB-01
// @module     MOD-01
```

---

## 模块总览

| 模块 ID | 模块 | 目录 | 子模块数 | 需求数 | 完成 |
|---|---|---|---:|---:|---:|
| MOD-01 | [模块1名称] | `[module]/` | 3 | 12 | 0% |
| MOD-02 | [模块2名称] | `[module]/` | 2 | 8 | 0% |
| | **合计** | | **5** | **20** | **0%** |

---

## MOD-01 · [模块1名称]

| 子模块 ID | 子模块 | 需求 ID | 需求描述 | 类型 | 复杂度 | RP 数 | 状态 | 文档 |
|---|---|---|---|---|---|---:|---|---|
| MOD-01-SUB-01 | [子模块名] | `fun-[module]-[submodule]-0001` | [需求描述] | Create | M | 6 | TODO | [→](./prd-0003-[项目名]-function-details.md#fp-1--功能点名称) |
| MOD-01-SUB-01 | [子模块名] | `fun-[module]-[submodule]-0002` | [需求描述] | Query | S | 4 | TODO | [→](./prd-0003-[项目名]-function-details.md#fp-2--功能点名称) |
| MOD-01-SUB-02 | [子模块名] | `fun-[module]-[submodule]-0003` | [需求描述] | Update | M | 5 | TODO | [→](./prd-0003-[项目名]-function-details.md#fp-3--功能点名称) |

---

## MOD-02 · [模块2名称]

| 子模块 ID | 子模块 | 需求 ID | 需求描述 | 类型 | 复杂度 | RP 数 | 状态 | 文档 |
|---|---|---|---|---|---|---:|---|---|
| MOD-02-SUB-01 | [子模块名] | `fun-[module]-[submodule]-0004` | [需求描述] | Workflow | L | 9 | TODO | [→](./prd-0003-[项目名]-function-details.md) |

---

## 作废需求

作废的需求保留在此，**不删除、不复用 ID**。

| 需求 ID | 需求描述 | 作废日期 | 原因 |
|---|---|---|---|
| `fun-[module]-[submodule]-000X` | [需求描述] | [YYYY-MM-DD] | [为什么不做了] |

---

## 统计

| 指标 | 数量 |
|---|---:|
| 模块（Feature） | [N] |
| 子模块 | [N] |
| 需求 / Function Points | [N] |
| Weighted FP | [N] |
| Rule Points | [N] |
| Estimated Effort | [N]h |
| 完成率（按 FP 权重） | [N]% |

> 进度按**已完成 FP 权重占比**计算，不按需求条数、更不按 Task 条数。

---

**文档状态：** [状态]
**最后更新：** [YYYY-MM-DD]
**负责人：** [负责人名称]
