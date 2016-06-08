#Git#

	#顯示Git設定
	git config --list

	#初次設定
	git config --global user.name "[NAME]"
	git config --global user.email [EMAIL]

	#Clone Git 專案
	git clone [SSH網址 ex:git@github.com:713han/notebook.git]

	#檢視分支
	git branch

	#建立分支
	git branch [NAME]

	#檢查專案狀況
	git checkout

	#切換分支
	git checkout [NAME]

	#合併分支
	git merge [NAME]

	#增加遠端儲存庫
	git remote add [NAME ex:origin] [URL ex:git@github.com:713han/notebook.git]

	#檢視遠端儲存庫
	git remote show

	#檢視某個遠端除儲存庫詳細資訊
	git remote show [NAME]

	#在Client下合併遠端分支
		#1.切換回master
		git checkout master

		#2.檢查master有無異動並更新
		git pull

		#3.合併遠端分支
		git pull origin [遠端分支名稱]

		#4.更新遠端分支
		git push -u origin master

	#命令檢視遠端多餘的分支：EX:  branch_XXXX_XXXX new ( next fetch will store in remotes/origin)
	git remote show origin
	
	#從遠端倉庫更新信息，可以到./git/log/refs/remotes/origin目錄下查看分支信息。
	git fetch origin

	#上傳新建/更新分支到遠端儲存庫產生該分支
	git push -u origin [branch name]

	#上傳修改變動的地方到本地版次儲存庫
	git add .
	git commit -m "註解"
	
	#抓取遠端分支到本地
	git checkout origin/[branch name]
	git checkout -b [branch name]
	
	##若要看已經暫存起來的文件和上次提交時的快照之間的差異
	git diff --cached
	
	#關閉 diff 畫面
	q
	
	#文件脫離git的版本管理，但不是會刪除它.
	git rm --cached file
	
	#資料夾內脫離git的版本管理，但不是會刪除它.
	git rm -r --cached file
	
	#如果系統中有一些配置文件在服務器上做了配置修改,然後後續開發又新添加一些配置項的時候,在發布這個配置文件的時候,會發生代碼衝突:
	#error: Your local changes to the following files would be overwritten by merge:
	#		protected/config/main.php
	#Please, commit your changes or stash them before you can merge.
	#如果希望保留生產服務器上所做的改動,僅僅併入新配置項, 處理方法如下:
	git stash
	git pull
	git stash pop

		#然後可以使用Git diff -w +文件名 來確認代碼自動合併的情況.
		#反過來,如果希望用代碼庫中的文件完全覆蓋本地工作版本. 方法如下:
		git reset --hard
		git pull

		#其中git reset是針對版本,如果想針對文件回退本地修改,使用
		git checkout HEAD file/to/restore  


	#刪除最後一個commit(本地退版)
	git reset -hard HEAD~1

	#本地退版後強制讓遠端伺服器也跟著退版
	git push -f -u origin master
