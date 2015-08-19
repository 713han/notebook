#Android APP 上架筆記#

##Eclipse##
Step1. 專案右鍵 > Android Tools > Export Signed Application Package...  
Step2. Next  
Step3. Use existing keystore OR Create new keystore  
Step4. 依造指示填完所有資訊  
Step5. 進行壓縮校準(避免出現:您上傳的 APK 未經壓縮校準，請對您的 APK 執行壓縮校準工具，然後重新上傳。)
	
	zipalign -f -v 4 input.apk output.apk

[參考:http://marco.easyusing.com/2013/03/android-apk-apk.html](http://marco.easyusing.com/2013/03/android-apk-apk.html)







