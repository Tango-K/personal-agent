# Lingxi 项目面试问答聊天记录

> 记录日期：2026-09-05  
> 记录范围：本次对话中的用户需求、面试问题和已生成答复。系统指令、运行环境信息、工具调用过程不在记录范围内。  
> 内容口径：公开材料未展示不代表实际没有实现；基于公开材料补充的合理工程方案统一标记为“补充推演”。

## 目录

1. [分析原则](#一分析原则)
2. [影石创新面经](#二影石创新面经)
3. [蚂蚁 AI 应用开发面经](#三蚂蚁-ai-应用开发面经)
4. [蚂蚁 Agent 开发一面](#四蚂蚁-agent-开发一面)
5. [ReAct、会话与工具隔离问题](#五react会话与工具隔离问题)
6. [阿里 AI 应用开发一面](#六阿里-ai-应用开发一面)
7. [小红书 Agent 开发二面](#七小红书-agent-开发二面)
8. [字节 Agent 开发一面及答复](#八字节-agent-开发一面及答复)
9. [TikTok Shop AI Agent 一面及答复](#九tiktok-shop-ai-agent-一面及答复)
10. [商汤 Agent 二面及答复](#十商汤-agent-二面及答复)

---

## 一、分析原则

### 用户需求

先探索并理解项目的架构方案，再探索理解不同模块的具体实现方案。后续所有分析回答遵循以下原则：

- 公开材料中没有，不代表实际没有实现，可能只是未上传到公开材料中。
- 基于已有公开材料，让分析和语言描述合理、有逻辑并能够闭环。
- 可以补充公开材料中没有展示的部分。
- 如果补充内容不是公开材料中真实可确认的实现，需要在回答前标注提醒。

### 后续回答的统一标记

- **【公开实现可确认】**：代码仓库可以直接验证。
- **【公开方案可确认】**：README、技术报告等公开材料可以验证。
- **【补充推演】**：公开材料未重点展示，但与 Lingxi 架构兼容、工程上能够闭环的方案。

---

## 二、影石创新面经

### 用户问题

结合 Lingxi 项目，回答以下问题：

1. 项目中出现模型幻觉时，如何处理？
2. Prompt 是如何设计和优化的？
3. 除了基础约束外，还使用过哪些 Prompt 优化方法？
4. Agent 的短期记忆和长期记忆如何实现？
5. 如果从零设计一个 Agent，需要考虑哪些核心模块？
6. 工具或 API 调用超时、报错时，系统如何处理？
7. 是否考虑过让大模型参与异常处理或自动恢复？
8. Token 消耗过快时，如何排查和优化？
9. 是否写过 Skill？通常如何设计和评估一个 Skill？
10. 常用哪些 AI Coding 工具？使用 Claude Code 时有哪些习惯？
11. 上下文为什么需要压缩？压缩后丢失细节如何处理？
12. 如何理解 Spec Coding 和 Harness？
13. Harness 为什么可能提升 Agent 的任务完成能力？

> 本轮问题在当时的聊天中仅完成了项目探索，尚未生成独立成稿答复。

---

## 三、蚂蚁 AI 应用开发面经

### 用户问题

结合 Lingxi 项目，回答以下问题：

1. 相比纯文本，Markdown 的层级结构对检索有什么好处？
2. Agent 是如何识别并选择正确工具的？是基于工具名称还是功能描述？
3. 向量检索和关键词检索各自有什么优缺点？
4. 为什么 RAG 比直接问大模型更能减少事实性错误？
5. 什么是 Query Rewrite？能解决用户提问中的什么问题？
6. CoT 的原理是什么？为什么能让模型处理复杂任务？Lingxi 中有用到 CoT 吗？
7. 模型进行工具调用时，输出的是直接结果，还是包含参数的 JSON 字符串？
8. 系统提示词和用户提示词在 Agent 约束力上有什么区别？
9. 开发 Agent 应用时，如何判断任务应该用 7B 小模型还是 70B 大模型？
10. 如果 Agent 总是重复执行同一个错误的工具调用，应该从哪个环节修复？

> 本轮问题在当时的聊天中仅完成了项目探索，尚未生成独立成稿答复。

---

## 四、蚂蚁 Agent 开发一面

### 用户问题

#### RAG 项目深挖

1. RAG 项目有何独到之处？
2. 对比 Graph RAG 与问题拆解方案，差异在哪里？
3. 为什么不直接用知识图谱做检索？
4. 搜索的召回率和准确率如何量化评测？
5. 评测集如何构建？评测方法论是什么？

#### Deep Research 场景设计

6. 面对海量内部文档做“组织洞察与风险识别”，如何设计系统？
7. 核心卡点在哪里？Query 改写不准怎么分析与优化？
8. 面对海量全网资料，模型需要具备哪些核心能力？

#### 上下文压缩与记忆架构

9. 压缩前后的信息损耗怎么评估？
10. 记忆的分类、写入、去重与情景提取怎么设计？
11. 记忆去重冲突，例如前后言行不一，如何处理与判断？
12. 记忆衰退周期靠模型自主判断还是规则？

#### 框架对比与 AI 编码

13. LangChain 和 LangGraph 等开源框架有何优劣势？
14. 内部 Workflow 编排能彻底解决产品问题吗？
15. 如何用 Claude Code 快速接手并迭代新项目？

#### 开放讨论

16. 如何让 Agent 自主运行长程任务？

> 本轮问题在当时的聊天中仅完成了项目探索，尚未生成独立成稿答复。

---

## 五、ReAct、会话与工具隔离问题

### 用户问题

1. 多个会话共用同一个全局变量会出现哪些隐患？
2. ReAct 框架中的异步逻辑一般怎么实现？
3. Agent 经常出现幻觉，有哪些可行的缓解手段？
4. 从代码耦合角度谈谈 Tool 工具隔离设计。
5. 工具任务需要运行半小时，应该怎么处理？
6. 直接把 Session ID 传入内部会带来哪些潜在问题？
7. 读并行、写加锁的设计有什么优势？
8. Tool 工具完全不考虑 Session 会话是否可行？
9. ReAct 和 CoT 的核心差别是什么？
10. 工具返回体量巨大的 JSON 会存在哪些局限？
11. 在业务会话中如何做好不同会话之间的工具隔离？
12. 带状态的 Tool 怎么保证只执行一次？
13. 子 Agent 执行长耗时工具任务，用什么方案通信比较合适？
14. 主 Agent 如何获取子 Agent 的执行过程和对话进度？

> 本轮问题在当时的聊天中仅完成了项目探索，尚未生成独立成稿答复。

---

## 六、阿里 AI 应用开发一面

### 用户问题

1. 大模型落地时，如何权衡微调与 Prompt 工程的性价比？什么情况下必须使用 SFT，什么情况下 RAG 可以解决？
2. 通用 Embedding 在专业术语上效果较差，除了继续预训练，还有哪些低成本方案可以提升语义匹配度？
3. 长文本推理时显存爆炸，除了量化，还有哪些 KV Cache 优化策略？
4. 如何判断一个需求应该写死代码，还是让模型自由发挥？边界在哪里？
5. 如何保证模型输出的 JSON 格式绝对合法？解析失败后的重试机制如何避免浪费 Token？
6. 并行调用多个 API 时，如果一个超时，是等待还是丢弃？结果如何合并才能不打断主流程？

> 本轮问题在当时的聊天中仅完成了项目探索，尚未生成独立成稿答复。

---

## 七、小红书 Agent 开发二面

### 用户问题

1. 多 Agent 如何进行上下文和状态隔离？
2. Workflow 和 Lingxi 架构有什么区别？
3. 如果用户通过 Prompt Injection 诱导 Agent 泄露 System Prompt 或绕过指令，应该如何防范？
4. Agent 不限制工具调用会产生哪些风险？Lingxi 对工具调用做了哪些限制？
5. 如果部分 Agent 未来需要扩展到大量设备，同时覆盖本地 Agent 和云端 Agent，如何设计？

> 本轮问题在当时的聊天中仅完成了项目探索，尚未生成独立成稿答复。

---

## 八、字节 Agent 开发一面及答复

### 用户问题

1. 提示词模板怎么搭建？分层逻辑是什么？
2. 多 Agent 之间怎么分工和通信？
3. 分支覆盖率统计原理是什么？代码插桩怎么实现？
4. 有没有做前置静态分析？怎么判断生成代码有效？怎么优化覆盖率指标？
5. 哪些场景会拉低生成准确度和测试覆盖率？AST、LSP 是什么？AST、LSP 都无法处理的单测代码怎么过滤？
6. Mock 机制是什么？怎么实现？踩过哪些坑？
7. 多维度查询改写怎么实现？改写缺少信息，需要用户补充时，技术上怎么落地？
8. Agent Skills 的底层原理是什么？

### 答复

#### 1. 提示词模板和分层逻辑

【公开实现可确认】Lingxi 的 Prompt 不是一整段自然语言，而是按照“稳定约束—阶段任务—动态上下文—输出协议”分层。

通常分为六层：

1. 全局约束层：身份、安全边界、自动化要求、失败处理原则。
2. 团队契约层：明确有哪些 Agent，以及各自职责。
3. 角色边界层：当前 Agent 只做什么、明确不做什么。
4. 阶段流程层：把复杂任务拆成可执行阶段。
5. 动态上下文层：Issue、历史开发知识、上游产物和仓库证据。
6. 输出协议层：固定结构、必填字段、结束条件和验收标准。

Lingxi 中：

- Problem Decoder 负责“问题是什么、根因在哪”，输出 `issue_analysis`。
- Solution Mapper 负责 Explore、Reproduce、Plan，输出 `code_change_plan`。
- Problem Solver 负责 Implement、Verify、Final Review。
- 每次工具调用后要求形成 Observation、Alternative Action、Next Action。

【补充推演】生产中还会把稳定 Prompt 放在前面以便缓存，用标签隔离不可信内容，并将 Prompt 作为具备输入 Schema、输出 Schema、错误码和评测集的接口管理。

#### 2. 多 Agent 分工和通信

【公开方案可确认】Lingxi 按软件修复的信息流拆分角色：

```text
Issue
  → Problem Decoder(s)
  → Aggregator
  → Solution Mapper
  → Problem Solver
  → Reviewer
       ↘ 失败后返回修复阶段
```

- Decoder 回答 What、Where。
- Aggregator 合并多个 Decoder 的分析。
- Mapper 回答 How、Why，生成修改计划和复现脚本。
- Solver 执行修改和测试。
- Reviewer 判断补丁是否真正解决问题。
- Supervisor 控制路由、重试和生命周期。

物理通信依赖 LangGraph 的 State、Command 和 Edge；逻辑通信主要传递 `issue_analysis`、`code_change_plan`、Patch 和测试报告等阶段产物，而不是持续传递完整历史。

#### 3. 分支覆盖率和代码插桩

【补充推演】分支覆盖率统计控制流图中有多少可执行边被测试走过：

```text
BranchCoverage = 已执行分支边数 / 可执行分支边总数
```

实现分为：

1. 根据 AST、字节码或 IR 构建 CFG，为分支边分配 ID。
2. 在跳转边或基本块插入 Probe。
3. 运行测试时记录命中位图。
4. 合并不同进程和测试的结果，映射回文件和行号。

Python 可以使用运行时 Trace 记录 `(from_line, to_line)`，Java 可以通过 Java Agent 改写字节码。短路表达式、异常边、生成器、异步函数和子进程需要单独处理。

#### 4. 静态分析、有效性与覆盖率优化

【公开实现可确认】Lingxi 有两类分析：

- 生成前用 Tree-sitter 提取文件、函数和方法结构，建立检索索引。
- 编辑后使用 Flake8 致命错误检查、Python `compile()` 和 Tree-sitter Syntax Check，并过滤已有错误，只报告本次修改新增的问题。

代码有效性通过分层 Gate 判断：Parse/Compile、Lint/Type、Diff Check、Reproduction、Related Tests、Regression Tests、Reviewer。

覆盖率不能只看总行覆盖率，应结合 Changed-code Branch Coverage、Mutation Score、原始问题复现、回归稳定性和新增覆盖率。合理循环为：运行基线测试，找出修改代码中的未覆盖分支，生成针对性测试，动态过滤，只保留产生新增覆盖或能杀死 Mutation 的测试。

#### 5. AST、LSP 与单测过滤

容易降低生成准确度和覆盖率的场景包括：需求模糊、动态导入、反射、Monkey Patch、并发、随机数、网络、原生扩展、环境版本不一致、弱断言、过度 Mock 和检索版本错误。

- AST 是抽象语法树，描述代码的语法结构。
- LSP 是编辑器与语言服务器之间的协议，可提供 Definition、Reference、Diagnostics、Type 等语义信息，但不是具体分析算法。

【补充推演】AST 和 LSP 无法判断的生成单测，需要放进隔离沙箱动态过滤：

1. 编译和测试收集。
2. 在超时、CPU、内存和网络限制下运行。
3. 通过 Coverage Trace 判断是否进入目标函数。
4. 检查有效断言。
5. 使用 Mutation Test 检查断言敏感度。
6. 重复运行识别 Flaky Test。
7. 分类为 Invalid、Environment Missing、Irrelevant 或 Flaky，而不是全部删除。

#### 6. Mock 机制

【补充推演】Mock 的本质是通过依赖替换，把不可控组件换成可配置、可记录行为的 Test Double。

优先级通常是依赖注入加 Fake、Stub、Mock/Spy、Monkey Patch，以及 Fake Server 或 Record-Replay。

常见问题：

- Patch 错命名空间；
- Mock 了核心被测逻辑；
- 返回结构和真实 API 不一致；
- 异步函数未使用 AsyncMock；
- 全局 Patch 未恢复；
- 只检查调用次数，不验证业务结果；
- 真实接口变化后 Mock 没同步。

应使用 `spec/autospec`，并至少保留 Contract Test 或集成测试。

#### 7. 多维度 Query Rewrite

【公开方案可确认】Lingxi 的 Decoder 会把 Issue 拆成 Problem、Root Cause、Current Behavior 和 Expected Behavior；V2 按阶段检索历史轨迹，使用规则匹配和 LLM Verifier 排除不适用知识。

【补充推演】Query Rewrite 可以输出结构化 QueryPlan，包含原始锚点、精确查询、语义查询、结构查询、历史查询、Metadata Filter 和缺失槽位。不同查询分别路由到关键词、向量、结构和历史检索，再通过 RRF 或 Reranker 合并。

缺失信息时使用状态机：

```text
UNDERSTAND
→ SLOT_CHECK
→ 信息是否阻塞检索？
   ├─ 否：带明确假设继续
   └─ 是：CLARIFY_USER → RESUME → RETRIEVE
```

可以使用 LangGraph `interrupt()` 保存 `pending_query_plan`，用户补充后从 Checkpoint 恢复。

#### 8. Agent Skills 底层原理

Agent Skill 不是模型微调，也不是一个更大的 Tool，而是可路由、按需加载、可版本化的能力包。

一般包含名称、描述、适用场景、版本、`SKILL.md`、References、Scripts 和 Assets。运行时先加载少量元数据，路由命中后再加载完整指令，执行到具体步骤时才读取 References 或运行 Script，这种机制称为渐进式披露。

- Tool 解决“能执行什么原子动作”。
- Skill 解决“什么时候、按什么流程组合工具”。
- Agent 负责理解目标、选择 Skill、调用 Tool、管理状态并验证结果。

映射到 Lingxi，可以把仓库 Issue 修复流程封装成 Skill，将 Decoder、Mapper、Solver、Reviewer 工作流写入 `SKILL.md`，把测试、静态分析和覆盖率脚本放入 Scripts，并配置相应工具白名单和沙箱权限。

---

## 九、TikTok Shop AI Agent 一面及答复

### 用户问题

1. 更希望从事 Agent 底层技术研发，还是具体业务场景中的 Agent 应用开发？
2. 在 Lingxi 项目中的核心产出是什么？
3. 认为项目最大的挑战是什么？
4. 多跳推理中，第一步只获取部分信息是否会导致后续所需信息丢失？如何解决？
5. 如何衡量推理过程的质量和重要性？
6. Lingxi 整体使用了什么技术栈？
7. 每天结算约一千万笔订单，部分订单因不同原因未完成结算，如何设计智能巡检或运维 Agent？
8. 该场景应该使用单 Agent 还是多 Agent？为什么？
9. 系统需要具备哪些 Skill？
10. 产品最终应该具备怎样的形态和能力？
11. 如何约束 Agent 的判断，避免结算状态和未结算原因误判？

### 答复

#### 1. 职业方向选择

如果二选一，现阶段更倾向于复杂业务场景中的 Agent 应用开发，但不是只做 Prompt 封装，而是重点关注 Agent Engineering，包括任务拆解、上下文管理、工具协议、状态机、评测和可靠性。

Lingxi 的经验说明，Agent 效果不只取决于底模，还取决于任务是否拆对、上下文是否准确、工具是否结构化、中间状态能否回溯，以及验证和纠错闭环是否可靠。业务场景还有明确的反馈指标，更容易形成数据闭环。

#### 2. 项目核心产出

【补充表述，需根据实际分工取舍】核心产出可以概括为：把一次性代码生成变成分阶段、可验证、可迭代的多 Agent 软件修复流程。

主要包括：

- Agent 任务拆解和接口契约；
- Tree-sitter、向量检索和历史知识组成的上下文系统；
- 阶段产物传递和消息压缩；
- 复现、精确编辑、静态检查、测试和 Reviewer 组成的可靠性闭环。

#### 3. 最大挑战

【公开方案可确认】最大挑战是控制多阶段任务中的错误传播，而不是代码生成本身。

```text
Decoder 根因定位错误
→ Mapper 为错误根因生成完整方案
→ Solver 高质量实现错误方案
→ Patch 看起来合理，但没有解决真实问题
```

解决方法包括清晰契约、多 Decoder 分析和聚合、先复现再计划、结构化阶段产物、保留证据和未决问题、测试与 Reviewer 验证，以及 V2 对历史经验进行适用性校验。

#### 4. 多跳推理的信息丢失

真正的风险不是第一跳信息不完整，而是系统过早把部分观察固化成事实。中间信息应拆成 Fact、Hypothesis、Unknown 和 Evidence，并记录来源、查询条件和时间。

应维护未决问题列表；摘要保留结论和证据引用；每一步允许修正或否定旧假设；采用阶段相关检索；使用 Checkpoint 回退；关键假设进行多路验证；阻塞信息通过 Human-in-the-loop 补充。

#### 5. 推理质量和重要性

不直接评价自由文本 CoT，而评价可观察轨迹：工具调用、证据、状态变化和最终结果。

质量分为：结果质量、证据质量、过程进展、鲁棒性、效率和安全。Lingxi 可以分别评估 Decoder 根因定位、Mapper 计划一致性和复现成功率、Solver Patch 与测试、Reviewer 的错误拦截率。

步骤重要性可以近似表示为：

```text
Importance = 对成功率的增量贡献 × 下游依赖度 × 业务风险
```

可以通过 Ablation、Counterfactual Replay、Information Gain 和 Dependency Centrality 评估。

#### 6. Lingxi 技术栈

Lingxi 使用 Python 3.12+、LangGraph、LangChain、Anthropic/OpenAI/DeepSeek 模型适配、TypedDict/Pydantic、Tree-sitter、Python AST、ChromaDB、OpenAI Embedding、Ripgrep、OHEditor、SWE-ReX、Docker、GitPython、Flake8、Pytest、Ruff、Mypy、SQLite Cache、DiskCache、LangSmith 和 LangGraph Studio。

#### 7. 千万级结算巡检 Agent

核心原则是：不能让 LLM 逐笔扫描一千万订单，也不能让 LLM 直接判定账务事实。系统应分为确定性数据面和 Agent 推理面。

```text
订单/支付/退款/履约/结算/账本/打款数据
                  ↓
       确定性对账与状态计算引擎
                  ↓
       异常聚合、分群和影响面计算
                  ↓
    Agent 诊断、查证、解释和生成方案
                  ↓
      独立校验、报告、工单和审批
```

首先建立统一状态机，例如 `PAID → SETTLEMENT_ELIGIBLE → CALCULATED → LEDGER_POSTED → PAYOUT_CREATED → COMPLETED`，同时保留 NOT_DUE、RISK_HOLD、REFUND_PENDING、FAILED、INCONSISTENT 和 UNKNOWN。

流处理做小时级巡检，批处理做日终全量对账；通过事件时间、Watermark、幂等和去重处理延迟及重复事件。先按照国家、商家、币种、原因码、账龄、服务版本等维度聚合成异常簇，再让 Agent 分析少量摘要和代表性样本。

Lingxi 工作流可以迁移为 Settlement Decoder、Diagnosis Mapper、Investigation Executor、Reviewer/Auditor 和 Report Agent。大模型默认只读，批量补单和账务修正必须经过审批、幂等、影响面预览和回滚。

#### 8. 单 Agent 还是多 Agent

推荐“确定性 Workflow + 少量专业 Agent”的混合架构。千万级规模本身不是使用多 Agent 的原因，真正原因是诊断和审核职责冲突、权限不同、需要独立验证，以及不同异常簇可以并行处理。

建议保留 Triage/Decoder、Diagnosis Agent、Verifier/Auditor 和 Report/Runbook Agent。Agent 之间通过结构化 Incident State 通信。Verifier 必须重新执行确定性查询或使用独立证据，不能仅靠换一个 Prompt 实现所谓独立审核。

#### 9. 所需 Skill

业务理解类包括订单生命周期、结算资格、区域币种税费、退款补贴和账本状态解释。

诊断类包括结算对账、异常分群、SQL 查询、日志与 Trace 排查、配置和发布变更关联、历史 Incident 检索、根因假设与证据验证。

治理类包括 Incident 报告、工单路由、Runbook、脱敏、权限合规、审批和回滚。

每个 Skill 需要定义触发条件、输入、前置条件、步骤、工具白名单、输出 Schema、弃答条件、权限、测试集和版本。

#### 10. 产品形态

最终产品不应只是聊天机器人，而应是“结算质量控制台 + 调查 Copilot + 受治理的自动化处置平台”。

产品能力包括主动巡检、可视化控制台、对话式调查和受控处置。成熟度从指标展示、异常解释、处理建议逐渐发展到低风险可回滚动作的自动执行；高风险账务修正始终保留人工审批。

#### 11. 约束结算判断

核心原则是：状态由确定性规则和权威数据源计算，大模型只能解释状态，不能创造状态。

系统采用多状态而不是二分类：SETTLED_CONFIRMED、UNSETTLED_CONFIRMED、NOT_DUE、PENDING_EXPECTED、INCONSISTENT、UNKNOWN。

每个判断必须带 `as_of_time`、Watermark 和规则版本。原因只能从受控枚举中选择，并配置最低证据要求。证据不足只能输出 UNKNOWN 或候选原因。

输出应包含状态、截止时间、原因码、规则版本、证据引用、置信度、矛盾项、建议动作和是否需要人工审核。Auditor 重新执行关键查询，并从结算、账本、打款系统交叉核对。上线前通过历史标注集、混淆矩阵、金额加权误判率和 Shadow Run 评估。

---

## 十、商汤 Agent 二面及答复

### 用户问题

1. Skill 和 RAG 是走不同 Pipeline 还是统一 Pipeline？怎么路由？
2. RAG 是自己实现还是使用开源框架？具体怎么搭？
3. 每个 Agent 的提示词怎么设计？
4. MCP 做了哪些封装？任务队列的幂等、限流怎么实现？
5. 幂等限流在 MCP 内部还是 Harness 层保证？限流维度怎么设计？用户系统和管理后台怎么搭建？
6. 代码题：使用 Python SDK 简单实现 MCP Server 工具注册。

### 答复

#### 1. Skill 和 RAG 的 Pipeline 与路由

入口和编排统一，执行子流程不强行统一。

- Skill 解决“这类任务按什么流程做”。
- RAG 解决“当前任务缺少哪些外部事实”。
- Tool/MCP 解决“具体执行什么动作”。

许多任务实际走 `Skill + RAG + Tool`：路由器先选择 Skill，Skill 执行过程中按需调用 RAG，再根据检索证据调用 Tool，结果回到统一 Agent State。

路由一般包括硬规则过滤、基于 Skill 描述的语义召回和 LLM 结构化选择。低置信度时补充检索或请求用户澄清。结合 Lingxi，解释代码可能只走 RAG，而修复 GitHub Issue 会触发完整 Skill，并在 Decoder、Mapper 阶段继续调用代码 RAG。

#### 2. RAG 的实现方式

【公开实现可确认】Lingxi 使用开源组件搭底座，自定义索引、检索组织和 Agent 集成逻辑。

离线阶段扫描 Python、Java 文件，同时构建文件级和 Tree-sitter 函数级 Document，使用 `RecursiveCharacterTextSplitter` 切分，通过 OpenAI `text-embedding-3-small` 生成向量并写入 Chroma。

在线阶段通过 `search_relevant_files(query, k=10)` 召回文件和函数片段，再由 LLM 生成相关性解释。精确错误码、类名和函数名可以使用 Ripgrep 补充。

【补充推演】生产版可以增加 Query Rewrite、混合检索、Metadata Filter、RRF、Reranker、增量索引、引用定位和完整检索评测。

#### 3. Agent Prompt 设计

Lingxi 将 Prompt 设计为包含 Role、Scope、Input、Process 和 Output Contract 的接口契约。

- Decoder 负责 What、Where，输出 `issue_analysis`，禁止创建复现脚本。
- Mapper 负责 Explore、Reproduce、Plan，输出 `code_change_plan`，禁止直接修改正式代码。
- Solver 负责 Implement、Verification、Final Review。
- Supervisor 使用结构化 Router 控制顺序和重试。
- Reviewer 根据 Issue、Patch 和测试结果返回 FIXED 或失败反馈。

所有 ReAct Agent 在工具调用后形成 Observation、Alternative Action 和 Next Action，以区分工具事实与模型判断。

#### 4. MCP、幂等和限流

【补充推演】公开 Lingxi 主要使用 LangChain Tool，生产化时可以将 `view_directory`、`view_file_content`、`search_relevant_files`、`str_replace_editor` 和 `run_shell_cmd` 封装成 MCP Tool。

MCP 层负责工具注册发现、Schema、参数校验、统一返回、权限上下文、超时重试、熔断、审计、结果截断和工具版本。长任务抽象成 `submit_job`、`get_job_status`、`get_job_result` 和 `cancel_job`。

幂等键应包含 Tenant、用户、工具名、工具版本、业务键和规范化参数 Hash，不能只使用 Session ID。任务通过数据库唯一索引、状态机、Transactional Outbox、Worker CAS、Lease、Heartbeat、结果持久化和 DLQ 实现有效一次执行。存在业务副作用时，下游也必须接受同一业务幂等键。

限流使用 Redis Lua Token Bucket 或 Sliding Window，维度包括全局、Tenant、用户、应用、Skill、Tool、外部服务、模型、资源、并发任务、Token 和费用预算。

#### 5. 幂等限流的责任层与管理系统

应采用分层防御：

- API Gateway 负责 IP、客户端和连接级保护。
- Harness 负责用户预算、任务幂等、跨工具并发和全局调度。
- MCP Gateway 负责身份透传、协议适配和统一观测。
- MCP Server 负责工具级 QPS、输入校验、下游保护和副作用幂等。
- 业务系统负责最终事务和业务唯一键。

全局权威应位于 Harness，因为它能够看到一个任务的完整 Agent、工具、Token 和成本；MCP Server 只能看到单次调用。

用户系统可以采用 OIDC/OAuth2、短期 JWT、PostgreSQL 用户和租户模型、RBAC/ABAC、Vault/KMS 和签名 InvocationContext。管理后台负责用户角色、Skill/Prompt/Tool 版本、MCP 注册、知识库、配额、任务队列、审计、成本、Eval、审批和 Kill Switch。

#### 6. MCP Server 工具注册代码

当前 MCP Python SDK v2 使用 `MCPServer`。最小实现：

```python
from mcp.server import MCPServer

mcp = MCPServer("lingxi-tools")


@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two integers."""
    return a + b


if __name__ == "__main__":
    mcp.run(
        transport="streamable-http",
        stateless_http=True,
        json_response=True,
    )
```

进一步封装工具注册器：

```python
import inspect
from collections.abc import Callable
from typing import Annotated, Any

from mcp.server import MCPServer
from pydantic import Field

ToolFunc = Callable[..., Any]


class MCPToolRegistry:
    def __init__(self, server_name: str) -> None:
        self.server = MCPServer(server_name)
        self._tool_names: set[str] = set()

    def register(
        self,
        *,
        name: str | None = None,
        description: str | None = None,
    ) -> Callable[[ToolFunc], ToolFunc]:
        def decorator(func: ToolFunc) -> ToolFunc:
            tool_name = name or func.__name__
            if tool_name in self._tool_names:
                raise ValueError(f"Duplicate MCP tool: {tool_name}")

            self.server.add_tool(
                func,
                name=tool_name,
                description=description or inspect.getdoc(func) or "",
            )
            self._tool_names.add(tool_name)
            return func

        return decorator

    def run(self) -> None:
        self.server.run(
            transport="streamable-http",
            stateless_http=True,
            json_response=True,
        )


registry = MCPToolRegistry("lingxi-mcp")


@registry.register(
    name="search_code",
    description="Search relevant code in the current repository.",
)
async def search_code(
    query: Annotated[str, Field(min_length=2, max_length=500)],
    top_k: Annotated[int, Field(ge=1, le=20)] = 5,
) -> dict[str, object]:
    return {
        "query": query,
        "top_k": top_k,
        "items": [],
    }


if __name__ == "__main__":
    registry.run()
```

装饰器捕获 Python 函数，`add_tool()` 将其注册到 Server。SDK 根据函数名、Docstring 和类型注解生成工具描述与 JSON Schema，在 `tools/call` 时完成参数校验、函数分发和结果序列化。

如果使用 MCP Python SDK v1，对应初始化方式为：

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("lingxi-tools")
```

---

## 附录：Lingxi 公开实现定位

- Agent 主图：`src/agent/supervisor_graph_demo.py`
- 层级图与 Reviewer：`src/agent/hierarchy_graph_demo.py`
- 状态定义：`src/agent/state.py`
- Prompt：`src/agent/prompt/`
- RAG 和项目知识：`src/agent/tool_set/context_tools.py`
- 代码编辑：`src/agent/tool_set/edit_tool.py`、`src/agent/tool_set/oheditor.py`
- 文件、关键词和 Shell 工具：`src/agent/tool_set/sepl_tools.py`
- 静态检查：`src/agent/tool_set/linter/`
- Runtime：`src/agent/runtime_config.py`、`src/agent/swerex_utils.py`
- 技术报告：`docs/Lingxi Technical Report 2505.pdf`、`docs/Lingxi v1.5 Technical Report 200725.pdf`、`docs/Lingxi v2.0 Technical Report 2026.pdf`
