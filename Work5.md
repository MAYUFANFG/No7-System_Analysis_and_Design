# UML 類別圖

```mermaid
classDiagram
    User "1" -- "0..*" edge : owns >
    User "1" -- "0..*" file : owns >
    User "1" -- "1" userFold : belongs to >
    userFold "1" -- "0..*" User : contains >
    file "1" -- "1" fileFold : belongs to >
    fileFold "1" -- "0..*" file : contains >
    edge "1" -- "1" edgeType : is of type >
    edgeType "1" -- "0..*" edge : contains >
    edge "1" -- "1" State : has >
    file "1" -- "1" State : has >
    User "1" -- "1" State : has >
    edgeType "1" -- "1" State : has >
    EdgeOwnersLink "1" -- "1" edge : links >
    EdgeOwnersLink "1" -- "1" User : links >

    class User{
        +String userID
        +String userName
        +String userType
        +State userState
        +void SetType(String userFoldID)
        +void SetState(String StateID)
    }
    class userFold{
        +String userFoldID
        +String userFoldName
        +int PermissionsLevel
    }
    class State{
        +String StateID
        +String StateTitle
        +String StateDetail
    }
    class edge{
        +String edgeID
        +String edgeName
        +edgeType edgeType
        +State edgeState
        +void SetState(String StateID)
    }
    class edgeType{
        +String edgeTypeID
        +String edgeTypeTitle
        +String edgeTypeDetail
        +String edgeTypeBasicConfig
        +void SetState(String StateID)
        +void SetTypeBasicConfig(String fileFold)
    }
    class EdgeOwnersLink{
        +String LinkID
        +edge edgeID
        +User userID
        +void SetedgeID(String edgeID)
        +void SetuserID(String userID)
    }
    class file{
        +String fileID
        +String fileName
        +String fileType
        +State fileState
        +void SetState(String StateID)
        +void SetType(String filefold)
    }
    class fileFold{
        +String filefoldID
        +String filefoldName
        +Boolean IsLTSVersion()
    }
```

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

```mermaid
graph LR
    A[開始] --> B[使用者登入]
    B --> C[確認使用者帳號後登入]
    C --> D[使用者資料請求]
    D --> E[使用者資訊請求]
    E --> F[回傳使用者資訊]
    F --> G[回傳使用者資料]
    G --> H[使用者主頁面]
    H --> I[使用者登入log]
    I --> J[儲存使用者登入log]
    J --> K[結束]
```

---

WebUI受測者page：

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

```mermaid
graph LR
    A[開始] --> B[受測者頁面]
    B --> C[受測者資料請求]
    C --> D[受測者資訊請求]
    D --> E[回傳受測者資訊]
    E --> F[回傳受測者資料]
    F --> G[結束]

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

```mermaid
graph LR
    A[開始] --> B[WebUI受測者頁面]
    B --> C[受測者頁面]
    C --> D[使用者填寫新受測者資料]
    D --> E[回傳新受測者資訊]
    E --> F[儲存新受測者資訊]
    F --> G[確認已儲存新受測者資訊]
    G --> H[回傳新受測者資料]
    H --> I[回傳新受測者資料]
    I --> J[WebUI受測者頁面]
    J --> K[結束]

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

```mermaid
graph LR
    A[開始] --> B[進入查詢所有測試結果頁面]
    B --> C[查詢使用者擁有的測試資料]
    C --> D[查詢使用者擁有的測試資料]
    D --> E[回傳使用者擁有的測試資料]
    E --> F[查詢使用者擁有的受測者]
    F --> G[回傳使用者擁有的受測者]
    G --> H[回傳使用者擁有的受測者+測試資料]
    H --> I[渲染所有結果頁面]
    I --> J[結束]

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

```mermaid
graph LR
    A[點選測試結果] --> B[進入當前測試結果]
    B --> C[查詢當前測試結果]
    C --> D[查詢當前受測者]
    D --> E[回傳當前受測者+當前測試結果]
    E --> F[渲染所有結果頁面]
```
