# 常用的 Command Line 指令
+ `pwd`：Print working directory，印出目前的工作路徑。
+ `ls`：List segment，列出目錄下的檔案。
	+ `ls -a`：列出目錄下所有的檔案，包含隱藏檔。
+ `cd`：Change directory，切換資料夾。
	+ `cd ..`：回上層資料夾。
	+ `cd ~`：切換至 /c/Users/使用者名稱 資料夾。
+ `touch <file>`：更改最後修改檔案時間，如果檔案不存在，會創建該檔案。
+ `mkdir <folder>`：Make directory，建立資料夾。
+ `rm`：Remove，刪除檔案。
+ `rmdir <folder>`：刪除資料夾指令。
+ `mv`：Move，移動文件或資料夾的位置，也可用來更改其名稱。
+ `cp`：Copy，複製檔案，e.g. `copy hello.txt hello2.txt`，為複製 hello.txt 檔案並命名為 hello2.txt。
+ `man`：Manual，教學手冊，e.g. `man ls`，為印出 `ls` 的使用方法。
+ `date`：印出現在的時間。
+ `cat`：Catenate，連結檔案 / 印出檔案內容。
+ `less`：分頁式印出檔案內容，可使用上下鍵滑動觀看。
+ `grep`：搜尋文件特定的關鍵字，e.g. `grep if jquery-plugin.js`，為在 jquery-plugin.js 中尋找 'if' 關鍵字。
+ `echo`：印出字串。
+ `clear`：清理畫面。

## 指令組合
+ `|`：Pipe，串接指令，把前面的輸出變成後面的輸入，e.g. `cat file.txt | grep hi`，為把 file.txt 印出的內容去尋找 'hi' 關鍵字。
+ `>`：Redirect，重新導向，e.g. `echo 123 > abc.txt`，為將 123 寫入 abc.txt 裡面。如果原本沒有這個文件，會順便建立了這個文件。