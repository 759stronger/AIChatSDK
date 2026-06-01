# AIChatSDK

一个支持多模型的智能对话系统，包含 C++ 后端 SDK、HTTP 服务器和 Web 前端界面。

## 功能特性

- 🤖 **多模型支持**：DeepSeek、ChatGPT、Gemini、Ollama 本地模型
- 🌊 **流式输出**：基于 SSE (Server-Sent Events) 的实时流式响应
- 💬 **会话管理**：创建、切换、删除会话，对话历史持久化
- 🎨 **Web 前端**：类 ChatGPT 风格的玻璃拟态 UI，支持深色主题
- 🔒 **安全稳定**：异常捕获保护，单模型故障不影响服务器运行

## 项目结构

```
AIChatSDK/
├── chatsdk/                    # 核心 SDK 库
│   ├── include/                # 头文件
│   │   ├── chat_sdk.h          # SDK 主接口
│   │   ├── LLMManager.h        # 模型管理器
│   │   ├── ILLMProvider.h      # 模型提供者接口
│   │   ├── GeminiProvider.h    # Gemini 提供者
│   │   ├── DeepSeekProvider.h  # DeepSeek 提供者
│   │   ├── ChatGPTProvider.h   # ChatGPT 提供者
│   │   ├── OllamaDeepSeekProvider.h  # Ollama 本地模型
│   │   ├── session_manager.h   # 会话管理
│   │   ├── dataManager.h       # 数据持久化 (SQLite)
│   │   └── common.h            # 通用数据结构
│   ├── src/                    # 源代码实现
│   └── CMakeLists.txt
├── chatServer/                 # HTTP 服务器
│   ├── main.cpp                # 入口 + 参数解析
│   ├── chatServer.cpp          # HTTP 路由处理
│   ├── chatServer.h            # 服务器配置
│   ├── CMakeLists.txt
│   └── www/                    # Web 前端
│       ├── index.html          # 主页面
│       ├── css/style.css       # 样式
│       └── js/                 # JS 脚本
│           ├── main.js         # 事件绑定
│           ├── chat.js         # 聊天逻辑 (SSE 流式)
│           ├── session.js      # 会话管理 API
│           ├── api.js          # 通用 API 封装
│           └── ui.js           # Toast/Modal 组件
└── test/                       # 测试代码
    └── testLLM.cpp
```

## 依赖项

| 依赖 | 用途 |
|------|------|
| C++17 编译器 (GCC 8+ / Clang 7+) | 编译 |
| CMake 3.10+ | 构建系统 |
| OpenSSL | HTTPS 请求 |
| jsoncpp | JSON 解析 |
| fmt | 格式化输出 |
| spdlog | 日志系统 |
| SQLite3 | 数据持久化 |
| gflags | 命令行参数 |
| httplib (header-only) | HTTP 客户端/服务端 |

### Ubuntu/Debian 安装

```bash
sudo apt install build-essential cmake libjsoncpp-dev libfmt-dev \
                 libspdlog-dev libsqlite3-dev libgflags-dev libssl-dev
```

## 编译 & 运行

### 1. 编译 SDK

```bash
cd chatsdk
mkdir -p build && cd build
cmake ..
make -j$(nproc)
sudo make install
```

### 2. 编译服务器

```bash
cd chatServer
mkdir -p build && cd build
cmake ..
make -j$(nproc)
```

### 3. 启动

```bash
cd chatServer
./build/AIChatServer --port=8080
```

## 命令行参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `--host` | `0.0.0.0` | 服务器监听地址 |
| `--port` | `8080` | 服务器端口 |
| `--log_level` | `DEBUG` | 日志级别 (DEBUG/INFO/WARN/ERROR) |
| `--temperature` | `0.7` | 模型采样温度 |
| `--max_tokens` | `2048` | 最大 token 数 |
| `--ollama_endpoint` | `http://127.0.0.1:11434` | Ollama 服务地址 |
| `--ollama_model_name` | `deepseek-r1:1.5b` | Ollama 模型名 |
| `--config_file` | `chatServer.conf` | 配置文件路径 |

也可以通过配置文件 `chatServer.conf` 设置：

```
--host=0.0.0.0
--port=8080
--ollama_endpoint=http://127.0.0.1:11434
--ollama_model_name=deepseek-r1:1.5b
```

## API 接口

### 获取模型列表
```
GET /api/models
```

### 创建会话
```
POST /api/sessions
Content-Type: application/json
{"model": "gemini-3.5-flash"}
```

### 获取会话列表
```
GET /api/sessions
```

### 删除会话
```
DELETE /api/sessions/{session_id}
```

### 发送消息 (流式 SSE)
```
POST /api/message/async
Content-Type: application/json
{"session_id": "...", "message": "你好"}
```

### 获取会话消息历史
```
GET /api/messages/{session_id}
```

## 部署到服务器

1. 将整个项目拷贝到服务器
2. 安装依赖并编译（步骤同上）
3. 启动服务器：`./build/AIChatServer --host=0.0.0.0 --port=8080`
4. 对外开放 8080 端口
5. 访问 `http://服务器IP:8080`

## 注意事项

- 使用 Gemini 模型时，如果服务器在国内需要配置 HTTP 代理（修改 `GeminiProvider.cpp` 中的 `set_proxy` 调用）
- DeepSeek 和 ChatGPT 需要有效的 API Key，通过配置文件设置
- Ollama 需要本地运行 Ollama 服务，默认地址 `127.0.0.1:11434`

## License

MIT
