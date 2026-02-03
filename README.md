# AlignX

<div align="center">

简单易用的音频/视频字幕自动对齐工具

[下载整合包](https://github.com/aitranslate/AlignX/releases) | [使用说明](#使用说明) | [常见问题](#常见问题)

</div>

---

## 简介

SubAlign 是一款免费的字幕制作工具，可以自动将你的文稿与音频/视频文件进行精确对齐，生成带有时间轴的字幕文件。

### 适用场景

- 翻译字幕制作
- 视频教程字幕
- 播客/访谈字幕
- 会议记录字幕
- 任何需要为音频/视频添加文字的场景

---

## 功能特点

- **简单易用** - 图形界面操作，无需编程基础
- **自动对齐** - AI 自动识别每个词的起止时间
- **视频支持** - 支持直接导入视频文件，自动提取音频
- **多语言** - 支持中文、英语、日语、韩语等 100+ 种语言
- **智能断句** - 自动处理中英文混排，智能断句分词
- **离线运行** - 无需联网，本地处理保护隐私
- **导出 SRT** - 一键导出标准 SRT 字幕文件

---

## 下载安装

### 整合包下载（推荐）

前往 [Releases 页面](https://github.com/aitranslate/AlignX/releases) 下载最新版本的整合包。

1. 下载压缩包后解压（约 3GB）
2. 双击运行 `AlignX.exe` 即可使用

### 系统要求

- Windows 10/11
- 8GB 内存以上
- 有 NVIDIA 独立显卡可加速处理

---

## 使用说明

### 第一步：准备文件

准备两个文件：
- **音频/视频文件**：支持 mp3、wav、mp4、mkv 等常见格式
- **文稿文件**：txt 格式，包含与音频对应的文字内容

### 第二步：导入文件

1. 点击「浏览」按钮选择音频/视频文件
2. 点击「浏览」按钮选择文稿文件
3. 文稿内容会显示在编辑框中，可以手动修改

### 第三步：开始对齐

点击「对齐」按钮，等待处理完成。

> 提示：首次运行或更换语言时会自动下载模型，请耐心等待。

### 第四步：导出字幕

对齐完成后，会自动显示结果窗口：

- **导出 SRT** - 生成可直接使用的字幕文件
- **导出 JSON** - 导出详细数据（供开发者使用）

---

## API 接口模式

本项目支持以 HTTP API 服务方式运行，方便其他程序调用。

### 启动 API 服务

```python
from gui.tools.api_server import start_api_server

# 启动 API 服务（默认端口 5000）
success, message = start_api_server(host='127.0.0.1', port=5000)
print(message)
```

或使用命令行启动：

```bash
python -c "from gui.tools.api_server import start_api_server; start_api_server()"
```

### 接口说明

#### 1. 音频对齐接口 `/align`

**请求方式**: POST

**请求参数**:
```json
{
    "audio_path": "音频/视频文件路径",
    "text": "要对齐的文本内容",
    "format": "返回格式：json(默认) 或 srt"
}
```

**请求示例**:
```bash
curl -X POST http://127.0.0.1:5000/align \
  -H "Content-Type: application/json" \
  -d '{
    "audio_path": "D:/audio.mp3",
    "text": "这是一段测试文本",
    "format": "json"
  }'
```

**返回示例 (JSON)**:
```json
{
    "success": true,
    "result": {
        "segments": [{
            "words": [
                {"word": "这", "start": 0.0, "end": 0.3, "duration": 0.3},
                {"word": "是", "start": 0.35, "end": 0.6, "duration": 0.25}
            ]
        }],
        "metadata": {
            "total_duration": 5.2,
            "word_count": 10,
            "export_time": "2025-02-03T12:00:00"
        }
    }
}
```

**返回示例 (SRT)**:
```json
{
    "success": true,
    "result": "1\n00:00:00,000 --> 00:00:00,300\n这\n\n2\n00:00:00,350 --> 00:00:00,600\n是"
}
```

### Python 调用示例

```python
import requests

# 对齐音频和文本
response = requests.post('http://127.0.0.1:5000/align', json={
    'audio_path': 'D:/audio.mp3',
    'text': '这是一段测试文本',
    'format': 'srt'
})

result = response.json()
if result['success']:
    print(result['result'])  # SRT 格式字幕
```

---

## 设置说明

点击界面左下角的设置按钮（⚙️）可以调整：

- **计算设备** - 选择使用显卡（GPU）或处理器（CPU）
- **字幕设置** - 调整每行最大长度、停顿间隔等
- **离线模式** - 开启后不会联网检查更新

---

## 运行截图
![1](https://github.com/user-attachments/assets/50e9fed1-4536-419b-9fb7-67a43fd2b7bc)![2](https://github.com/user-attachments/assets/2eb5fe52-ccd5-44de-92bf-a362149cfdac)![3](https://github.com/user-attachments/assets/226655da-3a54-4aa3-83b3-982ee2522257)




---

## 开源协议

本项目基于 CC-BY-NC 4.0 协议开源，仅供学习和个人使用。

---

## 致谢

本项目基于以下开源项目开发：
- [ctc-forced-aligner](https://github.com/MahmoudAshraf97/ctc-forced-aligner) - CTC 强制对齐算法
- [MMS](https://github.com/facebookresearch/fairseq/tree/main/examples/mms) - Facebook 多语言语音模型

