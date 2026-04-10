---
name: concept-card
description: 通用的知识概念卡片生成器。当用户要求制作任意主题的知识卡片、概念卡、学习卡、信息卡（如"帮我做一张关于XXX的卡片"、"生成一张解释YYY的科普卡"）时激活。支持任何主题：投资、金融、历史、科学、哲学、心理学、语言、编程等。输出专业视觉卡片图片（PNG，1200×675px，深色金黑配色）。
---

# 通用知识概念卡片 · SKILL.md

将任意主题制作成结构清晰、视觉专业的深色风格知识卡片（1200×675px，适合小红书/朋友圈/飞书/Notion分享）。

---

## 工作流程

### Step 1：分析主题，确定卡片类型

根据给定主题，判断属于哪一类，使用对应的内容结构：

| 主题类型 | 卡片结构 | 适用场景 |
|---------|---------|---------|
| 投资/金融 | 定义+公式+3模型+算例 | 理财概念 |
| 心理学/行为 | 定义+3效应+案例+误区 | 认知偏差 |
| 科学/技术 | 原理+3关键点+应用+趣闻 | 科普 |
| 历史/事件 | 背景+经过+3意义+启示 | 历史 |
| 哲学/思想 | 核心问题+3流派+结论 | 思维 |
| 语言/词汇 | 词源+含义+3用法+例句 | 学习 |
| 编程/技术 | 概念+3要点+代码示例+坑 | 开发 |

**如不确定，默认为"通用深度"结构（定义+3要点+案例+误区）。**

### Step 2：组织内容

为每个 slot 填写具体内容：

```
【顶部】标签（类型）+ 主标题 + 英文副标题

【左侧 · 定义区】
- 一句话核心定义（≤25字）
- 核心公式/核心等式（可选，没有则跳过）
- 误区提示（最常见的理解错误）

【中间 · 核心内容 × 3】
- 每个：图标emoji + 名称 + 标签 + 一句话解释

【右侧 · 案例/应用】
- 具体场景或算例
- 两个对比选项（含具体数字）
- 结论框

【底部】金句 + 标签
```

### Step 3：生成 HTML

复制 `assets/template.html`，替换以下内容：

**必须替换的字段：**

```html
<!-- 顶部标签 -->
<div class="tag">📈 投资 · 概念卡</div>

<!-- 主标题（关键词高亮用span包裹） -->
<div class="main-title">机会<span>成本</span></div>
<div class="sub-title">Opportunity Cost</div>

<!-- 公式框（无公式则删除整个formula-box块） -->
<div class="formula-box">...</div>

<!-- 误区（无误区则删除misconception块） -->
<div class="misconception">...</div>

<!-- 金句 -->
<div class="quote">...</div>

<!-- 中间3个model-card（替换emoji、名称、标签、描述） -->
<div class="model-card">...</div>
<div class="model-card">...</div>
<div class="model-card">...</div>

<!-- 右侧案例 -->
<div class="case-title">...</div>
<div class="case-scenario">...</div>
<div class="option option-a">...</div>
<div class="option option-b">...</div>
<div class="verdict">...</div>

<!-- 页脚标签 -->
<span class="footer-tag">#投资思维</span>
```

**没有公式/误区/右侧案例时的处理：**
- 删除对应 HTML 块即可，布局会自动适应

### Step 4：渲染截图

使用 canvas present 加载 HTML，snapshot 导出 PNG：

```
canvas(action=present, url="file:///workspace/skills/concept-card/assets/template.html")
canvas(action=snapshot, outputFormat="png", quality=1.0)
```

截图保存至：`/workspace/outputs/concept_card_{主题拼音}_{日期}.png`

---

## 文件索引

- **HTML模板**：`assets/template.html` — 深色金黑视觉风格，1200×675px
- **结构参考**：`references/content-types.md` — 7种主题类型的结构模板
- **金句库**：`references/quotes.md` — 各领域经典引言

---

## 视觉规范（直接复用，无需修改）

```
背景：    #0a0e1a  （深蓝黑）
金色：    #f0b429  （主色：标题/公式/关键词高亮）
白色：    #ffffff  （大标题）
灰色：    #9ca3af  （副标题/说明文字）
绿色：    #22c55e  （结论/正确选项）
红色：    #f87171  （陷阱/误区标签）
蓝色：    #3b82f6  （中性标签）
浅背景：  rgba(255,255,255,0.03)  （卡片/区块背景）
边框：    rgba(255,255,255,0.07)
```

---

## ⚠️ 禁止事项

- ❌ 禁止在卡片显示序号/期数（如"Day 1"）
- ❌ 禁止显示日期或第几天标注
- ✅ 只显示：标签 + 概念名 + 内容

## 快速模板（无公式/误区版）

适用于没有公式、不需要误区提示的主题（如历史事件、心理学效应）：

```
【左侧】定义区 = 标题 + 一句话 + 金句
【中间】3个 model-card
【右侧】案例/时间线/对比
【底部】标签
```

删除不需要的 HTML 块，布局自动弹性适应。
