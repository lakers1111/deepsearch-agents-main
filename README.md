# DeepSearch Agents

DeepSearch Agents 是一个基于 DeepAgents、FastAPI、WebSocket 和 React 的对话式多智能体研究系统。项目面向“深度资料检索与报告生成”场景，后端负责调度主智能体与多个工具/子智能体，前端负责提交任务、上传文件、展示执行过程和查看生成结果。

## 项目功能

- 对话式任务提交：用户输入研究任务后，后端异步执行。
- 多来源检索：支持网络搜索、MySQL 查询、RAGFlow 知识库查询和上传文件读取。
- 实时进度推送：通过 WebSocket 向前端展示智能体执行事件。
- 文件交付：可生成 Markdown、PDF 等结果文件，并在前端下载。
- 前后端分离：后端使用 FastAPI，前端使用 React + Vite。

## 目录结构

```text
.
├── app/                 # FastAPI 后端与 DeepAgents 智能体逻辑
│   ├── agent/           # 主智能体、子智能体、模型初始化
│   ├── api/             # HTTP / WebSocket 接口
│   ├── tools/           # 搜索、数据库、文件、RAGFlow 等工具
│   └── utils/           # 通用工具函数
├── docker/              # MySQL 本地开发环境
├── docs/                # 示例图片和知识库文档
├── examples/            # DeepAgents 学习示例
├── frontend/            # React 前端
├── .env.example         # 后端环境变量示例
├── pyproject.toml       # Python 依赖配置
└── requirements.txt     # Python 依赖列表
```

## 环境要求

- Python 3.12
- Node.js 20+
- pnpm 10+
- Docker（可选，用于启动本地 MySQL）

## 配置环境变量

复制后端环境变量示例：

```bash
cp .env.example .env
```

按实际情况修改 `.env`，至少需要配置：

```env
OPENAI_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
OPENAI_API_KEY=your-api-key
LLM_QWEN_MAX=qwen-max
TAVILY_API_KEY=your-tavily-api-key
```

如果使用 MySQL 或 RAGFlow，再补充对应配置：

```env
MYSQL_HOST=localhost
MYSQL_PORT=3307
MYSQL_USER=root
MYSQL_PASSWORD=root
MYSQL_DATABASE=deepsearch_db

RAGFLOW_API_URL=http://your-ragflow-host
RAGFLOW_API_KEY=your-ragflow-api-key
```

前端环境变量位于 `frontend/.env.example`，可按需复制：

```bash
cd frontend
cp .env.example .env
```

默认后端地址：

```env
VITE_API_BASE_URL=http://localhost:8000
VITE_WS_BASE_URL=ws://localhost:8000
```

## 启动后端

安装 Python 依赖：

```bash
pip install -r requirements.txt
```

启动 FastAPI 服务：

```bash
python app/api/server.py
```

后端默认运行在：

```text
http://localhost:8000
```

也可以使用 uvicorn 启动：

```bash
uvicorn app.api.server:app --host 0.0.0.0 --port 8000 --reload
```

## 启动前端

进入前端目录并安装依赖：

```bash
cd frontend
pnpm install
```

启动开发服务：

```bash
pnpm dev
```

前端默认运行在：

```text
http://localhost:5173
```

## 启动本地 MySQL（可选）

如果需要使用数据库查询工具，可以启动项目内置的 MySQL 示例环境：

```bash
cd docker
docker compose up -d
```

默认连接信息与 `.env.example` 保持一致：

```env
MYSQL_HOST=localhost
MYSQL_PORT=3307
MYSQL_USER=root
MYSQL_PASSWORD=root
MYSQL_DATABASE=deepsearch_db
```

## 基本使用流程

1. 启动后端服务。
2. 启动前端页面。
3. 在前端输入研究任务。
4. 如有需要，上传 PDF、Word、Excel、Markdown 或文本文件。
5. 等待 WebSocket 实时返回执行过程。
6. 在结果区查看回答或下载生成文件。

## 注意事项

- 不要提交真实 `.env` 文件或 API Key。
- `app/output/` 和 `app/updated/` 是运行时目录，已在 `.gitignore` 中忽略。
- 使用网络搜索、RAGFlow 或大模型服务前，请确认对应 API Key 已配置。
