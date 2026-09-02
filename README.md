# FIRST-SKILL

> 邱钢的第一个开源项目：一个让 WorkBuddy AI 在特定场景下更专业的 Skill 集合。

## 1. 项目解决什么问题

WorkBuddy 本身是一个通用 AI 助手，但在"把长文转成知识卡片"这类垂直任务上，没有现成的工作流约束。本项目就是为了填补这一类空白——把每个具体场景的"专业做法"写进一个个 Skill（`SKILL.md`），让 WorkBuddy 在对应场景下像有个专家在指挥。

具体而言：

- **对作者**：把"我摸索出来的方法"沉淀成可分发的 Skill，发布后别人也能复用。
- **对使用者**：省去每次都要长篇大论地跟 AI 解释"你要怎么干"。
- **对二次开发者**：每个 Skill 都自带示例输入输出，方便参考着改造出自己的版本。

## 2. 主要功能

当前仓库内置 1 个 Skill：

| Skill 路径 | 一句话功能 |
|---|---|
| [`skills/make-knowledge-cards`](skills/make-knowledge-cards/SKILL.md) | 把本地 Markdown / TXT 文章压成 5–8 张知识卡片 |

`make-knowledge-cards` 的具体能力：

- 输入一篇 `.md` 或 `.txt` 文章，**自动提炼 5–8 个最重要的知识点**。
- 每张卡片输出 4 个字段：标题、核心知识点、通俗解释、举例/自问。
- **严格忠于原文**：不会添加原文里没有的事实、数字或来源。
- **一卡一意**：每张卡片只承载一个独立知识点，自动合并重复角度。
- **明确边界**：不抓网页、不解析 PDF/EPUB、不导出 Anki、不生成图形化版式。

## 3. 安装方法

把仓库克隆到本地即可：

```bash
git clone https://github.com/Iron-greatwall/FIRST-SKILL.git
cd FIRST-SKILL
```

仓库本身不依赖任何运行时——没有 `package.json`、没有 `requirements.txt`，**不需要 `npm install`、不需要 `pip install`**。

要让某个 Skill 生效，只需把对应 Skill 的 `SKILL.md` 文件路径交给 WorkBuddy（例如粘贴或作为附件上传），WorkBuddy 就会按其中规则行事。

## 4. 使用方法

### 4.1 使用 make-knowledge-cards

**第 1 步：准备文章**
- 一份本地 `.md` 或 `.txt` 文件。
- 绝对或相对路径均可（建议用绝对路径，免得出错）。

**第 2 步：让 WorkBuddy 加载 Skill**
- 把 `skills/make-knowledge-cards/SKILL.md` 的内容贴给 WorkBuddy，或在 WorkBuddy 里选择加载该 Skill。

**第 3 步：发出指令**
- 例：`请按 make-knowledge-cards 这个 Skill，把 /path/to/your-article.md 转成知识卡片。`

**第 4 步：拿到结果**
- WorkBuddy 会直接在对话里输出 Markdown 卡片，可直接复制。
- 如果你指定了输出路径，WorkBuddy 也会按默认规则（`<原文件名>.cards.md`，写在原文件同目录）落地成文件。

### 4.2 想做二次开发

1. 复制 `skills/make-knowledge-cards/` 整个目录，改名成你要做的新 Skill。
2. 改 `SKILL.md` 里的 name / description / 触发规则 / 输出格式。
3. 在 `samples/` 里放几篇样本文章，自己手动跑一遍，验证 Skill 描述的输出与实际一致。
4. 跑 `scripts/quick_validate.py <你的 Skill 路径>` 验证合规。

## 5. 输入输出示例

### 5.1 make-knowledge-cards 输入

**输入文件**：`skills/make-knowledge-cards/samples/deep-work.md`（讲"深度工作"的方法论短文）。

### 5.2 make-knowledge-cards 输出

```markdown
# 知识卡片：深度工作

> 来源：deep-work.md
> 卡片数：6（5 ≤ 6 ≤ 8）

---

### 卡片 1：深度工作定义
- **核心知识点**：Cal Newport 提出的概念，指在没有干扰的专注状态下进行的专业活动，会把认知能力推向极限，产出新的有价值内容，且很难被复制。
- **通俗解释**：心无旁骛地啃下一件难事的整段时间，产出的东西质量高、别人难以模仿。
- **举例 / 自问**：今天我有连续 90 分钟没看手机、没回消息地写过东西吗？

---

### 卡片 2：浮浅工作定义
- **核心知识点**：对认知要求不高、常在分心状态下完成、不会带来太多新价值的事务，如回复邮件、参加冗长会议、刷社交媒体。
- **通俗解释**："看起来在工作其实没产出"的事。
- **举例 / 自问**：一天回 100 封邮件算深度工作吗？

---

### 卡片 3：深度工作为何稀缺
- **核心知识点**：三个原因叠加——知识工作产出越来越可衡量、注意力被切到分钟级、知识经济奖励快速响应而低估慢产出。
- **通俗解释**：环境越来越在意"反应快"，"想得深"反成稀有动物。
- **举例 / 自问**：上次连续思考超过 2 小时是什么时候？

---

### 卡片 4：深度时间块
- **核心知识点**：每天挑一段固定时间（如早 9–12），关闭所有通知，只做最重要的事。
- **通俗解释**：把"专心做事"像开重要会议一样写进日历，别人看到就不会打扰。
- **举例 / 自问**：今天日历上有"不可打断"的 2 小时块吗？

---

### 卡片 5：无聊训练
- **核心知识点**：从戒掉"一空就刷手机"开始，让自己重新适应独处与思考。
- **通俗解释**：脑子能独处才有专注力；一空就刷手机是把独处让给了算法。
- **举例 / 自问**：等电梯 30 秒不看手机，能忍吗？

---

### 卡片 6：定期离线
- **核心知识点**：每周留出半天完全离线，让大脑有机会做"后台整理"。
- **通俗解释**："洗完澡突然想到解法"就是离线时大脑在后台整理。
- **举例 / 自问**：这周有半天完全没碰屏幕吗？
```

更多完整示例见 `skills/make-knowledge-cards/samples/`。

---

## 项目结构

```
FIRST-SKILL/
├── README.md                     ← 你正在看的这个文件
└── skills/
    └── make-knowledge-cards/
        ├── SKILL.md              ← Skill 定义（加载到 WorkBuddy 后即生效）
        └── samples/              ← 测试样本
                ├── deep-work.md
                └── git-three-areas.md
```

## 开发者

邱钢 ([@Iron-greatwall](https://github.com/Iron-greatwall))

## 许可

[MIT License](LICENSE) — Copyright (c) 2026 邱钢 (Qiu Gang)