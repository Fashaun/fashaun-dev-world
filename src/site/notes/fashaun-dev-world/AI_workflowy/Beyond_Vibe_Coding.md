---
{"dg-publish":true,"permalink":"/fashaun-dev-world/ai-workflowy/beyond-vibe-coding/","noteIcon":""}
---




# Explain This
- Global Rule
	- https://www.notion.so/explainthisio/2-1-cursor-rules-1c22d1d1de188025a8befa53a65123c7
- Project Rule
	- https://github.com/PatrickJS/awesome-cursorrules
	- https://cursor.directory/

- Reasoning model
	- 格式
		- **Claude 3.7 Sonnet** (非推理模式)
		- **Claude 3.7 Sonnet Thinking** (推理模式)
	- 場景
		- **複雜的多步驟任務**：難解的 bug 或需要多階段規劃的功能實作。
		- **問題定義不清楚時**：推理模型會在回答前進行思考、規劃，甚至可能反問你以獲取更多資訊，逐步理清問題。

- Prompt
	- 可以給 sample code
		- https://supabase.com/docs/guides/getting-started/ai-prompts/database-functions
	- 多加入例子
	- 多加入思考步驟
	- 架構測試 
		- 可以自己設計
	- OpenAI 給的架構
		- **目標（Goal）**：清楚說明請AI完成什麼任務。
		- **回覆格式（Return Format）**：明確定義期望的輸出格式。
		- **注意事項（Warnings）**：提醒 AI 在處理任務時需特別注意的地方，寫出絕對不希望看到的避免 AI 去思考
		- **脈絡（Context）**：提供相關背景資訊（後續單元會詳細解說）。
- Context
	- Settings Docs
		- 可以直接給連結讓 Cursor 爬
	- @Web
		- 網頁搜尋
	- 如果需要更多資訊可以讓我知道
	- 決策脈絡 (避免 AI 失憶)
		- 請 AI 寫文件自己記錄 決策脈絡 / 開發筆記 
		- https://addyosmani.com/blog/automated-decision-logs/
	- 單一脈絡原則
		- 脈絡和改動記錄過多時，模型的輸出品質就會降低
		- Summarize what you did and output in Markdown format that I can copy directly.
		- Cursor 快捷鍵 @Summarize Composer


# Ref
- https://learning.oreilly.com/library/view/beyond-vibe-coding/9798341634749/