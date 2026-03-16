# Vanna ChatBI - 企业级自然语言 SQL 生成系统

本项目是一个基于 [Vanna.ai](https://vanna.ai/) 框架构建的智能数据库查询助手。它利用 **Google Gemini 2.5 Flash** 模型的强大推理能力，将自然语言问题转化为精准的 SQL 查询，实现“对话即报表”。

## 🚀 核心特性

- **双引擎驱动**：采用最新的 `Gemini 2.5 Flash` 模型，在保证生成速度的同时，具备理解复杂业务逻辑的高上下文能力。
- **高性能后端**：基于 `FastAPI` 构建异步服务，完美支持大规模并发请求与长连接。
- **企业级安全**：
  - **身份校验**：集成 `UserResolver` 机制，通过 Cookie (vanna_email) 识别用户身份。
  - **权限控制**：内置 `ToolRegistry` 权限矩阵，严格划分管理员（Admin）与普通用户（User）的操作权限。
  - **密钥管理**：遵循安全开发规范，使用 Google Colab Secrets 管理 API Key，杜绝代码泄露。
- **数据深度理解**：专门针对海量数据场景优化（如 4200 万行级的 `aatn_asset_cost_detailed` 表），支持 DDL 注入与 SQL 记忆微调。

------

## 🏗️ 系统架构

1. **用户层**：通过 Web 界面或 API 提交自然语言问题。
2. **逻辑层**：`FastAPI` 接收请求并调用 `Gemini 2.5 Flash` 进行语义解析。
3. **工具层**：
   - `RunSqlTool`: 执行生成的 SQL 并返回数据集。
   - `VisualizeDataTool`: 将查询结果自动转化为可视化图表。
4. **持久层**：通过 `DemoAgentMemory` 记录历史正确查询，加速同类问题的二次响应。

------

## 🛠️ 快速开始

### 1. 环境准备

确保在 Google Colab 或本地 Python 3.10+ 环境中安装必要依赖：

Bash

```
pip install 'vanna[gemini,flask]' --upgrade
```

### 2. 配置密钥

在 Colab 左侧的 **Secrets (钥匙图标)** 中添加以下变量：

- `GEMINI_KEY`: 你的 Google AI Studio API Key。

### 3. 数据绑定与运行

打开 `Vanna_quickstart.ipynb`，顺序执行单元格：

- **Step 1**: 挂载 Google Drive 并初始化数据库（支持 SQLite/MySQL 等）。
- **Step 2**: 运行 `server.run()`。
- **Step 3**: 点击控制台输出的公网隧道链接（`colab.googleusercontent.com`）进入 UI。

------

## 🔐 权限定义 (Access Control)

| **角色**  | **可执行操作**                                        |
| --------- | ----------------------------------------------------- |
| **Admin** | SQL 执行、数据可视化、**保存/修改训练数据**、全局配置 |
| **User**  | SQL 执行、数据可视化、基于现有训练数据的查询          |

------

## 📈 后续规划

- [ ] 对接公司内部 MySQL 生产数据库。
- [ ] 将 FastAPI 服务通过 Docker 容器化部署至生产环境。
- [ ] 针对 `aatn_asset_cost_detailed` 表进行特定的 Prompt Engineering 优化。

------

## 📄 许可证

本项目基于 MIT License 开源。

------

### 💡 建议

你可以将这段内容直接保存为 `README.md` 放在你的 GitHub 仓库根目录。如果你之后将数据库从 SQLite 切换到了公司正式的 **MySQL**，记得在“数据绑定”部分更新对应的驱动说明。
