## 第 2 節：執行環境與詞彙環境

### Syntax parsers 語法解析器

將語法解析器想像成一個程式，在你每次執行 JavaScript 時，這個中介程式會轉換你的程式碼

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/Syntax%20parsers%20%E8%AA%9E%E6%B3%95%E8%A7%A3%E6%9E%90%E5%99%A8.png?raw=true" style="zoom:67%;" />

### Execution contexts 執行環境（脈絡）

一個容器，幫助管理正在執行之程式。我們有許多的詞彙環境，程式碼的實際上所在位置，但哪個才是現在正在執行的？就是被執行環所管理。

執行環境包含了你寫的程式碼，正在執行的程式碼，但是它包含的不只你所寫的程式碼，你的程式碼正在被轉換，正在被另一個東西處理

另一個某人寫的程式（編譯器），所以它在執行你的程式碼，另外它也能夠做別的事情。

### Lexical environment 詞彙環境

當我們討論程式碼的詞彙環境時，我們其實是在討論：它被寫在哪裡？它的周圍環境是什麼？

### 物件

物件就是 `名稱/值` 配對的組合

A collection of name value pairs.

### 全域環境與全域物件

> 全域執行環境又稱「基礎執行環境」

當 JavaScript 檔案被執行，就會創造全域執行環境，JavaScript 會自動為你建立了這兩個東西：

1. 全域物件 
2. 一個特殊的變數 'this'

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%85%A8%E5%9F%9F%E7%92%B0%E5%A2%83%E8%88%87%E5%85%A8%E5%9F%9F%E7%89%A9%E4%BB%B6.png?raw=true" style="zoom: 67%;" />

如果你在瀏覽器裡全域物件就是 window，你還會得到一個特殊的變數 this，this 會指向全域物件所以就等於 window：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%85%A8%E5%9F%9F%E7%92%B0%E5%A2%83%E8%88%87%E5%85%A8%E5%9F%9F%E7%89%A9%E4%BB%B62.png?raw=true" style="zoom:50%;" />

無論你的程式碼何時執行，你的程式碼都被包在執行環境裡。

```js
//全域 
Global:
Not inside a function.
//程式碼或變數不在函數裡面就是全域的。
```

### 執行環境：創造與提升 (Hoisting)

當執行環境被創造時，在第一階段（ Creation Phase 創造階段），變數和函數（這邊指函數陳述式）會被設定放在記憶體裡面，這個步驟叫做「提升」（hoisting）。第二階段是執行。在創造階段我們已經設定好所有的東西了， 然後逐行編譯、轉換程式碼變成電腦看得懂的東西。

> 在創造階段還會與外部環境連結。

它不是真的把你的程式碼移到最上面，這表示在逐步執行程式碼之前 JavaScript 已經為變數和函數在記憶體中建立空間了，所以當程式被逐行執行時，變數和函數已經存在於記憶體中因此它們可以被找到。

變數的情況有些不同，JavaScript 為變數空出記憶體空間時並不知道它會是什麼值，直到它被執行才會知道，因此此時變數的值是 `undefined`，函數則是完全被設定好放進記憶體中。

> 所有 JavaScript 的變數在一開始都會被設定為 `undefined`。

瞭解提升之後，我們可以呼叫一個函數，儘管它在之後才被宣告，因為我們寫出的程式碼不會直接被執行而是會經過 JavaScript 的轉換，在執行環境的創造階段，JavaScript 會在記憶體中空出空間給函數還有變數，這些執行程式時會用到的東西。

範例：

```js
function b () {
    console.log('call b')
}

b()

console.log(a)

var a = 'Hello'

console.log(a)
```

在創造階段（第一階段），函數 b 和變數 a 被放進記憶體，變數 a 的值為 `undefined`。

接著執行階段（第二階段）：

1. 呼叫函數 b
2. 執行 `console.log(a)`（結果為 `undefined`）
3. 執行 `var a = 'Hello'`，將記憶體中的變數 a 的值設為字串 'Hello'
4. 執行最後一行 `console.log(a)`（結果為 'Hello'）

#### 執行環境重點（超重要）

1. 當執行 JavaScript 檔案就會創造一個「全域」執行環境
2. 當函數被呼叫也會創造一個執行環境
3. 每個執行環境都會有自己的變數環境和 this （this 在不同情況下會指向不同東西，這個後面會提到）
4. 全域物件 & this 不須程式碼就會被創造

### 觀念小叮嚀：JavaScript 與 undefined

``` js
console.log(a)
// Uncaught ReferenceError: a is not defined
```

當未宣告變數 a 時使用到 a 會發生「無法參照」的錯誤，這是因為執行環境被創造時，在創造階段沒有找到 `var a` ，所以 a 從未在記憶體中出現，所以執行到這行程式碼時會說 「嘿，我沒有在記憶體中找到 a 這個值」所以會顯示參照不到這個值的錯誤 'a is not defined.'

然而，

然而當我宣告 ` var a`，a 在創造階段時就被放進記憶體中，儘管我還沒設值  JavaScript 已經幫我設為 `undefined`，`undefined` 不代表為空的或不存在，實際上它會佔據記憶體空間，它是一個值，一個特殊關鍵字。

``` js 
var a
console.log(a)
// undefined
```

### 觀念小叮嚀：單執行緒 VS 同步執行

##### 單執行緒 Single Threaded：

One command at a time.  一次執行一個指令

##### 同步執行 Synchronous：

One at a time and in order. 一次執行一個且照順序

### 函數呼叫 (function invocation) 與執行堆 (execution stack)

```tex
Invocation: running a function 執行或是呼叫函數
In Javascript using parenthesis () 括號
```

**每一次 JavaScript 呼叫函數都會創造一個執行環境然後放進執行堆中，它會有自己的記憶體空間給變數與函數，它會經歷創造階段然後逐行執行函數中的程式碼**

範例：

``` js
function a () {
    b()
    var c
}
function b () {
    var d
}
a()
var e
```

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%87%BD%E6%95%B8%E5%91%BC%E5%8F%AB%20(function%20invocation)%20%E8%88%87%E5%9F%B7%E8%A1%8C%E5%A0%86%20(execution%20stack).png?raw=true" style="zoom:67%;" />

首先，全域執行環境會被創造，這會創造全域物件和 'this'，如果是在瀏覽器上全域物件就是 window 物件，然後將變數和函數放入記憶體中（在創造階段），所以函數 a 跟 b 會被放入記憶體（包含 `var c` 和 `var d` 和 `var e`），接著逐行執行程式碼也就是呼叫 a 函數：

``` js
function a () {
    b()
    var c
}
function b () {
    var d
}
a() // <= 這邊
var e
```

a 會創造自己的執行環境且進入執行堆中：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%87%BD%E6%95%B8%E5%91%BC%E5%8F%AB%20(function%20invocation)%20%E8%88%87%E5%9F%B7%E8%A1%8C%E5%A0%86%20(execution%20stack)2.png?raw=true" alt="image-20201118221518559" style="zoom: 67%;" />

然後在函數 a 中又呼叫了函數 b 所以 b 又創造了一個自己的執行環境且進入執行堆中的最上層，接著執行 `var d`，當 b 函數結束後，b 執行環境會離開執行堆回到 a 函數的執行環境繼續接著執行 `var c`：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%87%BD%E6%95%B8%E5%91%BC%E5%8F%AB%20(function%20invocation)%20%E8%88%87%E5%9F%B7%E8%A1%8C%E5%A0%86%20(execution%20stack)3.png?raw=true" alt="image-20201118221841199" style="zoom: 67%;" />

當 a 函數結束，a 的執行環境會離開執行堆回到全域執行環境執行程式碼最後一行的 `var e`。

### 變數環境 Variable environment

變數環境只是在描述你創造變數的位置，每個執行環境都有自己的變數環境。

```js
function b () {
    var myVar
    console.log(myVar)
}
function a () {
    var myVar = 2
    console.log(myVar)
    b()
}
var myVar = 1
console.log(myVar)
a()
console.log(myVar)
/* output to be
1
2
undefined
1
```

雖然 `myVar` 被宣告三次，但它們是不同的，彼此沒有關聯。

當 a 函數執行完畢，a 的執行環境會離開執行堆並回到全域執行環境，所以最後一行的 `console.log(myVar)` 是全域的 `1` 。

### 範圍鏈 Scope chain

當我們要處理變數時 JavaScript 不只會在目前的執行環境的變數環境中尋找，還會參照外部環境，每個執行環境都有一個可以參照的外部環境。當你需要某個執行環境內的程式碼的變數，如果它無法找到變數，它會到外部環境尋找變數。

>記住「範圍」代表我能夠取用這變數的地方，「鏈」是外部環境參照的連結

**如何判斷參考到的是哪種外部環境？答案：詞彙環境，程式碼被寫出來的實際位置**

```js
function b() {
	console.log(myVar)
}

function a() {
	var myVar = 2
	b()
}
var myVar = 1
a()
//output to be 1
//函數 a 跟函數 b 的外部環境都是全域執行環境
```

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%AF%84%E5%9C%8D%E9%8F%88.png?raw=true" style="zoom: 67%;" />

別的情況：

```js
function a() {
    function b() {
	console.log(myVar)
	}
    
	var myVar = 2
	b()
}
var myVar = 1
a()
//output to be 2
```

因為 b 函數被寫在 a 函數裡面，JavaScript 引擎會認定 b 的外部參照是 a，而 a 的外部環境仍然是全域執行環境，所以當 b 函數找不到 `myVar` 這個變數，它會往範圍鏈下層尋找，也就是它的外部環境 a，因此值為 `2` 。

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%AF%84%E5%9C%8D%E9%8F%882.png?raw=true" style="zoom: 67%;" />

如果在 a 裡面還是找不到，會再往外部找（a 的外部環境是全域執行環境），所以值是 `1` 。

### Scope

Scope 範圍或稱「作用域」是指變數可以被取用的區域。如果你呼叫同一個函數兩次，它會各有一個自己的執行環境，裡面的變數看起來相同，但是在記憶體位置中其實是兩個不同的變數。

### ES6 宣告變數方式 const 與 let

#### let

用來宣告一個變數像 `var` 一樣，不過 `let` 是塊級作用域， `var` 是函數作用域。

#### const

`const` 用來宣告一個常數且一定要賦值，其值不能再藉由指派運算子（例如等號運算子）進行變動，否則報錯。常數名稱可以用大寫表示以利區分：

``` js
//宣告常數一定要賦值
const A //Uncaught SyntaxError: Missing initializer in const declaration
const PI = 3.14
PI = 20
console.log(PI) // output to be => TypeError: Assignment to constant variable.
```

注意！當你用 `const` 宣告物件時有例外情況：

``` js
const obj = {
  b: 10
}

obj.b = 20
console.log(obj.b) // output to be 20

const arr = [1, 2, 3]
arr.push(4, 5)
console.log(arr) // output to be [1, 2, 3, 4, 5]
```

因為變數存取物件（包含陣列）的方式是存取記憶體位置，原則上此 obj 物件的記憶體位置（指引）沒有被更動，被更動的是指引指向的值。

> 回顧一下： [從 Object 的等號真正的理解變數](https://github.com/TsaiChihWei/learning-blog/issues/4) 與 [從博物館寄物櫃理解變數儲存模型](https://medium.com/@hulitw/variable-and-frontdesk-a53a0440af3c)。

不過若你用 `const` 宣告了一個物件，當你想要修改這個物件的內容時（再將其他值指派給這個變數），JavaScript 引擎會認為你要創建新的物件，所以就會有新的記憶體位址，這樣常數就會被改變，所以會報錯：

```js
const obj = {a: 1}
obj = {a: 2} //Uncaught TypeError: Assignment to constant variable.

//將其他值再指派給　obj 會有一個新的記憶體位址，
```

#### let 與 const 的共同特性

##### 作用域 scope

`let` 與 `const` 是區塊作用域 (block scope) `{}` 包起來的區域，`var` 則是函式作用域 (function scope)。

使用 `var` 宣告變數 a 時，可用範圍在 test function 內，即便 `console.log(a)` 在 `if` block 之外仍能讀取到 a 的值：

``` js
function test() {
    if (true) {
        var a = 10
    } 
    console.log(a);
}

test() // output to be 10
```

使用 `let` 或 `const` 宣告變數 a 時，可用範圍在 `if` block （大括號） 內，所以 `console.log(a)` 在離開 `if` block 的區域便無法讀取到 a 的值：

``` js
function test() {
    if (true) {
        let a = 10
    } 
    console.log(a)
}

test() // ReferenceError: a is not defined
```

##### 仍然會提升 (Hoisting)

使用 `let` 與 `const` 宣告變數，在創造階段變數一樣會被放入記憶體中，但沒有初始化為 undefined。到了執行階段，賦值之前嘗試取用的話會發生錯誤，這一段不能取用變數的期間稱為暫時性死死區 (Temporal Dead Zone)。

##### 不會出現在全域物件 window 裡面

```js
var a = 1
const b = 1
let c = 1

console.log(window.a) //1
console.log(window.b) //undefined
console.log(window.c) //undefined
```

##### 不能重複宣告

```js
let a = 1
let a = 3 //Uncaught SyntaxError: Identifier 'a' has already been declared
const b = 2
const b = 3 //Uncaught SyntaxError: Identifier 'b' has already been declared
```

因應 ES6 的出現，使用上建議不要再用 `var` 來宣告變數，優先使用 `const`，若須重新賦值再使用 `let` ，藉由限縮變數的活動範圍來減少發生錯誤的可能。

### 關於非同步回呼

> Asynchronous means more than one at a time.

非同步表示在一個時間點不只一個，可能有一段程式在執行時會開始執行另一段程式碼，然後又會再執行別的程式碼。

> 此小節內容節錄並改寫自 [Huang Pei](https://medium.com/@HuangPei/%E5%85%8B%E6%9C%8Djs%E5%A5%87%E6%80%AA%E7%9A%84%E9%83%A8%E5%88%86-%E9%97%9C%E6%96%BC%E9%9D%9E%E5%90%8C%E6%AD%A5%E5%9B%9E%E5%91%BC-93847c5a7cae)

先說結論：

JavaScript 可以不用一直等到一件事做完再做下一件事，它可以先把非同步事件丟到**事件佇列 (Event Queue)**，先去執行別的事（執行堆Execution Stacks 中的），等消化完執行堆後，再把事件佇列中的事拉出來完成。

所以**不是真的非同步**！JavaScript 不是一次做很多事，他只是調整了執行的順序，**用同步的方式處理非同步事件**。

> JavaScript 本身是同步的，它一次執行一行程式碼。

然而瀏覽器本身不是只有 JavaScript，還要和其他引擎 (Rendering Engine, Http request..) 相互連動運行，只有 JavaScript 是同步的，那麼要如何和其他引擎及請求互相配合時又繼續跑程式碼呢？

```js
The JavaScript engine has hooks where it can talk to the rendering engine and change what the web page looks like, or go out and request data.

while it may be running asynchronously meaning that the rendering engine and the JavaScript engine and request are running asynchronously inside the browse.
//渲染引擎跟 JavaScript 引擎還有請求發送它們之間的交流是非同步的，而JavaScript 引擎本身是同步的
```

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E9%97%9C%E6%96%BC%E9%9D%9E%E5%90%8C%E6%AD%A5%E5%9B%9E%E5%91%BC.png?raw=true" alt="Image for post" style="zoom: 80%;" />

JavaScript 引擎內的等待列則稱為**事件佇列（event queue）**

這裡面都是事件、事件通知，這些可能要發生的，所以當瀏覽器，在 JavaScript 引擎外的某處有一個需要被通知的事件，在 JavaScript 引擎裡會被放到佇列裡。

**當執行堆是空的 JavaScript 才會注意事件佇列。**

執行堆尚未被消化，而事件佇列中有尚位處理的 Click 和 HTTP：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E9%97%9C%E6%96%BC%E9%9D%9E%E5%90%8C%E6%AD%A5%E5%9B%9E%E5%91%BC2.png?raw=true" alt="Image for post" style="zoom: 80%;" />

當執行堆被清空了，才開始處理 Click 事件，將事件的函數拉出來執行，而 HTTP 待命中：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E9%97%9C%E6%96%BC%E9%9D%9E%E5%90%8C%E6%AD%A5%E5%9B%9E%E5%91%BC3.png?raw=true" alt="Image for post" style="zoom: 80%;" />

```js
//重要
直到 JavaScript 已經逐行執行完程式，執行堆空了，執行環境結束了，才會處理事件佇列的事件，所以這不是真正的非同步，
而是**瀏覽器**非同步的把東西放到事件佇列，但程式仍然繼續一行行執行。
```

範例：落落長的程式碼，結論在下面

```js
// long running function
function waitThreeSeconds() {
  var ms = 3000 + new Date().getTime();
  while (new Date() < ms) {
  console.log(‘finished function’)
}
function clickHandler() {
  console.log(‘click event!’)
}
// listen for the click event
document.addEventListener(‘click’, clickHandler)
waitThreeSeconds()
console.log(‘finished execution’)

/* output to be
finished function
finished execution
click event! ====> 三秒後之前點的滑鼠的事件要等執行堆中的執行環境都結束才被執行
```

就算設定了時間函數延遲了 3 秒，在這 3 秒間就算怎樣狂點滑鼠，click event! 的訊息也不會出現，因為要等三秒過後 waitThreeSeconds 函數執行環境結束 => 全域執行環境結束（execution stacks 清光了），才會跑事件佇列內的 click 事件。

## 第 3 節：**型別與運算子**

### 觀念小叮嚀：動態型別 Dynamic typing

JavaScript 處理型別的方式為「動態型別 」(Dynamic typing)

```js
/*Dynamic typing:
You don't tell the engine what type of data a variable holds, it figures it out with while your code is running.*/
意思是你不需要告訴 JavaScript 你的變數是何種型別的資料，你不需要在程式裡寫出來，相反地它會在運行程式時自動知道，所以當你執行程式時，一個變數可以在不同時候擁有不同型別，因為型別都是在執行時才知道的。
```

而像 Java 或者 C#，它們用「靜態型別 」(Static typing) 方式處理，這代表你必須在一開始就告訴編譯器你的變數是什麼資料型別，所以你可能會有一個關鍵字， 像是「bool」去表示這個變數的型別是布林值，可能是 True 或 False，如果你將其他型別的值放進這個變數就會得到 error。

```js
//Static typing
bool isNew = 'hello' // an error

//Dynamic typing
var isNew = true // no errors
isNew = 'yup'
isNew = 1
```

### Primitive type 純值（基本型別）

```js
Primitive type:
A type of data that represents a single value. That is, not an object.
//純值是一種資料的型別，表示一個值。換句話說，不是物件。
```

JavaScript 有六種基本型別（純值）：

1. undefined

```js
//undefined means lack of existence. You shouldn't set a variable to this.
Undefined 表示還不存在，這是 JavaScript 給所有變數的初始值，它會一直是 undefined 直到你給定變數一個值，所以你不應該設定一個變數等於 undefined。
```

2.  null

```js
null also represents lack of existence. You can set a variable to this.
// null 比較適合來表示一個東西不存在
```

3. boolean

```js
true or false
```

4. number 數值

```js
永遠都是浮點數 (floating point number)，但你可以假裝它是整數或其他數字型別。
```

5. string 字串

```js
A sequence of characters. Both '' and "" can be used.
一連串字符，單引號和雙引號都可以用來表示字串。
```

6. symbol

### 觀念小叮嚀：運算子 Operators

```js
Operator:
A special function that is written differently or syntactically.
```

**運算子只是一個特殊的函數**，但它和其他你自己寫的函數不同。一般來說，運算子需要兩個參數來回傳一個結果。例：加減乘除等運算符號 ( `+ - * /`)

### 運算子的優先性與相依性

```js
//運算子優先性
Operator precedence:
which operator function gets called first
```

運算子優先性表示哪個運算子被優先運算。

在同一行程式有不只一個運算子時，具有高優先性的運算子會先計算。

```js
//運算子相依性
Associativity:
what order operator functions get called in:
left-to-right or right-to-left when they have the same precedence.
```

如果有很多的運算子在同一行程式碼中，優先性能夠幫助判斷誰先，但如果全部的運算子優先性都相同，這時就要看相依性。

範例：

```js
var a = 2
var b = 3
var c = 4
a = b = c
console.log(a)
console.log(b)
console.log(c)
/*output to be
4
4
4
```

**重要**

等號 `=` 的相依性是 right-to-left，所以先執行 `b = c` ，值得注意的是 `b = c` 會**回傳等號右邊的參數**（也就是 c，值為 4），接著繼續往左執行 `a = 4` 。

> [運算子優先性及相依性參照表格](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

### 觀念小叮嚀：強制轉型 Coercion

```js
//強制轉型
Coercion:
coverting a value from one type to another.
//轉換一個值的型別
```

範例：

```js
var a = 1 + '2'
console.log(a)
/*12
```

數值 1 加上字串 2，數值 1 會被強制轉型成字串
**數值 + 字串 會變成一個字串**

範例：

```js
console.log(3 < 2 < 1)
/* true
```

1. 小於運算子是左相依性 left-to-right，所以由從左邊執行
2. `3 < 2` 的回傳值為 false
3. `false < 1`，此時 false 會被強制轉型成數字 0
4. `0 < 1`，所以結果為 true

#### 補充

當以下這些值被轉成數值時的情況：

+ undefined 會被強制轉型為 NaN

+ null 會被強制轉型為 0
+ false 會被強制轉型為 0
+ true 會被強制轉型為 1

#### 雙等號 == 和三等號 === 的差別

雙等號會進行強制轉型而三等號不會

```js
console.log(1 == '1') //true
console.log(1 === '1') //false
console.log(false == 0) //true
console.log(false === 0) //false
```

記得前面說過 `null` 被強制轉換成數值時會變成 0，但是 `null == 0` 的結果是 false，很奇怪吧！雙等號很有可能會造成非預期的行為還有很多疑問，所以你應該盡量使用三等號 `===` 而不是雙等號 `==`，除非你需要或是故意使用雙等號。

> 什麼是 Object.is？ 
>
> [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) simply has to be thought of in terms of its specific characteristics, rather than its looseness or strictness with regard to the equality operators.
>
> 簡單來說，Object.is 可以用來處理一些特別的情況，例如 NaN, +0, -0。

### 存在與布林（實用！）

```js
var a
if (a) {
    console.log('Something is there!')
}else {
    console.log('Nothing is there!')
}
//Nothing is there
由於 a 為 undefined 所以在 if 陳述句會被強制轉型為 false
```

if 陳述句中的條件（括號裡面的的程式碼）會被強制轉型成布林值，所以 a 是什麼其實不重要，我們可以利用強制型轉的特性來檢查變數有沒有值。

可以試著用布林內建函數來看看一些特定值被轉成布林值的情況：

```js
Boolean(undefined) //false
Boolean(null) //false
Boolean("") //false
Boolean(0) //數值 0 被強制轉型為 false
```

### 預設值：利用 「或」||

「或」運算子 `||` 的回傳值是什麼？

```js
undefined || 'hello'
//hello

null || 'hello'
//hello

'hi' || 'hello'
//hi
```

「或」運算子會回傳**第一個**可以被強制轉型為 `true` 的值，記住回傳不是 `true` 或者 `false` 而是一個**值**。

下面是一個未幫函數的參數添加預設值的例子：

```js
function greet (name) {
    console.log(`Hello ${name}`)
}
greet() //Hello undefined
greet(Leo) //Hello Leo
```

加上預設值的寫法：

```js
function greet (name) {
    name = name || 'Jack'
    console.log(`Hello ${name}`)
}
greet() //Hello Jack
greet(Leo) //Hello Leo
```

**ES6 更簡單的預設值寫法：**

```js
function greet (name = 'Jack') {
    console.log(`Hello ${name}`)
}
greet() //Hello Jack
greet(Leo) //Hello Leo
```

## 第 4 節：物件與函數

### 物件與「點」

```js
var person = {}
person['fisrtname'] = 'Leonardo'
person['lastname'] = 'DiCaprio'

var firstNameProperty = 'fisrtname'
console.log(person['fisrtname']) //Leonardo
console.log(person[firstNameProperty]) //Leonardo

person.address = {}
person.address.street = '111 Main St.'
person.address.city = 'New York'
console.log(person['address']['street']) //111 Main St.
console.log(person.address.city) //New York
```

這些「點」和「中括號」是 Property accessors 又稱「屬性存取器」，他們都只是函數，是**運算子**，一個取出物件內容的方式。參考運算子相依性表格可以發現「點」和「中括號」都是左相依性 letf-to-right，遇到連續的屬性存取器記得從左邊往右開始執行。

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%89%A9%E4%BB%B6%E8%88%87%E3%80%8C%E9%BB%9E%E3%80%8D.png?raw=true" style="zoom:67%;" />

補充：建議你只用「點」運算子，他很簡潔、很清楚也很容易除錯，除非你真的需要用動態字串取用屬性。

### 物件與物件實體 Object literals

直接用大括號 `{}` 的方式來宣告一個物件就是物件實體語法，這是一個比較快速也建議用的方式：

```js
var person = {}
```

還能同時建立屬性和方法：

```js
var person = {
    firstname: 'Leonardo',
    lastname: 'DiCaprio'
}
```

混和「點」運算子和物件實體語法：

```js
var person = {
    firstname: 'Leonardo',
    lastname: 'DiCaprio'
}

person.address = {
    street: '111 Main St.',
    ciry: 'New York'
}
```

為什麼可以這樣做？因為你寫的程式碼並不是真的直接被處理，它會先被JavaScript轉化成電腦能懂的東西。

### 框架小叮嚀：偽裝命名空間

```js
Namespace:
A container for variables and functions
命名空間是變數和函數的容器
```

簡單來說偽裝命名空間就是利用**創造物件**來避免相同名稱的變數被覆寫的問題

```js
var greet = 'Hello!'
var greet = 'Hola!'
console.log(greet) //Hola!
```

```js
var english = {
    greet: 'Hello!'
}
var spanish = {
    greet: 'Hola!'
}
console.log(english.greet) //Hello!
console.log(spanish.greet) //Hola!
```

### JSON 與物件實體

JSON 是一種資料交換的格式，它和物件實體非常相像因為它就是被物件實體寫法所啟發。

```js
var objectLiteral = {
    "name": "Mary",
    "isProgrammer": true
}
```

上面的物件是一個有效的物件實體寫法，物件實體中的屬性「可以」被引號包起來（單雙引號都可），但是在 JSON 中的屬性「一定」要被引號包起來。所以一個有效的 JSON 格式就是一個有效的物件實體，但不是所有的物件實體語法在 JSON 格式都是有效的。

##### JavaScript 內建轉換 JSON 函數

+ `JSON.stringify()`：將一個物件實體轉換成 JSON 字串。

+ `JSON.parse()`：將一個 JSON 字串轉換成物件實體。

### 函數就是物件

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%87%BD%E6%95%B8%E5%B0%B1%E6%98%AF%E7%89%A9%E4%BB%B6.png?raw=true" alt="Image for post" style="zoom:80%;" />

函數可以有屬性和方法，為什麼？因為它就只是個物件！所以它可以連結到純值、連結到物件和連結到其他函數。

在 JavaScript 函數物件有一些隱藏版的特殊屬性，其中有兩個重要的：

+ 第一個是名稱，JavaScript 的函數不一定要有名稱，一個函數可以是匿名的，但它可以有名字。

+ 第二個重要屬性為「程式屬性」（code property），你寫的程式碼會成為函數物件的特殊屬性，你寫的程式碼並非就是函數本身。這個函數是有其他屬性的物件，你寫的程式碼只是其中一種屬性，是你加上去的，這個屬性特別的是，它是可以呼叫的。

範例：

```js
function greet () {
    console.log('hi');
}
greet.language = 'english'
console.log(greet.language)
//english
```

我替 greet 函數新增了一個屬性，因為函數就是物件！

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%87%BD%E6%95%B8%E5%B0%B1%E6%98%AF%E7%89%A9%E4%BB%B62.png?raw=true" style="zoom:67%;" />

當這個 greet 函數被創造，這個函數物件會被放進記憶體，這個情況中，是全域物件，它有名稱，它的名稱是 greet 因為我幫這函數命名的，然後有程式屬性，包含了我寫的程式碼，你可以想像函數只是程式碼的容器。如果我用括號呼叫 greet，這會呼叫函數，讓它執行，讓執行環境被創造。

#### **補充**

> 什麼是一級函數？

```js
//一級函數定義
First class functions:
Everything you can do with other types you can do with funtions.
```

你可以對別的型別，如物件、字串、數值、布林做的事，你都可以對函數做，你可以指派一個變數的值為函數，你也可以將函數當做參數傳入另一個函數。

### 函數陳述式與函數表示式

+ 表示式又稱「表達式」或「運算式」，**會形成一個值**，而這個值不一定要儲存在某個變數。

```js
//表達式定義
Expression:
A unit of code that results in a value. It doesn't have to save to a variable.
```

下面的範例都是表達式：

```js
a = 3 //回傳 3
1 + 2 //回傳 3
b = {} //回傳一個空物件
```

+ 陳述式會做一些事情（你寫的程式碼）但不會回傳值。

以 if 陳述式為例：

```js
if (a === 3) {
    //do something
}
```

你不能再將一個 if 陳述式指定給一個變數， 因為 if 陳述式是陳述式，它沒有回傳值。

#### 函數運算式（重要）

通常會宣告一個變數，它的值是一個函數，記得函數就是物件，我們運用「等號」運算子將函數物件存入記憶體中，這個變數會指向它的記憶體位址。

範例：

```js
var anonymousGreet = function () {
    console.log('hi')
}
anonymousGreet()
```

這個例子中，這個函數物件被放到記憶體中並且指向 `anonymousGreet` 這個變數的位址，要注意的是 `anonymousGreet ` 不是函數的名稱，它是記憶體位址。這個函數沒有名稱，在 `function 關鍵字 ` 的括號前面沒有放任何東西而且也不需要，因為我已經知道指向物件位址的變數，所以我不需要一個名稱去參照它，因此這個函數又稱為「匿名函數」。匿名函數就是沒有名稱屬性的函數，但這並沒有關係，因為你可以利用指向物件位址的變數名稱來參照到它。

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%87%BD%E6%95%B8%E9%81%8B%E7%AE%97%E5%BC%8F.png?raw=true" style="zoom: 67%;" />

#### 函數表示式不會被提升

回顧一下提升：

> 當執行環境被創造時，在第一階段（ Creation Phase 創造階段），變數和函數（這邊指函數陳述式）會被設定放在記憶體裡面，這個步驟叫做「提升」（hoisting）。

從 JavaScript 的運作觀點來看，函數表示式只會提升變數而所有 JavaScript 的變數在一開始都會被設定為 `undefined` ，因此在執行到函數表示式之前（指派一個函數物件給一個變數），這個變數還是 `undefined` ，所以它不能拿來當作一個函數呼叫。

```js
anonymousGreet()
var anonymousGreet = function () {
    console.log('hi')
}
//Uncaught TypeError: anonymousGreet is not a function
```

### 觀念小叮嚀：傳值和傳參考

#### 傳值 (by value)

當我有一個變數 a 且其值為**純值**，當執行 `b = a`（將 a 的值指派給 b）的時候，變數 b 會指向一個新的記憶體位址且其值是拷貝 a 而來與 a 相同，所以當我改變 a，它不會對 b 有任何影響，因為 b 只是 a 的拷貝，它有自己的記憶體位址。

```js
let a = 3
let b = a
a = 2
console.log(a) //2
console.log(b) //3
```



<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%82%B3%E5%80%BC.png?raw=true" alt="Image for post" style="zoom: 80%;" />

傳值就是藉由拷貝一個值到另一個不同的記憶體位址。

#### 傳參考 (by reference)

當我有一個變數 a 且其值為**物件**（包含函數和陣列），當執行 `b = a`（將 a 的值指派給 b）的時候，變數 b 不會得到一個新的記憶體位址而是會指向 a 的記憶體位址，a 的值不會被拷貝，所以當我改變 a， b 也會被影響，因為它們指向同一個記憶體位址。

```js
let a = {greet: 'hi'}
let b = a
a.greet = 'hello'
console.log(a) //{greet: "hello"}
console.log(b) //{greet: "hello"}
```



<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%82%B3%E5%8F%83%E8%80%83.png?raw=true" alt="Image for post"  />

#### 小結（懶人包）

+ 純值是傳值，會拷貝值並放入一個新的記憶體位址。
+ 物件是傳參考，會指向相同的記憶體位址。

更多範例：

```js
const obj1 = {a: 1}
const obj2 = obj1
console.log(obj1 === obj2) // true 
//obj1 與 obj2 指向同一個記憶體位址所以結果為 true
```

```js
const obj1 = {a: 1}
const obj2 = obj1
obj2.a = 2
console.log(obj1) // {a: 2}
console.log(obj2) // {a: 2}

console.log(obj1 === obj2) // true 
//更改 obj2 物件也會改動到 obj1 物件，因為 obj1 與 obj2 指向同一個記憶體位址。
```

#### **補充**

```js
// mutate 就是 change 的高級用法而已，例如：mutate a object.
mutate:
To change something.
"Immutable" means it can't be changed.

equals operator sets up new memory space (new address)
等號運算子會設定一個新的記憶體空間
```

### 從 Object 的等號真正的理解變數儲存模型

> 此篇章為上完 [JavaScript 全攻略：克服 JS 的奇怪部分](https://www.udemy.com/course/javascriptjs/) 和看完 [從博物館寄物櫃理解變數儲存模型](https://hulitw.medium.com/variable-and-frontdesk-a53a0440af3c) 後經過個人領悟所整理的筆記，若有謬誤請不吝告知，非常感謝！

**當你想存取的變數是「純值」 Primitive values （數字、字串等等）的時候，變數裡面存的內容就真的是那個值。
但如果你想存物件的時候，變數裡面存的內容其實是記憶體位址。**

``` js
let a = [1, 2, 3]
let b = [1, 2, 3]
console.log(a === b)  // false

let c = 123
let d = 123
console.log(c === d)  // true

let obj1 = {a: 1}
let obj2 = {a: 1}
console.log(obj1 === obj2)  // false
```
a 跟 b 看起來長的一樣卻不相等是因為變數存取物件（包含函數和陣列）**內容**的方式是存取**記憶體位置**，a 與 b 指向不同記憶體位置，所以不相等，obj1 和 obj2 同理。然而，c 和 d 存取的內容是純值（這邊是數值），內容就是值本身，所以 `c === d` 的結果為 `true`，儘管它們的的記憶體位址不同。

### 物件、函數與「this」（重要）

當函數被呼叫時會創造新的執行環境，每個執行環境會有自己的**變數環境**，會有參照的**外部環境**，JavaScript 引擎還會給我們一個不曾宣告的變數**「this」**。在不同情況下，this 會根據函數是如何被呼叫的而指向不同的東西。

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%89%A9%E4%BB%B6%E5%87%BD%E6%95%B8%E8%88%87this.png?raw=true" alt="img" style="zoom:80%;" />

在全域環境下變數 this 會指向全域物件，在瀏覽器裡就是 window：

```js
console.log(this)
```

結果：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%89%A9%E4%BB%B6%E5%87%BD%E6%95%B8%E8%88%87this2.png?raw=true" style="zoom: 80%;" />

那麼呼叫函數的時候呢？

```js
function a () {
    console.log(this)
}
const b = function () {
    console.log(this)
}

a()
b()
```

結果：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%89%A9%E4%BB%B6%E5%87%BD%E6%95%B8%E8%88%87this3.png?raw=true" style="zoom:80%;" />

不管是函數陳述式還是函數表示式，它們的 this 都會指向全域物件，在瀏覽器裡就是 window。

那麼物件方法呢？

> 若一個物件的屬性的值為函數稱為「方法」。

```js
let c = {
    name: 'Leo',
    log: function () {
        console.log(this)
    }
}
c.log()
```

結果：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%89%A9%E4%BB%B6%E5%87%BD%E6%95%B8%E8%88%87this4.png?raw=true" style="zoom:80%;" />

物件方法中的 this 會指向包含這個方法的物件（變數 c）。這非常好用，我們可以取用到同一物件的方法或屬性。

例如，利用 this 改變包含這個方法的物件：

```js
let c = {
    name: 'Leo',
    log: function () {
        this.name = 'Jack'
    }
}
c.log()
console.log(c.name) // 結果為 Jack
```

這裡有一個需要注意的地方，如若在方法裡面又宣告一個函數，這個變數裡的 this 卻會指向全域物件 (window)，很多人認為這是 JavaScript 的 bug，沒有程式語言是完美的，它們都有奇怪的地方， JavaScript 也是：

```js
let c = {
    name: 'Leo',
    log: function () {
        let setName = function () {
            console.log(this)
        }
        setName()
    }
}
c.log()
```

結果：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%89%A9%E4%BB%B6%E5%87%BD%E6%95%B8%E8%88%87this5.png?raw=true" style="zoom: 80%;" />

那要怎麼改善這個問題呢？要確保在方法中宣告的函數裡面的 this 會指向物件本身，可以利用物件傳參考的特性，將變數 this 指派給另外一個變數（這邊取為變數 self）：

```js
let c = {
    name: 'Leo',
    log: function () {
        let self = this
        let setName = function () {
            console.log(self)
        }
        setName()
    }
}
c.log()
```

因為傳參考的特性，變數 self 會指向變數 this 的記憶體位址（方法中的 this 指向 c 物件本身），然而在 setName 這個子函數中沒有宣告變數 self，JavaScript 會從範圍鏈找，setName 子函數的外部環境便是 log 方法，所以能夠取用到變數 self。

結果：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%89%A9%E4%BB%B6%E5%87%BD%E6%95%B8%E8%88%87this6.png?raw=true" style="zoom: 80%;" />

### 觀念小叮嚀：陣列——任何東西的集合

```js
Arrays:
Collections of anything.
```

範例：

```js
let arr = [
    1,
    false,
    {
        name: 'Leo',
        address: '111 Main St.'
    },
    function (name) {
        let greet = 'Hello'
        console.log(`${greet} ${name}`);
    },
    'hey'
]
console.log(arr[3](arr[2].name))
/*output to be 
Hello Leo
```

###  Arguments 參數

```js
Arguments:
The parameters you pass to a function.
```

**參數 (arguments) 只是你傳入函數的變數的另一個稱呼而已，也可稱呼參數為 parameters，它們都是一樣的。**

`arguments` 本身也是一個特殊關鍵字，它代表了所有傳入到所呼叫的函數裡作為參數的值。它很像陣列 (array-like) 卻不是陣列，因為它只有一部份的陣列功能。範例：

```js
function greet (firstname, lastname) {
    console.log(arguments)
    console.log(arguments.length)
    console.log(arguments[0])
    console.log(arguments[1])
}
greet('Leo')
```

結果：

![](https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/Arguments%20%E5%8F%83%E6%95%B8.png?raw=true)

**隨著 ES6 其餘運算子的出現，`arguments` 會逐漸過時但它仍然存在，不過不是最好的方式了。**

### 危險小叮嚀：自動插入分號

如果你結束了一行程式碼，按下 Enter 鍵， 就出現一個 carriage return，它是個看不見的字元，但的確是個字元。在某些情況下，當語法解析器看到 carriage return 會自動幫你插入分號，所以你可以不自己打出來並不是因為不需要，而是因為 JavaScript 引擎會幫你在它認為該放的地方補上。

然而，這有時候可能會造成一些問題，例如當 JavaScript 引擎在關鍵字 `return` 後面發現 carrige return（就是按下 Enter 鍵換行意思）會自動替你加上分號也就是會變成 `return;` ，你會得到 `undefined` 的結果，因為你並沒有 return 任何東西。

```js
function getPerson() {
    return 
    {
        name: 'Leo'
    }
}
console.log(getPerson()) //undefined
```

要解決這問題，我們需要告訴語法解析器我們正在做什麼，因為語法解析器會隨著一個字母一個字母解析，所以我們在 `return` 後面用空格接一個大括號（把大括號移到跟 `return` 同一行），告訴語法解析器：「我們要開始用物件實體語法了喔！」之後它看見 carrige return 就不會自動插入分號了。

```js
function getPerson() {
    return {
        name: 'Leo'
    }
}
console.log(getPerson()) //{name: "Leo"}
```

你可能會注意到大括號幾乎都放在跟函數、for 迴圈還有 if 陳述句同一行，這不是每次都是必須的，這樣做是為了萬無一失，避免產生非預期的問題。

### 立即呼叫的函數表示式（IIFEs）

範例：

```js
let greet = function (name) {
    return 'Hello ' + name
}('Jack')

console.log(greet) //Hello Jack
```

其他範例（最常見的樣子，標準的 IIFE）：

```js
(function (name) {
    console.log('Hello '+ name);
}('Leo'))
//Hello Leo
```

**注意** 這個範例中的 IIFE 是被括號包起來，如果沒有括號包起來，當 JavaScript 引擎看到 `function 關鍵字` 作為程式碼的第一個字或是在分號之後，會認為它是一個函數陳述式，然而函數陳述式必須要有一個名稱，不可以是匿名的，否則會報錯。

### 框架小叮嚀：IIFEs 與安全程式碼

整體概念就是將程式碼都包在 IIFE 裡保證它不會和其他東西衝突（其他被 include 進來的程式碼），所以程式碼是安全的。因此在很多資源庫或框架中，如果你打開他們的原始碼，第一個看到的東西就是括號和函數。

範例：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E6%A1%86%E6%9E%B6%E5%B0%8F%E5%8F%AE%E5%9A%80%EF%BC%9AIIFEs%20%E8%88%87%E5%AE%89%E5%85%A8%E7%A8%8B%E5%BC%8F%E7%A2%BC.png?raw=true" style="zoom:67%;" />

一個 `greeting 變數` 存在於全域環境中，另一個在 IIFE 創造的執行環境中，它們兩個都存在，只是在不同的執行環境裡還有不同的記憶體位址。

### 瞭解閉包（一）

閉包結論：執行環境會將它的外部變數包住，那些它應該要參考到的變數，這個包住所有取用的變數的現象稱為閉包。閉包只是一個功能，確保當你執行函數時，可以取用到外部變數，它不在乎外部的執行環境已經運行完畢了沒。

範例：

```js
function greet (whattosay) {
    return function (name) {
        console.log(whattosay + ' ' + name)
    }
}
var sayHi = greet('Hi')
sayHi('Tony') // Hi Tony
```

執行完這段程式碼的 console.log 結果為 `Hi Tony`，看起來沒什麼問題，然而，不尋常的事情已經發生了。我們停下來思考一下。當 greet 函數被呼叫的時候，變數 whattosay 被創造在這個函數的執行環境裡面，然後這個函數就結束了，它會離開執行堆 (execution stack)。接著我們呼叫 sayHi 函數，sayHi 函數還是能夠參照到變數 whattosay 的值 ('Hi')，即便變數 whattosay 所存在於的 greet 函數的執行環境已經從執行堆離開。為什麼 sayHi 函數仍然知道變數 whattosay （也就是 'Hi'）是什麼呢？因為它是閉包！

解析：

當程式開始，全域執行被創造：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E9%96%89%E5%8C%85%EF%BC%88%E4%B8%80%EF%BC%891.png?raw=true" style="zoom:67%;" />

執行到這一行 `var sayHi = greet('Hi')` 時，會呼叫 greet函數，一個新的執行環境被創造然後進入執行堆，變數 whattosay ('Hi') 被傳入到這個變數環境， 接著 greet 函數會立刻創造一個新的函數並且回傳：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E9%96%89%E5%8C%85%EF%BC%88%E4%B8%80%EF%BC%892.png?raw=true" style="zoom:67%;" />

在回傳後，greet 函數的執行環境就會離開執行堆，然而**當執行環境結束時記憶體空間仍然存在**（不必要的記憶體會被 JavaScript 引擎的垃圾回收機制清除）：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E9%96%89%E5%8C%85%EF%BC%88%E4%B8%80%EF%BC%893.png?raw=true" style="zoom:67%;" />

現在我們回到全域執行環境中了，然後會呼叫 sayHi 指向的函數，一個匿名函數，它會創造新的執行環境，還有我們傳入的的變數 name ('Tony')：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E9%96%89%E5%8C%85%EF%BC%88%E4%B8%80%EF%BC%894.png?raw=true" style="zoom:67%;" />

當執行到 `console.log(whattosay + ' ' + name)` 這行的時候，JavaScript 引擎看到變數 whattosay 時，它會回到範圍鏈去參照外部環境。**即使 greet 函數的執行環境已經沒了，已經離開執行堆了， sayHi 的執行環境仍然可以參考到變數 whattosay，仍然可以參考到它的記憶體位址。** 

執行環境將它的外部變數包住，那些它應該要參考到的變數，這個包住所有取用的變數的現象稱為閉包 (closure)：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E9%96%89%E5%8C%85%EF%BC%88%E4%B8%80%EF%BC%895.png?raw=true" style="zoom:67%;" />

**我們不需要在意函數何時呼叫也不用擔心它的外部環境是否還在執行，JavaScript 引擎永遠會確保無論我在執行哪個函數，它都能取用到應該要取用到的變數。**閉包只是一個功能，確保當你執行函數時，它會正常運作，可以取用到外部變數，它不在乎外部的執行環境已經運行完畢了沒。

### 瞭解閉包（二）

範例：

```js
function buildFunctions () {
    var arr = []
    for (var i = 0; i < 3; i++) {
        arr.push(
            function () {
                console.log(i)
            }
        )
    }
    return arr
}

var fs = buildFunctions()
fs[0]() //3
fs[1]() //3
fs[2]() //3
```

fs 內的三個函數的 cosole.log 結果都為 3，是因為 `變數 i` 是由 var 宣告，var 是函數作用域，在 buildFunctions 函數內的 `變數 i` 只有一個，所以經過 for 迴圈後 `變數 i` 的值會一直被覆寫，跳離迴圈後 `i` 的值為 3。當呼叫 fs 內的函數時，會去參照外部環境的 `變數 i` 也就是  buildFunctions 函數內的 `變數 i`，值為 3，所以 fs 內的三個不同的函數都會參照到同一個值為 3 的 `i `，因次就會印出三次 3。

需要注意的是 `console.log(i)` 這行是在程式碼最後三行呼叫函數的時候才會執行，在 for 迴圈裡只是創造函數而已並還沒有呼叫。

記得呼叫 fs 內的函數的時候，儘管 buildFunctions 函數的執行環境已經離開執行堆，存在於 buildFunctions 函數的執行環境內的變數的記憶體位址仍然還在，所以還是能夠參照到，因為閉包的關係：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E9%96%89%E5%8C%85%EF%BC%88%E4%BA%8C%EF%BC%891.png?raw=true" style="zoom: 50%;" />

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E9%96%89%E5%8C%85%EF%BC%88%E4%BA%8C%EF%BC%892.png?raw=true" alt="Image for post" style="zoom: 50%;" />

那麼要如何才能讓依序呼叫 fs 內的函數的 console.log 結果為 `0 1 2` 而不是 `3 3 3`？

**第一種方式，方便快速也是比較建議的方法，使用 ES6 的 let 來宣告 `變數 i` ：**

```js
//差別只有在 for 迴圈括號中的 var 改成 let
function buildFunctions () {
    var arr = []
    for (let i = 0; i < 3; i++) {
        arr.push(
            function () {
                console.log(i)
            }
        )
    }
    return arr
}

var fs = buildFunctions()
fs[0]() //0
fs[1]() //1
fs[2]() //2
```

因為 let 是區塊作用域 Block scope（`{}` 大括號範圍內），所以每一次 for 迴圈宣告的 `變數 i` 都是新的獨立變數，它的值不會被覆寫。你可以將 for 迴圈的程式碼想成這以下這樣：

```js
var arr = []
{ // 塊級作用域
  let i = 0
  if (i < 3) {
      arr.push(
            function () {
                console.log(i)
            }
        )
  }
}
{ // 塊級作用域
  let i = 1
  if (i < 3) {
      arr.push(
            function () {
                console.log(i)
            }
        )
  }
}
//...略
arr[0]() //0
arr[1]() //1
arr[2]() //2
```

更詳細精確的 for 迴圈執行模擬可參考 [Day 05: ES6篇 - let與const](https://ithelp.ithome.com.tw/articles/10185142)。

> 本小節參考資料： [阮一峰 let 和 const 命令](https://es6.ruanyifeng.com/#docs/let) ｜ [從 V8 bytecode 探討 let 與 var 的效能問題](https://blog.huli.tw/2020/02/20/let-vs-var-bytecode/)

第二種方式，為了要保存不同的 `變數 i` 可使用 IIFE （立即呼叫函數）包住：

```js
function buildFunctions() {
    var arr = []
    for (var i = 0; i < 3; i++) {
        arr.push(
            (function (j) {
                return function () {
                    console.log(j)
                }
            }(i))
        )
    }
    return arr
}

var fs = buildFunctions()
fs[0]() //0
fs[1]() //1
fs[2]() //2
```

每當迴圈執行時，都會有一個立即呼叫的函數 (IIFE)，它會創造個別的執行環境，保存當下不同的 `i` 的值（也就是 `變數 j`）。因為閉包的關係，所有的 `j` 都會好好待在分別不同的執行環境，所以當要呼叫被回傳的匿名函數的時，它能參照到對應的 `外部變數 j` 而不是跑到最外層去找只有一個且會被覆寫的 `i` 。

### 框架小叮嚀：Function Factories

瞭解閉包的特性後，我們能將程式碼寫的易讀性更高。

以下程式碼範例來自章節 *框架小叮嚀：重載函式* ：

```js
function greet (firstname, lastname, language) {
    language = language || 'en';

    if (language === 'en') {
        console.log('Hello ' + firstname + ' ' + lastname);
    } else if (language === 'es') {
        console.log('Hola ' + firstname + ' ' + lastname);
    }
}

greet('John', 'Doe', 'en') //Hello John Doe
greet('John', 'Doe', 'es') //Hola John Doe
```

我們可以將上面的程式碼改寫成 factory 的模式（可以將 factory 想像成一個工廠，它會回傳幫我們做事的函數），不需要每次都傳入一樣的參數，我們可以回傳（創造）新的函數，用閉包製造預設的參數：

```js
function makeGreeting (language) {
    return function (firstname, lastname) {
        if (language === 'en') {
            console.log('Hello ' + firstname + ' ' + lastname)
        }

        if (language === 'es') {
            console.log('Hola ' + firstname + ' ' + lastname)
        }
    }
}

var greetEnglish = makeGreeting('en')
var greetSpanish = makeGreeting('es')

greetEnglish('John', 'Doe') //Hello John Doe
greetSpanish('John', 'Doe') //Hola John Doe
```

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E6%A1%86%E6%9E%B6%E5%B0%8F%E5%8F%AE%E5%9A%80%EF%BC%9AFunction%20Factories.png?raw=true" style="zoom: 67%;" />

### 閉包與回呼 (callback)

JavaScirpt 內建的 `setTimeOut()` 與監聽事件都是非同步事件，它們都會進入事件佇列 (Event Queue)，當執行堆清空後（執行環境都結束了）才會開始跑事件佇列，所以其實也會運用到了閉包的特性：

```js
function sayHiLater () {
    const greeting = 'Hi!'
    setTimeout(function () {
        console.log(greeting)
    }, 3000)
}
sayHiLater() //3秒後印出 Hi!
```

當執行到 `setTimeOut()` 裡的 ` console.log(greeting)` 時，還是能夠參照到 `外部變數 greeting` ，儘管 `sayHiLater()` 的執行環境已經結束。

### call()、apply() 與 bind()

所有函數都可以取用 `call()`、`apply()` 與 `bind()`這三種方法：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/call()%E3%80%81apply()%20%E8%88%87%20bind().png?raw=true" style="zoom:67%;" />

+ `bind()` 方法會回傳原函數的拷貝，在 `bind()` 被呼叫時，傳入 `bind()` 的第一個參數就會被指定為這個新函數的 `this` ，傳入的剩下的參數也會作為新函數的參數。

語法：

```
function.bind(thisArg[, arg1[, arg2[, ...]]])
```

範例：

```js
const person = {
    firstname: 'John',
    lastname: 'Doe',
    getFullName: function () {
        const fullname = this.firstname + ' ' + this.lastname
        return fullname
    }
}
const logName = function () {
    console.log('Logged ' + this.getFullName())
}
// logName() ---> 如果在這邊直接呼叫 logName() 會報錯，因為 logName()中 this 會指向全域物件，並不是 person 物件
const logPersonName = logName.bind(person) //藉由 bind() 將 this 改變指向為 person 物件
logPersonName() //Logged John Doe
```

也可以直接這樣寫：

```js
const person = {
    firstname: 'John',
    lastname: 'Doe',
    getFullName: function () {
        const fullname = this.firstname + ' ' + this.lastname
        return fullname
    }
}
const logName = function () {
    console.log('Logged ' + this.getFullName())
}.bind(person)
logName() //Logged John Doe
```

##### `bind()` 相關：Function currying

```
function currying:
Creating a copy of a function but with some preset parameters.
建立一個函數的拷貝並設定預設的參數
```

範例：

```js
function multiply(a, b) {
    return a * b
}

//拷貝的函數的第一個參數會永遠是我輸入的 2
const multiplyByTwo = multiply.bind(this, 2)
console.log(multiplyByTwo(4)) //2*4 = 8
console.log(multiplyByTwo(5)) //2*5 = 10

//這邊將兩個參數都預設成 2，不管你怎樣再輸入參數呼叫，結果都是 2*2 = 4
const multiplyByTwoAndTwo = multiply.bind(this, 2, 2)
console.log(multiplyByTwoAndTwo(4)) //4
console.log(multiplyByTwoAndTwo()) //4
console.log(multiplyByTwoAndTwo(6,8)) //4
```

+ `call()` 不像 `bind()` 會回傳一個原函數的拷貝而是真的會呼叫它，傳入 `call()` 的第一個參數一樣可以改變 this 指向的物件，剩下的就是傳給函數的參數

範例：

```js
const person = {
    firstname: 'John',
    lastname: 'Doe',
    getFullName: function () {
        const fullname = this.firstname + ' ' + this.lastname
        return fullname
    }
}
const logName = function (lan1, lan2) {
    console.log('Logged ' + this.getFullName())
    console.log('arguments: ' + lan1 + ' ' + lan2);
}.call(person, 'en', 'es') //--> call() 會直接呼叫函數

//Logged John Doe
//arguments: en es
```

+ `apply()` 基本上與 `call()` 一模一樣，只是 `apply()` 接受完第一個參數後，必須接受一個陣列作為參數。

語法：

```js
function.apply(thisArg, [argsArray])
```

範例：

```js
const person = {
    firstname: 'John',
    lastname: 'Doe',
    getFullName: function () {
        const fullname = this.firstname + ' ' + this.lastname
        return fullname
    }
}
const person2 = {
    firstname: 'Jane',
    lastname: 'Doe'
}
console.log(person.getFullName.apply(person2)) //Jane Doe
console.log(person.getFullName.bind(person2)()) //Jane Doe
```

我們藉由使用 `call()`、`apply()` 或 `bind()` 來讓 person2 物件借用 person 物件中的 `getFullName()` 方法 ，這種行為稱為**「函數借用」(Function borrowing)**。

## **第 5 節：JavaScript 的物件導向與原型繼承**

```js
Inheritance:
one object gets access to the properties and methods of another object.
繼承：
一個物件取用另一個物件的屬性或方法。
```

### 瞭解原型

JavaScript 中的所有物件（包含函數）都有一個屬性為 `__proto__` ，這個屬性會參考到另一個物件，我們稱為 proto，proto 就是該物件的原型 (prototype)。

假設我們現在有一個 `物件 obj`，它有屬性 prop1，還會有一個名稱屬性為 `__proto__`，這個原型屬性會參照到 `物件 proto` ，而 `物件 proto` 就是 `物件 obj` 的原型，`物件 proto` 則有屬性 prop2。

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E5%8E%9F%E5%9E%8B.png?raw=true" style="zoom: 50%;" />

如果我們用「點」運算子去存取 prop2 屬性：

```js
obj.prop2
```

在 `物件 obj` 並沒有屬性 prop2，它便會往原型（也就是 `物件 proto`）找，找到之後回傳。這看起來很像 prop2 在 `物件 obj` 上，但其實在我們的原型物件上。

然而，原型物件也可以指向另一個物件，每個物件可以有自己的原型。我們在假設 `物件 obj` 的原型 `物件 proto` 也有自己的原型並且有屬性 prop3：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E5%8E%9F%E5%9E%8B2.png?raw=true" style="zoom:50%;" />

我們用「點」運算子去存取 prop3 屬性：

```js
obj.prop3
```

`物件 obj` 上沒有找到屬性 prop3，所以往它的原型 `物件 proto`上找，`物件 proto` 也沒有找到prop3，所以它會繼續往另一個原型proto 找，最後找到 prop3 後回傳。

這個一直往原型尋找屬性的範圍又稱為「原型練 (Prototype chain)」：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E5%8E%9F%E5%9E%8B3.png?raw=true" style="zoom:50%;" />

原型屬性是隱藏起來的，所以我們並不用寫 `obj.proto.proto.prop3`，只要寫 `obj.prop3` 就好，JavaScript 引擎會搜尋原型鏈找屬性和方法。

有趣的是，如果我有另一個 `物件 obj2`，它可以指向同一個原型，所以物件可以分享一樣的原型：

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E7%9E%AD%E8%A7%A3%E5%8E%9F%E5%9E%8B4.png?raw=true" style="zoom:50%;" />

當我取用 `obj2.prop2` 的時候，會和 `obj.prop2` 一樣，它們有同樣的記憶體位址，共享一個屬性。prop2 不在 obj 和 obj2 上，這是當JavaScript 引擎到原型鏈上搜索時會指向同一個地方。

實際範例：

```js
const person = {
    firstname: 'Default',
    lastname: 'Default',
    getFullName: function () {
        return this.firstname + ' ' + this.lastname
    }
}

const john = {
    firstname: 'John',
    lastname: 'Doe'
}
//將 john 的原型設為 person ---> 注意！此作法只用於範例演示，請不要在實務上使用！
john.__proto__ =  person
console.log(john.getFullName()) //John Doe
console.log(john.firstname) //John

const jane = {
    firstname: 'Jane'
}
//將 jane 的原型設為 person ---> 注意！此作法只用於範例演示，請不要在實務上使用！
jane.__proto__ = person
console.log(jane.getFullName()) //Jane Default
```

1. `物件 john` 並沒有 `getFullName()` 這個方法，所以往原型練找，**`getFullName()`方法中的 this 會指向呼叫它的物件**，在這邊就是指 `物件 john` ，所以會印出 John Doe。

2. `物件 john` 已經有 firstname 屬性，所以不會再往原型練尋找，直接印出 John。

3. `物件 jane` 並沒有 `getFullName()` 這個方法，所以往原型練找，**`getFullName()`方法中的 this 會指向呼叫它的物件**，在這邊就是指 `物件 jane`。然而，jane 也沒有 lastname 屬性，所以再往原型 person 找，最後印出 Jane Default。

#### 補充

若我們使用 for in 迴圈將 `物件 john` 的屬性印出來，會發現原型鏈上的屬性都會被找到印出，這很有用：

```js
const person = {
    firstname: 'Default',
    lastname: 'Default',
    getFullName: function () {
        return this.firstname + ' ' + this.lastname
    }
}

const john = {
    firstname: 'John',
    lastname: 'Doe'
}
//將 john 的原型設為 person ---> 注意！此作法只用於範例演示，請不要在實務上使用！
john.__proto__ = person

for (const prop in john) {
    console.log(prop + ': ' + john[prop])
}
/*output to be:
firstname: John
lastname: Doe
getFullName: function () {
      return this.firstname + ' ' + this.lastname
    }
*/
```

但是如果只想找物件上的屬性而不想要去原型鏈上找的話，可以使用 `hasOwnProperty()` 方法，這是 JavaScript 基本物件的方法：

```js
for (const prop in john) {
    if (john.hasOwnProperty(prop)) {
        console.log(prop + ': ' + john[prop])
    }
}
/*
firstname: John
lastname: Doe
*/
```

### 所有東西都是物件（或純值）

部分的純值和物件都有原型，除了 JavaScript 的基本物件。

#### 補充

`null` 被歸類為物件（這是一個萬年 bug 但 JS 也沒有要修正它），它沒有原型，它是**原型鏈**（prototype chain）的終點。

## **第 6 節：建立物件**

### 函數建構子、「new」與 JavaScript 的歷史

JavaScript 的創造者 Brandon Eich 當時工作的公司為了要吸引 Java 的開發者而將 JavaScript 命名為 JavaScript。javaScript 聽起來與 Java 相似，但其實一點都不像 Java，這只是行銷手法。其中一個行銷元素就是 Java 的開發者習慣用關鍵字 new 去建立物件，所以    JavaScript 也跟進或者說模仿用關鍵字 new 來建立物件。

#### 函數建構子

```
Function constructors:
A normal function that is used to construct objects.
函數建構子：
一個用來建立物件的正常函數。
```

#### 關鍵字 new

new 是一個運算子，語法如下：

```js
new constructor[([arguments])]
```

建構子 (constructor) 可以是一個 class 或是 function。

描述：

new 運算子會先建立一個空物件，然後呼叫函數建構子，**此執行環境中的 this 會指向這個被建立的新物件**，如若這個函數建構子沒有回傳值或者回傳值不是物件的話，new 運算子就會回傳 this；如若這個函數建構子的回傳值是物件的話，就回傳該物件。

範例：

```js
function person (firstname, lastname) {
    this.firstname = firstname
    this.lastname = lastname
    console.log('This function is invoked.');
}

const john = new person('John', 'Doe') //This function is invoked.
console.log(john) //{firstname: "John", lastname: "Doe"}

const jane = new person('Jane', 'Doe') //This function is invoked.
console.log(jane) //{firstname: "Jane", lastname: "Doe"}
```

### 函數建構子與「.prototype」

JavaScript 所有的函數都有原型屬性 (`.prototype` property)，從它是空物件時就誕生，除非你將函數作為函數建構子來使用，不然它就只是待在那，永遠不會用到，不過一旦你用 new 運算子呼叫函數，它 (`.prototype` property) 就有意義了。

<img src="https://github.com/TsaiChihWei/learning-blog/blob/master/JavaScript%20Understanding%20the%20Weird%20Part/pics/%E5%87%BD%E6%95%B8%E5%BB%BA%E6%A7%8B%E5%AD%90%E8%88%87%E3%80%8C.prototype%E3%80%8D.png?raw=true" style="zoom:50%;" />

**注意！劃重點：函數建構子的原型屬性 (`.prototype` property) 就是你用函數建構子創造的物件的原型 (`.__proto__`)。**

範例：

```js
function person (firstname, lastname) {
    this.firstname = firstname,
    this.lastname = lastname
}

const john = new person('John', 'Doe')
const jane = new person("Jane", "Doe")
console.log(person.prototype === john.__proto__) //true
console.log(person.prototype === jane.__proto__) //true
```

#### 觀念釐清（重要）

函數的原型屬性 (`.prototype` property) 並不是函數本身的原型喔！ `.prototype` 屬性是函數作為建構子才有意義的屬性，而 `.__proto__` 屬性是每個物件（包含函數）都有且 `.__proto__` 的屬性值就是該物件的原型。

#### 小觀念：方法 (methods) 通常都寫在原型裡面

承上我們為物件 john 和 jane 添加一個 `getFullName()` 方法：

```js
function person (firstname, lastname) {
    this.firstname = firstname,
    this.lastname = lastname
}
person.prototype.getFullName = function () {
    return this.firstname + ' ' + this.lastname
}
const john = new person('John', 'Doe')
const jane = new person("Jane", "Doe")
console.log(john.getFullName()) //John Doe
console.log(jane.getFullName()) //Jane Doe
//jone & jane 並沒有 getFullName()方法，便會從原型(person.prototype)尋找
```

那為什麼不將方法跟屬性一樣寫在建構子裡面呢？像以下這樣：

```js
function person (firstname, lastname) {
    this.firstname = firstname,
    this.lastname = lastname,
    this.getFullName = function name() {
        return this.firstname + ' ' + this.lastname
    }
}
```

因為函數就是物件，它們會佔據記憶體空間，如果我要 new 1000 個 person，這樣就會有一千個 `getFullName()` 方法，但如果我是增在原型上就只會有一個。雖然我有一千個物件，但只有一個方法，就效能的觀點來看，將方法放在原型上比較好。

### 危險小叮嚀：「new 」與函數

此章節重點就是建議永遠把函數建構子的名稱的第一個字母設為大寫，這樣比較容易除錯。所以上面的範例都應該改成：

```js
function Person (firstname, lastname) {
    this.firstname = firstname,
    this.lastname = lastname
}
const john = new Person('John', 'Doe')
const jane = new Person("Jane", "Doe")
```

### 觀念小叮嚀：內建的函數建構子

> 這單元如果看不太懂，可以參考一下後面的補充。

JavaScript 內建的函數建構子的名稱也都是首字母為大寫，這此以 `String()` 為例：

```js
const a = new String(123) //數值 123 會被轉為字串 '123'
console.log(a)
//String {"123"}
```

new 運算子會建立一個**物件**，所以 a 是一個物件但包含著一個字串還有一些附加功能，附加功能指的是 a 的原型 (`.__proto__`) 就是建構子 `String()` 的原型屬性 (`.prototype`)，所以 a 能取用一些在原型上的屬性或方法，例如：`a.length` 或 `a.repeat(2)` 等等。（a 本身並沒有這些屬性和方法，所以會往原型找。）

所以我們能夠添加一些自定義的方法給所有同一個函數建構子所建立的物件使用，同樣以 `String()` 為例：

```js
const a = new String(123) //數值 123 會被轉為一個包含字串 '123'的物件
String.prototype.isLengthGreaterThan = function (x) {
    return this.length > x.length
}
//a 能夠取用到原型上的方法
console.log(a.isLengthGreaterThan('John')) //fasle
```

再以內建的 `Number()` 建構子為例：

```js
const b = new Number('-1') //字串 '-1' 會被轉為包含數值 -1的物件
console.log(b) //Number {-1}

//在所有 Number() 建構子所建立的物件的原型上添加自定義方法
Number.prototype.isPositive = function () {
    return this > 0
}
console.log(b.isPositive()) //false
```

#### 補充

當你宣告一個數值、字串或布林值時，你會發現其實它們的原型就是 JavaScipt 內建的建構子的原型屬性：

```js
const a = 3
const b = 'Leo'
const c = false
console.log(a.__proto__ === Number.prototype) //true
console.log(b.__proto__ === String.prototype) //true
console.log(c.__proto__ === Boolean.prototype) //true
```

或者當你不宣告直接使用時，它們也會是 JavaScipt 內建的建構子的原型屬性：

```js
console.log((3).__proto__ === Number.prototype) //true
console.log('Leo'.__proto__ === String.prototype) //true
console.log(false.__proto__ === Boolean.prototype) //true
```

> 備註：數值沒辦法直接使用「點」運算子，必須用括號包起來或者寫成浮點數的形式，數值 3 要寫成 3.0，不然會報錯。不過實作的時候通常都會宣告變數來用，所以應該會很少遇到這個情況。

所以這些純值其實能夠使用它們原型上的屬性或方法：

```js
Number.prototype.isPositive = function () {
    return this > 0
}
String.prototype.isLengthGreaterThan = function (x) {
    return this.length > x.length
}
const a = 3
const b = 'Leo'

console.log(a.isPositive()) //true
console.log((-5).isPositive()) //false

console.log(b.isLengthGreaterThan('John')) //false
console.log('Maggie'.isLengthGreaterThan('Cleo')) //true
```

### 危險小叮嚀：內建的函數建構子

```js
const a = 3
const b = new Number(3)
console.log(a == b) //true
console.log(a === b) //false
```

a 是純值，b 是物件，在用雙等號運算子比較時 b 會進行強制轉型成數值，所以相等；使用三等號運算子則不會強制轉型，a 就是純值，b 就是是物件，所以不相等。

**一般來說你應該使用實體語法而不要用函數建構子比較好，除非你知道自己在做什麼。**

#### 補充

> 課程作者推薦使用 [moment.js](https://momentjs.com/) 處理日期或時間相關程式碼，可參考六角學院的[介紹文章](https://ithelp.ithome.com.tw/articles/10208995)。

### 危險小叮嚀：陣列與 for in

```js
const arr = ['John', 'Jane', 'Jim']
Array.prototype.myCustomFeature = 'Cool'
for (prop in arr) {
    console.log(prop + ': ' + arr[prop])
}
/*
0: John
1: Jane
2: Jim
myCustomFeature: Cool
*/
```

當你用 for 迴圈遍歷一個陣列時，你會發現索引值 (0、1、2) 其實是屬性名稱，陣列中的值 ('John'、'Jane'、'Jim') 其實是屬性值，因為陣列就是物件。

要注意的是在 *第 5 節：JavaScript 的物件導向與原型繼承* 的 **瞭解原型**單元已經有提到過，若使用 for in 迴圈遍歷物件屬性，該物件的原型鏈中的屬性都會被找出來。

所以上面案例，我在陣列 arr 的原型 `.__proto__`（也就等於 Array() 建構子的原型屬性 `.prototype`）新增的自定義屬性 myCustomFeature 也會被印出來，所以使用標準的 for 迴圈是比較安全的。

```js
//...略
for (let prop = 0; prop < arr.length; prop++) {
    console.log(prop + ': ' + arr[prop])
}
```

### Object.create() 純粹的原型繼承

#### 另一種建立物件的方式

`Object.create()` 會建立一個**空物件**，它接收另一個物件作為參數且這個參數物件會是新建立的空物件的原型。

範例：

```js
const person = {
    firstname: 'Default',
    lastname:　'Default',
    greet: function () {
        return ('Hi ' + this.firstname)
    }
}

const john = Object.create(person)
console.log(john) //{}  ---> 空物件
console.log(john.__proto__ === person) //true

//在 john 上建立一個 firstname 屬性，便可以覆蓋原型上的 firstname
john.firstname = 'Leo'
console.log(john.greet()) //Hi Leo
```

#### 什麼是 Polyfill ？

```js
Pollyfill:
Code that adds a feature which the engine may lack.

//polyfill 是小段程式碼能夠讓舊有的瀏覽器補足新的 API，讓你能夠使用起來與現代瀏覽器無異。
```

如若瀏覽器沒有支援 `Object.create()` 的話可以自己寫一個 Polyfill：

```js
//polyfill
if (!Object.create) {
    Object.create = function (o) {
        if (arguments.length > 1) {
            throw new Error('Object.create implementation only accepts the first parameter.')
        }
        function F() {}
        F.prototype = o
        return new F()
    }
}
```

### ES6 的 Classes（類別）

> 課程講師在此章節沒有著墨太多，所以我查了一些資料作為補充。

**提前結論：類別 (Classes) 只是語法糖而已，它與函數建構子其實在做一模一樣的事情。**

定義：

Classes 實際上是一種特別的**函數**，就跟你可以平常使用函數一樣，你可以用類別表達式 ([class expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/class)) 和類別宣告式 ([class declarations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/class))。需要注意的是 Class 並不會提升 (Hoisting)，你必須先宣告你的類別，然後才能存取它，否則會報錯：

```js
const p = new Polygon() // ReferenceError

class Polygon {}
```

範例：

```js
class Person {
    constructor (firstname, lastname) {
        this.firstname = firstname
        this.lastname = lastname
    }
    
    //方法會被添加在 Person.prototype 裡面
    greet() {
        return 'Hi' + this.firstname
    }
}

const john = new Person('John', 'Doe')
console.log(john) //{firstname: "John", lastname: "Doe"}
console.log(john.__proto__ === Person.prototype) //true
```

+ Classes 的主體是大括號 `{}` 包含的部分，裡面只能有關鍵字 `constructor` 和其他方法 (methods)。

+ 關鍵字 `constructor` 也是一個方法，用來建立和初始化一個 class 物件，且一個 class 只能有一個 constructor 否則會拋出 SyntaxError。

#### 用 extends 建立子類別

關鍵字 `extends` 用來建立子類別：

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak () {
        return (this.name + ' makes a noise.')
    }
}

class Dog extends Animal {
    speak () {
        return (this.name + ' barks.')
    }
}

const dog1 = new Dog('Max')

console.log(Dog.__proto__ === Animal) //true
console.log(dog1.__proto__ === Dog.prototype) //true

console.log(dog1) //{name: "Max"}
console.log(dog1.speak()) //Max barks.
```

類別 Dog 繼承了類別 Animal，Dog 的原型 (`.__proto__`) 就是 Animal。

#### 關鍵字 super （此小節為個人補充非原課程講師之授課內容）

若子類別想要取用變數 this 必須在 `constructor` 中先呼叫 `super()`，否則會報錯。`super()` 也可以用來呼叫父類別的方法：

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak () {
        return (this.name + ' makes a noise.')
    }
}

class Dog extends Animal {
    constructor() {
        super('Max') //這裡的 super() 會呼叫父類別 Animal 的 constructor，個人理解為類似預設參數的概念
    }
    speak () {
        return (this.name + ' barks.')
    }
}
const dog1 = new Dog() //在宣告類別 Dog 時已呼叫 super() 並填入參數，所以這邊不用再填
console.log(dog1) //{name: "Max"}
```

##### 幫子類別 Dog 新增別的屬性：

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak () {
        return (this.name + ' makes a noise.')
    }
}

class Dog extends Animal {
    constructor(age) {
        super('Max') //預設參數
        this.age = age
    }
    speak () {
        return (this.name + ' barks.')
    }
}

const dog1 = new Dog(5) //新增的屬性必須在 new 時候填入

console.log(dog1) //{name: "Max", age: 5}
```

##### 在子類別中呼叫父類別的方法 (method)：

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak () {
        return (this.name + ' makes a noise.')
    }
}

class Dog extends Animal {
    speak () {
        return (this.name + ' barks.')
    }
    userFatherSpeak(){
        return super.speak() //利用 super. 取用父類方法
    }
}

const dog1 = new Dog('Max')

console.log(dog1.userFatherSpeak()) //Max makes a noise.
```

## 雜談

### 「typeof」、「instanceof」與搞清楚這是什麼

+ typeof 運算子會判別一個值得型態並會傳一個字串結果：

```js
console.log(typeof 3) //number
console.log(typeof '') //string
console.log(typeof true) //boolean

console.log(typeof {}) //object
console.log(typeof []) //object
console.log(typeof new Date()) //object
console.log(typeof function () {}) //function

console.log(typeof undefined) //undefined
console.log(typeof null) //object
```

若想判別某值是否為陣列型態可使用 `Array.isArray()`：

```js
console.log(Array.isArray([1, 2, 3])) //true
```

+ instanceof 運算子判別一個建構子的原型屬性 (`.prototype`) 是否在一個物件的原型鏈上並回傳一個布林值：

```js
//instanceof 就是在判別一個物件的原型 (.__proto__) 是否等
class Animal {
    constructor (name) {
        this.name = name
    }
    speak() {
        return this.name + ' made a noise.'
    }
}

const dog = new Animal()
console.log(dog instanceof Animal) //true
```

## 第八節以後待續

