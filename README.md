# CIC — 照片異常紀錄系統

GitHub Pages 靜態網站，支援手機與桌面。每筆紀錄包含一張照片、異常描述及三層固定錯誤原因。

## 使用

1. 上傳 JPG / PNG / WebP（單張最多 15 MB）。
2. 填寫描述並選擇完整三層原因，按「儲存紀錄」。
3. 紀錄與原始照片保存在目前瀏覽器的 IndexedDB。
4. 點選紀錄的「匯出 ZIP」或「匯出全部 ZIP」。每筆資料各有資料夾，內含 `record.json` 與 `photo.jpg/png/webp`。

清除網站資料、切換瀏覽器或裝置不會帶走本機紀錄。請匯出備份或同步 GitHub。網站不會自動讀入其他裝置或 GitHub 的紀錄。

## GitHub 同步

Pages 本身不提供寫入後端；「儲存設定」可啟用 GitHub REST API 同步：

1. 建立一個**已初始化**的資料 repository（建議 private，先建立 README）。
2. 建立 fine-grained personal access token，限定這個 repository，授予 **Contents: Read and write**。
3. 設定 `owner/repo` 與 token。Token 只存目前頁面記憶體，關閉／重新整理後失效，不存入 IndexedDB。
4. 每次儲存先保存在本機，再將 JSON 與照片以**同一個 commit**存到資料庫的 `data` branch，路徑 `records/<id>/`。同步失敗可在紀錄列表重試。

每位同步使用者需要自行持有資料庫寫入權限。不可將共用 token 寫進網站原始碼。公開 repository 的照片和 JSON 為公開資料；敏感內容請用私人資料庫。網站來源與資料庫可以分開。

## 修改三層原因

編輯 `docs/categories.json`。結構為第一層陣列，每項有 `id`、`label`、`children`，第二層同樣有 `children`，第三層只有 `id` 與 `label`。現有 A/B/C 為可替換的範例。

## 部署與開發

GitHub Pages 來源為 `main` branch 的 `/docs`。不需要安裝套件或建置。

```sh
python3 -m http.server 8080 --directory docs
```

使用瀏覽器開啟 http://localhost:8080 。不能直接以 file:// 開啟，分類設定需經 HTTP 讀取。
