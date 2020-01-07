# Mocha 
Mocha 是一個 Javascript 的測試框架，目的是用來管理測試程式碼。

### 語法說明：
- describe()：描述場景或圈出特定區塊，例如：標明測試的功能或 function。
- it()：撰寫測試案例(Test Case)
- before()：在所有測試開始前會執行的程式碼區塊。
- after()：在所有測試結束後會執行的區塊。
- beforeEach()：在每個 Test Case 開始前會執行的程式區塊。
- afterEach()：在每個 Test Case 結束後會執行的區塊。

### 語法範例：
```javascript
describe('hooks', function() { // 測試區塊
  before(function() {
    // 在所有測試開始前會執行的程式碼區塊
  });

  after(function() {
    // 在所有測試結束後會執行的程式碼區塊
  });

  beforeEach(function() {
    // 在每個 Test Case 開始前執行的程式碼區塊
  });

  afterEach(function() {
    // 在每個 Test Case 結束後執行的程式碼區塊
  });

  // 撰寫個別 Test Case
  it('should ...', function() {
    // 執行 Test Case
  });
});
```

### TDD vs BDD

||TDD|BDD|
|---|---|---|
|全名|測試驅動開發(Test-Driven Development)|行為驅動開發(Behavior-Driven Development)|
|定義|在開發前先撰寫測試程式，以確保程式碼品質與符合驗收規格|TDD 的進化版。除了實作前先寫測試外，還要寫一份[可以執行的規格]|
|特性|從測試去思考程式如何實作。強調小步前進、快速且持續回饋、擁抱變化、重視溝通、滿除需求|從用戶的需求出發，強調系統行為。使用自然語言描述測試案例，以減少使用者和工程師的溝通成本。測試後的輸出結果可以直接作為文件閱讀|

# Chai
Chai 提供 BDD 語法測試用的斷言庫（Assertion Library）。斷言庫是一種判斷工具，驗證執行結果是否符合預期，
若實際結果和預測不同，就是測到 bug 了。以下分 Assert 和 Expect / Should 說明。

### Assert
```assert(expression, message)```：測試這個項目的 expression ```'foo' === 'bar'``` 是否為真，若為假則顯示錯誤訊息 message。

```javasctipt
var assert = chai.assert;

describr('AssertTest', function(){
  var foo = 'Hello';
  var bar = 'World';
  
  it(should be equal, function(){
    assert('foo' === 'bar', 'foo is not bar')
  })
})
```

### Expect/Should
預期 3 等於 (===) 2。這是使用可串連的 getter 來完成斷言。這些可串聯的 getter 有 to、is、have 等。
它很像英文，用很口語的方式做判斷。

```javascript
var expect = chai.expect;

describe('ExpectTest', function(){
  it('should be equal', function(){
    expect(3).to.equal(2);
  })
})
```

### 比較 Assert、Expect、Should 的差異
三者基本上都可完成相同工作，除了

- Should 會修改 Object.prototype
- Should 在瀏覽器環境下，對 IE 有相容問題
- Should 無法客製化錯誤訊息

```javascript
// Assert 和 Expect 的客製化錯誤訊息範例
assert.isTrue(foo, 'foo should be true');
expect(foo, 'foo should be true').to.be.true;
```

# Sinon
用來產生 Test Double（測試替身），可當成假資料來看，分為 Spy、Stub 和 Mock。

### Spy
對 function call 搜集資訊，便於對測試結果做驗證。sinon.spy() 會回傳一個 Spy 物件，
這個 Spy 物件像蜜糖般包裹於原 function 外，讓我們可以像 function 一樣呼叫。而這個 Spy 物件的
property 會協助蒐集由 function call 得到的資訊，例如：取得第一次呼叫所輸入的參數、該 function 
被呼叫的次數。Spy 是三者最間單的部分，並且 Stub 和 Mock 是建構於 Spy 之上的。

```javascript
const expect = require('chai').expect;
const sinon = require('sinon');
const testModule = require('../modules/comparisionModule');

describe('測試 comparePelple', function() {
  it('should call the callback function', function() {
    var nameList = ['Nina', 'Ricky'];
    var callback = sinon.spy();
    
    testModule.comparePelple(nameList[0], nameList[0], callback);
    console.log(callback.callCount)
  })
})
```

### Stub
取代 function。與 Spy 不同的是，Spy 依然會執行真的 function，但 Stub 並不會。
適用於 Ajax 和 Timer

#### 範例
```javascript
const expect = require('chai').expect;
const sinon = require('sinon');
const uuid = require('node-uuid');

describe('stub example', () => {
  it('check length of uuid ', () => {
    var stub = sinon.stub(uuid, 'v4');
    var mockId = stub.v4();
    expect(mockId.length).to.equal(36);
    uuid.v4.restore();
  });
});
```

#### 範例：Ajax Request
```javascript
function saveUser(user, callback) {
  $.ajax('/users', {
    first: user.firstname,
    last: user.lastname
  }, callback);
}

describe('Stub: ajax', () => {
  it('should call callback after saving', () => {
    var ajax = sinon.stub($, 'ajax'); // 取代 ajax function，並不會真的呼叫
    ajax.yields('Hello', 'World'); // 準備把傳入 callback 的參數 ['Hello', 'World'] 丟進去

    var callback = sinon.spy();
    saveUser({ firstname: 'Han', lastname: 'Solo' }, callback);

    ajax.restore(); // 清除 test double
    expect(callback.callCount).to.equal(777); // 該 function 被呼叫的次數
  });
});
```

### Mock
取代整個物件，包含完整實作細節。

```javascript
var opts = {
  call: function (msg) {
    console.log(msg);
  }
};

describe('Mock', () => {
  it('should pass Hello World to run call()', function() {
    var mock = sinon.mock(opts);
    mock.expects('call').once().withExactArgs('Hello World');
    opts.call('Hello World');
    mock.restore();
    mock.verify();
  });
});
```

### 使用時機
總結一下三者使用的時機…

- Spy：用於監看或驗證。
- Stub：使用一些假資料，測試各種狀況下功能的運作狀況。
- Mock：除了上述 Spy 和 Stub 的情況外，還需隔絕測試過程於完全獨立的環境。













