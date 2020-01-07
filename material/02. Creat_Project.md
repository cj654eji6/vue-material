## 建立環境與新增專案

### 建立環境
開始之前，電腦需要有Node.js環境與NPM(Node Package Manager)套件管理工具

裝好node環境與npm後，即可使用套件管理工具npm安裝好**Vue環境**
```
npm install vue
```

### 新增專案

透過npm全域安裝vue-cli，安裝完後即可使用vue指令
```
npm install -g vue-cli
```

初始化專案
```
vue init [template] [project_name]
```

[template] 可以採用官方提供的幾種template，若不知道有什麼template，可以下指令查看
```
vue list
```
![vuejs](https://ithelp.ithome.com.tw/upload/images/20171224/201076739P81kCGDjJ.png "vuejs")

以下使用webpack / webpack-simple來做範例專案。

### 快速建立專案：使用webpack / webpack-simple樣板

#### 認識webpack與vue-loader
1. webpack是一個前端打包工具
2. vue-loader是一個webpack的載入器(loader)，可將vue組件(.vue檔)轉換為JavaScript模組

#### 選用webpack的原因
1. 找出各項靜態資源之間的關聯性與整合，使用vue-loader將它們模組化，產出優化過後的code
2. Hot-loader：Hot-loader可以在修改與存檔好code後，就直接更新畫面，不需要按重新整理，相較live-reloader來說，方便許多

#### 建立新專案步驟
使用webpack初始化專案
```
vue init webpack [project_name]
```

進入專案資料夾
```
cd [project_name]
```

在專案資料夾底下，安裝所需要的模組
```
npm install
```

啟動http server
```
開發版，本地開發(localhost)適用，進入http://localhost:8080可看到結果
$ npm run dev
```

打開瀏覽器，網址輸入http://localhost:8080

## 熟悉Webpack專案架構
### 專案整體架構
我們使用webpack樣板初始化一個完整的vue專案，該專案資料夾內基本架構如下圖：

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673BCdwJt7xaM.png "vuejs")

> static資料夾內存放的是“真正的”靜態資源，他們不會被webpack處理。

### 認識babel與ESLint
1. babel是一個轉碼器，因為目前瀏覽器對於新型態JavaScript語法支援度不高，babel-loader可將ES6或ES7語法轉為支援度高的ES5的語法。
2. ESLint目的是改善程式碼品質，發現與修正程式碼的問題並達到一制性，所以有安裝此套件的話，
常常會在編譯執行時看到很多warning訊息，如果安裝之後想要忽略ESLint檢查，可以到.eslintignore文件中做修改。

### src資料夾底下架構
而我們主要編輯的code檔案會放在src這個資料夾內，src資料夾內架構如下

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/2010767362LeB88tPF.png "vuejs")

從上圖可以了解到，main.js是整個專案的主要入口點，他會去連接到這個專案主要的根實體App.vue。

## Webpack 專案運作流程
在大致了解以webpack樣板建置的專案架構後，我們接下來來了解整個app運作流程。

當我們下npm run dev這個指令後，啟動http server，這個指令會同時開啟根目錄下的index.html與src資料夾內的main.js這兩個檔案

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/201076733G2mB9i68k.png "vuejs")
![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/201076736HIAtMBSGd.png "vuejs")

而main.js會同時運行App.vue以及在router資料夾內的index.js

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673WBhdrsy9Fj.png "vuejs")

<router-view/>是路由器顯示標籤，為vue-router使用，在index.js下Router函數中所使用的UI元件皆會套用至這個標籤當中。

index.js中，可以在Router這個函數內，自定義url路徑名稱(path)，components下可以放入寫好的UI元件。

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673nKqrjUECc9.png "vuejs")

因此，如果我們要產生新的UI元件，就要寫一個.vue檔，可放置在components資料夾之下；如果我們要將這個vue元件顯示出來，
就需要到index.js中修改路由配置，下面舉例實作看看。

### 實作：新增一個顯示Hello Vue的component
在components資料夾內先新增一個testhello.vue的檔案

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673zxhxFLE1yR.png "vuejs")

然後修改index.js，新增一個新的url路徑與components

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673Ihrka25D2E.png "vuejs")

最後下指令啟動server後，瀏覽器輸入http://localhost:8080/#/testhello。
