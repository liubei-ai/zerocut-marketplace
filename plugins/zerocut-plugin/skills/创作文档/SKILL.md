---
description: 当用户希望使用 pandoc 基于素材生成文档时使用，例如将生成的素材图片转成pptx、docx、pdf时，触发此 skill。
---

## 核心约束
- 除非用户指定，生成图片的模型默认使用 `banana2`，默认横屏2K，图片大小为 2560x1440
- ‼️因为pandoc不支持中文文件名，生成的图片素材的文件名只能用数字、英文、下划线、中划线，不能用其他unicode字幕
- 使用 `run-pandoc-command` MCP 工具执行实际的文档生成。
  - 为了更好地排版，合成内容的 md 不直接使用任何文字，请务必将文字直接生成到图片中
- 除非工具执行成功，否则不要声称文档已经创建完成。
- 生成 PDF 时，pandoc 的 `--pdf-engine` 默认使用 wkhtmltopdf
- 生成 PPT 时，不要使用任何内容填充页面，只设置每一页的背景即可
  ```md
  ## {background-image="001_....png"}

  ## {background-image="002_....png"}
  
  ## {background-image="003_....png"}
  ...
  ```

## 详细步骤
1. 分析并拆解用户需求
2. 根据拆解的需求依次创建图片（文字直接生成到图片中）
3. 根据创建的图片，使用 `run-pandoc-command` MCP 工具执行实际的文档生成。
  - 根据创建的素材生成用于合成的 markdown 文件。
  - 将 markdown 文件和素材图片一起作为 resources 参数同步到沙箱，并通过 command 参数执行相应的命令

