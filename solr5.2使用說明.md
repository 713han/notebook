#Solr5.2 使用說明

#範例黏在一起版

	http://[SERVER]:[PORT]/solr/[CORE]/select?wt=json&indent=true&fl=*%2Cscore&rows=10&qf=[欄位1]^2+[欄位2]^1+[欄位3]^2+[欄位4]^1&facet=true&facet.field=[ex:Catalog_ids]&facet.limit=-1&facet.mincount=1&facet.range=[ex:Price]&facet.range.start=0&facet.range.end=200000&facet.range.gap=1000&q.op=AND&q=[KEYWORD]&defType=dismax&fq=%7B!frange+l%3D0.8%7Dquery(%7B!dismax+v%3D%24q%7D)

#範例拆開來比較好閱讀版

	http://[SERVER]:[PORT]/solr/[CORE]/select?wt=json&indent=true
	&fl=*%2Cscore
	&rows=10
	&facet=true&facet.field=[ex:Catalog_ids]&facet.limit=-1&facet.mincount=1
	&facet.range=[ex:Price]&facet.range.start=0&facet.range.end=200000&facet.range.gap=1000                        
	&q.op=AND
	&q=[KEYWORD]
	&defType=dismax 
	&qf=[欄位1]^2+[欄位2]^1+[欄位3]^2+[欄位4]
	&fq=%7B!frange+l%3D0.8%7Dquery(%7B!dismax+v%3D%24q%7D)

#查詢網址

	http://[SERVER]:[PORT]/solr/[CORE]/select?

#參數解釋

#一般查詢參數

`wt=json` 使用json格式  
`indent=true` 回應結果是否斷行(true 比較容易供人眼辨識閱讀)  
`fl=*,score` 列出所有欄位，同時顯示搜尋結果分數(score)  
`start=0` 由第幾筆開始顯示  
`rows=10` 每次顯示幾筆  
`q.op=AND` 查詢操作 and 或 or  
`q=[關鍵字]` 關鍵字 BJ4  

#搜尋結果分群依據分類 ex:熱血(10) 懸疑(5) 文藝(1) ...

`facet=true` 搜尋結果分群  
`facet.field=Catalog_ids` 搜尋結果分群依據 Catalog_ids 欄位來分群   
`facet.limit=-1` 分群數量 -1 = 取出所有分群  
`facet.mincount=1` 群組內數量最少須有多少結果才顯示 ex:參數為1時，全部可分為3群:熱血(0) 文藝(1) 懸疑(2)，則結果只會顯示 文藝(1) 懸疑(2)，若參數為0則3群皆會顯示  

#搜尋結果分群依據範圍 例:價格0\~1000(56) 1001\~2000(15) 2001\~3000(7) ...

`facet.range=Price` 搜尋結果分群依據 Price 欄位來分群  
`facet.range.start=0` 範圍最小值  
`facet.range.end=200000` 範圍最大值  
`facet.range.gap=1000` 每1000分一群0\~1000,1001\~2000,2001\~3000 ....  

#權重搜尋

`defType=dismax`   
`qf=欄位1^2 欄位2^1 欄位3^2 欄位4^1 ...`  
`fq={!frange l=0.8}query({!dismax v=$q})` 限制搜尋分數在0.8以上的才被列出來，防止結果太過發散  

#複合條件查詢

`fq=boolean_column:true|false` 指定boolean\_column條件為true或false  
`fq=string_id_column:1,2,3,4...` 查詢string\_id\_column有 1,2,3,4 ...  
`fq=unix_time:[* TO 1420070400]` 查詢unix\_time為 1420070400(2015-01-01) 以前的影片  
`fq=unix_time:[1420070400 TO *]` 查詢unix\_time為 1420070400(2015-01-01) 以後的影片  
`fq=unix_time:[1420070400 TO 1451520000]` 查詢unix\_time為 1420070400(2015-01-01) ~ 1451520000(2015-12-31) 的影片  

#排序

`sort=unix_time asc` 依據unix\_time 升冪排序  
`sort=unix_time desc` 依據unix\_time 降冪排序

#統計

`stats.field=[欄位1]&stats.field=[欄位2]&stats=true`

#Solr command

完整建立:  
`http://[SERVER]:[PORT]/solr/[CORE]/dataimport?command=full-import&clean=true&commit=true&optimize=true&wt=json&indent=true&entity=[ENTITY]&verbose=false&debug=false`

差異更新:  
`http://[SERVER]:[PORT]/solr/[CORE]/dataimport?command=delta-import&commit=true&optimize=true&wt=json&indent=true&entity=[ENTITY]&verbose=false&clean=false&debug=false`

用ID刪除:  
`http://[SERVER]:[PORT]/solr/[CORE]/update?stream.body=<delete><query>id:[ID]</query></delete>&commit=true&wt=json`

查詢狀態:  
`http://[SERVER]:[PORT]/solr/[CORE]/dataimport?command=status&indent=true&wt=json`

資料寫入:
`curl -u [USERNAME]:[PASSWORD] -X POST -H 'Content-Type: application/json' 'http://[SERVER]:[PORT]/solr/[CORE]/update?commit=true' --data-binary '[{[OBJECT1]},{[OBJECT2]}]'`

資料刪除:
`curl -u [USERNAME]:[PASSWORD] -X POST -H 'Content-Type: application/json' 'http://[SERVER]:[PORT]/solr/[CORE]/update?commit=true' --data-binary '{"delete":{"query":"[QUERY]"}}'`


