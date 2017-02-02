#ffmpeg 使用筆記#

	#mpg > mp4
	ffmpeg -i source.mpg -movflags faststart target.mp4
	
	#mpg > mp4 720p
	ffmpeg -i source.mpg -s hd720 -movflags faststart target.mp4
	
	#mpg > mp4 同時轉出多個格式
	ffmpeg -i source.mpg -movflags faststart target_1080p.mp4 -s hd720 -movflags faststart target_720p.mp4
	
	#一次轉出所有檔案(Ｗindows)
	for %%A IN (*.mpg) DO ffmpeg -i "%%A" "%%A.mp4"
	
	#一次轉出所有檔案(Mac)
	for A in *.MOV;do ffmpeg -i "$A" -c copy -bsf:v h264_mp4toannexb -f mpegts "$A.ts";done

	#影片合併
	ffmpeg -i "concat:CAR_0149.MOV.ts|CAR_0150.MOV.ts|CAR_0151.MOV.ts ..." -c copy -bsf:a aac_adtstoasc output.mp4