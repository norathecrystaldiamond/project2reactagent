# project2reactagent
# 🧠 手写 ReAct 循环与持久化记忆 Agent (Pure Python ReAct Agent)

## 📖 项目简介

本项目是 AI Agent 开发的最底层、最原汁原味的实战演练。
在未使用 LangChain 或 LangGraph 等高级框架的情况下，本项目使用 **纯 Python 代码**，硬编码实现了 AI Agent 的 **ReAct (Reasoning and Acting)** 循环机制，并接入了 **Redis 持久化记忆** 以及 **真实的网页抓取工具**。

**核心价值**：彻底剥开 AI Agent 的“黑盒”，展示“大模型思考 -> 正则解析 -> 代码执行工具 -> 结果喂回大模型”的完整底层逻辑。

## ✨ 核心亮点

1. **手写 ReAct 状态机**：通过 `while` 循环和正则表达式（`re.search`），精准提取大模型输出的 `Thought`、`Action` 和 `Action Input`，并强制执行本地工具。
2. **真实工具调用 (Tool Calling)**：手写实现了本地文件读取工具（`read_local_file`）和真实的网页文本抓取工具（基于 `requests` 与 `BeautifulSoup`），并加入了防封禁的 `User-Agent` 伪装。
3. **Redis 持久化外挂大脑**：摒弃了容易丢失的本地内存变量，通过将对话历史 `messages` 序列化为 JSON 存入 Redis，实现了跨会话的长期记忆能力。
4. **自我纠错与死循环防护**：内置 `max_steps` 防止 Token 无限燃烧；当大模型不按格式输出时，触发 `else` 分支，强制将错误提示喂回给大模型要求重试（自我反思）。

## 🛠️ 环境依赖

运行本项目需要本地安装并启动 Redis 服务，并安装以下 Python 依赖：

```bash
pip install openai redis requests beautifulsoup4
💻 快速运行指令
1. 前置准备
确保你电脑本地的 Redis 服务已经启动（双击运行 redis-server.exe，看到红方块即代表成功）。

2. 启动 Agent
在项目目录下打开终端，执行以下指令：

bash
python my_agent_project2.py
3. 交互体验
程序启动后，终端会进入类似微信聊天的交互模式。你可以尝试输入：

第一次输入：我叫小明，我最喜欢的数字是 7。

紧接着输入：你还记得我叫什么名字吗？我最喜欢的数字是几？

(此时你会看到控制台打印 🧠 [Redis] 发现历史记忆，正在加载...，证明长期记忆生效)
