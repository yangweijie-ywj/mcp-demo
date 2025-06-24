# MCP (Model Context Protocol) 示例项目

这个项目展示了如何使用 MCP (Model Context Protocol) 来创建和使用各种智能助手和工具。项目包含了多个示例，展示了不同场景下的 MCP 应用。

## 项目结构

```
day11_MCP_A2A/
├── CASE-A2A/               # Agent to Agent 示例案例
├── MCP-demo01/             # MCP 基础示例
└── MCP-demo02/             # MCP 进阶示例
```

## 主要功能模块

### 1. CASE-A2A

Agent to Agent (A2A) 通信示例，展示了不同 Agent 之间如何通过标准协议进行协作。

#### 组件说明

- `BasketBallAgent.py`: 篮球活动安排代理
  - 通过 A2A 协议与 WeatherAgent 通信
  - 根据天气情况智能决策是否安排篮球活动
  - 支持自定义日期的活动安排
  - 包含完整的错误处理机制

- `WeatherAgent.py`: 天气服务代理
  - 基于 FastAPI 实现的 RESTful 服务
  - 提供标准的 A2A 协议接口
  - 支持 Agent Card 声明（/.well-known/agent.json）
  - 模拟天气数据存储和查询功能

#### A2A 协议实现

1. Agent Card 规范
```json
{
    "name": "WeatherAgent",
    "version": "1.0",
    "description": "提供指定日期的天气数据查询",
    "endpoints": {
        "task_submit": "/api/tasks/weather",
        "sse_subscribe": "/api/tasks/updates"
    },
    "input_schema": {
        "type": "object",
        "properties": {
            "date": {"type": "string", "format": "date"},
            "location": {"type": "string", "enum": ["北京"]}
        },
        "required": ["date"]
    }
}
```

2. 通信流程
   - BasketBallAgent 获取 WeatherAgent 的能力描述
   - 构造符合规范的任务请求
   - 发送请求并处理响应
   - 根据天气数据进行决策

#### 运行说明

1. 启动 WeatherAgent 服务：
```bash
cd CASE-A2A
python WeatherAgent.py
```

2. 运行 BasketBallAgent 示例：
```bash
python BasketBallAgent.py
```

#### 依赖要求

```bash
pip install fastapi uvicorn requests pydantic
```

#### 示例输出

```python
# 晴天示例
篮球安排结果: {'status': 'confirmed', 'weather': {'temperature': '22℃', 'condition': '多云转晴'}}

# 雨天示例
篮球安排结果: {'status': 'cancelled', 'reason': '恶劣天气'}
```

### 2. MCP-demo01

基础 MCP 功能示例，展示了如何使用 MCP 创建各种工具和服务。

- `assistant_mcp_amap_bot.py`: 集成了高德地图 API 的智能助手
  - 支持地理编码和逆地理编码
  - 提供路线规划功能
  - 支持周边搜索和天气查询
  
- `assistant_mcp_txt_bot.py`: 文本处理智能助手
  - 文本统计和分析
  - 文档解析功能
  
- `txt_counter.py`: 桌面 TXT 文件管理工具
  - 统计桌面上 .txt 文件的数量
  - 列出所有桌面 .txt 文件
  - 读取指定 txt 文件的内容
  - 支持 UTF-8 编码的文本文件
  
- `dalian_tour.html`: 大连旅游行程规划网页
  - 完整的一日游行程安排
  - 打印友好的 A4 格式
  - 包含交通、餐饮等详细信息

### 3. MCP-demo02

进阶 MCP 功能示例，展示了多种 MCP 服务器的组合应用场景。

- `assistant_bot.py`: 多功能智能助手
  - 集成了三种 MCP 服务器：
    1. 高德地图服务 (amap-maps)：提供地理位置和路线规划
    2. 网页内容获取服务 (fetch)：支持网页内容提取和转换
    3. 必应搜索服务 (bing-cn-mcp-server)：提供搜索功能
  - 支持三种交互模式：
    - GUI 模式：Web 界面，带预设查询建议
    - TUI 模式：终端交互，支持连续对话
    - 测试模式：单次查询测试
  - 高级功能：
    - 网页内容提取和转换（HTML 转 Markdown）
    - 智能搜索和信息聚合
    - 多模态输入支持
    - 上下文感知的对话管理

运行示例：
```bash
cd MCP-demo02

# 配置必要的环境变量
export DASHSCOPE_API_KEY='your-api-key'

# 运行（默认为 GUI 模式）
python assistant_bot.py
```

示例查询：
1. 网页内容提取：
   ```
   将 https://example.com 网页转化为 Markdown 格式
   ```

2. 地点查询：
   ```
   帮我找一下西湖附近的咖啡厅
   ```

3. 综合搜索：
   ```
   搜索最近一周的 AI 技术新闻
   ```

注意：使用前请确保已安装所有必要的 MCP 服务器：
```bash
npm install -g @amap/amap-maps-mcp-server
npm install -g @bing/bing-cn-mcp-server
npm install -g @fetch/fetch-mcp-server
```
  
## 工具集成

项目集成了多个实用工具：

- `doc_parser`: 文档解析工具
- `simple_doc_parser`: 简单文档解析器

## 环境要求

- Python 3.7+
- Node.js 和 npm（用于 MCP 服务器）
- 相关 Python 包：
  - dashscope==1.22.1
  - mcp==1.7.1
  - qwen_agent==0.0.19

## 配置说明

### 1. API Keys 配置

1. DashScope API Key（必需）:
   - 获取地址：https://dashscope.console.aliyun.com/
   - 设置方法：
     ```bash
     export DASHSCOPE_API_KEY='your-api-key'
     ```
   - 或在代码中直接设置（不推荐）

2. 高德地图 API Key（使用地图功能时必需）:
   - 获取地址：https://console.amap.com/dev/key/app
   - 在 assistant_mcp_amap_bot.py 中配置

### 2. 安装依赖

```bash
# 安装 Python 依赖
pip install -r MCP-demo01/requirements.txt

# 安装 Node.js 依赖（如果使用地图功能）
npm install -g @amap/amap-maps-mcp-server
```

## 运行说明

### 1. 高德地图助手 (assistant_mcp_amap_bot.py)

#### 环境准备
```bash
# 1. 安装必要的依赖
pip install dashscope qwen-agent requests

# 2. 配置环境变量
export DASHSCOPE_API_KEY='your-api-key'
export AMAP_API_KEY='your-amap-key'

# 3. 安装高德地图 MCP 服务器
npm install -g @amap/amap-maps-mcp-server
```

#### 运行模式

1. GUI 模式（默认）- Web 界面
```bash
python MCP-demo01/assistant_mcp_amap_bot.py
# 访问 http://localhost:8080 开始使用
```

2. TUI 模式 - 终端交互
```python
# 修改文件末尾为：
if __name__ == '__main__':
    app_tui()
```

3. 测试模式 - 单次查询
```python
# 修改文件末尾为：
if __name__ == '__main__':
    test()
```

#### 示例查询

1. 地点查询：
```
北京故宫的具体地址在哪里？
```

2. 路线规划：
```
从北京南站到天安门怎么走最快？
```

3. 周边搜索：
```
帮我找一下西湖附近评分高的餐厅
```

4. 旅游规划：
```
帮我规划一下杭州两日游行程，主要想去西湖和灵隐寺
```

### 2. 文本处理助手 (assistant_mcp_txt_bot.py)

#### 环境准备
```bash
# 1. 安装依赖
pip install dashscope qwen-agent

# 2. 启动本地文本计数服务
cd MCP-demo01
python txt_counter.py
```

#### 运行模式

1. GUI 模式（默认）
```bash
python MCP-demo01/assistant_mcp_txt_bot.py
# 访问 http://localhost:8080 开始使用
```

2. TUI 模式
```python
# 修改文件末尾为：
if __name__ == '__main__':
    app_tui()
```

3. 测试模式
```python
# 修改文件末尾为：
if __name__ == '__main__':
    test()
```

#### 示例查询

1. 基础文本统计：
```
统计这段文本的字数和行数：
Hello World!
你好，世界！
こんにちは、世界！
```

2. 代码分析：
```
统计这段 Python 代码的有效行数（不包括空行和注释）：
def hello():
    # 这是一个注释
    print("Hello World!")
    
    return True
```

3. 文本格式化：
```
帮我格式化这段 JSON：
{"name":"John","age":30,"city":"New York"}
```

4. 多语言分析：
```
分析这段多语言文本中每种语言的字符数量：
你好世界 Hello World こんにちは
```

## 注意事项

1. 使用高德地图功能前需要配置有效的 API 密钥
2. 部分功能可能需要网络连接
3. 确保所有依赖都正确安装

## 贡献指南

欢迎提交问题和改进建议！请确保在提交前：

1. 检查代码风格符合项目规范
2. 添加适当的测试用例
3. 更新相关文档

## 许可证

本项目采用 MIT 许可证。详见 LICENSE 文件。
