# [圖文滿版區塊 NO001](https://www.youtube.com/watch?v=rwTMBmnIHcY&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo)
![no001](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/NO001.png?raw=true)
## 本集重點：
+ **在使用 `Flex` 的預設情況下，元素會自動填滿次軸的寬或高，可藉由 `align-items` 來調整。**

+ [什麼是 linear-gradient？](https://www.w3cplus.com/css3/do-you-really-understand-css-linear-gradients.html)

範例：
``` scss
.banner {
    width: 100%;
    height: 400px;
    background-color: #ccc;
    background: linear-gradient(115deg, #7bf 50%, transparent 50%),
                url(https://fakeimg.pl/1200x400/eee/000) right center; 
                /*此範例為多重背景*/
}
```

> 快速回顧 CSS background 屬性：[CSS基本樣式-Background(上)](https://ithelp.ithome.com.tw/articles/10223887)｜[CSS基本樣式-Background(下)](https://ithelp.ithome.com.tw/articles/10224214)｜[快速演練](https://www.1keydata.com/css-tutorial/tw/background.php#attachment)


# [互動圖文卡片 hover 使用 NO002](https://www.youtube.com/watch?v=IocyLERRdko&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=2)
![no002](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no002.gif)
## 本集重點：

+ 若將一物件設定絕對定位 `position: absolute;`，此時這個物件將自己位於一個層，且尚未知道這一層的空間大小多少，可將其的 `top`、`bottom`、`left`﹑`right`均設為 0，便可抓取父層的寬高。
``` scss
/* .item 為 .txt 的父層 */
.item {
  position: relative;
}

.txt {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
```

> 此內容節錄並改寫自：[使用 absolute + margin auto 來達到CSS垂直置中效果](https://ithelp.ithome.com.tw/articles/10202245)

+ 藉由調整 `opacity` 透明度屬性達成文字浮現的效果：

``` scss
.txt {
  opacity: 0; /* 1.先將想要浮現的物件的 opacity 屬性設為 0 ，先讓它不能被看到*/
  transition: .6s; /* 3.可以加入 tranisition 動畫讓畫面圓滑一點 */
}

.item:hover .txt {  /* 2. .txt 的父層 .item 寫入 hover 狀態，此時 .txt 的 opacity 設回 1：當父層有滑鼠移入時，.txt 浮現出來*/
  opacity: 1; 
}
```
+ 由左而右的黃色線條動畫：
``` scss
/* 在你想要的地方添加偽元素::before 或 ::after （這邊是在 h2 標籤） */
/* 設定偽元素屬性 content: ''、display: block;（預設為 inline） 和定位、寬度等等 （這裡寬度先設為 0%） */
/* 利用 border-bottom 屬性製造一條線 */
.txt h2::after {
  content: '';
  display: block;
  width: 0%;
  border-bottom: 2px solid #ff0;
  transition: width .5s .5s; /* 這裡前面的 0.5s 是指持續時間，後面的 0.5s 是指延遲 0.5s */
}

/* :hover 偽類一樣寫在 .item 上，此時寬度設為 100% ：本來 h2::after 的寬度是 0% 當滑鼠移入父層時寬度變回 100% */
.item:hover h2::after {
  width: 100%;
}
```

# [人員介紹卡片 NO003](https://www.youtube.com/watch?v=2Qs0EuqJIYA&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=3)
![no003](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no003.png)
## 本集重點：
+ [如何用 CSS 的 `border` 屬性繪製三角形？](https://medium.com/ui-ux%E7%B7%B4%E5%8A%9F%E5%9D%8A/%E5%A6%82%E4%BD%95%E7%94%A8css%E7%B9%AA%E8%A3%BD%E4%B8%89%E8%A7%92%E5%BD%A2-71478539fb1b)

+ 上面圖片案例說明：
	1. 利用偽元素製造一個 `content`且設定 `border`，寬高為 0 和定位
	2. 將 `border-top` （紅色的三角形）的顏色改為 transparent （透明）
	3. `border-left` 和 `border-right` 色顏（綠色的三角形）改為白色（和字卡底色一樣），就能達到跟上圖右邊字卡一樣的效果
``` scss
.txt {
    padding: 20px;
    position: relative;
}

.txt::before {
    content: '';
    position: absolute;
    width: 0;
    height: 0;
    top: 0;
    left: 0;
    border-top: 50px solid transparent;
    border-left: 184px solid #fff;
    border-right: 184px solid #fff;
    transform: translateY(-100%);
}
```

# [交錯漂浮版 NO004](https://www.youtube.com/watch?v=aN7zFs_AT8s&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=4)
![no004](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no004.png)
## 本集重點：

+ 在父層寬度為 1200px 且 `display: flex;` 情況下，子層的 `.pic` 和 `.txt` 寬度設為 55% 會沒有作用，因為 flex-shink 預設為 1（預防爆版），本集因應版型需求改為 0。
> 回顧一下：[什麼是 flex-grow, flex-shink, flex-basis？](https://ithelp.ithome.com.tw/articles/10208741)
+ 能夠理解下面的 CSS 選取器嗎？
``` scss
.item > :first-child {
    margin-right: -10%;
}
```
> 回顧一下：[:first-child & :last-child 頭尾選取器](https://ithelp.ithome.com.tw/articles/10225779)

+ 若對一物件設定 `margin-right: -10%;` 其原本右邊的物件會往左邊挪動。
	+ 圖片案例右上粉紅區塊會往左疊在圖片之上
	+ 圖片案例右下圖片會往左疊在粉色區塊之上，但已針對粉色區塊設定 `z-index`，所以粉色區塊還是在最上面

# [超通用橫式版面 NO005](https://www.youtube.com/watch?v=-mmzaE6eLzY&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=5)
![no005](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no005.png)
## 本集重點：
+ [object-fit 屬性讓圖片好好裝在盒子裡](https://ithelp.ithome.com.tw/articles/10204973)

+ `margin-top: auto`：將剩餘空間都分配於 `margin-top`，所以可以得到靠下對齊

+ 使用 `flex-wrap: wrap` 的時候須仔細計算 margin 的數值，否則可能會無法達成想要的欄數　（物件們的寬度加上它們的 margin 可能會超過父層寬度所以會換行）


# [網頁頁尾版塊 NO006](https://www.youtube.com/watch?v=Y02yl_QQNv0&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=7)
![no006](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no006.png)
## 本集重點：

+ flex 偷懶用法：將父層設定 `display: flex;`，子層們的寬度設為 0 且 `flex-grow` 設為 1，子層們的寬度會被自動計算出一個平均值
+ 圖例最右側 `訂閱電子報` 欄位的 `<form>` 標籤設定 `margin: auto 0;` 達到垂直置中效果，但父層須先設定 `display: flex;` 且 `flex-direction: column` 去撐開高度。

# [導覽列 NO007](https://www.youtube.com/watch?v=7BydlKueTgY&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=8)
![no007](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no007.gif)
## 本集重點：

+ 從中間往兩側伸展的白色動畫線條：利用偽元素新增 `content: ''`、`position: absolute;`、`border-bottom`，但是 `left` 和 `right` 先設為 50%（從中間開始往兩側），當滑鼠移入時（hover）再設回 0
``` scss
.main-header .main-nav a::after {
  content: '';
  position: absolute;
  left: 50%;
  right: 50%;
  bottom: -5px;
  border-bottom: 1px solid #fff;
  -webkit-transition: .4s;
  transition: .4s;
}

.main-header .main-nav a:hover::after {
  left: 0;
  right: 0;
}
```

+ 滑鼠移入導覽列項目，項目向上飄動的效果：使用 `transform: translateY(負值)`，需要注意的是 `<a>` 標籤預設為 `display: inline;` 而無法作用，此時將父層設為 `display: flex;` 即可
``` scss
.main-header .main-nav a {
  text-decoration: none;
  color: #fff;
  padding: 5px 1em;
  margin: 0 5px;
  position: relative;
  transform: translateY(0);
  transition: .5s;
}

.main-header .main-nav a:hover {
          transform: translateY(-10px);
}
```

# [方塊酥版面 NO010](https://www.youtube.com/watch?v=Xhhzzc9YZW4&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=11)
![no010](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no010.gif)
## 本集重點：

+ Hover 從中心往四周放大的動畫效果：設定 `transform: scale(0)`，滑鼠移入時變回 `transform: scale(1)`，並加入 `transition`


# [破格式設計 NO011](https://www.youtube.com/watch?v=l-sQNXNrw3s&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=12)
![no011-3](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no011-3.png)
## 本集重點：

+ `line-height` 屬性設為與高度相同可以用在單行文字的垂直居中

+ `.icon` 設定 `margin: -75px auto 0;`：將 `margin-top` 數值設為負數可以有向上位移的效果

![no011-1](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no011-1.png?raw=true)

+ **注意！** 這邊的 `.icon` 的 `border` 是用偽元素額外做的，因為為了符合版型設計需要 `transform: rotate(-45deg);`，才不會讓裡面的 `<i>` 標籤也跟著旋轉
	+ 備註：影片案例中的這個偽元素是在 `box-sizing: content-box` 屬性下製作，為了要完整貼合 `.icon` 的內容
+ 圖案晃動動畫可用 `transform: rotate()` 完成
![no011-2](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no011-2.gif)



# [表格怎麼切 NO012](https://www.youtube.com/watch?v=zRREfvlLFIU&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=13)
![no012](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no012.png)
## 本集重點：

+ 使用 `<cation>` 作為表格的標題，`<cation>` 標籤必須放在 `<table>` 標籤的下方
	+ `<cation>` 標籤可用 `caption-side` CSS 屬性去控制它的位置
> "The `<caption>` tag must be inserted immediately after the `<table>` tag." from [w3schools.com](https://www.w3schools.com/tags/tag_caption.asp)

+ `<td>` 標籤中的 `scope` 屬性作用是什麼？
> 回顧一下：[認識表格細節，完善 `<table>` 的使用](https://medium.com/unalai/%E8%AA%8D%E8%AD%98%E8%A1%A8%E6%A0%BC%E7%B4%B0%E7%AF%80-%E5%AE%8C%E5%96%84-table-%E7%9A%84%E4%BD%BF%E7%94%A8-36b0952115f1)

# [側邊選單怎麼切 NO013](https://www.youtube.com/watch?v=yB3_LtwBiaE&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=14)
![no013](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no013.gif)
## 本集重點：

+ 項目列下面的白色底線是利用偽元素設定 `position; absolute;` 製作，因為可以控制線條寬度（不想讓線條滿版）

+ 善用 <input type="search"> 標籤

# [收合式側邊選單 NO014](https://www.youtube.com/watch?v=-KPbFhZmBPE&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=15)
![no014](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no014.gif)
## 本集重點：

+ 利用 `<input type="checkbox" id="自訂">` 和 `<label for="自訂">` 標籤製作一個假按鈕來控制收合側邊選單
	1. 先將側邊選單移出畫面外：`transform: translateX(100%);`
	2. 當標籤是被選取狀態時再讓選單移回來：`transform: translateX(0%);`
	3. 把 `<input>` 標籤作 `display: none;`　隱藏在畫面中

+ `<label>` 標籤已設定 `position: absolute;` 和寬高，但須設置 `top: 0;` 和 `bottom: 0;` 去抓取父層高度才能使用 `margin: auto;` 做到垂直居中的效果

+ `<label>` 標籤裡面的箭頭圖案向可用 `transform: scaleX(-1)` 達到沝平翻轉的效果

> 回顧一下：[CSS動畫- Transform](https://ithelp.ithome.com.tw/articles/10200713)

# [訂單進度條 NO016](https://www.youtube.com/watch?v=AhHDJcys5tc&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=17)
![no016](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no016.png)
## 本集重點：

+ 利用偽元素連結每個進度之間的白線

+ `box-shadow` 屬性也可以拿來製作 border：`box-shadow: h-shadow | v-shadow | blur | spread | color | inset;

# [登入表單 NO017](https://www.youtube.com/watch?v=G5MA36MboNw&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=18)
![no017](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no017.png)
## 本集重點：

+ backdrop-filter： `backdrop-filter` CSS 屬性可以讓你為一個元素後面區域添加圖形效果（如模糊或顏色偏移）。因為它適用於元素背後的所有元素，為了看到效果，必須使元素或其背景至少部分透明。
	+ 本集案例使用 `backdrop-filter` 中的 blur

# [訊息對話紀錄 NO018](https://www.youtube.com/watch?v=1tYhnmhdGNY&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=19)
![mo018](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no018.png)
## 本集重點：

+ 利用 CSS `border` 屬性製作三角形（對話框的箭頭）

+ `order` 屬性改變物件的順序（右側本地端的訊息在左，人物圖像在右）



# [時間軸 NO019](https://www.youtube.com/watch?v=AiR22hCQOGs&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=20&pbjreload=101)
![no019](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no019.png)
## 本集重點：

+ `<a>` 超連結標籤屬於 `inline` 屬性，設置 `border` 可能會無法達到預期效果，所以重設為 `display: block;`


+ `ul>li.box${text}*9` Emmet 快速指令：
``` html
<ul>
        <li class="box1">text</li>
        <li class="box2">text</li>
        <li class="box3">text</li>
        <li class="box4">text</li>
        <li class="box5">text</li>
        <li class="box6">text</li>
        <li class="box7">text</li>
        <li class="box8">text</li>
        <li class="box9">text</li>
</ul>
```

# [旋轉拼接方塊 NO020](https://www.youtube.com/watch?v=QKGhYoRHJnI&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=21)
![no020](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no20.gif)
## 本集重點：

+ CSS 選取器：`li:nth-child(n+4) {...}`，代表要選到的是包含第四個 `<li>` 開始及其後所有 `<li>`
	+ `li:nth-child(n+7) {...}`，代表要選到的是包含第七個 `<li>` 開始及其後所有 `<li>`
+ **注意!** CSS 選取器：`li::before:hover {...}`，**此寫法不可行！**
	+ 偽元素無法添加偽類，可改寫為 ``li:hover::before {...}``，但本質上與前者意義不同喔！

+ 利用為偽元素製造同 `<li>` 的方形再將其旋轉，不直接在 `<li>` 上直接旋轉的原因是裡面的字也會被旋轉， margin 的 X 和 Y 軸也會改變


# [不規則邊緣（球狀） NO021](https://www.youtube.com/watch?v=7SFuF9XE24s&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=23)
![no021](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no021.png?raw=true)
## 本集重點：

+ 本集利用 `box-shadow` 屬性製造影分身

+ **注意！** z-index only affects elements that have a position value other than static (the default).

# [文字排版-上集 NO022](https://www.youtube.com/watch?v=-2sRROXi2pI&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=24)
## 本集重點：

###NO022-1
![no022-1](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no022.png?raw=true)
+ `column-count: 2;`：將一個文字段落分為兩欄
+ `column-gap: 15px`：設定欄之間的距離

###NO022-2
![no022-2](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no022-2.png)
+ 文繞圖使用 `float` 屬性

###NO022-3
![no022-3](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no022-3.png?raw=true)
+ `p:first-letter` 選取段落的第一個字母，設定 font-size

+ 對 `p:first-letter` 使用 float


# [文字排版-中集 NO023](https://www.youtube.com/watch?v=YYHqbVVXIGM&list=PLqivELodHt3hxeuLX8PYaI8u1GcDaBoJo&index=25)
## 本集重點：

###NO023-1
![no023-1](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no023-1.png?raw=true)
+ `column-count`
+ `column-gap`

###NO023-2
![no023-2](https://github.com/TsaiChihWei/learning-blog/blob/master/goldfish/pics/no023-2.png?raw=true)

