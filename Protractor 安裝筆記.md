#安裝 Java 8 for Mac OSX

[http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

#安裝 Node.js v0.10.33

[https://nodejs.org/dist/v0.10.33/node-v0.10.33.pkg](https://nodejs.org/dist/v0.10.33/node-v0.10.33.pkg)

#安裝 Protractor@2.1.0

	sudo npm install -g protractor@2.1.0

#啟動 selenium
	
	cd /usr/local/lib/node_modules/protractor/bin
	sudo webdriver-manager update
	sudo webdriver-manager start

#建置ISCH測試專案

	git clone git@bitbucket.org:713han/protractordashboard.git	
	cd protractordashboard
	sudo npm install
	mkdir public
	cd public
	mkdir result
	mkdir screenshots

#啟動 Protractor

	cd protractordashboard
	protractor protractorConf.js --suite vod_1_1

#參考資料

[https://github.com/meistudioli/protractorEx](https://github.com/meistudioli/protractorEx)
	
	

