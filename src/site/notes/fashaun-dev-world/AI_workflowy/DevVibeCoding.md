---
{"dg-publish":true,"permalink":"/fashaun-dev-world/ai-workflowy/dev-vibe-coding/","noteIcon":""}
---

# AI Knowledge
- MCP (https://modelcontextprotocol.io/introduction)
	- Model Compression and Pruning
	- Model Context Protocol

模型理解用户意图后，调用外部工具或API的能力。
比如：问今天多少度，模型就会调用相应插件，返回其结果。
Function call 它类似于品牌专属，服务于特定模型的生态系统，灵活性高，但兼容性受限。  

**而 MCP 的优点的话是整合之前各家不同大模型不同的 function call 的标准，整合成了一个统一的标准协议。**

![](/img/user/fashaun-dev-world/attachment/MCP.png)

情境：
- AI 需要 MCP 避免資料過時
- 不同的模型對接不同的工具 function 無法重複被利用


- RAG 
	- RAG 的核心思想是**在生成回答之前，先從外部知識庫中「檢索」相關訊息**，然後將這些資訊與使用者的問題一起發送給大語言模型(LLM)，以便產生更準確的答案。
	- ![](/img/user/publications/journals-workflowy/swd/content/kb/Pasted image 20250610092330.png)
可以搜尋 文檔/資料庫
- OCR
- LlamaIndex

# Chatbot 
- 無法自主思考
- 沒有決策能力

# AI Assistant  (Generative AI)
> 無法自動完成任務，一問一答，通常指沒有特別外掛模組 (ex: 記憶模組)
- Claude
- ChatGPT
- Gemini 
- Ollama
- Grok
- LlamaIndex

# AI Agent
> 依人類需要介入的程度決定
- n8n
	- n8n 算什麼???
	- 使用情境
- Cursor
- DeepWiki(https://deepwiki.com/)
	- Cognition AI Lab (https://cognition.ai/)
- notebookLM
- Google AI Studio (超爛)
- Google Agentspace (需申請)

| **項目**        | **AI Agent**              | **Gen AI**        |
| ------------- | ------------------------- | ----------------- |
| **核心功能**      | 任務執行、系統整合、  <br>目標導向行動    | 內容生成  <br>（文字、圖片） |
| **工作模式**      | 理解目標 ➔ 拆解任務 ➔ 呼叫工具 ➔ 動態修正 | 依據提示輸出單次結果        |
| **決策能力**      | 具備自主決策能力                  | 無法自主決策            |
| **是否可以自主行動？** | 可以  <br>可在有限監督下自主運作       | 不行  <br>需要人類逐步指示  |

![](/img/user/fashaun-dev-world/attachment/GoogleAgents.png)

# Multi-modal
- 提供給 AI 多種形式的輸入
	- 圖片
	- 文字
	- 語音 (~= 文字?)
# LLM
> Generative AI 背後的 Model

- LLM Ranking
	- https://llm-stats.com/
# AI Task Manager 
- https://cjo4m06.github.io/mcp-shrimp-task-manager/
	- 提供 Cursor MCP 配置
- Task Master 
	- https://www.youtube.com/watch?v=1L509JK8p1I
	- https://github.com/eyaltoledano/claude-task-master


# MCP in Cursor

## MCP Type
- Local 
- Rmote

## 以 playwright 爲例

https://github.com/microsoft/playwright-mcp

貼上 json 在 .cursor/mcp.json 然後在 Cursor 中 enable

![](/img/user/fashaun-dev-world/attachment/MCPinCursor.png)


**Simple MCP**
![](/img/user/fashaun-dev-world/attachment/MCPinCursor-1.png)

**Github MCP**
![](/img/user/fashaun-dev-world/attachment/GithubMCP.png)

## 開發自己的 MCP Server
- https://github.com/modelcontextprotocol/servers

# VibeCoding Flow

- AI Agent 
	- Cursor
- AI Assistent
	- Claude
- n8n

1. Research Topic 
2. RAG Analysis by n8n flow and AI Assistent 
		- Oreilly ASK
3. Implementation by cursor and obsidian
4. Write Testing by cursor
5. Run Testing by n8n  


# Course
- https://github.com/microsoft/generative-ai-for-beginners
# Ref
- https://www.ifb.me/zh/blog/zh/ai/rag-kuai-su-ru-men-3