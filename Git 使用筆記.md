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
