#ffmpeg 使用筆記#

	#mpg > mp4
	ffmpeg -i source.mpg target.mp4
	
	#mpg > mp4 720p
	ffmpeg -i source.mpg -s hd720 target.mp4
	
	#mpg > mp4 同時轉出多個格式
	ffmpeg -i source.mpg target_1080p.mp4 -s hd720 target_720p.mp4
	
	#一次轉出所有檔案(Ｗindows)
	for %%A IN (*.mpg) DO ffmpeg -i "%%A" "%%A.mp4"