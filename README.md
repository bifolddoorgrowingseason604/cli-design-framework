<a id="top"></a>

# CLI Design Framework

<p align="left">
  <img alt="Classification First" src="https://img.shields.io/badge/method-classification--first-f59e0b">
  <img alt="Agents OpenClaw Claude Code Codex" src="https://img.shields.io/badge/agents-openclaw%20%7C%20claude--code%20%7C%20codex-111827">
</p>

> Classify first. Design second.
>
> Inspired by Justin Poehnelt's essay, [*You Need to Rewrite Your CLI for AI Agents*](https://justin.poehnelt.com/posts/rewrite-your-cli-for-ai-agents/).
> This skill takes that provocation seriously, but starts one step earlier: before deciding how agent-friendly a CLI should be, first decide what kind of CLI it actually is.

**Language / 语言**

- [中文](#cn)
- [English](#en)

---

<a id="cn"></a>

## 中文

[返回顶部](#top) · [跳到 English](#en)

**快速导航**

- [如何安装](#cn-install)
- [为什么会有这份 Skill](#cn-why)
- [一个极小示例](#cn-example)
- [这份 Skill 会教你什么](#cn-teaches)
- [30 秒教学](#cn-lesson)
- [什么时候用 / 什么时候别用](#cn-when)
- [怎么使用](#cn-how)
- [常见问题](#cn-faq)
- [包结构与兼容性](#cn-package)

| 项目 | 说明 |
| --- | --- |
| 定位 | 一套 classification-first 的 CLI 设计与评审框架 |
| 核心原则 | `Classify first. Design second.` |
| 灵感起点 | Justin Poehnelt 关于 agent-era CLI 的文章 |
| 适合场景 | 新 CLI 设计、已有 CLI 评审、分类争议澄清 |

<a id="cn-install"></a>

### 如何安装

你可以通过两种方式安装这份 skill。

#### 方式一：通过 ClawHub 安装

```bash
clawhub install cli-design-framework
```

如果你想安装某个指定版本，也可以加上版本号：

```bash
clawhub install cli-design-framework --version 1.0.0
```

#### 方式二：通过 `npx skills add` 安装

如果这份 skill 已经放在一个公开仓库里，可以直接从仓库安装：

```bash
npx skills add wangnov/cli-design-framework --list
npx skills add wangnov/cli-design-framework
```

如果仓库里包含多个 skill，建议先用 `--list` 看可安装项，再继续安装。

兼容的 agent 包括：

- Claude Code
- Codex
- OpenClaw
- Other agents supported by the `skills` CLI ecosystem

<a id="cn-why"></a>

### 为什么会有这份 Skill

这份 skill 的直接启发，来自 Justin Poehnelt 那篇关于 agent 时代 CLI 的文章。那篇文章点中了一个越来越现实的变化：

今天的 CLI，很多时候已经不只是被坐在终端前的人类操作。它也越来越频繁地被 agent、脚本、自动化系统、编排流程和其他机器调用。

但与这种使用方式的变化相比，CLI 的设计默认值往往还停留在人类优先的时代。很多工具在帮助信息、交互流程、输出形式、错误提示和状态模型上，首先考虑的仍然是“人怎么读、怎么点、怎么手动修”，而不是“agent 怎么理解、怎么约束、怎么恢复、怎么稳定调用”。

像 OpenClaw 这样的开源 agent，已经会频繁地为自己或用户设计新的 CLI、控制面和交互入口。但今天大多数 AI agent 的知识库里，关于 agent-first CLI 设计的经验仍然非常稀缺，往往只有零散的 JSON 输出建议，缺少成体系的分类判断、surface 设计、风险护栏和状态模型经验。

这正是这份 skill 想回应的问题：当 CLI 的操作者结构已经改变，设计方法也不能还完全停留在旧默认上。

它也想补上另一个缺口：当 agent 开始自己参与 CLI 设计时，它们需要的不只是“会用 CLI”，还需要“知道什么样的 CLI 适合被 agent 使用和演化”。

不过，这份 skill 的结论也不是“所有 CLI 都应该直接变成 agent-first”。真正的问题不是简单站队，而是先判断这个 CLI 到底是什么类型、主要服务谁、通过什么 surface 被使用，然后再决定它应该把哪些 agent-era 能力提升为核心设计。

`cli-design-framework` 的目标，就是把这个顺序倒过来：先分类，再设计。先判断它是什么，再讨论它应该长什么样。

> 如果你还没判断它是什么 CLI，就先别决定它该不该做成 agent-first。

<a id="cn-example"></a>

### 一个极小示例

假设有人说：

> “我们要做一个 CLI，用来启动、查看、停止 AI agent 任务，也许以后还要支持 attach 和实时事件流。”

这套框架不会先问“命令树怎么排”，而会先给出类似这样的判断：

- purpose: manage and observe agent runs
- likely primary role: `Runtime`
- likely primary user: `Balanced` 或 `Agent-Primary`，取决于主要操作者是人还是自动化系统
- likely interaction form: 当前更像 `Interactive CLI`，未来可能增加 `TUI` 作为 secondary surface
- likely statefulness: 不是无状态工具；runtime / session identity 很重要
- design consequence: session lifecycle、attach / resume 语义、事件契约和风险护栏，比“子命令名字漂不漂亮”更重要

这就是它的价值：它先帮你抓住“这是什么”，再决定“怎么设计”。

<a id="cn-teaches"></a>

### 这份 Skill 会教你什么

这份 skill 不只是“给建议”，它会训练你按一套更稳定的顺序思考 CLI：

1. 先写清楚 CLI 的一句话目的。
2. 再判断它的 primary role / control surface。
3. 再判断 primary user type。
4. 再判断 primary interaction form。
5. 再判断 statefulness 和 risk profile。
6. 最后才把这些判断翻译成命令树、帮助系统、输出契约、会话模型和 guardrails。

<a id="cn-lesson"></a>

### 30 秒教学

如果你脑子里第一个问题是：

- “要不要全都加 `--json`？”
- “要不要直接做成 TUI？”
- “要不要一开始就做成 agent-first？”

先停一下。这套框架会先问：

- 这个 CLI 本质上是 Capability、Runtime、Workflow，还是 Environment / Workspace？
- 它主要服务人，还是脚本，还是 agent？
- 机器可读输出是 primary surface，还是 secondary surface？
- 它真的需要 session / attach / resume 这样的状态模型吗？

<a id="cn-when"></a>

### 什么时候用 / 什么时候别用

**适合用：**

- 设计一个新 CLI，但你还不确定它应该长成什么样。
- 评审一个已有 CLI，但你感觉它的 help、输出、命令树彼此不协调。
- 团队在争论 “human-first / script-first / agent-first” 时缺少统一判断框架。
- 你怀疑一个 CLI 的问题不是“细节没打磨好”，而是“分类从一开始就错了”。

**不适合用：**

- CLI 的分类已经非常明确，你只是在选 parser 库、仓库结构或 flag 命名。
- 任务只是润色文案，没有任何设计后果。
- 你只想讨论某个单点功能，而不是整个 CLI 的设计取向。

<a id="cn-how"></a>

### 怎么使用

#### 1. Quick Pass / 快速判断

适合“先给我一个方向”的场景。让 agent 输出：

- purpose
- classification
- short reasoning
- top design consequences
- only the unresolved questions that really matter

示例提示词：

```text
Use cli-design-framework to classify this CLI idea and give me a compressed design pass.
```

#### 2. Full Blueprint / 完整蓝图

适合做新 CLI 的 v1 方案，或者需要对齐团队设计方向时使用。此时它会把 taxonomy 和 blueprint template 一起拉进来，输出更完整的设计约束。

示例提示词：

```text
Use cli-design-framework to produce a full blueprint for this CLI.
Focus on classification, design stance, command shape, output contracts, and v1 boundaries.
```

#### 3. Structured Review / 结构化评审

适合对已有 CLI 做设计评审。它会把问题拆成两层：

- classification fit
- execution quality

这样就不会把“类型错了”和“同类型下做得不够好”混在一起。

示例提示词：

```text
Use cli-design-framework to review this CLI.
Separate classification mistakes from in-category execution weaknesses.
```

<a id="cn-faq"></a>

### 常见问题

#### 这是不是只适合 agent-first CLI？

不是。

这份 skill 的起点确实受 agent 时代讨论启发，但它最终变成的是一套更宽的 classification-first 框架。它同样适合 human-primary、balanced、script-friendly，甚至强状态的 runtime / workspace 类 CLI。

#### classification-first 是否意味着反对 JSON 或自动化？

不是。

它反对的不是 `--json`，而是“在还没搞清楚 CLI 类型之前，就默认所有工具都该优先围绕 `--json`、raw payload 或 agent surfaces 设计”。

#### 每个 CLI 都需要 session、TUI 或 MCP surface 吗？

不需要。

很多 CLI 根本不该被状态化，也不该为了“看起来现代”强行引入 TUI 或复杂 surface。这个框架的一个核心作用，就是帮助你识别哪些能力是真需求，哪些只是风格冲动。

#### 它只能拿来设计新 CLI 吗？

当然可以。

这份 skill 同时支持 design mode 和 review mode，而且 review 时会刻意把“分类错了”和“在本类别下执行得不够好”拆开。

<a id="cn-package"></a>

### 包结构与兼容性

```text
README.md
LICENSE
skills/
└── cli-design-framework/
    ├── SKILL.md
    ├── manifest.yaml
    ├── references/
    │   ├── taxonomy.md
    │   ├── output-templates.md
    │   └── classification-examples.md
    └── examples/
        ├── design-blueprint-example.md
        ├── review-example.md
        └── anti-patterns.md
```

- `README.md`：仓库首页说明，面向 GitHub 浏览、安装和教学
- `LICENSE`：`MIT-0` 许可证
- `skills/cli-design-framework/SKILL.md`：运行时主说明，定义工作流、约束和使用方式
- `skills/cli-design-framework/references/taxonomy.md`：分类体系
- `skills/cli-design-framework/references/output-templates.md`：输出模板
- `skills/cli-design-framework/references/classification-examples.md`：分类锚点
- `skills/cli-design-framework/examples/`：正例与反例

[返回顶部](#top) · [Jump to English](#en)

---

<a id="en"></a>

## English

[Back to top](#top) · [跳到中文](#cn)

**Quick links**

- [Install](#en-install)
- [Why This Skill Exists](#en-why)
- [A Tiny Example](#en-example)
- [What This Skill Teaches](#en-teaches)
- [A 30-Second Lesson](#en-lesson)
- [When to Use / When Not to Use](#en-when)
- [How to Use It](#en-how)
- [FAQ](#en-faq)
- [Package Layout and Compatibility](#en-package)

| Item | Summary |
| --- | --- |
| Positioning | A classification-first framework for CLI design and review |
| Core principle | `Classify first. Design second.` |
| Inspiration | Justin Poehnelt's essay on agent-era CLI design |
| Best for | New CLI design, existing CLI review, and category disputes |

<a id="en-install"></a>

### Install

You can install this skill in two common ways.

#### Option 1: Install from ClawHub

```bash
clawhub install cli-design-framework
```

To install a specific version:

```bash
clawhub install cli-design-framework --version 1.0.0
```

#### Option 2: Install from a repository with `npx skills add`

If this skill is published in a public repository, install it from the repository directly:

```bash
npx skills add wangnov/cli-design-framework --list
npx skills add wangnov/cli-design-framework
```

If the repository contains multiple skills, use `--list` first to inspect what is available.

Compatible agents include:

- Claude Code
- Codex
- OpenClaw
- Other agents supported by the `skills` CLI ecosystem

<a id="en-why"></a>

### Why This Skill Exists

The immediate spark for this skill came from Justin Poehnelt's essay about CLI design in the agent era. The essay points at a change that is becoming hard to ignore:

today's CLIs are often no longer operated only by a human sitting at a terminal. They are increasingly invoked by agents, scripts, automation systems, orchestration layers, and other machine consumers.

Yet the default design assumptions of many CLIs still belong to a mostly human-first era. Help surfaces, interaction flows, output shapes, error messages, and state models are often designed first around how a person reads, clicks, retries, and repairs things manually, not around how an agent understands, constrains, recovers, and calls them reliably.

OpenClaw and similar open-source agents already end up designing new CLIs, control planes, and interaction surfaces for themselves and for users. But most AI agent knowledge bases still contain very little real agent-first CLI design experience. They may include scattered advice about JSON output, but they usually lack a more complete model for classification, surface design, risk guardrails, and state semantics.

That is the tension this skill responds to: if the operator mix has changed, the design method cannot stay entirely frozen in the old default.

It also fills a second gap: once agents start participating in CLI design, they need more than the ability to use a CLI. They need a clearer model of what makes a CLI suitable for agent use and agent-driven evolution.

At the same time, this skill does not reduce the answer to "make every CLI agent-first." The real question is not ideological alignment. The real question is what kind of CLI this is, who it primarily serves, and which surfaces matter most, so you can decide which agent-era capabilities should become core design commitments.

`cli-design-framework` reverses that order: classify first, then design. Decide what the CLI is before deciding what it should look like.

> If you have not decided what kind of CLI it is, do not decide yet whether it should be agent-first.

<a id="en-example"></a>

### A Tiny Example

Suppose someone says:

> "We need a CLI to start, inspect, and stop AI agent runs. Maybe later it should support attach and live event streams."

This framework does not start with command naming. It starts with a classification like this:

- purpose: manage and observe agent runs
- likely primary role: `Runtime`
- likely primary user: `Balanced` or `Agent-Primary`, depending on whether humans or automation dominate
- likely interaction form: `Interactive CLI` now, maybe `TUI` as a secondary surface later
- likely statefulness: not stateless; runtime / session identity matters
- design consequence: session lifecycle, attach / resume semantics, event contracts, and risk guardrails matter more than clever subcommand naming

That is the point of the framework: first decide what the CLI is, then decide how to design it.

<a id="en-teaches"></a>

### What This Skill Teaches

This skill does more than offer tips. It teaches a more reliable order of thinking:

1. State the CLI's purpose in one sentence.
2. Identify its primary role / control surface.
3. Identify its primary user type.
4. Identify its primary interaction form.
5. Identify its statefulness and risk profile.
6. Only then derive command shape, help surfaces, output contracts, session models, and guardrails.

<a id="en-lesson"></a>

### A 30-Second Lesson

If your first instinct is:

- "Should everything support `--json`?"
- "Should this become a TUI?"
- "Should it be agent-first from day one?"

Pause. The framework asks a different set of questions first:

- Is this fundamentally a Capability, Runtime, Workflow, or Environment / Workspace CLI?
- Is it primarily for humans, scripts, or agents?
- Is machine-readable output a primary surface or only a secondary one?
- Does it really need sessions, attach / resume, or other long-lived state?

<a id="en-when"></a>

### When to Use / When Not to Use

**Use it when:**

- You are designing a new CLI and the right shape is still unclear.
- You are reviewing an existing CLI whose help, output, and command structure feel mismatched.
- Your team keeps circling around human-first, script-first, or agent-first debates without a shared model.
- You suspect the real problem is category drift, not weak polish.

**Do not use it when:**

- The classification is already settled and you only need implementation mechanics.
- The task is pure copy editing with no design consequence.
- The question is about one isolated feature rather than the CLI's overall design stance.

<a id="en-how"></a>

### How to Use It

#### 1. Quick Pass

Use this when you want a direction first. Ask the agent to produce:

- purpose
- classification
- short reasoning
- top design consequences
- only the unresolved questions that really matter

Prompt:

```text
Use cli-design-framework to classify this CLI idea and give me a compressed design pass.
```

#### 2. Full Blueprint

Use this for a v1 CLI design or when a team needs a more explicit design target. The skill will pull in the taxonomy and blueprint template and produce a fuller design constraint set.

Prompt:

```text
Use cli-design-framework to produce a full blueprint for this CLI.
Focus on classification, design stance, command shape, output contracts, and v1 boundaries.
```

#### 3. Structured Review

Use this for design review on an existing CLI. It separates:

- classification fit
- execution quality

That keeps category mistakes separate from weak execution inside the chosen category.

Prompt:

```text
Use cli-design-framework to review this CLI.
Separate classification mistakes from in-category execution weaknesses.
```

<a id="en-faq"></a>

### FAQ

#### Is this only for agent-first CLIs?

No.

The inspiration came from agent-era CLI discussions, but the skill itself is broader. It is meant for human-primary, balanced, script-friendly, and strongly stateful runtime or workspace CLIs too.

#### Does classification-first mean anti-JSON or anti-automation?

No.

It is not anti-JSON. It is anti-defaulting every CLI toward JSON-first, raw-payload-first, or agent-first design before understanding the CLI category.

#### Does every CLI need sessions, TUI, or MCP surfaces?

No.

Many CLIs should stay mostly stateless and never grow a TUI or a complex multi-surface model. One of the framework's main jobs is to separate real design needs from fashionable overreach.

#### Can I use this for review, not just creation?

Yes.

The skill supports both design mode and review mode. In review mode it intentionally separates category mistakes from weak execution inside the chosen category.

<a id="en-package"></a>

### Package Layout and Compatibility

```text
README.md
LICENSE
skills/
└── cli-design-framework/
    ├── SKILL.md
    ├── manifest.yaml
    ├── references/
    │   ├── taxonomy.md
    │   ├── output-templates.md
    │   └── classification-examples.md
    └── examples/
        ├── design-blueprint-example.md
        ├── review-example.md
        └── anti-patterns.md
```

- `README.md`: repository landing page for GitHub readers, installation, and teaching
- `LICENSE`: the `MIT-0` license
- `skills/cli-design-framework/SKILL.md`: canonical runtime instructions
- `skills/cli-design-framework/references/taxonomy.md`: taxonomy
- `skills/cli-design-framework/references/output-templates.md`: output templates
- `skills/cli-design-framework/references/classification-examples.md`: classification anchors
- `skills/cli-design-framework/examples/`: worked examples and anti-patterns

[Back to top](#top) · [跳到中文](#cn)
