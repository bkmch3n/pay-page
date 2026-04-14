# 收款頁面 Pay Page

一個簡單、輕量的靜態收款資訊頁面。透過編輯 `links.json`，可以自由新增付款連結（如 LINE Pay、街口、Richart）或可複製的銀行帳號。無需後端，部署簡單。

---

## 預覽

頁面會顯示一排按鈕，分為兩種類型：
- **連結按鈕**（綠色）：點擊後在新分頁開啟付款頁面
- **複製按鈕**（深色）：點擊後將帳號複製到剪貼簿，並顯示「✅ 已複製!」確認提示

---

## 檔案結構

```
pay-page/
├── index.html          # 主頁面（通常不需要修改）
├── links.json          # 付款資訊設定檔（主要編輯這個）
├── nginx.conf          # Nginx 設定（使用 Docker 部署時使用）
└── docker-compose.yml  # Docker Compose 設定
```

---

## 如何設定（編輯 links.json）

`links.json` 是一個陣列，每個物件代表一個按鈕。支援兩種類型：

### 連結按鈕（`"action": "link"`）

點擊後在新分頁開啟指定網址，適合 LINE Pay、街口、Richart 等有付款連結的服務。

```json
{
  "action": "link",
  "text": "👉 透過 LINE Pay 付款（點擊跳轉）",
  "value": "https://line.me/ti/p/你的連結"
}
```

### 複製按鈕（`"action": "copy"`）

點擊後將帳號複製到剪貼簿，適合銀行帳號、匯款帳號等需要手動輸入的資訊。

```json
{
  "action": "copy",
  "text": "👉 國泰銀行：(013) 末四碼1234（點擊複製）",
  "value": "完整帳號數字"
}
```

### 欄位說明

| 欄位 | 說明 |
|---|---|
| `action` | `"link"` 或 `"copy"`，決定按鈕行為 |
| `text` | 按鈕上顯示的文字 |
| `value` | `link` 類型填網址；`copy` 類型填要複製的內容（如純帳號） |
| `_comment` | 說明用途，程式不會讀取，可自由刪除 |

---

## 如何部署

### 方法一：Docker（推薦）

1. Clone 此專案：
   ```bash
   git clone https://github.com/bkmch3n/pay-page.git
   cd pay-page
   ```

2. 編輯 `links.json`，填入你自己的付款資訊

3. 編輯 `index.html` 第 82 行，將 `Payment to Your Name` 改成你的名字

4. 啟動容器：
   ```bash
   docker compose up -d
   ```

5. 頁面預設在 `http://localhost:5080`

### 方法二：純靜態（任何 Web 伺服器）

直接將 `index.html`、`links.json` 放到任何靜態伺服器或 CDN 即可。由於頁面透過 `fetch()` 讀取 `links.json`，兩個檔案必須放在同一目錄下。

---

## 自訂樣式

顏色設定在 `index.html` 的 `<style>` 區塊中：

| 變數 | 預設值 | 說明 |
|---|---|---|
| 背景色 | `#1a1a2e` | 深藍紫色背景 |
| 連結按鈕色 | `#4ecca3` | 綠色 |
| 複製按鈕色 | `#16213e` | 深藍色 |

---

## 授權

MIT License — 自由使用、修改、分發。
