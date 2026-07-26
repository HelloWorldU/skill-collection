---
name: progressive-abstraction
description: 渐进式抽象的 issue 调研方法论——在陌生代码库中拿到一个 issue 后，不陷入技术细节，按三层抽象递进判断：架构修复方向 → 数据流与边界语义 → 具体方案。Use when the user asks to analyze / triage / investigate an issue or bug in an unfamiliar codebase, decide where a fix belongs before writing code, judge whether something is a product decision or a correctness problem, prepare an OSS contribution (issue comment / PR) that must survive maintainer review, or evaluate third-party / AI-generated review opinions against code evidence. 触发词：调研 issue、判断修复方向、分析根因、看这个 issue 怎么修、review 别人（或别的 AI）的判断。
---

# Progressive Abstraction：Issue 调研的三层递进

核心原则：**在架构层判断修复方向之前，绝不进入实现细节。** 拿到 issue 的第一反应不是读代码，而是问"这个问题属于哪一层"。

三层按顺序执行，每层有明确的输出物；上一层没有结论，不进入下一层。

## L1：架构方向判断

目标：确定修复的**责任层**，并区分问题性质。

1. 读 issue 全文和整个 comment thread。记录：谁报了问题、谁已诊断、团队有没有人参与、赛道是否已被占。
2. 列出系统的候选层（例如：provider 适配 / 传输 / parse / schema / validation / 执行 / 投影），给出问题**应该**归属的一层，一句话说明理由。同时明确"修复**不**应该落在哪一层"——括号式排除（"at the parse boundary, not in the schema"）比正面描述更有信息量。
3. 定性问题：**correctness 还是产品决策？** 判断标准：错误的行为是否会真实发生。
   - 会发生错误行为 → correctness，直接修，不需要问任何人。
   - 只是"覆盖不全 / 帮不到某些场景" → completeness，不是 correctness，不许被任何人（包括 AI reviewer）用"不正确"带节奏。
   - 修不修取决于项目立场（如"要不要为第三方兜底"）→ 产品决策，**显式抛给 maintainer**，把决策写在评论里，不要静默替项目做选择。
4. 本层输出：一句话修复方向 + 一句话问题定性 + 一句话赛道判断。

## L2：数据流与边界语义

目标：用**真实代码**验证 L1 的判断，并识别不可忽视的边界语义。

1. 从输入到失败点走一遍完整数据流，每一跳标注 `file:line`。**行号必须亲手验证**（基于当前 main / 确切版本），任何人（包括 AI）给的行号一律视为待验证假设。
2. 画出数据流图：输入 → 各处理节点（file:line）→ 失败点。图是 L2 的核心输出物，不是装饰。
3. 识别边界语义——那些"不看就会判错方案"的隐含事实。常见类型：
   - **双重身份**：一个制品对两个读者的语义不同（如 schema 既对模型做广告、又对输入执法）；
   - **反馈回路**：错误文本的读者是模型，它决定下一轮行为；
   - **安全方向**：失败模式偏向哪边（如"漏转安全、错转才是事故"）——这决定保守策略是否成立；
   - **历史先例**：同文件/同模块里已有的同类处理（新代码应长得像它所在的文件）。
4. 本层输出：数据流图 + 边界语义清单（每条一句话，附 file:line 证据）。

## L3：具体方案

目标：给出**有界**的方案选项，每个方案自带"不做什么"。

1. 给出 2-4 个落点不同的选项（A/B/C/D），每个选项写清：落在哪层、改什么、**刻意不改什么**。
2. 优先考虑**组合方案**（如"归一化 + 可见警告"）：一个解决功能，一个保留可观测性——单独的静默修复往往是半个方案。
3. 方案的有界性即设计：明确声明 scope 外的东西（"不做 per-model 特例"、"不展开 `$ref`"），并说明为什么扩大范围是负收益（如"通用化的终点是重造已有的轮子"）。
4. 测试即契约：用测试把**有意为之的行为**钉死（包括"有意的欠处理"），让 reviewer 看到边界是选择而非疏漏。
5. 本层输出：方案对比 + 推荐组合 + scope 声明 + 测试策略。

## 证据纪律（贯穿三层）

- **锁对象**：先确认 issue 涉及的确切对象（模型、版本、文件），再调研。对象不可验证（闭源、无公开 artifact）→ 降级到最近的可验证证据，并在措辞里如实标注边界，绝不外推。
- **一手验证**：关键断言必须有原件支撑（拉原始文件读原文），不用二手转述——包括其他 AI 的结论、摘要、转引的"官方行为"。
- **每条结论可辩护**：写进报告/评论/PR 的每一句话，问自己"maintainer 质疑这句时，我能拿出什么"。拿不出的，删掉或降级为推测并标注。
- **评审第三方意见**（含 AI reviewer）：逐条判真伪，用代码证据说话；真的修，假的用仓库现存先例驳回；对方说"correctness"时先做 L1.3 的定性检验。

## 执行礼仪（方案确定之后）

- **先 comment 后 PR**：评论给增量内容，不重复前人诊断（引用 + 递进）；方位词带参照物（"on the harness side of the model boundary"，不写裸 "downstream"）；结尾领活（"I can implement this"）——方案 + 领活 = PR，方案 + 没下文 = 空气。
- **PR 是提案不是决策**：决策权在 maintainer，body 越短越安全；证据留在评论区作弹药，被问时再甩，不要 front-load。
- **响应纪律**：review 意见 20 分钟～2 小时内响应；逐条判定公开处理。

## 反模式（全是实战翻车点）

- 拿到 issue 直接开始写代码；
- 从第一行顺序读大文件（应沿一个变量的生命周期或一条数据流读）；
- 把 AI 给的行号 / 定性当事实直接使用；
- 为了"通用"扩大 scope，把手写 mini 解释器做成半个标准库；
- 在 PR body 里写不可验证的断言；
- comment 里复述别人已给出的诊断、不给增量、不领活。
