# Notion Paper Reading 管理

本目录为 Notion Paper Reading 数据库的本地资产管理目录，配合 Notion MCP 使用。

## 目录结构

```
notion/
├── README.md                   # 本文件
├── papers/                     # 论文资产（按论文 slug 分子目录）
│   └── {paper-slug}/           # 例: bernini
│       ├── figures/            # 页面正文图片与封面
│       │   ├── *-cover.png     # 手绘封面图
│       │   └── fig*.png/webp   # 论文插图
│       ├── *-paper.pdf         # 原始 PDF
│       ├── *-paper-zh.pdf      # 中文翻译 PDF
│       └── *-paper-dual.pdf    # 双语对照 PDF（可选，不默认展示）
└── ...
```

## Notion 数据库信息

| 项目 | 值 |
|------|-----|
| Database | 📚 Paper Reading |
| URL | https://app.notion.com/p/7df16e1957634aeaa7e3da54d9b545cb |
| Data Source ID | 57a3a0ff-6045-4b8a-bbe6-0acaa787dcf0 |

## 数据库字段

| 字段 | 类型 | 说明 |
|------|------|------|
| Title | TITLE | 论文标题 |
| Status | SELECT | Unread / Reading / Finished / Revisit |
| Relevance | SELECT | 精读复现 / 细读笔记 / 略读摘要 / 存档备查 |
| Domain | SELECT | Video Generation / Image Generation / Diffusion / Attention / Segmentation / Super-Resolution / Denoising / Dehazing / Medical / LLM / Other |
| Tags | MULTI_SELECT | 按 5 维正交分类，见下方 Tags 体系说明 |
| Venue | SELECT | CVPR / ICCV / ECCV / NeurIPS / ICML / ICLR / AAAI / ARXIV / SIGGRAPH / Other |
| Published | DATE | 发表年月 |
| Institution | MULTI_SELECT | ByteDance / Google / Meta / Microsoft / NVIDIA / OpenAI / Tencent / Alibaba / Baidu / Academic / Other |
| Open Source | MULTI_SELECT | Weights / Inference Code / Training Code / Dataset / Pending / Closed |
| Links | RICH_TEXT | 格式: Paper: url / Code: url / Project: url（每行一个） |
| PDFs | FILES | 原文 + 翻译 + 双语 PDF（需通过 Notion 网页端上传或填外部 URL） |
| Brief | RICH_TEXT | 一句话概括论文核心贡献 |
| Added | DATE | 入库日期 |
| Cover | FILES | 手绘风格封面图（Gallery 视图卡片封面来源） |

## Tags 体系

按 5 个正交维度组织，每篇论文选取最具区分性的 tags（通常 4-8 个）：

| 维度 | Tags | 说明 |
|------|------|------|
| 媒体 | Video, Image, Audio, 3D, Avatar | 处理什么媒体类型 |
| 任务 | Generate, Edit, Restore, Understand | 做什么任务 |
| 方法 | Diffusion, Autoregressive, Flow, GAN, Tokenizer | 核心生成范式 |
| 架构 | DiT, UNet, MLLM, ViT, VAE | 关键模块 |
| 关注点 | Data, Alignment, Distillation, Efficiency, World-Model, Eval, Camera, Motion, Controllable | 论文核心贡献方向 |

示例：Bernini → `Video, Generate, Edit, Diffusion, MLLM, DiT, Alignment, Data`

## 页面写作规范

论文页面按 **Lilian Weng 技术博客** 风格撰写，参考：

- https://lilianweng.github.io/posts/2021-07-11-diffusion-models/
- https://lilianweng.github.io/posts/2023-06-23-agent/
- https://lilianweng.github.io/posts/2024-07-07-hallucination/

核心目标：**人类友好、可连续阅读、像讲课一样解释论文**，不是字段表、摘要翻译或 bullet 清单。

### 叙事结构

1. **开篇先给冲突**：从真实痛点 / 反直觉现象 / 读者熟悉的问题切入
2. **再给核心洞察**：用一个 callout 讲清论文真正解决什么
3. **按 Why → How → Evidence → Limitation 展开**：先解释为什么难，再讲关键机制，再看实验和边界
4. **每个小节解决一个问题**：标题要像文章段落，不要像元数据字段
5. **结尾给 Take-away**：说明这篇对后续系统设计有什么启发

### 排版组件

- `<callout>`：用于高亮问题、核心观点、反直觉结论、关键数据、Take-away；不要每段都堆
- `<details>`：用于折叠公式推导、训练细节、长表格、复现风险，保证主线顺滑
- `<columns>`：页面顶部展示 PDF 文件；正文少量并排内容也可用
- `<table>`：只放真正有比较价值的信息，避免指标刷屏
- 代码块：可用于伪代码、流程摘要、检查清单，让复杂机制更直观
- 公式：只放支撑理解的关键公式；公式前后必须解释变量和直觉，避免裸公式
- 图片：引用前后要说明“这张图看什么”，不能只贴图

### 避免写法

- 不要只翻译 abstract
- 不要把论文信息卡整段塞进正文
- 不要连续 bullet point 堆模块名
- 不要大段罗列指标，关键数值进 callout 或折叠
- 不要把未披露的模型结构、训练 recipe、数据规模脑补成事实
- 商业技术报告要明确区分“已披露能力 / 未披露实现细节”

## 页面顶部 PDF 格式

统一在页面正文开头使用双栏 file block，和 Bernini 模板一致：

```markdown
<columns>
	<column>
		<file src="https://raw.githubusercontent.com/huarzone/notion/main/papers/{slug}/{slug}-paper.pdf">Original (xxMB)</file>
	</column>
	<column>
		<file src="https://raw.githubusercontent.com/huarzone/notion/main/papers/{slug}/{slug}-paper-zh.pdf">Chinese (xxMB)</file>
	</column>
</columns>
---
```

注意：正文顶部默认只展示 `Original` 和 `Chinese` 两个文件；`*-paper-dual.pdf` 可保留在仓库，但不默认展示，避免文件过大和页面拥挤。

## 视图

| 视图名 | 类型 | 展示字段 | 说明 |
|--------|------|----------|------|
| 📖 全部论文 | Gallery | Title, Domain, Tags | 卡片展示，Cover 属性作为封面图 |
| 📋 详细表格 | Table | Title, Status, Relevance, Domain, Venue, Institution, Published, Brief | 全量筛选排序 |
| 🗂️ 阅读看板 | Board | Title, Brief | 按 Status 分组，拖拽管理进度 |

## 封面图生成

- API: `https://openapi.huarzone.com/v1`
- Model: `gpt-5.4-mini`
- 风格: 手绘 Sketchnote（marker strokes, pink/orange/teal 配色）
- 模板: `/workspace/outputs/dl_sketchnote/prompts/style_template.md`

## PDF 翻译

工具: pdf2zh (PDFMathTranslate)

安全要求：README 和任何提交内容中只允许写环境变量占位，严禁写入真实 API key / Notion token。公开仓库中一旦出现真实密钥，必须立即吊销并轮换。

```bash
OPENAI_BASE_URL="https://openapi.huarzone.com/v1" \
OPENAI_API_KEY="$OPENAI_API_KEY" \
pdf2zh paper.pdf -s "openai:gpt-5.4-mini" -li en -lo zh -t 4
```

输出:
- `paper-zh.pdf` — 中文翻译版
- `paper-dual.pdf` — 双语对照版

## 新增论文流程

1. 下载原始 PDF 到 `papers/{slug}/`
2. 运行 pdf2zh 翻译生成中文版和双语版
3. 生成手绘封面图
4. 在 Notion 创建条目，填写元信息
5. 上传 Cover 图（外部 URL 或 Notion 网页端）
6. 上传 PDFs（Notion 网页端拖拽，文件较大不适合外部图床）

## PDF 上传方案

本项目通过 GitHub 公开仓库托管 PDF 文件，使用 raw URL 写入 Notion：

- 仓库: https://github.com/huarzone/notion
- Raw URL 格式: `https://raw.githubusercontent.com/huarzone/notion/main/papers/{slug}/{filename}`

### MCP FILES 属性限制

- MCP 的 FILES 属性仅支持写入单个外部 URL（多个会被拼接为一个字符串）
- 推荐做法：PDFs 属性写入原始论文 URL；页面正文顶部用 `<columns>` + 两个 `<file>` 块展示 `Original (xxMB)` 与 `Chinese (xxMB)`

### Notion REST API 分片上传（备选）

对于已添加 Integration 连接的页面，可通过 REST API 直接上传大文件：

```bash
# 1. 创建 multi_part 上传对象
curl -X POST "https://api.notion.com/v1/file_uploads" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Notion-Version: 2026-03-11" \
  -H "Content-Type: application/json" \
  -d '{"mode":"multi_part","filename":"paper.pdf","content_type":"application/pdf","number_of_parts":3}'

# 2. 分片发送（每片 5-20MB，最后一片可小于 5MB）
curl -X POST "$UPLOAD_URL" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Notion-Version: 2026-03-11" \
  -F "file=@part_1.bin" -F "part_number=1"

# 3. 完成上传
curl -X POST "https://api.notion.com/v1/file_uploads/$UPLOAD_ID/complete" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Notion-Version: 2026-03-11" \
  -H "Content-Type: application/json" -d '{}'
```

前提：目标页面/数据库需共享给 Integration（Connections → 添加 integration）

## 注意事项

- Cover 字段: 可通过 MCP 填入外部图片 URL（如 freeimage.host）
- pdf2zh 依赖修复: 当前 numpy 版本需将 `np.fromstring` 改为 `np.frombuffer`（已修复）
- GitHub 单文件限制 100MB，超大双语 PDF 需注意

## 踩坑记录

### 图片显示

- **引用前核对资产是否存在**：写正文前先对比源目录 `sources/{source-slug}/images/` 与本仓库 `papers/{slug}/figures/`，正文引用到的每张图必须已复制到 `figures/` 并 push
- **缺图常见原因**：源论文目录已有图，但只复制了部分 figure 到 `/workspace/notion`；例如正文引用 `fig06-applications.png`，但仓库只含 `fig01`–`fig04`，Notion raw URL 就会 404
- **黑色/透明背景图片**：论文源码提取的 figure 可能有黑色背景（如 LPM fig05-framework），在 Notion 浅色模式下很突兀。用 PIL 将近黑像素 (R<30,G<30,B<30) 替换为白色 (255,255,255) 后重新推送
- **Raw URL 格式**：`https://raw.githubusercontent.com/huarzone/notion/main/papers/{slug}/figures/{filename}`
- **Notion 图片缓存**：更新 GitHub 上的图片后 Notion 不会自动刷新缓存，需要在 URL 后加查询参数（如 `?v=2`）强制 Notion 重新拉取图片

### Notion MCP 格式

- **Toggle (`<details>`) 必须缩进 children**：`<summary>` 后的内容必须用 tab 缩进，否则内容会跑到 toggle 外面
- **Callout 用 `<callout>` 标签**：不要用 `>` blockquote 模拟 callout，直接用 `<callout icon="💡" color="blue_bg">` + 缩进 children
- **`<details>` 不能转义**：写 `\<details\>` 会被 Notion 当作纯文本渲染，必须写原始 `<details>`
- **Multi-select 值格式**：必须用 JSON 数组字符串 `"[\"Weights\",\"Inference Code\"]"`，逗号分隔的字符串会报错
- **Columns 用 `<columns>` 标签**：PDF 链接等并排展示用 `<columns><column>...</column></columns>`

### 封面图生成 API

- **API 端点**：`https://openapi.huarzone.com/v1/images/generations`
- **返回格式是 base64**：`response.data[0].b64_json`，不是 URL，需要本地解码保存后推送到 GitHub
- **设置封面**：用 `update_page` 的 `cover` 参数传入 GitHub raw URL（不是 Cover 属性字段）

### 页面删除

- **Notion MCP 无删除 API**：只能通过 REST API `PATCH /v1/pages/{id}` 设置 `{"in_trash": true}`
- **前提是 Integration 有权限**：目标页面必须已共享给 Integration（Connections → 添加），否则返回 404
- **手动删除**：如果 API 无权限，只能在 Notion 网页端手动删除

### Institution 字段

- **不能修改已有 option 的颜色**：`ALTER COLUMN` 添加新选项时，已有选项必须保持原色，否则报 `Cannot update color of select with name: XXX`
- **新增选项**：只写新增的 option 即可，不需要重复列出所有已有选项
