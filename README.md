# 🏭 Industrial Control Simulator (工业控制调试模拟器)

[![Python](https://img.shields.io/badge/Python-3.7%2B-blue.svg)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.3.0-green.svg)](https://flask.palletsprojects.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20MacOS-lightgrey.svg)]()

一个完整的工业自动化调试模拟器，包含运动控制、视觉采集和MES系统模拟，专为C#客户端联调设计。无需硬件设备即可进行工业自动化系统的开发和测试。

## ✨ 特性

- **🎯 三大系统模拟**
  - 运动控制模拟器（多轴控制、回零、点动）
  - 视觉采集模拟器（多相机图像采集、分析）
  - MES系统模拟器（工单管理、生产上报、质量检查）

- **🌐 多种接口协议**
  - RESTful API（HTTP/JSON）
  - WebSocket实时通信
  - 完整的Web监控界面

- **🔧 开发友好**
  - 无需硬件设备
  - 支持C#、Python等多种客户端
  - 详细的API文档
  - 完整的错误处理和日志记录

## 📦 快速开始

### 系统要求
- Python 3.7+
- Windows 10/11, Linux, 或 macOS
- 网络连接（仅本地回环）

### 运行模拟器
- python app.py
### 项目结构
IC_Simulator/
├── app.py # 主应用入口
├── simple_simulators.py # 模拟器核心逻辑
├── requirements.txt # Python依赖
├── templates/ # HTML模板
│ └── index.html
└── static/ # 静态文件
├── css/
└── js/



