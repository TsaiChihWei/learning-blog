# 數字或數學相關內建函式
+ 將字串轉為數字
1. Number() 
``` js
let a = '123' //string
console.log(Number(a)) //number
```
2. parseInt(string, radix) 將一個字串轉換為 radix 進制的**整數**， radix 為介於 2 - 36 之間的數字。
``` js
console.log(parseInt('2', 10)) //2 		將 '2' 轉為 10 進位制
console.log(parseInt('2.888', 10)) //2 		若遇小數將無條件捨去
console.log(parseInt('123', 5)) //38		將 '123' 轉為 5 進位制

/*

註 1:
當第 1 個參數 string 不能轉換為數字或是 radix 不在 2 ~ 36 的範圍內會回傳 NaN 


註 2:
若想轉換為浮點數則使用 parseFloat()
console.log(parseFloat('2.888', 10)) ===> 2.88

*/

```
+ Math.ceil() 無條件進位 
``` js
let a = 5.3
console.log(Math.ceil(a)) //6
```

+ Math.floor() 無條件捨去 
``` js
let a = 5.3
console.log(Math.floor(a)) //5
```

+ Math.round() 四捨五入 
``` js
let a = 5.3
let b = 5.5
console.log(Math.round(a)) //5
console.log(Math.round(b)) //6
```

+ Math.abs() 絕對值 
``` js
let a = -5
let b = 5
console.log(Math.abs(a)) //5
console.log(Math.abs(b)) //5
```

+ Math.pow(base, exponent) 次方運算 
``` js
console.log(Math.pow(7, 3)) // 343 即 7 的 3 次方
console.log(Math.pow(2, 4)) // 16 即 2 的 4 次方
```

+ Math.sqrt() 開根號 
``` js
let a = 9
let b = 16
console.log(Math.sqrt(a)) //3
console.log(Math.sqrt(b)) //4
```

+ Math.ramdom() 隨機取數 
	+ 從 0 ~ 9 之間取任一小數 (包含 0 但不包含 1)
``` js
//examples
console.log(Math.random()) //0.3081682900388987
console.log(Math.random()) //0.18689516627171376
console.log(Math.random()) //0.08290022112004714 
//......

//可利用數學自定義隨機數範圍
//example
console.log(Math.floor(Math.random() * 10 + 1)) //隨機數範圍 1 ~ 10

```
+ toFixed.(digits) 取小數第幾位並四捨五入
``` js
console.log(123.456.toFixed(2)) //123.46
//如省略參數 預設值為 0
```
---
# 字串相關內建函式

+ toUpperCase() 將字串轉換為大寫
``` js
let a = 'abc'
console.log(toUpperCase(a)) //'ABC'
```

+ toLowerCase() 將字串轉換為小寫
``` js
let a = 'ABC'
console.log(toLowerCase(a)) //'abc'
```

+ indexOf() 回傳字串的第一個索引值
``` js
let a = 'Today is a good day.'
console.log(a.indexOf('a')) //3
console.log(a.indexOf('z')) //-1
//如找不到字串會回傳 -1
```

+ replace(substr, newSubstr) 替換指定字串
``` js
let a = 'Apples is round, and apples is juicy.'
console.log(a.replace('is', 'are')) //'Apples are round, and apples is juicy.' 只會替換首個指定字串
console.log(a.replace(/is/g, 'are')) 
//'Apples are round, and apples are juicy.' /g 代表 global 全域替換，將所有指定字串都替換

/*註: 

如要忽略大小寫可用 /i 參數 e.g., console.log(a.replace(/apples/gi, oranges))
expected output: 'oranges are round, and oranges are juicy.'

*/
```

+ trim() 去除字串前後的空白
``` js
let a = '      hello!     '
console.log(trim(a)) //'hello!'
```

+ split(separator, [limits]) 將字串拆解並回傳一個**陣列**
``` js
let a = 'Today!is!a!good!day.'
console.log(a.split('!')) // ["Today", "is", "a", "good", "day."]
console.log(a.split('!', 2)) // ["Today", "is"]
console.log(a.split('!', 3)) // ["Today", "is", "a"]
```
+ slice(bigin, [end]) 提取字串的某一個部分並回傳一個新字串
``` js
let a = 'hello~'
console.log(a.slice(3)) //'lo~'
console.log(a.slice(3, 5)) //'lo' 不包含 end
```

---
# 陣列相關內建函式

+ map(function) 原陣列的每一個元素經由回呼函式運算後所回傳一個新的陣列
``` js
let arr = [1, 2, 3]
function double (x) {
   return x*2
 }

console.log(arr.map(double)) // [2, 4, 6]
```

+ filter(function) 原陣列中通過回呼函式 (true) 檢驗之元素所回傳的新陣列
``` js
let arr = [1, 2, 3, 11, 12]
function isBig (x) {
   return x >= 10
 }

console.log(arr.filter(isBig)) // [11, 12]
// 1, 2, 3 代入 isBig function 之結果為 false 所以被 filter 掉
```

+ slice(begin, [end]) 擷取某陣列的部分並回傳一個新陣列
``` js
let arr = [1, 2, 3, 4, 5, 6]
console.log(arr.slice(1)) //[2, 3, 4, 5, 6]
console.log(arr.slice(1, 4)) //[2, 3, 4] 不包含 end
```

+ splice(start, [deleteCount], [item1], [item2].......])
	+ start 代表從哪個索引值開始更改陣列
	+ deleteCount 代表要刪除的元素數量
	+ item 為要添加的元素
	+ 可以藉由刪除既有元素並／或加入新元素來改變**原本**陣列的內容
	+ 會回傳一個被刪除的元素陣列。如果只有一個元素被刪除，依舊是回傳包含一個元素的陣列
	+ 倘若沒有元素被刪除，則會回傳空陣列
``` js
//從索引 0 的位置開始，刪除 2 個元素並插入「parrot」、「anemone」和「blue」
var myFish = ['angel', 'clown', 'trumpet', 'sturgeon'];
var removed = myFish.splice(0, 2, 'parrot', 'anemone', 'blue');

// myFish 為 ["parrot", "anemone", "blue", "trumpet", "sturgeon"] 
// removed 為 ["angel", "clown"]
```
以上範例來自 [Array.prototype.splice()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)。

+ sort([compareFunction])
	+ 對原陣列的所有元素進行排序(會更改原陣列)，並回傳此陣列。
	+ 若省略 compareFunction 排序順序會根據字串的 Unicode 編碼位置（code points）而定。
``` js
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months);
// expected output: Array ["Dec", "Feb", "Jan", "March"]

const array1 = [1, 30, 4, 21, 100000];
array1.sort();
console.log(array1);
// expected output: Array [1, 100000, 21, 30, 4]

//如是為了比較數字而不是字串，比較函式(compareFunction)可以僅僅利用 a 減 b。以下函式將會升冪排序陣列：
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
  return a - b;
});
console.log(numbers); // [1, 2, 3, 4, 5]

//降冪排序則用 b 減 a
numbers.sort(function(a, b) {
  return b - a;
});
console.log(numbers); // [5, 4, 3, 2, 1]
```
以上範例來自 [Array.prototype.sort()](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)。