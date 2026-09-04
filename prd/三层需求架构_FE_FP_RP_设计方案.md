# 三层需求架构设计方案（FE / FP / RP）

## 文档信息

| 字段     | 内容                       |
| -------- | -------------------------- |
| 版本     | 2.0.0                      |
| 日期     | 2026-09-04                 |
| 状态     | Approved                   |
| 适用范围 | 产品需求拆解模型；不含数据模型与接口设计 |

## 修订历史

| 版本  | 日期       | 作者 | 变更说明 |
| ----- | ---------- | ---- | -------- |
| 1.0.0 | 2026-08-22 |      | 初稿（20 节草稿体） |
| 2.0.0 | 2026-09-04 |      | 重新整理：合并四处重复定义；RP 改名 Rule Point；AC 并入 RP；PRD 由父节点改为来源证据；补「已拍板决策」章节 |

---

## 1. 三层模型

```text
Feature（FE）        产品功能 —— 管产品结构
  └── Function Point（FP）   功能点 —— 管规模与估算（唯一估算单位）
        └── Rule Point（RP）  规则点 —— 管详细业务规则（唯一验收单位）
```

| 层级 | 中文 | 缩写 | 定义 | 回答的问题 |
|---|---|---|---|---|
| Feature | 产品功能 | FE | 产品级功能与业务能力 | 产品有哪些主要功能？ |
| Function Point | 功能点 | FP | 可独立描述、独立估算、独立开发、独立验收的功能单元 | 每个功能包含哪些可独立实现的能力？ |
| Rule Point | 规则点 | RP | FP 内部最细粒度的产品需求与业务规则 | 每个功能具体需要满足什么要求？ |

核心原则：

> **Feature 管产品结构，Function Point 管规模与估算，Rule Point 管详细需求。**

三层各自承担的职责，互不重叠：

| 层级 | 主要负责 |
|---|---|
| FE | 产品功能地图 · Roadmap · Scope · 功能分类 · Release Planning · 需求分组 |
| FP | 功能规模 · Complexity · Estimated Effort · Duration · Cost · Progress |
| RP | 需求完整度 · 业务规则 · 开发追踪 · 需求变更 · 验收标准 · Test Coverage |

两条约束：

- **FE 不作为工时估算单位**——粒度太粗，估算一律落在 FP
- **RP 必须可判定**——每条 RP 都要能明确回答"满足 / 不满足"，否则应拆分或提升为 FP

### 1.1 完整示例

```text
FE-001 用户管理
│
├── FP-001 创建用户
│     ├── RP-001 Email 必填
│     ├── RP-002 Email 最大 255 字符
│     ├── RP-003 Email 格式必须合法
│     ├── RP-004 Email 前后空格自动 Trim
│     ├── RP-005 Email 统一转换为小写
│     ├── RP-006 同一 Tenant 内 Email 必须唯一
│     ├── RP-007 必须选择用户角色
│     ├── RP-008 默认状态为 Active
│     ├── RP-009 创建成功发送邀请邮件
│     └── RP-010 创建操作写入 Audit Log
│
├── FP-002 编辑用户
│     ├── RP-011 可以修改用户名
│     ├── RP-012 可以修改用户角色
│     └── RP-013 修改后记录操作日志
│
├── FP-003 禁用用户
│     ├── RP-014 Admin 可以禁用用户
│     ├── RP-015 禁用后用户不能登录
│     └── RP-016 记录禁用操作日志
│
└── FP-004 用户搜索
      ├── RP-017 支持 Email 搜索
      ├── RP-018 支持用户名搜索
      └── RP-019 支持用户状态筛选
```

---

## 2. 与上下游的关系

### 2.1 三层挂在产品上，PRD 是来源证据

```text
Product（产品，长期资产）
  │
  ├── Requirement（需求池条目）
  │     └── PRD 文档 v1.2 / v1.3 ─┐
  │                               │ source_ref（来源指向）
  └── Feature（FE）  ◄─────────────┤
        └── Function Point（FP） ◄─┤
              └── Rule Point（RP）◄┘
                    ├── Test Case
                    └── Development Task
```

三条规则：

1. **FE / FP / RP 属产品轴长期资产**，跨项目复用，随产品存在，不随项目结束而归档
2. **PRD 不是层级父节点**。每个节点通过 `source_ref` 指回 PRD 原文段落，说明"这条 FP 从哪来"。PRD 升版时 FE/FP/RP **保持同一身份**——这是变更影响分析能算出"RP-006 被修改了"的前提；若把 PRD 做成父节点，每次升版子节点要么被复制（ID 变、历史断）、要么被重挂（父子失效）
3. **FE/FP/RP 是 Requirement 的下游结构化产物**，不是它的替代品

### 2.2 RP 与 Task：N:M

不强制一对一。

```text
一个 RP 对应多个 Task              一个 Task 实现多个 RP
RP-006 Tenant 内 Email 唯一        TASK-200 User Validation Framework
├── TASK-101 数据查询              ├── RP-001 Email 必填
├── TASK-102 后端校验              ├── RP-002 Email 格式
└── TASK-103 单元测试              ├── RP-003 Email 长度
                                   └── RP-004 Email Trim
```

### 2.3 RP 与测试：验收标准内置于 RP

**不设独立的 Acceptance Criteria 层**——RP 的定义（可明确判定满足/不满足）本身就是验收标准的定义，再插一层是纯冗余，且要多维护一次同步。

链路：

```text
Rule Point ──► Test Case ──► Test Result
   （N:M）
```

RP 自带三个可选字段承载 Given/When/Then：

```text
RP-006
statement  同一 Tenant 内 Email 必须唯一          （必填）
given      Tenant A 已存在 user@example.com       （选填）
when       管理员再次创建同一 Email                （选填）
then       系统拒绝创建                            （选填）
     ↓
TC-1021  验证同 Tenant 重复 Email 无法创建用户
```

---

## 3. 编号

业务编号统一格式：

```text
FE-000001
FP-000001
RP-000001
```

规则：

- **不把父级编号编进子级编号**（不写 `FE001-FP003-RP006`），否则 FP/RP 在层级间移动时编号必须改写
- 编号一经分配永不复用、永不改写；作废只改状态
- 业务编号仅用于展示与引用：UI 展示、搜索、PRD 引用、评论引用、AI 引用、Task 关联、Test Case 关联、变更日志

---

## 4. 类型枚举

### 4.1 FP Type

```text
Create · Read · Update · Delete · Query · Search · Filter
Import · Export · Batch · Rule · Workflow · Integration
Notification · Report · Permission · AI · Other
```

| FP | Type |
|---|---|
| 创建用户 | Create |
| 用户列表 | Query |
| 用户搜索 | Search |
| CSV 导出 | Export |
| Google 登录 | Integration |
| AI 生成 PRD | AI |

### 4.2 RP Type

```text
Input · Output · Validation · Business Rule · Permission
State · Workflow · Error Handling · Notification · Audit
Integration · Performance · Security · Data · UI Behavior · Other
```

| RP | Type |
|---|---|
| Email 必填 | Validation |
| Tenant 内 Email 唯一 | Business Rule |
| 只有 Admin 可以删除 | Permission |
| 创建后发送 Email | Notification |
| 操作写入日志 | Audit |
| API 必须 2 秒内响应 | Performance |

---

## 5. 复杂度与权重

### 5.1 FP Complexity（斐波那契）

| Complexity | Weight |
|---|---:|
| XS | 1 |
| S | 2 |
| M | 3 |
| L | 5 |
| XL | 8 |
| XXL | 13 |

```text
FP-001 用户列表     S    2
FP-002 创建用户     M    3
FP-003 CSV 导出     M    3
FP-004 OAuth 登录   L    5
FP-005 多租户权限   XL   8

Function Point Count = 5
Weighted FP = 2+3+3+5+8 = 21
```

### 5.2 RP Complexity（三档）

RP 已是细粒度需求，不使用斐波那契。

```text
Simple = 1 · Normal = 2 · Complex = 3
```

| RP | Complexity |
|---|---:|
| Email 必填 | 1 |
| Email 格式检查 | 1 |
| Tenant 内 Email 唯一 | 2 |
| 权限验证 | 2 |
| 邀请邮件 | 3 |

> RP Score 是需求复杂度的**辅助指标**，不直接等同于开发工时。工时只从 FP 推导。

---

## 6. 开发估算

FP 是唯一 Estimate Unit。

```text
FP-003 创建用户    Complexity: M    Weight: 3
```

| Work Type | Estimate |
|---|---:|
| UI Design | 1h |
| Frontend | 4h |
| Backend | 5h |
| Database | 1h |
| Unit Test | 2h |
| E2E Test | 2h |
| Review | 1h |
| **Total** | **16h** |

```text
FP + Complexity + Work Breakdown + Historical Data = Estimated Effort
```

---

## 7. 进度与统计

**不要只显示一个笼统的 Progress。**三层各自统计，再叠加工程与测试维度：

```text
Features                 8 / 12     66.7%
Function Points         37 / 60     61.7%
Rule Points            186 / 280    66.4%

Engineering Progress               58.2%
Test Coverage                      52.3%
Release Readiness                  41.0%
```

产品 / 项目级汇总：

```text
Features                 12
Function Points          68
Weighted FP             172
Rule Points             386

Estimated Effort      1,260h
Actual Effort           830h
Estimated Cost         ¥8.2M
Forecast Cost          ¥8.8M

FP Progress              62%
RP Progress              68%
Engineering Progress     59%
Test Coverage            53%
```

> 项目进度按**已完成 FP 权重占比**计算，不按 Task 条数——`132/200 Tasks = 66%` 这类数字没有意义。

---

## 8. 需求变更影响分析

三层结构的主要价值所在。PRD 升版后，逐条比对 RP：

```text
PRD v1.2 ──► PRD v1.3

FE-001 用户管理 › FP-003 创建用户

RP-001  Unchanged
RP-002  Unchanged
RP-003  Modified   旧：Email 全系统唯一
                   新：Email 在 Tenant 内唯一
RP-011  Added      Tenant 用户数量不能超过 License 限制
```

系统据此计算：

```text
Affected Features      1
Affected FP            1
Affected RP            2
Affected Tasks         4
Affected Tests         6

Estimated Rework      13h
Estimated Cost    ¥82,000
```

> 能算出这些的前提，是 §2.1 的第 2 条：FE/FP/RP 跨 PRD 版本保持同一身份。

---

## 9. AI 自动拆解

三级拆解，**每级都有人工复核**，不允许一次性拆到底：

```text
PRD
 ↓  AI 需求分析
Feature Detection      → Human Review
 ↓
Function Point Detection → Human Review
 ↓
Rule Point Detection     → Human Review
```

```text
FE-001 用户管理          Detected Function Points: 7
FP-001 用户列表   FP-002 用户详情   FP-003 创建用户   FP-004 编辑用户
FP-005 禁用用户   FP-006 用户搜索   FP-007 CSV 导出

FP-003 创建用户          Detected Rule Points: 10
RP-001 Email 必填    RP-002 Email 格式合法   RP-003 Email 长度限制
RP-004 Email Trim    RP-005 Email 小写化     RP-006 Tenant 内唯一
RP-007 用户角色      RP-008 默认 Active      RP-009 邀请邮件
RP-010 Audit Log
```

复核操作：`Accept` / `Edit` / `Split` / `Merge` / `Delete` / `Add`

---

## 10. 已拍板决策

以下为本模型的前置结论，不再重新讨论：

| # | 决策 | 理由 |
|---|---|---|
| 1 | **Story 层废弃**，FP 取代 | Story 的定义（可独立估算、开发、验收）与 FP 完全重合；两个估算单位共存必然漂移 |
| 2 | **FP 是唯一估算单位** | 不用 Story、不用 Task、不用 Feature |
| 3 | **RP 是唯一验收单位** | Acceptance Criteria 并入 RP 的 given/when/then，不单独建层 |
| 4 | **RP = Rule Point（规则点），不叫 Requirement Point** | 避免与上游需求池的 `Requirement` 撞名——同一个词横跨模型两端 |
| 5 | **PRD 不是层级父节点** | 仅作 `source_ref` 指向的来源证据，见 §2.1 |
| 6 | **三层挂在 Product 上** | 产品轴长期资产，跨项目复用，不挂 Project |

> ⚠️ 历史文档中出现的 `Requirement Point`、独立的 `Acceptance Criteria` 层、`Requirement/PRD → Feature` 的父子结构，均为本版之前的写法，一律以本文为准。

---

**文档状态：** Approved
**最后更新：** 2026-09-04
**负责人：**
