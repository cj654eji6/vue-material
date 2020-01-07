## Instance Lifecycle

### 認識 Instance Lifecycle 與 Instance Lifecycle Hooks

![vuejs](https://ithelp.ithome.com.tw/upload/images/20171222/20107673ckgtStpvnc.png "Vue.js")

上圖為官網所繪製的Instance Lifecycle(生命週期)，Vue其實在我們下執行命令後，會做很多事情，因為它將資料(data)與UI樣板(template)綁在一起，
開發者只需要宣告好資料、填入正確的UI components以及router的path設定好後，結果就會呈現。

而在我們執行後，Vue在這個Lifecycle中，會建立Vue Instance、綁定資料、事件配置、編譯樣板、經過無限修改更新資料等步驟，
直到整個Vue Instance被銷毀(destroyed)，這個Vue Instance底下的資料、樣板、事件、元件才會解除綁定，完成一整個Lifecycle。

那什麼是Instance Lifecycle Hooks(生命週期鉤子)呢？看到上面的Lifecycle diagram，鉤子就是上圖虛線延伸出去白底紅字的8個方法，
這些鉤子的用意是在Vue在Lifecycle中做每件事情的時機點前後，可以讓你有選擇處理的方式，相當客製化。

這8個鉤子的資料型態皆為function，以下我們就介紹這8個鉤子分別可以使用的時機

#### beforeCreate
在初始化vue instance並開啟整個Lifecycle後，資料綁定與事件配置之前。目前階段還無法調用$data。
應用場景：loading進頁面的事件

#### created
vue instance創建完成，$data已可以取得，屬性與事件也已綁定好。目前階段尚未掛載el，DOM也尚未生成。

#### beforeMount
在掛載el開始之前。目前階段是相關render函式首次被調用，尚未被DOM給綁定。

#### mounted
el被剛創建好的vm.$el替換取代，並且掛載到vm上。目前階段已被DOM綁定。
應用場景：對後端發出請求或讀取新資料

#### beforeUpdate
在資料更新時調用，Virtual DOM重新render與patch之前，可以在這個階段變更資料狀態。目前階段還不會繪製view。

#### updated
資料更新後會使Virtual DOM重新render頁面。目前階段會繪製出正確的view。

#### beforeDestroy
在vue instance被銷毀前調用。目前階段還可以完全使用這個vue instance。

#### destroyed
vue instance銷毀後可以調用，調用後這個vue instance底下的資料與樣板會解除綁定，事件會取消監聽，所有子元件也會被銷毀。

**Virtual DOM是用JavaScript物件來模擬DOM Tree，操作物件以提升效能。**

## Virtual DOM & V-Node
### 認識 Virtual DOM 

**DOM**(Document Object Model)的中文翻成「文件物件模型」，是HTML、XML、SVG文件可以使用的一組API。它提供了文件結構化的表示法(樹狀結構)，
並定義讓程式可以存取並改變文件架構、風格和內容的方法，目的是為了搭起網頁與程式碼之間溝通的橋樑，將網頁與程式碼(或script)連結在一起。

**Virtual DOM**(虛擬DOM)是以JavaScript物件模擬特定DOM Tree，也就是不直接操作DOM，而是改用模擬結構的方式，
達到優化效能的目的，讓頁面刷新載入的速度變快。

下面用一張圖來看看Virtual DOM的操作原理：

![vuejs](https://ithelp.ithome.com.tw/upload/images/20180117/20107673y8HiTX3yGP.png "vuejs")

Virtual DOM不會讓瀏覽器掃描整個DOM Tree，也就是不會刷新整個頁面，它會使用DOM diff這個演算法去比較這一次跟上一次Virtual DOM的差異，
接著處理有差異的部分，然後更新需要被更新的元件。

### Vue.js實現的Virtual DOM：VNode
Vue在版本2.0之後才加入Virtual DOM，Vue的Virtual DOM是VNode，一個VNode的結構包含以下這些屬性：

1. tag：該節點的html標籤
2. data：該節點的數據資料
3. children：該節點底下的子節點
4. text：該節點的文本
5. elm：當前虛擬節點對應的真實DOM節點
6. ns：該節點的namespace
7. context：編譯範圍
8. functionalContext：函數化元件的編譯範圍
9. key：該節點的key屬性，用來辨識該節點
10. componentOptions：創建vue instance會用到的資訊
11. child：該節點對應的vue instance
12. parent：該節點的父節點
13. raw：raw html
14. isStatic：該節點是否為靜態節點
15. sRootInsert：該節點是否作為根節點插入tree，值為false
16. isComment：該節點是否作為註釋節點
17. isCloned：該節點是否為克隆節點
18. isOnce：該節點是否有v-once指令

而我們透過new實體化的VNode可分為以下幾類，下面為比較常見的五個分類，還有其他分類，就不細項列出。

1. EmptyVNode：沒有內容的註釋節點
2. TextVNode：文本節點
3. ElementVNode：普通元素節點
4. ComponentVNode：元件節點
5. CloneVNode：克隆節點，可生成上面任意類型一模一樣的副本節點
在Vue.js的實作中，如果想透過JavaScript來操作元件，我們可以使用render function來操作Virtual DOM。
