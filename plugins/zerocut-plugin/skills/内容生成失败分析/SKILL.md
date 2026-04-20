---
description: 当用户调用工具生成素材失败时，使用该技能分析失败原因，并提供解决方案
---

## 图片生成错误
1. 如果有错误码，根据错误码查询错误信息，予以解决或反馈
2. 如果没有错误码，根据错误信息判断问题，通常可能的错误原因和解法包括：
  - 参考图大小超过10M或者宽高之一超过4000px；解决方案：通过`run-ffmpeg-command`工具压缩参考图大小或调整宽高
  - 内容敏感或不合规；解决方案：
    a).检查提示词是否包含敏感内容，如有问题，修改提示词合规
    b).通过`media-analyzer`工具分析参考图内容，如有问题，给出修改建议

## 生图、生视频错误码

### Vidu

Vidu系列视频模型以及banana和banana-pro生图模型参考以下错误码：

- BadRequest: 不合法的请求
- FieldLacking: 缺少字段,具体字段见错误信息
- FieldUnwanted: 不需要传某些字段，具体字段见错误信息
- FieldItemCountOutOfRange：字段超限制
- PageSizeOutOfRange：图像尺寸有问题。要求：图片大小需小于50M,格式只支持 jpg/jpeg/png/webp, 图片长宽比需要小于1:4或者4:1，跳舞特效的图片长宽比需要在 1:1.2 至 1:2 之间
- ImageDownloadFailure：下载用户图片url失败，请检查链接的有效性
- OperationInProcess：请求在处理中
- TaskPromptPolicyViolation：Prompt 触发安审风控
- ImageFormatInvalid：图像格式不符合要求
- AuditSubmitIllegal：输入没有通过安全审核，检查输入内容包括提示词、参考图等是否合规
- CreditInsufficient：积分不足
- CreationPolicyViolation：生成物触发风控，检查、调整提示词重新生成
- ModelUnavailable：请求的模型不可用，调用任务失败，请检查模型类型并重试
- UserCancelled：用户手动终止任务执行
- FieldInvalid：传入参数未通过合法性校验
- ImageCheckBodyJointsFailed：输入图人体检测失败，请重新上传
- ImageCheckFaceFailed：输入图人脸检测失败，请重新上传
- ImageObjectsUndetected：输入图的人体或人脸有遮挡，请重新上传

## Seedance & Seedream

Pro视频模型以及seedream系列生图模型参考以下错误码：

- MissingParameter：缺少必要参数
- InvalidParameter：包含非法参数
- InputTextRiskDetection：火山引擎风险识别产品检测到输入文本可能包含敏感信息，请更换后重试
- InputImageRiskDetection：火山引擎风险识别产品检测到输入图片可能包含敏感信息，请更换后重试
- OutputTextRiskDetection：火山引擎风险识别产品检测到输出文本可能包含敏感信息，请更换后重试
- OutputImageRiskDetection：火山引擎风险识别产品检测到输出图片可能包含敏感信息，请更换后重试
- SensitiveContentDetected：输入文本可能包含敏感信息，请使用其他 prompt
- SensitiveContentDetected.SevereViolation： 输入文本可能包含严重违规相关信息，请您使用其他 prompt
- SensitiveContentDetected.Violence：输入文本可能包含激进行为相关信息，请使用其他 prompt
- InputTextSensitiveContentDetected：输入文本可能包含敏感信息，请使用其他 prompt
- InputImageSensitiveContentDetected：输入图片可能包含敏感信息，请使用其他 prompt
- InputVideoSensitiveContentDetected：输入视频可能包含敏感信息，请使用其他 prompt
- OutputTextSensitiveContentDetected：输出文本可能包含敏感信息，更换输入内容后重试
- OutputImageSensitiveContentDetected：输出图片可能包含敏感信息，更换输入内容后重试
- OutputVideoSensitiveContentDetected：输出视频可能包含敏感信息，更换输入内容后重试
- InvalidParameter.{{Parameter}}：请求参数值不合法。请检查参数值的正确性后重试
- MissingParameter.{{Parameter}}：缺少必要参数 {{Parameter}}。请检查请求参数是否包含 {{Parameter}} 后重试
- InvalidImageURL.EmptyURL：传入的图片 URL 为空
- InvalidImageURL.InvalidFormat：无法解析或处理图片，可能是 Base64 格式不正确、图片数据损坏或格式不支持
- OutofContextError：当请求中包含图片时，文本和图片编码后的总 token 数超过了模型上下文长度限制

