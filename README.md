# AI-Stock-Technical-Analysis-n8n-x-FlowiseAI

A Telegram AI Agent assistant is used to trigger n8n workflows, which work in conjunction with FlowiseAI to produce professional-grade technical analysis reports. The system identifies high-potential trading opportunities and stores them in Airtable, alongside recent earnings summaries and a thorough, 2000–3000-word deep research article. Furthermore, the n8n workflow is scheduled to regularly deliver technical analysis updates of selected stocks directly to Telegram.

## License

AI-Stock-Technical-Analysis-n8n-x-FlowiseAI © 2025 by Gucci Chang is licensed under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International.

## Setup Instructions

### Airtable Database
- Copy the base by clicking "Copy Base": https://airtable.com/apppiTSWzJFZxliMi/shrnqmNkrNqXwizkw

### Flowise Assistant Setup
1. Import the Custom Tool 
2. Create a new Agent Flow, import "AgentFlow-Smart Option Strategy Agents"
3. Since Assistant cannot be imported, you need to create it manually:
   * Select "Custom Assistant"
   * For Knowledge, if you want to use the RAG knowledge base, use the prompt: "This document provides company's quarterly financial information, including income statements, balance sheets, and management analysis, essential for investors and analysts assessing the company's performance and compliance with SEC regulations."
   * Add the following Tools: Get_technical_analysis, perplexity_ai_search, fetch_option_data, Serp API
   * Use the following important Instructions:
你是一名擅長整理美股資訊的資深分析師，善於找出公司財報亮點與重要 insight，懂得股價技術分析與選擇權策略。

當使用者詢問財報問題時，優先從 Knowledge(document store) 提取資料，如果找不到該財報則用perplexity_ai_search 來找尋資料。
然後從資料中提取以下幾個關鍵要素：


公司的主要財務指標（如營收、淨利、每股盈餘等）
財報中的亮點或特別事件（如新產品發布、併購消息等）
重要的市場趨勢或行業動態
*注意行業特有的風險和機會
對未來的展望或預測

財報分析步驟

瀏覽財報，記錄主要財務指標
標記財報中的亮點或特別事件
研究相關市場趨勢或行業動態，並與財報內容進行對比
提出對未來的展望或預測，基於分析結果。

財報範例

Input: 公司A的最新財報
Output:

主要財務指標：營收為X億，淨利為X億，淨利為
X億，淨利為Y億，每股盈餘為$Z。

亮點：公司A推出了新產品，預計將提升市場佔有率。
市場趨勢：行業整體增長率為X%，公司A在此趨勢中表現突出。
未來展望：預計下一季度營收將增長X%。


當使用者提問股價走勢或技術分析時，使用 get_technical_analysis。使用這個工具時，不要作任何修改，保留原有Tool的回覆結果。如有圖片連結也務必輸出。
當使用者提問財報或未來展望資訊時，使用 perplexity_ai_search 來找尋資料
當使用者提問公司相關新聞(不包含財報)時，使用 Serp API 來找尋資料

其他要求

確保分析的準確性，並引用具體數據支持結論
注意行業特有的風險和機會，並在分析中提及
當調用 custom工具時，在文章結尾註記用了哪個 Tool
使用通順的繁體中文回覆

＃可調用的Tools

當使用者提問股價走勢或技術分析時，使用 get_technical_analysis
當使用者提問選擇權分析或選擇權策略時，使用 fetch_option_data
當使用者查詢財報資訊時，優先使用Knowledge(document store)，找不到時改用 perplexity_ai_search 來找尋資料
當使用者查詢公司相關新聞，使用 Serp API 來找尋資料

輸出格式
請以段落形式輸出，包含每個要素的詳細說明。如果資訊從網路取得，請附上原文連結。
### Option Data API Setup
For the Fetch_Option_Data Tool that fetches option data, you have several deployment options:

- **Replit** (Recommended): Use Replit for the easiest setup
  - Replit link: https://www.guccidgi.com/replit
  - Code repository: https://replit.com/@gucci0915/StockOptionsChainAPI?v=1
  - Just click "deploy", select host mode and specifications to complete the setup

- **Self-hosted**: You can deploy on your own cloud server like DigitalOcean

### n8n Templates Import
Import all 7 n8n templates and configure them as follows:

1. **[n8n-Personal AI Assistant with Telegram]**
   - Create Airtable connection and re-map the Airtable base/table, even if the names are the same

2. **[n8n-Flowise Earnings]**
   - Modify the Node: Flowise_Assistant / Open_Analysis with your Flowise API url
   - If Flowise has authentication enabled, create a corresponding API key or select "none" for testing

3. **[n8n-Stock Technical Analysis]**
   - For Tradingview Daily/Weekly, create Header Auth with Name: x-api-key and value as your API_Key
   - Update Telegram chat_id to your own
   - Update the Deep Research long article url to match your Deep Research AI Agent-webhook's webhook url

4. **[n8n-News Agent Perplexity]**
   - Modify the recipient email address

5. **[n8n-FMP API]**
   - Update with your FMP API Key

6. **[Deep Research AI Agent-webhook]**
   - No specific changes needed, but make sure to update the webhook url in [n8n-Stock Technical Analysis]'s Deep Research section

7. **[n8n-Schedule Watch Ticker]**
   - No specific configuration needed
