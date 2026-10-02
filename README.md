# 日常用品庫存總管

這是一個以純 HTML + Tailwind CDN + Vanilla JavaScript 製作的日常用品庫存管理系統，支援：

- 商品新增、編輯、刪除
- 分類管理
- 庫存統計與低庫存提醒
- 有效期限提醒
- 表格 / 卡片雙檢視模式
- 首頁標題與背景自訂
- Google 帳號登入後才能進入系統
- Google Drive appData 雲端同步與帳號分區
- LocalStorage 帳號分區快取
- 唯讀更新歷程由程式碼中的 `CHANGELOG_CONTENT` 維護；登入者每次內容更新後會看到一次提示
- 新增用品事件快照，可依新增日期或購買日期篩選

## 使用方式

### 1. 開啟專案

直接在瀏覽器中開啟 `index.html` 即可。

如果你想要用本地伺服器方式啟動：

```bash
cd "c:\Users\User\Desktop\雲端日常用品管理"
python -m http.server 8000
```

然後在瀏覽器打開：

```text
http://localhost:8000
```

在登入畫面點選「本機測試模式（不登入）」即可測試用品管理功能。此模式只會在 `localhost` 或 `127.0.0.1` 顯示，資料留在獨立的本機測試空間，不會同步到 Google Drive；按「結束測試」可回到登入畫面。重新整理後會恢復本機測試模式。

## Google Drive 設定（必填）

這個專案內建了 Google 登入與 Drive appData 同步功能，但需要你自行建立 Google OAuth 2.0 Client ID。

### 1. 建立 Google Cloud 專案

1. 前往 Google Cloud Console：https://console.cloud.google.com/
2. 建立新的專案
3. 啟用 Google Drive API
4. 進入 APIs & Services → Credentials
5. 建立 OAuth 2.0 Client ID

### 2. 設定授權 JavaScript 原始碼

在 OAuth Client 設定中，加入以下網址：

```text
http://localhost:8000
```

如果你要部署到 GitHub Pages 或正式網站，請加入「origin」，只填協定與主機名稱，不包含專案路徑或結尾斜線：

```text
https://你的帳號.github.io
```

### 3. 填入 Client ID

在 [index.html](index.html) 的最上方 JavaScript 區塊中，將：

```js
const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_CLIENT_ID';
```

改成你自己的 Client ID，例如：

```js
const GOOGLE_CLIENT_ID = '1234567890-abcdefghijklmno.apps.googleusercontent.com';
```

### 4. 授權範圍

本專案使用以下 OAuth scopes 取得 Google 帳號識別並存取應用程式專屬 Drive 資料：

```text
openid email profile
https://www.googleapis.com/auth/drive.appdata
```

登入後才能使用管理介面。每個 Google 帳號的庫存、分類、欄位、首頁設定與檢視模式會分開儲存在該帳號的 Drive appData 和本機快取中。登入偏好會保留；重新整理時會嘗試向 Google 靜默取得新的短效 token，不會把 access token 存入 LocalStorage。若 Google 工作階段失效，仍需重新登入。

## GitHub 部署方式

### 1. 初始化 Git

```bash
git init
git branch -M main
git add .
git commit -m "Initial commit"
```

### 2. 建立 GitHub Repository

在 GitHub 網站建立一個空的 repository，例：

```text
https://github.com/你的帳號/你的專案名稱.git
```

### 3. 連接遠端

```bash
git remote add origin https://github.com/你的帳號/你的專案名稱.git
git push -u origin main
```

## GitHub Pages 部署（推薦）

如果你想直接用 GitHub Pages 免費部署這個靜態網站：

1. 進入 GitHub Repository
2. 點選 Settings
3. 左側選擇 Pages
4. Source 選擇 `Deploy from a branch`
5. Branch 選擇 `main`、Folder 選 `/(root)`
6. 儲存後就會得到一個公開網址

## 重要提醒

- OAuth Client 必須是 Web application，並設定正確的 Authorized JavaScript origins。
- GitHub Pages 是純靜態託管；長期登入依賴瀏覽器仍有有效的 Google 工作階段與授權。若要使用伺服器端 refresh token，需要另外建置安全的後端，不能將 refresh token 放在前端。

## 檔案說明

- `index.html`：主頁面與所有前端邏輯
- `README.md`：專案說明
- `.gitignore`：忽略不必要的檔案

## 作者

這個專案適合個人/居家物資管理使用。
