<sub>🌐 <a href="README.md">English</a> · <b>中文</b></sub>

# Claude Design Skill

一个可移植的 Claude skill，让 Claude 在被要求做设计时进入"资深设计师"模式，输出高质量的 HTML 设计产物：落地页、幻灯片、可交互原型、动效视频、海报、线框图、视觉探索画板等。

改编自 Claude.ai 内部 *Design* 产品的系统提示词，按 [Claude Skill 规范](https://docs.claude.com/en/docs/claude-code/skills) 重新组织：`SKILL.md` 保持精简，细节拆分为按需加载的 `references/`，可复用的起手模板放在 `assets/`，真实示例产出放在 `demos/`。

---

## 目录

- [这个 Skill 能做什么](#这个-skill-能做什么)
- [Skill 结构](#skill-结构)
- [安装](#安装)
- [使用](#使用)
- [触发策略](#触发策略)
- [示例产出](#示例产出)
- [测试用例](#测试用例)
- [自定义](#自定义)
- [许可](#许可)
- [English README](README.md)

---

## 这个 Skill 能做什么

当你让 Claude 做设计——做 deck、落地页、原型、动效——这个 Skill 会激活并为 Claude 提供：

- **事实验证（优先级 #0）**：任务涉及具体产品名时，第一步强制 `WebSearch` 验证存在性/版本/规格。10 秒搜索的成本 << 基于错误假设返工 1-2 小时。
- **核心资产协议**：涉及具体品牌时，5 步硬流程收集资产——**logo / 产品图 / UI 截图是一等公民**，不是只抓色值和字体。结果固化到 `brand-spec.md`。
- **设计方向顾问模式**：需求模糊时（"帮我设计个好看的"），自动切到 Advisor 模式，从 10 种设计哲学（横跨 5 个流派）里推荐 3 个差异化方向，等用户选完再动手。
- **输出格式手册**：设计画板 / 幻灯片 / 可交互原型 / 时间线动画 / 线框图，每种都给骨架和陷阱。
- **变体策略**：什么时候给并排多套选项，什么时候给一份带 Tweaks 面板的原型（色值/字体/变体实时切换）。
- **反 AI slop 清单**：明确禁止过度渐变背景、emoji 作项目符号、带左侧彩色边条的圆角卡片、用 CSS 剪影代替真实产品图、用渐变圆球表示"AI"等机器生成的典型套路。
- **React + Babel 陷阱提示**：内联 JSX 常见踩坑点（style 对象命名冲突、Babel 作用域隔离、integrity 哈希校验）。
- **起手模板**：可直接复制的自适应缩放 deck 舞台、设计画板网格、原型外壳、带 Tweaks 面板的页面、时间线动画引擎，以及 iOS / Android / macOS / 浏览器四种设备框架。
- **3 个真实示例产出**：可以直接打开看 Skill 的产出质量。

该 Skill **与具体环境无关**，不依赖 Claude.ai 内部工具。可在 Claude Code、Claude Agent SDK，或任何支持 skills 的环境中使用。

## Skill 结构

```
claude-design-skill/                # 仓库根 = 这个 skill 本身
├── SKILL.md                        # 入口文件——工作流 + 优先级规则 + 路由
├── README.md / README.zh.md        # 中英双语 README（Agent 不读）
├── LICENSE
├── references/
│   ├── fact-verification.md        # 优先级 #0——先搜再假设
│   ├── workflow.md                 # 问问题、收集上下文、迭代方式
│   ├── brand-context.md            # 核心资产协议——5 步硬流程
│   ├── design-styles.md            # 10 种设计哲学 × 5 流派，Advisor 模式用
│   ├── design-principles.md        # 反 slop、craft 规则、内容纪律
│   ├── output-formats.md           # deck / 画板 / 原型 / 动画 / 线框图骨架
│   ├── variations-and-tweaks.md    # 变体混合策略、Tweaks 协议
│   ├── react-babel.md              # 版本固定、作用域规则、integrity 哈希
│   └── verification.md             # 交付前真实浏览器里检查什么
├── assets/
│   ├── deck-stage.html             # 带缩放、导航、localStorage、打印PDF 的幻灯片外壳
│   ├── design-canvas.html          # 带标签的变体网格
│   ├── prototype-shell.html        # React + Babel 加载器（固定版本 + SRI）
│   ├── tweaks-starter.html         # 带 Tweaks 面板的可实时调参页面
│   ├── animations.jsx              # Stage / Sprite / useTime / interpolate / Easing
│   └── device-frames.md            # iOS / Android / macOS / 浏览器框架 CSS
├── demos/
│   ├── demo-1-deck.html            # Swiss Editorial 风格的 5 页 pitch deck
│   ├── demo-2-canvas.html          # 4 个不同流派的 hero 变体对比
│   └── demo-3-prototype.html       # 可交互的 3 屏 iOS 读书追踪 app
├── test-prompts.json               # 6 个场景化测试用例，覆盖主要分支
└── docs/images/                    # README 截图
```

整个仓库就是这个 skill。`SKILL.md` 直接在根，`npx skills add` 和任何遵循 Agent Skills 规范的 CLI 都能一眼找到。

## 安装

### 方式 A · `npx skills`（推荐）

同一条命令对 40+ 种 coding agent 生效（Claude Code、Codex、Cursor、OpenCode、Gemini CLI、Cline 等）。全局安装：

```bash
npx skills add jiji262/claude-design-skill -g
```

或装到当前项目：

```bash
npx skills add jiji262/claude-design-skill
```

用 `-a` 指定某个 agent（如 `-a claude-code`、`-a cursor`），或用 `-y` 跑 CI 非交互安装。完整参数见 [skills CLI 文档](https://www.npmjs.com/package/skills)。

### 方式 B · clone + symlink 到 Claude Code

如果你想自己跟踪这个 repo（改造、贡献等），clone 下来 symlink 到 `~/.claude/skills/`：

```bash
git clone https://github.com/jiji262/claude-design-skill ~/code/claude-design-skill
ln -s ~/code/claude-design-skill ~/.claude/skills/claude-design
```

symlink（而不是 `cp -r`）的好处是 `git pull` 之后已安装的 skill 自动同步更新。新开一个 Claude Code 会话，skill 会自动注册。

### 方式 C · 作为插件的一部分

把这个 repo（或子目录）作为 Claude Code 插件内的 skill 分发。参见 Claude Code 插件开发文档的目录约定。

### 方式 D · 手动指向

不安装也可以：直接让 Claude 读取 [SKILL.md](SKILL.md) 并按其中指引行事。

## 使用

直接让 Claude 做设计即可。Skill 在以下类型的请求上触发：

- "帮我做一份 pitch deck"
- "设计一个落地页"
- "把这个 onboarding 流程做成原型"
- "把这个概念做成动画"
- "把这张截图做成可点击的 mockup"
- "hero 区块给我 3 个方向"
- "做一张海报"
- "先画几个线框图探索一下方向"
- "做个好看的——不知道要什么风格" *（触发 Advisor 模式）*
- "给 <某产品> 做一个发布动画" *（触发事实验证 + 核心资产协议）*

Claude 会：

1. **先搜再做**：如果需求提到具体产品/版本/最近发布，第一步 `WebSearch` 而不是提问。
2. **收集设计上下文**：推着你提供代码库、设计系统、UI kit、参考品牌，而不是凭空想象。
3. **抓品牌资产**：品牌任务先找 logo / 产品图 / UI 截图，再看色值和字体。
4. **Advisor 模式兜底**：需求模糊时，提 3 个不同流派的差异化方向让你选。
5. **动手前先声明系统**：字体阶、色板、布局节奏先讲清楚再画像素。
6. **带占位符的初稿先给你看**，然后迭代。
7. **交付多个变体**：从保守到大胆。
8. **真实浏览器里验证**：clean console、可缩放、可交互，才算 done。

## 触发策略

Skill 的 description 写得"略激进"——即使你没说"设计"这个词，只要请求落在常见设计场景里，它都会激活。完整触发语列表见 [SKILL.md](SKILL.md) 的 `description` 字段。

但 Skill **不会**在普通前端编码任务（修 bug、重构组件等）上触发——那些场景走正常代码流程，不是设计流程。

## 示例产出

直接在浏览器里打开这些文件，都是 skill 按本仓库规则产出的真实样例：

### 1 · 幻灯片 · Swiss Editorial 风格

<p align="center"><img src="docs/images/demo-1-deck.png" width="100%" alt="Swiss Editorial 风格的 pitch deck"></p>

**[demos/demo-1-deck.html](demos/demo-1-deck.html)** — Swiss Editorial 风格（Pentagram / Vignelli 血脉）的 5 页 pitch deck。超大折号、细线分隔、单色重点、Inter 字体多字号。键盘导航、1-indexed 幻灯片标签、localStorage 持久化。

### 2 · 设计画板 · 4 个不同流派的 hero 变体

<p align="center"><img src="docs/images/demo-2-canvas.png" width="100%" alt="4 个不同流派的 hero 变体"></p>

**[demos/demo-2-canvas.html](demos/demo-2-canvas.html)** — 同一虚构产品的 4 个 hero 变体，横跨 4 个设计流派（Swiss Editorial / Kenya Hara 极简 / Brutalist Web / Editorial Magazine）。这是 Advisor 模式产出的标准形态。

### 3 · 可交互 iOS 原型 · 带真实状态

<p align="center"><img src="docs/images/demo-3-prototype.png" width="100%" alt="3 屏 iOS 读书追踪原型，带实时状态"></p>

**[demos/demo-3-prototype.html](demos/demo-3-prototype.html)** — 可交互的 3 屏 iOS 读书追踪 app 原型。在详情屏点 "+5p / +20p / Finish" 按钮，观察主屏的"当前书"卡片和统计屏的数字实时更新。

更多"注意什么"见 [demos/README.md](demos/README.md)。

## 测试用例

[test-prompts.json](test-prompts.json) 里有 6 个真实场景化的测试，每个都标了它要考的守则：

1. 模糊的落地页需求 → 触发 Advisor 模式
2. 指定品牌任务 → 触发事实验证 + 核心资产协议
3. Pitch deck → 系统纪律 + 字号阶梯
4. 动效设计 → 节奏纪律 + 不用"渐变圆球代表 AI"
5. iOS 原型 → 真实图源 + 信息密度 + React 版本固定
6. Tweaks 代替 N 个分离文件 → 变体策略

可以用来自检 skill，或作为你自己 eval harness 的种子数据。

## 自定义

- **挂载你的品牌/设计系统**：把品牌手册、设计 tokens、UI kit 以 Markdown / HTML 放到 `references/` 或 `assets/`，并在 `SKILL.md` 里加一条指引把 Claude 引过去；
- **调整触发范围**：改 `SKILL.md` 顶部 frontmatter 里的 `description`，可以让 Skill 变得更保守或更激进；
- **替换起手模板**：`assets/` 里的文件是明确的起点，按你团队风格改写后 Claude 会使用你的版本；
- **扩充风格库**：在 `references/design-styles.md` 里按已有结构（pitch / flagship / keywords / signature-moves / when-to-use）加新条目；
- **新增输出格式**：在 `references/` 下加一份（如 `references/email-designs.md`），并在 `SKILL.md` 的索引里指过去。

## 设计哲学

这个 skill 故意保持精简：markdown + 少量 HTML 起手模板 + 几个示例产出，不放任何二进制资产。核心主张是：**好的 hi-fi 设计从已有上下文（品牌、代码库、UI kit）长出来，不是凭空画的**。所以 skill 最重要的任务是——让 Claude 去收集真实上下文、承诺一套视觉系统、避开"预训练路径最小阻力"导致的 AI-slop。这里的每一条规则、每一份参考、每一个资产，都是为这个目标服务的。

## 许可

MIT 协议，见 [LICENSE](LICENSE)。原始 Claude Design 系统提示词版权归属 Anthropic；本仓库是按 Claude Skill 规范重构后的改编版本，用于让 skill 可被外部环境加载。
