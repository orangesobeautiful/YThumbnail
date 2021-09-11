# YThumbnail

Youtube 影片封面、縮圖查詢  
這個網頁只是方便查詢而已，沒甚麼技術成分，想要手動獲取封面的可以參考 [如何做到的](#如何做到的)

## 如何使用

- 查詢封面方法

  - 方法 1：  
    在搜尋框中輸入 Youtube 影片網址

  * 方法 2：  
    在網站 URL 後面接上 Youtube 影片網址的路徑和參數

    例如:  
    某個影片網址為 `https://www.youtube.com/watch?v=videoID`  
    查詢搜圖網站的網址為 `https://www.example.com/`  
    此時可以直接透過連結 `https://www.example.com/watch?v=videoID` 查詢影片封面

  - 方法 2 進階(有域名限制)：  
    如果查詢網站可以透過 `https://www.youtube.domain.com/` 連線 (domain 為查詢網站的域名)  
    在 Youtube 影片頁面直接在網址列的 youtube 和 .com 中間加上 .domain 便可以查詢影片封面

    例如:  
    查詢網站(示範網站) `https://www.youtube.owo9.com/` (domain = owo9)  
    影片網址為 `https://www.youtube.com/watch?v=videoID`  
    那麼突然好奇某個影片的封面時，只要在網址列的 youtube 和 .com 間打上 .owo9 便可直接查詢影片封面

* 下載圖片方法  
  對圖片右鍵 > 圖片另存新檔

## 如何做到的

Youtube 的封面可以透過  
 `https://i.ytimg.com/vi/videoID/xxx.jpg`  
 或  
 `https://img.youtube.com/vi/videoID/xxx.jpg`  
 videoID 可以透過影片網址 watch?v= 後面的值取得  
 xxx.jpg 可以透過 Youtube API 找到想要的尺寸類型

提供一些封面格式範例:  
| Size |xxx.jpg |
|-----------|-----------------|
|1280 X 720 |maxresdefault.jpg|
|640 x 480 |sddefault.jpg |
|480 x 360 |hqdefault.jpg |

## 我想自己架設

下載 release 壓縮檔，例如

```bash
wget https://github.com/orangesobeautiful/YThumbnail/releases/download/0.1.0/YThumbnail-0.1.0.tar.gz
```

解壓縮

```bash
tar xzf YThumbnail*.tar.gz
```

此時目錄下應該可以看到 YThumbnail-x.x.x 的資料夾  
再把網頁伺服器的根目錄導向包含資料夾的路徑即可，以 nginx 為例(沒寫得很完整，參考形式就好)

```nginx
server {
        server_name www.domain.com;
        listen 80;
        root /path/to/your/release/dir;

        # 導向所有路由至 index.html
        location / {
                try_files $uri $uri/ /index.html;
        }
}
```

因為路由是採用 history 模式，所以需要把所有路由導向 index.html，詳情請參考 [HTML5 History Mode](https://router.vuejs.org/guide/essentials/history-mode.html)

## 開發者篇

這個專案是用 quasar 開發的，你需要安裝 Node 或 Yarn，為了開發方便可能還會需要 Quasar CLI

### 安裝 Quasar CLI

參考 https://quasar.dev/quasar-cli/installation

```bash
yarn global add @quasar/cli
```

### 安裝依賴套件

```bash
yarn
```

### 開發模式 (熱更新、錯誤回報...)

```bash
quasar dev
```

### Lint the files

```bash
yarn run lint
```

### Build the app for production

參考 https://quasar.dev/quasar-cli/build-commands

```bash
quasar build
```

### 客製化設定

參考 [Configuring quasar.conf.js](https://v2.quasar.dev/quasar-cli/quasar-conf-js).
