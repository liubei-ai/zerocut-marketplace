---
description: 核心规则（不触发）
---

# 执行逻辑

执行任何任务前，根据用户需求，访问 `./.trae/skills` 目录下的技能文件，调用相应的技能
  * ‼️ 解决任务时，选择技能优先于工具调用，如无匹配的技能，使用现有工具组合
  * 使用技能时，技能中的设定优先级高于通用规则，如与通用规则矛盾，应覆盖通用规则设定
  * 如果你还不了解技能，`skills_index.md` 中有列出所有可用技能列表，可查阅该文件
  * 技能文件中的规则若与核心规则冲突，以技能文件中的规则为准

## 重要步骤（‼️务必执行）

### skill_context.md

在执行具体技能前，先查看对应技能的文件（`./.trae/skills/技能名称/SKILL.md`），确认技能的具体内容和约束条件

在 `./.trae/rules` 目录下创建或查看 `skill_context.md` 文件，判断技能是否改变，如改变，**直接覆盖文件原有内容**，写入新内容：

- 技能名称：[技能名称]
- 技能描述：[技能描述]
- 技能核心约束（如有）：[技能核心约束]
- 技能默认设定：（比如技能中规定的默认模型、默认参数等）
- 其他注意事项

在每次聊天任务执行中，均应通过查看 `skill_context.md` 文件，确认所调用的技能是否变化，如变化，重写覆盖 `skill_context.md` 文件，更新为新技能的相关信息

技能核心约束中禁止使用的MCP工具，严格禁用，不得在技能执行中调用

### action_context.md

在执行具体任务后，根据任务的执行结果，务必创建或更新 `./.trae/actions/action_context.md` 文件，**增量**内容添加到**文件末尾**：

- 做了什么：[做了什么]
  - 用了什么工具：[用了什么工具]
  - 用了什么参数：[用了什么参数]
  - 用了什么技能：[用了什么技能]
- 执行的结果：[执行的结果]
- 其他注意事项

在每次聊天中，检查`action_context.md`文件内容，与对话上下文印证，确保指令能被准确无误执行

---

## 项目结构与规范

```
projects/<id>/
  ├─ materials/      # 素材文件
  ├─ output/         # 成品输出（可选）
  ├─ storyboard.json    # 分镜脚本
  └─ media_logs.json  # 素材生成日志
```

### 视频合成规范
- 画幅：提前确定横竖屏，竖屏720x1280，横屏1280x720，如无特殊要求，竖屏(720x1280)优先
- 分辨率：**只有用户明确指定时才使用720x1280和1280x720之外的分辨率**，禁止擅自使用其他分辨率
- 字幕样式：
  * 默认字体：`"Noto Sans CJK SC"`
- BGM 音量控制
  * 音量：默认BGM音量控制为-15db（`audioVolume=0.177`），合成时通过设置`audio-video-async`的`audioVolume`参数可以调整BGM音量

## 素材规则

### 核心原则（不得违反‼️）
0. 只能使用 materials 目录下的素材，若用户要使用其他目录下的素材作为参考图或参考视频，应将它们先复制到 materials 目录下
1. 有 storyboard.json 的场景
  * 素材生成时，一律默认不跳过一致性检查`skipConsistencyCheck=false`
  * 只有当用户明确要求跳过一致性检查时，才指定`skipConsistencyCheck=true`
2. 无 storyboard.json 的场景
  * 素材生成时，默认跳过一致性检查`skipConsistencyCheck=true`
3. 素材生成**不可并行**（禁止Trae的并行任务），必须严格按顺序生成，生成一个素材成功才能接着生成另一个
4. 生成素材命名用语义明确的文件名命名，优先使用`递增数字ID_中文`的命名规则命名文件
  - 递增序号根据本地 materials 目录下已存在的文件确定，不用检查沙箱文件
  - 例如：`001_介绍.mp4`、`002_主体.mp4`、`003_结尾.mp4`等

### 视频合成规范
- 画幅：提前确定横竖屏，竖屏720x1280，横屏1280x720，如无特殊要求，竖屏(720x1280)优先
- 分辨率：**只有用户明确指定时才使用720x1280和1280x720之外的分辨率**，禁止擅自使用其他分辨率
- 字幕样式：
  * 默认字体：`"Noto Sans CJK SC"`
- BGM 音量控制
  * 音量：默认BGM音量控制为-15db（`audioVolume=0.177`），合成时通过设置`audio-video-async`的`audioVolume`参数可以调整BGM音量

## JSON 文件一致性与质量规则

### 通用
1. 引号规则：JSON格式中，字符串中如包含引号，优先使用单引号，双引号需转义

### storyboard.json
1. storyboard.json 中的 start_frame、end_frame 分别对应视频生成`generate-video`的首帧和尾帧
2. storyboard.json 中的 video_prompt 对应视频生成`generate-video`的 prompt

## 知识库

### 通用技巧及术语
1. 生成视频的几种方式
  - 参考生视频（默认）：根据用户要求，0～6张参考图（最多6张），以 video_prompt 的提示词用`generate-video`生成视频，通过 `aspectRatio` 参数来控制横竖屏。
  - 首帧图生视频（图生）：先根据 start_frame 生成首帧图片如 sc01_start.png，然后用该图片作为视频的第一帧，以 video_prompt 的提示词用`generate-video`生成视频
  - 首尾帧生视频（连续镜头）：先根据 start_frame 生成首帧图片如 sc01_start.png，再根据 end_frame 生成尾帧图片如 sc01_end.png，用这两张图片作为视频的首尾帧，以 video_prompt 的提示词用`generate-video`生成视频

### 故障排查和自动处理
  * `generate-image` 失败，若用户未指定模型，可以换一个模型重试，替换顺序为 `banana` → `seedream` → `banana-pro`
    - 如果用户指定了模型，则不重试，直接通知用户失败的结果，让用户自行选择是否要换一个模型
  * `generate-video` 失败且失败原因是内容相关（如包含敏感信息）
    - **禁止**随意更换模型，应删除或修改可能的敏感信息后重试；若还是失败，停下来报告原因，并询问用户要如何继续

### 部分工具使用说明

* 生成素材时，若用户指定了使用的模型，且当前素材生成失败，应通知用户失败的结果，让用户自行选择是否要更换模型，而不是擅自作主替用户自动更换，因为不同模型意味着不同的价格，改用贵的模型不一定是用户期望的结果
* 生成素材的所有工具中的 prompt 参数，除非用户主动要求优化，否则严格按照用户要求填写，不要自行添加额外内容

1. 生成图片`generate-image`
  * 支持以下模型
    - banana / banana-pro / banana2 - gemini nano-banana 系列
    - seedream / seedream-pro / seedream-5l - seedream 系列
    - zero 模型
    - wan / wan-pro 通义万相系列
    - mj / mj-niji - “midjourney”系列
    - qwen 通义千问图片编辑模型，适合用于转换拍摄角度
  * 支持以下类型图片
    - default 通用默认图片
    - line-sketch 黑白线稿
    - storyboard 分镜图
    - subject-turnaround 主体三视图
    - texture-grid 纹理图网格图，注意如果只是生成普通的法线或深度图使用 default

2. 生成视频 `generate-video`
  * prompt 尽量忠于用户的原始需求，简洁描述，不要添加与视频描述无关的内容，若传了sceneIndex，则必须与对应分镜的video_prompt设置完全一致
  * 标准提示词格式：景别（可选）+主体+对话（可选）+运动+运镜/切镜（可选）
    - 例：中远景，乌鸦从河边的枯树上飞起，快速飞到河对岸，停在对岸石头上，镜头环绕跟随
  * 支持以下模型(type)
    - seedance-1.5-pro / seedance-2.0-fast / seedance-2.0 - 即梦 seedance 系列
    - vidu / vidu-pro / viduq3 / viduq3-turbo / viduq3-pro - Vidu 系列
    - kling / kling-v3 - 可灵系列
    - pixverse - 拍我系列
    - wan / wan-flash - 通义万相系列
    - zerocut3.0 / zerocut3.0-turbo / zerocut3.0-c1 / zerocut3.0-pro / zerocut3.0-pro-fast - ZeroCut 优化模型
    - zerocut-avatar-1.0 / zerocut-avatar-1.5 - ZeroCut 数字人模型，可生成5-240秒的数字人口播视频
    - zerocut-mv-1.0 - ZeroCut 音乐视频模型，可生成240秒内的 Music Video
  * 使用方式
    - 参考（多参）生视频：默认方式，参考图 images 根据情况传 `{ type: 'reference' }` 表示参考图或 `{ type: 'storyboard' }` 表示分镜图，可传入多张图（最多9张），用 aspectRatio 来控制横竖屏。
      - 若需要使用**写实风格的真实人物**形象（包括人物三视图），务必用 `{ type: 'person' }`，AI 会根据这个图片建模人物形象
      - 如非写实风格的真实人物形象类型，比如动漫人物、非真人主体（动物等），参考图一律默认用 `reference` 类型
    - 首（尾）帧生视频：当用户明确说图生视频，或者说以xx图为首帧时，参考图 images 传 `{ type: 'first_frame', ... }` 和可选的 `{ type: 'last_frame', ...}` 为图生视频（首尾帧），此时不能传其他类型的参考图，视频画面宽高自适应图片，不需要 aspectRatio。

  * 模型约束条件
    - 第一次生成视频时，除非用户明确提出使用其他模型，否则默认用 vidu 模型；
    - 除非用户明确要求高清或指定分辨率，否则 resolution 一律用 720p
    - 若用户之前生成过视频，无特别指定，优先使用上一轮使用的视频模型及规格参数

3. 文本转语音 `text-to-speech`
  * prompt 语音音色描述，和voice二选一，如传了voice，则prompt会被忽略
    - 标准格式：性别+年龄区间+声音属性+语速+情绪基线，例如：
        - 一名女性，年龄区间约为 20–25 岁。她的声音音域偏高，带有轻微气声，整体听感清亮、偏薄，声音存在感较轻。语速为中等偏慢，句尾常带有轻微延音。情绪基线温和而内敛，隐约带着一丝不安与犹豫，给人以敏感、克制的印象。说中文普通话。    

4. 搜索工具 `search-context`
  * 如果用户提交的内容包含的信息不完整，你可以要求用户补充信息，也可以用搜索工具帮你完善信息
  * 如系统有自有搜索工具，优先使用系统自有搜索工具，可不使用本工具
