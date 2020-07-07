## 為什要要使用 Git ?
### 記錄
Git 記錄修改，而不是檔案。資料夾對於 Git 來說只是路徑，並不會判別資料夾，除非在資料夾內新增檔案或者資料夾內的檔案有所變動。
### 回復
寫錯東西或者後悔變動的檔案可以回復之前的樣子。
### 合作
Git Branch 可以開拓分支進行平行運作而不互相干擾，最後還可以合併在一起。

## 常用指令
+ `git init`：進行版本控制。
+ `git status`：狀態檢查，看有無變動。
+ `git add`：指令把檔案從工作目錄移至暫存區。
	+ `git add .`：將所有變更加入至暫存區。
+ `git commit -m '<commit message>'`：指令把暫存區的內容移至儲存庫。
	+ `git commit --amend -m '<commit message>'`：與上一個 commit 合併。使用時機：Commit message 打錯或者不想一直新增 commit。
+ `git hist`：觀看 Commit 歷史。
	+ 註：使用此指令需先輸入 `git config --global alias.hist "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short"` (Aliase 設定)。
	+ 更多方便設定請見 [其它方便的設定](https://gitbook.tw/chapters/config/convenient-settings.html) 與 [11. Aliases](https://githowto.com/aliases)。
+ `git commit -m '<commit message>'`：指令把暫存區的內容移至儲存庫。
+ `git checkout <commit code>`：回到某個 commit 的版本。
+ `git clone`：[從伺服器上取得 Repository](https://gitbook.tw/chapters/github/clone-repository.html)。
+ `git merge <branch name>`：將某分支合併入 master。詳細使用方式可參閱 [合併分支](https://gitbook.tw/chapters/branch/merge-branch.html)與 [另一種合併方式 rebase](https://gitbook.tw/chapters/branch/merge-with-rebase.html)。
	+ `git merge <branch name> --no --ff`：詳細使用方式請見 [【狀況題】為什麼我的分支都沒有「小耳朵」？](https://gitbook.tw/chapters/branch/merge-commit.html)。
---
+ `git branch <branch name>`：建立分支。
+ `git branch -d <branch name>`：刪除分支。
+ `git branch -v`：列出分支。(包含 master)
+ `git checkout -b <branch name>`：等同 `git branch <branch name>` + `git checkout <branch name>`。
---
## *Git 狀況劇*
### 我 commit 了可是又不想 commit 了！
+ `git reset HEAD^ --soft`：僅移除 commit，保留新增的檔案，已修改的檔案保留修改。
	+ 新增的檔案與已修改的檔案皆存放在staging area 裡面。
	+ 註：`HEAD^` 代表回到 HEAD的上一個版本，`HEAD~2`則為回到上兩個版本。
	+ 註：`HEAD^` 可替換成任一版本的 commit code。
+ `git reset HEAD^ --mixed`：沒有設定參數時的預設值，移除 commit，保留新增的檔案，已修改的檔案保留修改。
	+ 新增的檔案與已修改的檔案皆 *不* 存放在staging area 裡面，需要再使用 `git add .` 指令加入至暫存區。
+ `git reset HEAD^ --hard`：移除 commit，移除新增的檔案，已修改的檔案回復到未修改前。

### 我用了git reset 指令，可是我又後悔了怎麼辦？
+ `git reflog`：能看到使用 `git reset` 指令前的 commit 歷史。
	1. 先使用 `git reflog` 指令。
	2. 再使用 `git reset <commit code> --hard` 回到過去版本。詳細說明可考[【狀況題】不小心使用 hard 模式 Reset 了某個 Commit，救得回來嗎？](https://gitbook.tw/chapters/using-git/restore-hard-reset-commit.html)。
### 我還沒 commit，但我改的東西我也不想要了！
+ `git checkout -- <file>`：讓該檔案回到上一個 commit 的狀態。
+ `git checkout -- .`：讓所有檔案回到上一個 commit 的狀態。
