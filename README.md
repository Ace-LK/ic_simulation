🏭 Industrial Automation Simulator (工业自动化调试模拟器)

https://img.shields.io/badge/Python-3.7%2B-blue.svg

https://img.shields.io/badge/Flask-2.3.0-green.svg

https://img.shields.io/badge/License-MIT-yellow.svg

https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20MacOS-lightgrey.svg

一个完整的工业自动化调试模拟器，包含运动控制、视觉采集和MES系统模拟，专为C#客户端联调设计。无需硬件设备即可进行工业自动化系统的开发和测试。

✨ 特性

🎯 三大系统模拟

运动控制模拟器（多轴控制、回零、点动）

视觉采集模拟器（多相机图像采集、分析）

MES系统模拟器（工单管理、生产上报、质量检查）

🌐 多种接口协议

RESTful API（HTTP/JSON）

WebSocket实时通信

完整的Web监控界面

🔧 开发友好

无需硬件设备

支持C#、Python等多种客户端

详细的API文档

完整的错误处理和日志记录

📦 快速开始
系统要求

Python 3.7+

Windows 10/11, Linux, 或 macOS

网络连接（仅本地回环）

安装步骤
1. 克隆或下载项目
bash
复制
git clone https://github.com/yourusername/industrial-automation-simulator.git
cd industrial-automation-simulator
2. 安装依赖
bash
复制
pip install -r requirements.txt

或使用国内镜像源：

bash
复制
pip install -i https://mirrors.aliyun.com/pypi/simple/ -r requirements.txt
3. 运行模拟器
bash
复制
# Windows
start.bat

# Linux/Mac
python app.py
4. 访问Web界面

打开浏览器访问：http://localhost:5000

📡 API 接口
运动控制 API
复制
GET    /api/motion/axes            # 获取所有轴状态
GET    /api/motion/axis/{id}       # 获取指定轴状态
POST   /api/motion/move            # 移动轴到指定位置
POST   /api/motion/home           # 轴回零
POST   /api/motion/stop           # 停止轴移动
视觉系统 API
复制
GET    /api/vision/cameras         # 获取相机列表
POST   /api/vision/acquire         # 采集图像
GET    /api/vision/latest/{id}     # 获取最新图像
POST   /api/vision/analyze         # 图像分析
MES 系统 API
复制
GET    /api/mes/workorders         # 获取所有工单
GET    /api/mes/workorder/current  # 获取当前工单
POST   /api/mes/workorder/start    # 开始工单
POST   /api/mes/production/report  # 上报生产数据
POST   /api/mes/quality/check      # 质量检查
系统控制 API
复制
GET    /api/status                 # 系统状态
GET    /api/health                 # 健康检查
POST   /api/control/start          # 启动模拟器
POST   /api/control/stop           # 停止模拟器
POST   /api/control/reset          # 重置模拟器
💻 C# 客户端示例
安装 NuGet 包
xml
复制
<PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
基础客户端类
csharp
复制
public class SimulatorClient
{
    private readonly HttpClient _client = new HttpClient();
    private readonly string _baseUrl = "http://localhost:5000";
    
    public async Task<List<AxisInfo>> GetAxesAsync()
    {
        var response = await _client.GetAsync($"{_baseUrl}/api/motion/axes");
        response.EnsureSuccessStatusCode();
        
        var json = await response.Content.ReadAsStringAsync();
        return JsonConvert.DeserializeObject<List<AxisInfo>>(json);
    }
    
    public async Task<bool> MoveAxisAsync(int axisId, double position)
    {
        var data = new { axis_id = axisId, position = position };
        var json = JsonConvert.SerializeObject(data);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        
        var response = await _client.PostAsync($"{_baseUrl}/api/motion/move", content);
        return response.IsSuccessStatusCode;
    }
}
使用示例
csharp
复制
class Program
{
    static async Task Main(string[] args)
    {
        var client = new SimulatorClient();
        
        Console.WriteLine("连接到模拟器...");
        
        // 获取轴状态
        var axes = await client.GetAxesAsync();
        foreach (var axis in axes)
        {
            Console.WriteLine($"轴{axis.AxisId}: {axis.Name} 位置={axis.CurrentPosition}");
        }
        
        // 移动X轴
        await client.MoveAxisAsync(0, 100.0);
        Console.WriteLine("X轴移动到位置100");
        
        // 上报生产
        await client.ReportProductionAsync("WO-20240001", 
            new { temperature = 25.5, humidity = 60 });
    }
}
🔌 WebSocket 实时通信
连接示例
javascript
下载
复制
// JavaScript
const socket = io('http://localhost:5000');

socket.on('connect', () => {
    console.log('已连接到模拟器');
});

socket.on('status_update', (data) => {
    console.log('状态更新:', data);
    
    // 更新运动控制状态
    if (data.motion) {
        updateMotionStatus(data.motion);
    }
    
    // 更新视觉系统状态
    if (data.vision) {
        updateVisionStatus(data.vision);
    }
});
事件类型

status_update- 系统状态更新（每秒2次）

motion_event- 运动控制事件

vision_event- 视觉采集事件

mes_event- MES系统事件

🎨 Web 监控界面

模拟器包含完整的Web监控界面，功能包括：

📊 实时仪表板​ - 三大系统状态实时监控

🎮 控制面板​ - 手动控制运动轴、采集图像

📈 数据可视化​ - 生产数据图表展示

📋 日志查看​ - 系统操作日志实时查看

📖 API 文档​ - 交互式API文档和测试工具

🔧 配置选项
修改配置文件

创建 config.yaml：

yaml
复制
server:
  host: 0.0.0.0
  port: 5000
  debug: false

simulator:
  motion:
    axes: 4
    limits: [-1000, 1000]
  vision:
    cameras: 2
    resolution: [640, 480]
  mes:
    auto_start: true
    yield_rate: 0.95
命令行参数
bash
复制
# 开发模式
python app.py --debug --port 8080

# 生产模式
python app.py --prod --host 0.0.0.0 --port 80

# 指定Python版本
py -3.10 app.py
🐳 Docker 部署
Docker Compose
yaml
复制
# docker-compose.yml
version: '3.8'

services:
  simulator:
    build: .
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=production
    volumes:
      - ./logs:/app/logs
    restart: unless-stopped
Dockerfile
dockerfile
复制
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000
CMD ["python", "app.py", "--prod"]
🔍 故障排除
常见问题
1. 端口被占用
bash
复制
# 查找占用端口的进程
netstat -ano | findstr :5000

# 终止进程
taskkill /PID [PID] /F
2. Python 版本问题
bash
复制
# 检查Python版本
python --version

# 使用py启动器指定版本
py -3.10 app.py
3. 依赖安装失败
bash
复制
# 使用国内镜像源
pip install -i https://mirrors.aliyun.com/pypi/simple/ -r requirements.txt

# 或使用代理
set http_proxy=http://your-proxy:port
set https_proxy=http://your-proxy:port
4. WebSocket 连接失败
javascript
下载
复制
// 修改连接配置
const socket = io('http://localhost:5000', {
  transports: ['websocket', 'polling'],
  reconnection: true,
  reconnectionAttempts: 5
});
📁 项目结构
复制
IndustrialSimulator/
├── app.py                    # 主应用入口
├── requirements.txt          # Python依赖
├── start.bat                # Windows启动脚本
├── start.sh                 # Linux/Mac启动脚本
├── config.yaml              # 配置文件
├── simple_simulators.py     # 模拟器核心逻辑
├── static/                  # 静态文件
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── main.js
├── templates/               # HTML模板
│   ├── index.html
│   └── api_docs.html
├── logs/                    # 日志目录
│   └── simulator.log
├── examples/                # 示例代码
│   ├── csharp/             # C#客户端示例
│   │   ├── SimulatorClient.cs
│   │   └── Program.cs
│   └── python/             # Python客户端示例
│       └── client.py
└── docs/                   # 文档
    ├── api.md             # API文档
    └── deployment.md      # 部署文档
🧪 测试
运行单元测试
bash
复制
python -m pytest tests/
API 测试
bash
复制
# 使用curl测试API
curl http://localhost:5000/api/status
curl -X POST http://localhost:5000/api/motion/move -H "Content-Type: application/json" -d '{"axis_id":0,"position":100}'
自动化测试脚本
python
下载
复制
# test_simulator.py
import requests
import json

BASE_URL = "http://localhost:5000"

def test_api():
    # 测试系统状态
    response = requests.get(f"{BASE_URL}/api/status")
    print(f"Status: {response.json()}")
    
    # 测试运动控制
    data = {"axis_id": 0, "position": 100}
    response = requests.post(f"{BASE_URL}/api/motion/move", json=data)
    print(f"Move axis: {response.json()}")
📄 许可证

本项目基于 MIT 许可证开源 - 查看 LICENSE
文件了解详情。

🤝 贡献

欢迎提交 Issue 和 Pull Request！

Fork 项目

创建功能分支 (git checkout -b feature/AmazingFeature)

提交更改 (git commit -m 'Add some AmazingFeature')

推送到分支 (git push origin feature/AmazingFeature)

开启 Pull Request

📞 支持

📧 邮箱：your-email@example.com

💬 问题：提交 GitHub Issue

📖 文档：在线文档

🌟 致谢

感谢以下开源项目：

Flask
- Web框架

Flask-SocketIO
- WebSocket支持

Pillow
- 图像处理

Socket.IO
- 实时通信库

🏭 Industrial Automation Simulator​ - 让工业自动化开发更简单！
