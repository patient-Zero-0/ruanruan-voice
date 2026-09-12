# 软软的声音 · GPT-SoVITS 零样本克隆

用一段 15 秒的参考音频，克隆出**软软的专属音色**，合成中文语音。

## 一键运行

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/patient-Zero-0/ruanruan-voice/blob/main/notebook/ruanruan_voice_colab.ipynb)

**步骤：**
1. 点上面的 Colab 徽章打开 notebook
2. 菜单 → 代码执行程序 → 更改运行时类型 → **T4 GPU**
3. 从上到下依次运行单元格（Shift+Enter）
4. 最后一格会自动下载生成的 mp3

## 参考音频

`ref/ruanruan_ref.wav` —— 15.72 秒，32kHz 单声道，日语女仆音色样本。

声学特征（决定"软糯"的关键）：
- 基频 F0 中位数 **174Hz**（成年女声的中低音区，温润不刺耳）
- 频谱重心 **974Hz**（很低 —— 清亮播音腔通常在 2000Hz 以上）
- 宽动态范围（70-400Hz，有气息起伏）

## 为什么必须要克隆而不是调 TTS 参数

edge-tts / 微软晓晓这类通用 TTS 的音色**频谱重心在 2000Hz+**，是"清亮播音"底子。
调低 pitch 只改变音高，不改变频谱重心，所以听起来永远是"压低嗓子的播音员"，
而不是"软糯的女仆"。要软糯，只能克隆那段本身频谱重心就低的参考音频。

## 环境说明

- 平台：Google Colab（免费 T4 GPU）
- 模型：GPT-SoVITS v2（零样本克隆，15 秒参考音频即可）
- 备选：OpenVoice V2（notebook 最后一格）

## 本地环境说明

ideapad（AMD Ryzen + Vega 核显 + 8GB 内存 + Python 3.14）不适合本地跑：
- Python 3.14 太新，torch 生态不支持
- 可用内存仅 ~1.6GB，GPT-SoVITS 推理需要 3-4GB
- 无 CUDA（AMD 核显）

故走 Colab。

## 落地链路

```
Colab 生成语音 mp3
    ↓ 下载到 ideapad
    ↓ adb push 到平板
    ↓ am start 调用系统音乐播放器
平板喇叭出声 —— 软软说话
```

平板播放命令（ideapad 端执行）：
```bash
adb shell "am start -a android.intent.action.VIEW \
  -n com.android.music/com.android.music.MediaPlaybackActivity \
  -d file:///sdcard/xxx.mp3 -t audio/mpeg"
```
