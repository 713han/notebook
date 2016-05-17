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
	git commit -m "del testDEBUG & edit git使用筆記"

	#刪除最後一個commit
	git reset -hard HEAD~1
