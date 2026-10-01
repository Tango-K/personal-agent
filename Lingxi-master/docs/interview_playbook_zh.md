# 企业级代码问题诊断与修复 Agent：面试手册

> 用途：大模型应用算法、Agent 算法、AI 售前架构师社招面试。
>
> 对外统一使用匿名名称“企业级代码问题诊断与修复 Agent 平台”。不要在简历中出现 Lingxi、开源地址、公开榜单成绩或论文指标。

## 1. 统一事实口径

### 1.1 可以直接讲的项目背景

- 项目周期：4 个月内完成 V1→V2 主版本演进；V2 落地后继续参与迭代。
- 团队规模：5～6 人。
- 项目形态：公司内部研发效能平台，已有真实用户/试用团队，V2 已落地投入使用。
- 个人角色：核心开发者和设计者，参与全项目，重点主导多 Agent 编排、上下文管理、工具体系、历史知识/长期记忆与 V2 轨迹经验模块。
- 任务目标：输入问题单和代码仓库，自动完成仓库理解、根因定位、修复方案设计、代码修改和测试验证，并沉淀成功处理经验。
- 保密边界：对外只讲通用架构、设计取舍和脱敏案例；内部服务名、业务代码、用户量、仓库数及精确线上指标不披露。

### 1.2 暂时不能主动讲的数字

- 不讲公开项目中的 74.6%、81.2%。
- 不虚构用户数、仓库数、日调用量、精确成功率、延迟、Token 或成本降幅。
- 被问到业务效果时统一回答：内部使用“任务完成率、有效补丁率、人工采纳率、平均工具步数、端到端耗时、Token 成本”评估；精确数字受保密限制，可以说明评价方法、相对趋势和典型 bad case，但当前不能提供具体绝对值。
- 如果之后能从公司材料中确认脱敏区间，再加入简历。当前简历使用“已完成内部试点并投入使用”“通过离线回归集和人工抽检验证”等非数字表述。

### 1.3 三类证据必须区分

|类别|含义|面试说法|
|---|---|---|
|早期公开/裁剪代码可证明|LangGraph 图编排、三阶段 Agent、Supervisor/Reviewer、ReAct Tool、Chroma 代码 RAG、Tree-sitter、消息摘要、模型适配、运行沙箱|可以深入到类、状态字段、工具输入输出与失败路径|
|内部 V1/V2 已落地，当前目录未包含|历史问题检索与重排、多 Decoder 并行分析和聚合、分阶段轨迹、分层抽象树、stage-specific plan、经验回写|按本文的企业版设计口径讲；若某处与真实内部实现不同，必须以后者为准修订|
|仅作为后续优化或通用知识|MCP 全面改造、复杂权限平台、完整生产级熔断、跨集群消息总线等|只能说“可以如何演进”，不能说已经实现|

## 2. 一句话、30 秒与 5 分钟讲法

### 2.1 一句话

我主导过一个面向公司内部研发流程的仓库级问题诊断与修复 Agent，核心不是让一个模型从头聊到尾，而是用 LangGraph 把根因分析、方案规划、代码执行和独立评审拆成有状态的多 Agent 流程，并通过分层上下文压缩、代码 RAG 和历史修复轨迹记忆提升长链路任务的稳定性。

### 2.2 30 秒版本

这个项目输入内部问题单和目标代码仓库，输出经过测试验证的修复补丁。早期单 Agent 方案有两个问题：工具输出很快挤满上下文，而且模型经常没有定位清楚根因就开始改代码。我主导把流程拆成 Decoder、Mapper、Solver、Reviewer，由 Supervisor 基于共享状态编排；同时设计代码检索与编辑工具、阶段级上下文压缩和历史修复知识库。V2 又把成功任务的完整轨迹按分析、规划、执行阶段切分，抽象成不同粒度的可复用经验，检索后先生成当前任务的阶段计划，再指导 Agent 执行。项目目前已在内部投入使用。

### 2.3 五分钟版本

#### 第一段：背景和问题（约 45 秒）

项目是公司内部研发效能平台，目标是根据问题单和代码仓库自动完成问题理解、根因定位、修复规划、代码修改与测试验证。团队 5～6 人，我是核心开发者和设计者，重点负责 Agent 框架、上下文管理、工具与经验记忆模块。我们最初验证过单 Agent ReAct，但很快遇到三个问题：第一，仓库探索与测试日志导致上下文快速膨胀；第二，分析、规划、编辑混在一个角色里，模型容易过早修改；第三，同类问题每次都从零探索，成功经验没有沉淀。

#### 第二段：V1 架构（约 90 秒）

V1 使用 LangGraph 显式组织工作流。主链路不是按照“开发、经理”这种社会角色拆分，而是按照任务阶段拆分：Problem Decoder 只负责理解问题、定位根因和影响范围；Solution Mapper 根据证据生成文件级、函数级修改计划并设计复现方式；Problem Solver 只执行最小修改并验证；Reviewer 在独立上下文中检查补丁和测试结果。Supervisor 负责路由、重试、结束判断和必要的人机确认。

Agent 内部仍使用 ReAct，因为仓库探索路径无法预先完全确定；Agent 之间则采用 Plan-and-Execute 式的阶段协议。所有节点通过结构化 State 通信，而不是互相传整段聊天。State 中保留问题描述、当前阶段、阶段产物、补丁、测试结果、迭代计数与错误状态，节点输入输出有明确 schema。

#### 第三段：Tool 与代码 RAG（约 60 秒）

工具设计遵循“最小工具集、最大有效信息”。目录工具会自动收缩遍历深度，避免一次返回整个仓库；文件读取支持行范围和超长截断；关键词检索使用 ripgrep；语义代码检索用 Tree-sitter 同时构建文件级和函数级 chunk，再存入向量库；编辑工具使用精确的字符串替换，要求 old string 唯一匹配，并在编辑后运行静态检查；Reviewer 才拥有 shell/test 权限。这不仅是提示词分工，也是最小权限设计，能降低误操作与搜索空间。

#### 第四段：Context 与 Memory（约 75 秒）

Context 和 Memory 我会严格区分。Context 是当前一次推理真正送进模型窗口的内容；Memory 是窗口外可持久化、可检索的信息。短期 working memory 放在 LangGraph State，包括当前目标、证据、计划和最近错误。Agent 内保留必要的 ReAct 轨迹；跨 Agent 只传结构化结论，把冗长 ToolMessage 折叠成 action、关键 observation、证据指针和未决项。不可压缩的信息包括系统约束、用户验收条件、文件/符号定位、测试结果与未解决错误；可压缩的是重复文件内容、成功工具调用的原始输出与已被更高层结论覆盖的探索路径。

长期记忆包括两部分：代码知识索引用于当前仓库的事实检索；历史修复经验库存储过去问题、补丁与解决过程。V1 从历史 issue-patch 反推根因分析、修复模式、设计规范和检查清单，并按 Decoder/Mapper/Solver 的需要做角色化注入，避免把所有知识塞给所有 Agent。

#### 第五段：V2 演进（约 60 秒）

V1 的问题是把历史经验压成一份静态摘要，容易丢失先后依赖，也可能因为表面相似而产生负迁移。V2 改为 trajectory-to-guidance：先记录一次任务在分析、规划、执行阶段的结构化轨迹；离线按连续的程序意图自底向上聚合成多层抽象树，低层保留具体子目标，中层保留调查策略，高层保留通用原则，每个节点还记录失败陷阱。在线先做规则或向量召回，再由 LLM 验证适用性，并恢复原始执行顺序，最后不是直接注入旧轨迹，而是结合当前问题生成每个阶段的计划。计划是软约束，新证据可以触发修订。完成后的新轨迹经过验证再回写经验库，形成数据飞轮。

#### 第六段：结果和反思（约 30 秒）

项目已从试点走向内部投入使用。由于公司保密，我不会披露精确业务数据，但我们不是只看最终补丁是否生成，而是拆解评估根因定位、计划质量、补丁测试通过、人工采纳、工具失败、步数、延迟和 Token 成本。最大的经验是，多 Agent 的价值不在于“人多”，而在于任务边界、状态契约、上下文隔离和可分阶段评测；历史经验也不能原样塞进 Prompt，必须经过抽象、检索校验和当前任务适配。

## 3. 推荐简历描述

### 项目名称

**企业级代码问题诊断与修复 Agent 平台｜核心开发者/设计者**

### 四条版（推荐）

- 面向公司内部研发流程，参与并主导构建仓库级问题诊断与自动修复 Agent，将问题理解、根因定位、方案规划、代码修改和测试评审拆分为 LangGraph 有状态多 Agent 工作流，支持阶段重试、人机确认与模型切换。
- 主导分层上下文工程：以结构化 State 管理任务 working memory，在 Agent 内保留必要 ReAct 轨迹、跨 Agent 压缩为 action/observation/evidence/unresolved items，折叠冗长工具结果并设计不可丢失信息白名单，缓解长链路中的上下文稀释。
- 设计最小权限工具与代码 RAG 体系，结合 ripgrep、Tree-sitter 文件/函数级切分、向量检索和精确编辑/静态检查工具，为不同 Agent 动态分配只读、编辑与测试能力，降低误调用和无效仓库探索。
- 推动 V1 静态历史修复知识升级为 V2 分阶段轨迹记忆：将成功处理轨迹抽象为“具体子目标—调查策略—通用原则”的多层经验树，经召回、适用性校验和阶段计划生成后指导新任务，并将验证通过的轨迹回写形成持续迭代闭环；平台已在内部投入使用。

### 为什么这四条是“钩子”

- 第一条引导：为什么多 Agent、为什么 LangGraph、Agent 如何通信、如何恢复。
- 第二条引导：Context 与 Memory 区别、压缩触发、如何防信息丢失、Lost in the Middle。
- 第三条引导：Function Calling、Tool schema、RAG 切分、向量/关键词检索、最小权限与错误恢复。
- 第四条引导：长期记忆、历史知识、负迁移、V1→V2 工程演进、评估和数据飞轮。

### 不推荐写法

- “使用 LangChain/LangGraph 搭建多 Agent”——只有技术名，没有问题、取舍和结果。
- “准确率显著提升”“Token 降低 38%”——没有可确认的内部口径时不能写。
- “实现 MCP、全链路高可用、千万级知识库”——当前没有证据，不要主动制造追问风险。
- “独立完成整个平台”——与 5～6 人团队矛盾；应说主导核心模块、参与全链路。

## 4. 白板架构

```text
Issue / Repository
        |
        v
Input Normalizer + Runtime/Sandbox
        |
        v
Supervisor / LangGraph State
        |
        +--> History Retrieval --> top candidates --> applicability verifier
        |                                      |
        |                                      v
        |                              stage-specific guidance
        |
        +--> Decoder(s) --evidence reports--> Aggregator
        |                                      |
        |                                      v
        +--> Solution Mapper ----------------> repair plan
        |                                      |
        |                                      v
        +--> Problem Solver -----------------> patch + test trace
        |                                      |
        +--> Reviewer <------------------------+
        |          |
        |          +--> fail: feedback/retry/rollback
        |          +--> pass: finish
        v
Validated trajectory --> stage split --> hierarchical abstraction --> experience store

Shared infrastructure:
- working state / checkpoint
- context compressor and artifact references
- code index: keyword + file/function vector chunks
- tool registry: read/search/edit/test
- trace, evaluation and bad-case loop
```

## 5. 源码角度的真实架构分析

### 5.1 图编排

- `supervisor_graph_demo.py` 定义三类 ReAct Agent 和 Supervisor 路由。
- 每个 Agent 的新消息被标注 agent name，再写回共享 `messages`。
- Supervisor 使用结构化输出 `Router(next_agent, thought)`，按 Decoder → Mapper → Solver 路由，并支持失败后重调。
- Human-in-the-loop 使用 LangGraph `interrupt`；阶段切换时先对消息做摘要，再让用户决定重跑上一阶段或继续。
- `hierarchy_graph_demo.py` 在主修复子图之外增加 Multi-Agent Manager 和 Reviewer，形成“执行—评审”层级图。

### 5.2 State 与缓存

- `CustomState` 保存 messages、last/next agent、summary、HITL 等基础字段。
- 扩展 `State` 已预留 dev knowledge、多个 decoder/mapper/solver outputs、generated patches 和迭代计数。
- 自定义 messages reducer 先使用 `add_messages` 合并，再只给最后一个内容块设置 Anthropic ephemeral cache，避免超过 provider 的 cache block 限制。
- 注意：Prompt cache 不是 Memory，也不是语义层的 context compression；它只降低重复前缀的计算/费用。

### 5.3 代码 RAG

- `create_project_knowledge` 扫描 Python/Java 文件。
- 文件整体被递归切成 1000 字符、256 overlap 的 chunk。
- Tree-sitter 额外抽取函数/方法，建立函数级 document；元数据记录 file path、func name、type。
- 使用 OpenAI embedding 与 Chroma 按仓库持久化。
- `search_relevant_files` 检索 top-k 代码块后，再让 LLM 生成“为什么相关”的结构化解释。
- 面试时要主动承认早期实现的局限：仅支持 Python/Java、固定字符切分会割裂语义、缺少 BM25/重排/增量索引、API 使用了已逐渐过时的 retriever 方法；内部版本应采用 AST/符号边界、混合检索、rerank 与 commit-aware 增量更新。

### 5.4 Tool

- `view_directory` 只看 Git tracked files，并在结果过多时自动降低 depth。
- `search_files_by_keywords` 使用 ripgrep，支持文件名和内容匹配，限制关键词数量与返回文件数。
- `view_file_content` 复用 editor 的 view 能力，支持 line range 与超长截断提示。
- `str_replace_editor` 使用唯一 old string 匹配，编辑 Python 后对 diff 做静态检查。
- `run_shell_cmd` 同时支持本地和 SWEREx 沙箱，在超时时尝试重建会话并重试。
- 不同 Agent 的工具集合不同：Decoder/Mapper 只读，Solver 可编辑，Reviewer 可运行 shell。这是能力边界，也是安全和搜索空间控制。

### 5.5 Context compression

- `stage_message_processor` 排除 ToolMessage，再让 summarizer 生成阶段摘要。
- `compress_agent_thinking_observation_action` 把 Agent 轨迹转为结构化步骤，保留 thinking/observation/action/conclusion，并统计工具频率。
- 早期 demo graph 只在 HITL 阶段明确调用摘要；内部版应在所有 stage boundary 使用，并把大对象外置为 artifact reference。

### 5.6 历史开发知识

- `dev_knowledge.py` 从历史 issue、patch、对应 commit snapshot 反向重建问题处理过程。
- 首先生成 issue-specific analysis，再抽象为 XML 知识：root-cause steps、bug type、architecture、components、fix pattern、checklist、design practices 等。
- 不同角色只拿所需标签：Decoder 偏根因与架构，Mapper 偏修复模式和 checklist，Solver 偏编码规范。
- 已存在多个 design version，体现从单历史问题、角色化注入、引导问题到 top-N `DevKnowledge` 的迭代。
- 当前裁剪目录缺少依赖的数据目录，且 demo graph 没有连接这些工具，因此不能把这个目录描述成可直接复现的完整 V1.5/V2 生产版本。

## 6. 核心设计题标准答案

### 6.1 为什么不是单 Agent？

不要回答“多 Agent 更聪明”。正确答案：

1. 仓库修复具有清晰的阶段性中间产物：根因报告、修复计划、补丁、测试证据，适合定义可验收契约。
2. 单 Agent 把探索噪声一直带到编辑阶段，会产生 context dilution，且容易跳过分析直接改代码。
3. 阶段拆分允许工具最小权限、单阶段重试、独立评测和更换不同成本档位模型。
4. 代价是延迟、协调和错误传播。因此只有当任务可分解且各阶段有明确验收标准时才使用多 Agent；简单问答或一次检索不需要。

### 6.2 为什么选 LangGraph？

因为这里存在条件路由、循环、评审回退、人机中断、共享状态和长任务恢复，图比隐式 while/if 更可观测、更容易测试。代价是 state schema、reducer、checkpoint 和图迁移复杂；如果流程只是线性调用，用普通函数或简单 ReAct 足够。

### 6.3 Agent 怎么通信？

生产口径：Supervisor 中转 + 共享黑板 State。上游不把全部 messages 直接交给下游，而是写入结构化阶段产物：

- DecoderOutput：problem, evidence[], root_cause, affected_symbols[], uncertainties[]。
- MapperOutput：reproduction, changes[], risks[], verification_plan。
- SolverOutput：patch_ref, modified_files[], tests[], unresolved_errors[]。
- ReviewerOutput：verdict, evidence, feedback, retry_target。

大文件、日志与 diff 写外部 artifact store，State 只保存引用、摘要和 hash。这样避免 checkpoint 膨胀，也方便 schema 版本化。

### 6.4 Context 压缩怎么保证不丢关键信息？

使用“规则抽取 + 受约束摘要 + 可回溯引用”，不是纯 LLM 自由总结。

1. 硬保留：用户约束、验收条件、文件/函数/行号、错误码、测试命令与结果、未决问题。
2. 去重折叠：重复文件内容、成功命令完整 stdout、已被确认结论覆盖的探索。
3. 摘要 schema 固定为 action/evidence/observation/conclusion/unresolved。
4. 原文不物理删除，保存在 artifact store，摘要中带 reference id，需要时可回查。
5. 质量评估看 entity/constraint/test preservation、摘要前后下游任务成功率和 Token/延迟，而不仅是压缩率。

### 6.5 Memory 如何分层？

- Episodic/working memory：当前任务 State 与轨迹，生命周期到任务结束。
- Semantic code memory：当前 commit 的代码索引，回答“代码是什么”。
- Procedural memory：历史修复经验，回答“类似问题应该怎样定位、规划和验证”。
- Artifact memory：原始日志、diff、测试报告，只通过引用进入上下文。

写入长期 procedural memory 必须满足：任务经过 Reviewer/测试验证；切除敏感数据；记录 repo/version/stage/source/outcome；相似经验去重；失败经验可以保存为 pitfall，但必须带失败标签，不能当成功方案检索。

### 6.6 RAG 如何设计？

代码 RAG 与历史经验 RAG 是两套索引：

- 代码 RAG：query 是 issue/假设/符号，document 是 AST/函数/文件 chunk；强调 commit 一致性与符号关系。
- 历史 RAG：query 是结构化 issue summary；document 是历史问题与抽象经验节点；强调时间过滤、防数据泄漏、阶段匹配和适用性校验。

理想内部版检索链路：结构化 query rewrite → 关键词/BM25 与 dense 并行召回 → RRF/加权融合 → cross-encoder 或 LLM rerank → 阈值/多样性过滤 → stage-aware prompt。向量相似不等于可用，必须校验版本、组件、错误类型、前置条件和冲突。

### 6.7 Tool Calling 的底层流程

1. 框架把工具 name、description、JSON schema 与对话发送给模型。
2. 模型输出结构化 tool call，而不是自己执行函数。
3. 执行器做工具白名单、参数类型、路径/权限、预算校验。
4. 执行工具并把结果包装为 ToolMessage，使用 tool_call_id 关联。
5. 模型读取 observation，决定继续调用、修正参数或结束。

生产稳定性要有：timeout、有限重试、错误分类、幂等键、重复调用检测、输出截断、审计日志和高风险操作审批。

### 6.8 V1 为什么要演进 V2？

V1 的静态知识解决“没有经验”，但仍有三类缺陷：

- 信息损失：issue-patch 反推只能重建可能的路径，摘要又压掉了阶段依赖。
- 粒度固定：太具体不能迁移，太抽象不能执行。
- 负迁移：语义相似的旧问题可能拥有不同架构前置条件。

V2 使用真实轨迹、stage alignment、multi-level abstraction、pitfall、compatibility verifier 与 target-specific plan。核心不是多存历史，而是让经验以正确的粒度、在正确阶段、经过当前任务适配后进入上下文。

### 6.9 V2 抽象树如何构建？

1. 标准化 trajectory event：stage、step、goal、tool、args summary、observation、evidence、outcome。
2. 按 Decoder/Mapper/Solver 分段，避免不同阶段的信号混合。
3. 对连续且具有相同 procedural intent 的步骤聚类，生成低层 sub-goal。
4. 自底向上合并相邻节点，生成中层 strategy 与高层 principle。
5. 每节点存 source step ids、preconditions、pitfalls、success/failure evidence、embedding 与版本。
6. 人工抽检和自动一致性校验：父节点必须能被子节点证据支持，禁止产生轨迹中没有依据的事实。

### 6.10 怎么判断 Agent 结束或继续？

不是只靠模型说 FINISH。使用组合条件：

- 阶段产物通过 schema 校验。
- Mapper 必须有可复现/验证计划；Solver 必须产生非空 diff 或解释无需修改。
- 相关测试通过且 Reviewer 有证据地判定完成。
- 达到最大步数、Token、时间或重复调用阈值时终止并返回失败/人工处理。
- 相同 tool+args 重复、连续 observation 无新增信息时判定 no-progress，触发换策略或回退阶段。

## 7. 评测体系与指标口径

### 7.1 离线评测集

- 公开 benchmark 用于横向验证通用能力，但不等于内部效果。
- 内部集来自脱敏历史问题单，按仓库、问题类型、难度、是否跨文件、是否需要测试环境分层。
- 严格按时间切分；历史记忆只能检索目标问题发生前的案例，防止把答案泄漏进知识库。
- 每次 Prompt、模型、工具、检索或图结构更新都跑固定回归集与挑战集。

### 7.2 最终指标

- Task Success / Valid Patch Rate：是否通过目标测试并满足验收条件。
- Human Acceptance Rate：开发者是否采纳、修改量多大。
- Regression Rate：是否破坏原有测试或引入新问题。

### 7.3 分阶段指标

- Localization Recall@file/function、root-cause judge score。
- Plan coverage、plan-to-diff consistency。
- Tool success/error/retry rate、无效/重复工具调用率。
- Reviewer precision/recall：是否能识别错误补丁，防止自我确认偏差。

### 7.4 效率指标

- 端到端 P50/P95 latency、LLM calls、tool steps、input/output tokens、单任务成本。
- 压缩模块看 token saving 之外，还要看 key-information retention 与下游成功率。

### 7.5 Ablation

- 单 Agent vs 多 Agent。
- 不压缩 vs 纯摘要 vs 结构化摘要+引用。
- 无历史知识 vs raw issue/patch vs V1 abstraction vs V2 trajectory guidance。
- dense only vs hybrid retrieval vs rerank/compatibility verification。
- 无 Reviewer vs 独立 Reviewer。

## 8. 典型故障案例

### 案例 A：上下文压缩丢失否定约束

- 现象：上游明确“不得修改公共接口”，摘要后只剩“兼容公共接口”，下游仍修改签名。
- 根因：自由摘要没有约束白名单，否定词在压缩时语义漂移。
- 修复：把 constraints 单独做结构化字段；规则抽取 must/must-not；摘要只处理探索轨迹；Reviewer 比较 diff 与 constraints。
- 验证：建立含否定、数字边界、文件白名单的摘要回归集，检查 constraint preservation。

### 案例 B：历史知识负迁移

- 现象：检索到报错词相同的旧 issue，Agent 直接复用旧模块的修复模式，定位错误。
- 根因：只按文本相似度，不验证仓库版本、调用链和前置条件。
- 修复：先结构化 query；混合召回后做 applicability verification；经验节点带 preconditions/pitfalls；计划明确标记“需当前代码证据确认”的假设；证据冲突时放弃历史指导。
- 验证：对“表面相似但根因不同”构建 hard negative，观察错误知识采纳率。

### 案例 C：Agent 重复搜索死循环

- 现象：Decoder 不断以相近 query 调相同搜索工具，没有新信息。
- 根因：工具返回缺少 progress signal，State 没记录 query/tool fingerprint。
- 修复：对 tool+canonical args+result hash 做 fingerprint；连续两次无新增证据触发 strategy reset；达到阈值回退或终止；Supervisor 根据 unresolved hypothesis 分配下一步。
- 验证：记录重复调用率、平均步骤和 no-progress termination accuracy。

### 案例 D：Reviewer 自我确认

- 现象：Reviewer 看到 Solver 的长解释后接受了一个只修症状的补丁。
- 根因：Reviewer 与 Solver 共享过多推理结论，产生 anchoring；测试只覆盖复现脚本。
- 修复：Reviewer 使用独立上下文，只接收原始问题、diff、测试证据和验收条件；先独立生成反例，再看 Solver rationale；相关回归测试与变异/边界测试组合。
- 验证：测错误 patch 检出率，而不只看 reviewer agreement。

## 9. 追问题库地图

### A. 项目总览

1. 三分钟介绍项目和你的贡献。
2. 为什么公司需要它，而不是直接购买通用 Coding Agent？
3. 输入、输出、边界、真实用户、接入研发流程的位置分别是什么？
4. 五六个人怎么分工？你提交了哪些核心设计？
5. 最难的问题是什么？为什么它不是简单 Prompt 调优？

### B. 多 Agent 与 LangGraph

6. 为什么多 Agent 优于单 Agent？给出反例。
7. 为什么按任务阶段拆，而不是按人类职位拆？
8. Supervisor 是规则还是 LLM？LLM 路由失败怎么办？
9. Agent 间通信的 schema 是什么？谁能写哪些字段？
10. 两个 Agent 意见冲突怎么处理？
11. 如何避免死循环、活锁和错误在下游放大？
12. Checkpoint 如何存、恢复与清理？非幂等工具怎么重放？
13. Reviewer 为什么需要独立上下文？

### C. Context 与 Memory

14. Context、State、Memory、Prompt cache 有什么区别？
15. 压缩触发条件是什么？按 token、阶段还是语义事件？
16. 哪些信息绝对不能压缩？
17. 摘要幻觉、语义漂移如何检测？
18. Memory 写入和读取策略是什么？如何处理冲突和过期？
19. 失败轨迹是否写入？如何防止污染？
20. 为什么不是把所有历史对话交给长上下文模型？

### D. RAG 与历史知识

21. 代码如何 chunk？为什么同时做文件级与函数级？
22. 1000/256 是怎么来的？有什么不足？
23. dense、BM25、reranker 各解决什么问题？
24. top-k 如何选择？如何做 hard negative？
25. 如何避免历史答案泄漏和错误知识负迁移？
26. V1 静态知识和 V2 轨迹知识有什么本质区别？
27. 抽象树为什么需要多个层级？

### E. Tool

28. Function Calling 底层流程。
29. 为什么工具越少有时效果越好？
30. Tool description 怎么写，如何处理 100 个 Tool？
31. 工具超时、空结果、参数错、部分成功分别如何处理？
32. Tool、Skill、MCP 的区别；当前项目有没有 MCP？
33. 编辑工具如何防止改错文件和非唯一替换？

### F. 评测与工程

34. 如何证明提升来自框架，而不是更强模型？
35. Agent 评测为什么不能只看最终成功率？
36. 如何构建离线回归集和线上监控？
37. 你做过哪些 ablation？如何控制变量？
38. 成本、延迟与效果如何权衡？
39. 一个线上 bad case 从发现到修复的闭环是什么？
40. 如果目标公司要把它迁移到客服/售前 Agent，哪些可复用、哪些要重做？

## 10. AI 售前架构师加试口径

售前岗位不应只讲算法，回答需要增加四层：

1. 业务目标：人工处理时长、研发吞吐、缺陷回归风险、知识复用。
2. 方案边界：Agent 给建议还是自动提交；什么操作需审批；什么问题转人工。
3. 集成路径：工单/代码托管/CI/知识库/权限/审计；POC 先只读诊断，再开放受控修改。
4. 价值验证：选代表性仓库和历史问题做时间切分 POC，比较人工基线、任务成功、采纳、成本与上线风险。

客户问“为什么不用更强模型”时，回答：模型能力是上限，但企业落地还由上下文质量、工具可靠性、状态恢复、权限审计、评测闭环决定；更强模型不能替代可控系统设计，同时我们保留模型适配层，允许按任务复杂度做 model routing。

## 11. 面试中必须避免的表述

- 不说“我们所有 Agent 都共享完整历史”；内部口径应是结构化 State + 阶段产物 + 必要摘要。
- 不把 Chroma 代码索引说成长期用户记忆；它是 semantic code memory。
- 不把 prompt cache 说成上下文压缩；两者作用不同。
- 不说 Supervisor 完全智能地自由调度；主阶段顺序是强约束，LLM 主要处理重试/回退等局部路由。
- 不说 Reviewer 保证正确；它只能提高错误检出率，最终以可执行测试和人工验收为准。
- 不说 Pass@1 等于线上成功率；benchmark 与内部业务分布、工具、环境、数据泄漏风险不同。
- 不说 V2 计划会强制执行；它是 guidance，新证据必须可以覆盖旧经验。
- 不说所有历史轨迹都会写入；只有经过验证、脱敏、去重和版本标注的轨迹才能进入长期记忆。

## 12. 当前信息缺口

以下问题必须在训练过程中逐步补齐，否则最终仍会被严格面试官击穿：

- 内部落地的具体使用场景：IDE、工单系统、代码评审还是独立 Web 平台？
- 用户类型和真实工作流：谁提交、谁审核、谁采纳？
- 可以公开的脱敏效果区间或至少趋势数据。
- 内部 V2 的真实存储、检索模型、重排方式、并发和 checkpoint 实现。
- 你亲历的一个可脱敏成功案例和两个失败案例。
- 团队 5～6 人具体分工以及你的可核验交付物。

这些缺口不能用“同事负责”全部掩盖，因为它们属于项目负责人必须了解的系统级问题。可以把不相关的安全接入、前端、账号权限归给同事，但核心链路必须能说明接口和取舍。
