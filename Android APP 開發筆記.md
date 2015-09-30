# Install Android Facebook SDK #

##Eclipse##
[參考:http://stackoverflow.com/questions/29379890/android-facebook-sdk-4-in-eclipse](http://stackoverflow.com/questions/29379890/android-facebook-sdk-4-in-eclipse)  

* Step1. [Download Facebook SDK](https://developers.facebook.com/resources/facebook-android-sdk-current.zip)  
* Step2. Import > Android > Existing Android Code Into Workspace > select [facebook]  
* Step3. import support.v4 library:   
library位置:[sdk]/extras/android/support/v4  
"Properties" > "Java Build Path" > "Libraries" > "Add External JARs", and choose android-support-v4.jar  
* Step4. import bolts library  
library下載:[link](http://mvnrepository.com/artifact/com.parse.bolts/bolts-android)
"Properties" > "Java Build Path" > "Libraries" > "Add External JARs", and choose bolts-android-1.2.*.jar 
* Step5. "Properties" > "Java Complier" > "Compiler compilance level" > "1.7"
* Step6. 將以建置好的FB SDK專案設定為 Libary專案  
"Properties" > "Android" > Libary區塊 > Is Library 打勾
* Step7. 將SDK加入要使用SDK的APP
"Properties" > "Android" > "Library" > "Add", 加入建置好的 facebook SDK 專案.

##IntelliJ##
[參考:https://developers.facebook.com/docs/android/getting-started/](https://developers.facebook.com/docs/android/getting-started/)

## 設定Facebook APP ##

設定FB的APP依造指示做就可以了，但需要準備以下資訊

1. Google Play Package Name:這跟要上架的APP所用的Package Name一致，設定的過程中FB會去Google Play檢查你是否上架，可以不必理會錯誤訊息，這項資訊可以在 AndroidManifest.xml 裡面找到
	
		<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    			  package="[你的Google Play Package Name]"
    			  android:versionCode="1"
    			  android:versionName="1.0" >

2. Class Name:APP主要啟動的Class
3. Key Hashes:可以設定多組，一個正式上線用的 一個DEBUG用的，你可以用以下指令找到你的Key Hashes，設定Keystore的方式請參考[Android APP 上架筆記](https://github.com/713han/notebook/blob/master/Android%20APP%20%E4%B8%8A%E6%9E%B6%E7%AD%86%E8%A8%98.md)  
正式上線用的 Key Hashes:
		
		keytool -exportcert -alias [key的別名] -keystore [keystore 路徑] | openssl sha1 -binary | openssl base64
Debug用的 Keystore 預設資訊[參考](http://stackoverflow.com/questions/18589694/i-have-never-set-any-passwords-to-my-keystore-and-alias-so-how-are-they-created):  
		
		Keystore name: "debug.keystore"
		Keystore password: "android"
		Key alias: "androiddebugkey"
		Key password: "android"
		CN: "CN=Android Debug,O=Android,C=US"
你可以在 eclipse > Window > Preferences > Android > Build 找到相關設定。  
用以下指令可以找到別名資訊預設密碼是 android
		
		keytool -list -keystore [debug keystore 路徑]		
在用以下指令可找到 debug keystore 的 Key Hashes:
		
		keytool -exportcert -alias [key的別名] -keystore [keystore 路徑] | openssl sha1 -binary | openssl base64

設定完後即可測試APP能否登入 Facebook 並取回資訊

相關資訊如下:  
[Getting Started Android SDK](https://developers.facebook.com/docs/android/getting-started)  
[Facebook Login for Android(v2.4)](https://developers.facebook.com/docs/facebook-login/android/v2.4)

		