---
name: spoken-script-to-ppt
description: Convert a written Chinese spoken script, narration manuscript, video voiceover script, or lecture manuscript into a sparse, visual-first PPTX that supports the talk instead of transcribing it. Use when the text script exists before the slides and the user asks to extract key messages, plan illustrations, photos, charts, or icons, or build the final deck; do not use for raw audio/video transcription, slide-to-script writing, teleprompters, or ordinary text-heavy reports.
---

# 口播稿视觉 PPT

## 核心任务

把已经定稿或基本定稿的口播文案转成“视觉表达层”。口播负责解释、铺垫、故事、情绪和细节；PPT 负责结论、结构、证据、关系和记忆点。

PPT 不是文案摘要，也不是同步字幕。保留原稿的信息顺序、立场、限定词和因果边界；不要把“可能”改成“一定”，不要把“相关”画成“导致”。页面顺序必须跟随口播推进，除非用户同时授权修改口播。

## 开始前

1. 创建或编辑 `.pptx` 时，先加载并遵循可用的 `Presentations` skill。本 skill 负责内容筛选、口播同步和默认视觉意图；`Presentations` 负责一次性澄清、模板/现有文件路由、文件生成、字体、来源备注、渲染和技术验收。
2. 从原稿和上下文推断主题、受众、目的、场景和语气，并按 `Presentations` 的澄清规则只询问仍然缺失且会实质改变成品的维度，不重复提问。
3. 用户提供的现有 PPT、模板、品牌规范或明确视觉参考优先于本 skill 的默认画风，并按 `Presentations` 的现有文件/模板路线保留其母版与结构。没有这些输入时，默认中文、16:9、少字多图、现代编辑漫画/商业插画风；这构成明确的自定义视觉方向，不再另选无关通用模板。
4. 需要生成解释型插画时加载 `imagegen` skill。需要核验会变化的事实、数据、品牌或素材时使用实时网页来源，优先一手来源。
5. 若用户只要拆解、提纲或视觉分镜，停在相应产物；否则默认端到端生成并验收 PPTX，不在每一步强制等待确认。

能力降级必须显式：若当前环境没有 `Presentations`，只交付原稿地图、内容筛选、页面蓝图和视觉素材方案，不得声称已生成或验收 PPTX；若没有 `imagegen`，保留可直接执行的插画提示词和素材缺口，不用占位图冒充成品；若无法联网核验，标出待核验事实和素材来源，不伪造来源。

## 工作流

### 1. 建立原稿地图

- 保留完整原稿，并按语义编号为 `P01`、`P02`……；不要按换行机械切页。
- 识别钩子、问题、论点、转折、证据、机制、步骤、案例、限定和 CTA。
- 记录每个单元在原稿中的准确范围，以及适合切换页面的原文锚点。
- 阅读 [content-distillation.md](references/content-distillation.md)，将单元标为 `OWN_SLIDE`、`MERGE`、`VISUAL_ONLY`、`ORAL_ONLY` 或 `EXCLUDE`。

### 2. 提炼信息主脊柱

用一句话分别回答：听众从什么认知出发、经过哪些关键转变、最终应相信什么、记住什么、做什么。

按照“一个认知任务一页”组织页面。连续几段若只是解释同一观点，让一张核心画面覆盖整段；只有新内容构成独立认知任务时才切页。证据与结论、问题与答案能否拆开，以 [content-distillation.md](references/content-distillation.md) 的合并/拆分优先级为准。

### 3. 生成页面蓝图

正式制作前，为每页生成蓝图：

```text
slide_id
source_paragraph_ids
cue_in / cue_out
slide_role
audience_shift
message_headline
visible_copy
visible_text_count
narration_delta
visual_type
visual_brief
asset_or_data_need
layout
speaker_notes
```

`narration_delta` 必须写明“这页刻意没有展示、而由口播补充的内容”。`cue_in` 和 `cue_out` 优先使用原稿短语；封面、结尾、重复短语或已对齐音频使用 [storyboard-and-qa.md](references/storyboard-and-qa.md) 规定的回退锚点，保证翻页时点与口播同步。

### 4. 确定视觉语法

- 每页只选择一种主视觉：真实照片/截图、编辑插画、数据图表、流程或关系图、成熟矢量图标、重点文字。
- 阅读 [visual-routing.md](references/visual-routing.md)，按信息关系选择视觉，不要先找一张泛图再硬套内容。
- 先锁定整套“视觉基因”：色板、线稿、圆角、阴影、颗粒、照片处理、图标家族和人物设定。
- 视觉一般占主内容区的 65%–85%。每页一个主视觉，最多一个辅助视觉；视觉必须承担解释、证据或记忆功能，不能只做装饰。

### 5. 获取和制作素材

- 真实实体或证据：用户素材 > 官方素材 > 授权明确的图库。记录来源和授权状态。
- 抽象概念、情绪、隐喻或不可拍摄机制：生成现代编辑漫画/商业插画。图片内不生成文字、数字、Logo、界面或水印；根据版式预留文字安全区。
- 数据图表：只使用可追溯的数据，单位、时间范围、基线和来源准确。没有数据时改用定性对比或机制图，不伪造图表。
- 图标：使用同一家族的成熟 SVG 图标；官方品牌使用官方文件。禁止 emoji、Unicode 符号、AI 伪图标或混搭图标库。
- 简单流程和图表尽量保持为可编辑原生元素；不要用含有生成式错字的图片替代。

### 6. 制作 PPT

- 一页只承担一个认知任务。观点、证据和对比页用结论式标题；封面保持极简，钩子页可用问题，步骤/行动页可用动词式标题，不强迫所有页面写成判断句。
- 普通观点页标题理想为 8–16 个汉字，硬上限 22 个；正文理想为 0–30 个汉字，硬上限 40 个；最多 3 组短标签，不出现完整段落。步骤、分层、分类和图表页使用 [content-distillation.md](references/content-distillation.md) 的按页型预算。
- 口播中的解释、例子、铺垫和次要限定进入 speaker notes，不复制上屏。
- 把对应原稿段落、翻页锚点和外部来源写入 speaker notes；外部事实和素材继续使用 `Presentations` skill 要求的 `[Sources]` 区块。
- 不在观众可见页面暴露段落编号、制作提示、计时标记、素材状态或其他内部术语。

### 7. 渲染、检查、修订

渲染全部页面，逐页全尺寸检查，再用联系表检查整套节奏。按照 [storyboard-and-qa.md](references/storyboard-and-qa.md) 的内容门、视觉门、口播协同门、来源门和技术门修订，直到所有硬门槛通过。

## 不可妥协的规则

- 不把原稿逐段复制到 PPT，也不让“一段原稿等于一页”。
- 不擅自补造原稿中没有的数据、案例、权威背书或确定性结论。
- 不用 AI 生成的人物、产品、Logo、界面或事件画面冒充真实证据。
- 不为追求“漫画感”扭曲照片、数据比例或品牌识别。
- 不用泛化库存图、装饰图、卡片墙或小图标堆砌来伪装“视觉丰富”。
- 不提前在页面泄露尚未讲到的关键结论。
- 不交付含占位符、乱码、水印、模糊图、拉伸、错误裁切、非预期重叠或溢出的 PPTX。

## 输出

默认只把最终 `.pptx` 作为主要交付物；原稿地图、页面蓝图、来源记录与 QA 记录放在临时工作区，只有用户要求时才额外交付。若调用 `imagegen` 生成了嵌入 PPT 的素材，仍须遵循其交付约定：把选中的项目素材保存在工作区，并在最终说明中报告保存路径、最终提示词和执行模式。

终检问题：删除口播后，PPT 能否让人看懂主线但不会变成完整报告？删除 PPT 后，口播是否仍然完整？两者结合时，视觉是否提供了文字无法提供的理解？
