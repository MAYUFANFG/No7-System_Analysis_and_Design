# 功能順序圖(WebUI)
### WebUI使用者登入：

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant FS
    participant Analysis
		participant edgeGUI
    WebUI->> UserPage: /login/user 使用者登入
		WebUI->> WebUI: 確認使用者帳號候登入
		WebUI->> Agent: 使用者資料請求
		Agent->> DB: 使用者資訊請求
		DB->> Agent: 回傳使用者資訊(send data)
		Agent->> WebUI: 回傳使用者資料(send data)
    WebUI->> UserPage: /{{user_id}}/main 使用者主頁面
		WebUI->> Agent: 使用者登入log
		Agent->> DB: 儲存使用者登入log
```

---

### WebUI受測者page：

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant Analysis
    participant FS
		participant edgeGUI
    WebUI->> UserPage: /{{user_id}}/testee  受測者page
		WebUI->> Agent: 受測者資料請求 (send user_id)
		Agent->> DB: 受測者資訊請求(send user_id)
		DB->> Agent: 回傳受測者資訊(send data)
		Agent->> WebUI: 回傳受測者資料(send data)
```

---

### WebUI添加受測者：

link：

- [WebUI受測者page：](https://www.notion.so/WebUI-page-5fdd8d1ebbde48218223c065637705cf?pvs=21)

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant Analysis
    participant FS
		participant edgeGUI
    WebUI->> UserPage: /{{user_id}}/testee [WebUI受測者page]
    WebUI->> UserPage: /{{user_id}}/testee/add 受測者page
    UserPage->> WebUI: /{{user_id}}/testee/add 使用者填寫新受測者資料
		WebUI->> Agent: 回傳新受測者資訊
		Agent->> DB: 儲存新受測者資訊
		Agent->> DB: 確認已儲存新受測者資訊
		DB->> Agent: 回傳新受測者資料
		Agent->> WebUI:  回傳新受測者資料
		WebUI->> UserPage:  /{{user_id}}/testee/{{testee_id}} [WebUI受測者page]			
```

---

### WebUI 查詢測試結果(總覽)：

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant Analysis
    participant FS
		participant edgeGUI
    WebUI->> UserPage: /{{user_id}}/detail/all 進入查詢所有測試結果頁面
    WebUI->> Agent: 查詢使用者擁有的測試資料
    Agent->> DB: 查詢使用者擁有的測試資料
    DB->> Agent: 回傳使用者擁有的測試資料
    Agent->> DB: 查詢使用者擁有的受測者
    DB->> Agent: 回傳使用者擁有的受測者
    Agent->> WebUI: 回傳使用者擁有的受測者+測試資料
    WebUI->> UserPage: /{{user_id}}/detail/all 渲染所有結果頁面
```

---

### WebUI 查詢測試結果(細節)：

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant Analysis
    participant FS
		participant edgeGUI
    WebUI->> UserPage: /{{user_id}}/detail/all 點選測試結果
    WebUI->> UserPage: /{{user_id}}/detail/{{detail_id}} 進入當前測試結果
    WebUI->> Agent: 查詢當前測試結果
    Agent->> DB: 查詢當前測試結果
    DB->> Agent: 回傳當前測試結果
    Agent->> DB: 查詢當前受測者
    DB->> Agent: 回傳當前受測者
    Agent->> WebUI: 回傳當前受測者+當前測試結果
    WebUI->> UserPage: /{{user_id}}/detail/{{detail_id}} 渲染所有結果頁面
```

---

### edgeGUI啟動：

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant Analysis
    participant FS
		participant edgeGUI
    edgeGUI->> edgeGUI:bootup 
    edgeGUI->> FS:bootup notify
    FS->> Agent:打包edge開機請求
		Agent->> DB: 儲存edge上線狀態資訊
		Agent->> DB: 打包edge需要讀取的config位置
		Agent->> WebUI: edge上線 websocket通知
		DB->> Agent: 回傳edge需要讀取的config位置
		Agent->> FS: 回傳edge需要讀取的config位置
		FS->> edgeGUI: 回傳edge需要讀取的config

```

---

### edgeGUI登入：

Note: 可以部份沿用WebUI使用者登入

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant Analysis
    participant FS
		participant edgeGUI
    edgeGUI->> FS:登入請求
    FS->> Agent:要求登入請求連結
		Agent->> WebUI:/login/edge/UUID 建立登陸請求頁面
    Agent->> FS:回傳登入請求連結
    FS->> edgeGUI:回傳登入請求連結
    edgeGUI->> edgeGUI:建立QRcode
    edgeGUI->> edgeGUI:渲染QRcode
    edgeGUI->> UserPage:使用者掃描QRcode

    WebUI->> UserPage: /login/edge/UUID 使用者登入
		WebUI->> WebUI: 確認使用者帳號候登入
		WebUI->> Agent: 使用者資料請求
		Agent->> DB: 使用者資訊請求
		DB->> Agent: 回傳使用者資訊(send data)
		Agent->> WebUI: 回傳使用者資料(send data)
		Agent->> FS: 回傳使用者資料(send data)
		FS->> edgeGUI: 回傳使用者資料(send data)
    edgeGUI->> edgeGUI:渲染主頁面
    WebUI->> UserPage: /{{user_id}}/edge 使用者登入裝置主頁面
		WebUI->> Agent: 使用者登入log
		Agent->> DB: 儲存使用者登入log
		

```

---

### edgeGUI結果查詢：

Note: 可以部份沿用WebUI使用者登入

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant Analysis
    participant FS
		participant edgeGUI
    edgeGUI->> edgeGUI:點選結果查詢
    edgeGUI->> edgeGUI:確認有無登入帳號 (無則[edgeGUI登入])
    edgeGUI->> FS:所有本機紀錄結果請求		
    FS->> Agent:所有本機紀錄結果請求
    Agent->> DB:所有本機紀錄結果請求
    DB->> Agent:回傳本機紀錄結果請求
    Agent->> FS:回傳本機紀錄結果請求
    FS->> edgeGUI:回傳本機紀錄結果請求

```

---

### edgeGUI開始實驗：

link:

- [edgeGUI登入：](https://www.notion.so/edgeGUI-0d1772ae04554b73890852595d6088c6?pvs=21)

```mermaid
sequenceDiagram
    participant Server
		participant edgeGUI
		participant DoEx
    edgeGUI->> edgeGUI:點選開始測試
    edgeGUI->> edgeGUI:確認有無登入帳號 (無則[edgeGUI登入])
    edgeGUI->> edgeGUI:點選受測者
    edgeGUI->> edgeGUI:測試參數選擇 (會有buttom)
    edgeGUI->> DoEx:調度實驗用py 
    DoEx->> DoEx: 執行實驗用py main.py
    DoEx->> Server: 打包完傳遞至Server
```

```mermaid
sequenceDiagram
    participant UserPage
    participant WebUI
    participant Agent
    participant DB
    participant Analysis
    participant FS
		participant edgeGUI
    DoEx->> FS: 通知已完成
    FS->> Agent: 要求儲存位置
    Agent->> DB: 建立儲存位置
    Agent->> FS: 回傳儲存位置
    FS->> FS: 建立具體儲存位置
    DoEx->> FS: 打包完傳遞至Server
    FS->> Agent: 通知已儲存+解析json結果
    Agent->> Analysis: 執行分析R語言，儲存位置請求
    Analysis->> FS: 獲得被分析檔案
    Analysis->> Agent: 執行完畢，儲存並通知
    Agent->> DB: 解析json結果並儲存
    Agent->> WebUI: json結果 websocket通知
    Agent->> edgeGUI: json結果 websocket通知
```

