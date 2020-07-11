# 數字或數學相關
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