# 使用ffmpeg整体放大或者缩小音量

先使用dynaudnorm把整体音量`动态`调整成一致的，偶尔标高的某些毛刺会被滤下来，使整体语音的最大音量在同一个级别。
再使用loudnorm把整体`线性`放大或者缩小。

如果不先使用dynaudnorm把整体音量调整成一致的，会导致loudnorm调整时被高音的毛刺限制最大值。

```bash
ffmpeg -i xxx.wav -filter_complex "dynaudnorm=framelen=10:compress=5,loudnorm=I=-10:TP=-3:" -y output.wav
```
