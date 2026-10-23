# 刺蝟抓錯小遊戲：設定步驟

## 1. 建立 Firebase 資料庫（約 5 分鐘）
1. 到 https://console.firebase.google.com 建立專案（可關閉 Analytics）。
2. 左側 Build → Realtime Database → 建立資料庫（地區選 asia-southeast1，模式選「鎖定模式」）。
3. 在「規則」分頁貼上下面規則並發布：

```json
{
  "rules": {
    "answers": {
      "$session": {
        ".read": true,
        "$id": {
          ".write": "!data.exists()",
          ".validate": "newData.hasChildren(['picks','t'])",
          "picks": { ".validate": "newData.isString() && newData.val().length < 200" },
          "t":     { ".validate": "newData.isNumber()" },
          "name":  { ".validate": "newData.isString() && newData.val().length <= 20" },
          "$other": { ".validate": false }
        }
      }
    }
  }
}
```
這會讓任何人只能「新增」答案，不能修改或刪除；統計頁可讀取。多餘的欄位一律拒收，避免有人塞入大量資料佔用免費額度。

4. 資料庫頁面上方的網址（https://xxx-default-rtdb....firebasedatabase.app）貼到 `config.js` 的 `DB_URL`。

## 2. 上傳到 GitHub Pages
1. 建立 repo，把這個資料夾所有檔案放進去。
2. Settings → Pages → Branch 選 main / root。
3. 網址：學員頁 `https://帳號.github.io/repo名/`，講師頁 `.../dashboard.html`。

## 3. 現場使用
- QR code 指向學員頁。講師螢幕開 `dashboard.html`。
- 每場活動修改 `config.js` 的 `SESSION`，統計不會混在一起。
- 測試後想清空：Firebase 控制台刪除 `answers/場次代號`。
- 影片做好後，如錯誤項目有變，只改 `questions.js`。
