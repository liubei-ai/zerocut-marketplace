---
name: 转为横屏视频
description: 将用户给出的一个视频转为横屏视频（16:9）
---

## 操作步骤
1. 确保项目已正确开启：`project-open` 已被调用
2. 调用 `run-ffmpeg-command` 将视频中的音轨提取出，**格式mp3**，保存为 `<用户视频>-audio.mp3`
3. 调用 `run-ffmpeg-command` 获取视频总时长，然后按如下操作：
  * **多次**调用 `run-ffmpeg-command` ，依次将视频按照每7秒切分为多个片段，每个命令生成一段视频，保存为 `<用户视频>-part-<序号>.mp4`
    - 切片命令格式： `ffmpeg -ss <开始时间> -i <用户视频>.mp4 -t 7 -an <用户视频>-part-<序号>.mp4`
    - 例如原视频15秒，将被切分为3个片段：`<用户视频>-part-0.mp4`, `<用户视频>-part-1.mp4`, `<用户视频>-part-2.mp4`，其中part-0为0-7秒，part-1为7-14秒，part-2为14-15秒
    - 多次调用，确保每次只生成单个视频片段，不要同时生成多个
  * 调用 `run-ffmpeg-command` 校验切分后的视频片段时长，确保未超过7秒，如超过，则说明切分有误，应重新切分
  * 调用 `generate-video` 将各切分片段生成横屏视频，model 为 `vidu-pro`，prompt 为`将原视频比例改为16:9，保持原视频内容及人、物比例不变，满屏，无字幕`，aspect_ratio 为 `16:9`，时长与当前切片时长相同
4. 调用 `audio-video-sync` 将所有横屏视频片段合并为一个视频，与 `<用户视频>-audio.mp3` 合并，保存为 `<用户视频>-horizontal.mp4` 输出
5. 调用 `project-close` 结束任务并返回执行结果

