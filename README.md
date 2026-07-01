# TranslatorMinecraftApp

基于 **Qt6 (C++)** 的 [TranslatorMinecraft](https://github.com/lingxingmiao/Translator-Minecraft) 桌面客户端，通过 QProcess 管理 Python 翻译后端，提供完整的任务队列、配置管理和实时日志监控。

**代码全部由AI完成（DeepSeek V4 Pro）**

## 架构

```
┌───────────────────────────────────────┐
│  Qt6 C++ 前端 (TranslatorMinecraftApp) │
│  · 无边框半透明窗口                     │
│  · Iris Mod 双层边框风格                │
│  · 任务列表 + 实时日志                  │
│  · 云端/本地设置管理                    │
│  · LLM 端点管理                        │
└──────────┬────────────────────────────┘
           │ QProcess + HTTP
┌──────────▼──────────────────────┐
│  Python 后端 (TranslatorAPI.py) │
│  · FastAPI + uvicorn            │
│  · 翻译/分离/合并任务            │
│  · SQLite 持久化                │
└─────────────────────────────────┘
```

## 功能

- **三种任务类型**：翻译文件、分离语言更新、合并语言更新
- **R1.3 工作队列**：任务持久化（tasks.json），支持重命名/删除
- **实时日志**：内容去重、自动滚动、行数上限可配置
- **双层设置系统**：云端配置（config.json）+ 本地设置（API URL/KEY/路径）
- **LLM 端点管理**：多 LLM 端点配置（LLM0/LLM1/...），含 `_build_tier` 全部字段
- **Minecraft 1.16+ 语言文件**：UI 文本 + 提示均从 `gui_zh_CN.json` 加载

## 构建

### 依赖
- **Qt 6.8+** (Core, Gui, Widgets, Network)
- **CMake 3.16+**
- **MSVC 2022+** (Windows) 或 GCC/Clang (Linux)
- **Python 3.12+** (运行时，用于后端)

### 编译
```bash
cmake -S qt-gui -B qt-gui/build \
  -G "Visual Studio 18 2026" -A x64 \
  -DCMAKE_PREFIX_PATH=C:/Qt/6.8.2/msvc2022_64

cmake --build qt-gui/build --config Release
```

### 部署
```bash
windeployqt qt-gui/build/Release/TranslatorMinecraft.exe --release
```

## 运行

确保 Python 在 PATH 中，且 `TranslatorAPI.py` 在同一目录下。

```bash
cd qt-gui/build/Release
TranslatorMinecraft.exe
```

首次启动会自动查找并启动 Python 后端（默认端口 25561）。

## 项目结构

```
qt-gui/
├── CMakeLists.txt          # Qt6 CMake 构建配置
├── gui_zh_CN.json          # 中文语言文件 (Minecraft 1.16+ 格式)
└── src/
    ├── main.cpp            # 入口：无边框窗口 + 字体加载
    ├── mainwindow.h/cpp    # 主窗口：任务列表、日志、设置
    └── pythonbackend.h/cpp # Python 后端：QProcess + HTTP 轮询
```

## 许可证

[GNU AGPL-3.0](LICENSE) — Copyright (C) 2026 lingxingmiao
