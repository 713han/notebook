## Install Android Facebook SDK ##

#Eclipse#
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


