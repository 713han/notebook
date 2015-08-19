#安裝Java 8 form Oracle#

[參考:http://linuxpilot.com/ubuntu-java](http://linuxpilot.com/ubuntu-java)

	sudo add-apt-repository ppa:webupd8team/java
	sudo apt-get update
	sudo apt-get install oracle-java8-installer
	sudo apt-get install oracle-java8-set-default

使用 `java -version` 檢查訊息是否為 `java version "1.8.0_45"`


#安裝Solr 5.2(需要root權限)#
[參考:https://cwiki.apache.org/confluence/display/solr/Taking+Solr+to+Production](https://cwiki.apache.org/confluence/display/solr/Taking+Solr+to+Production)

	wget http://apache.stu.edu.tw/lucene/solr/5.2.0/solr-5.2.0.tgz

	tar xzf solr-5.2.0.tgz solr-5.2.0/bin/install_solr_service.sh --strip-components=2

	#sudo bash ./install_solr_service.sh solr-5.2.0.tgz -i /opt -d /var/solr -u solr -s solr -p 8983
	sudo bash ./install_solr_service.sh solr-5.2.0.tgz

	#For Demo
	#sudo mkdir /var/solr/data/isch
	#sudo chown -R solr:solr /var/solr
	
	#For Production
	sudo mkdir /var/solr/data/isch_videos
	sudo mkdir /var/solr/data/isch_programs
	sudo mkdir /var/solr/data/isch_content
	sudo mkdir /var/solr/data/isch_users
	sudo chown -R solr:solr /var/solr
	sudo chown -R solr:solr /storage/solr


##修改環境設定 ex:記憶體用量##
	sudo vim /var/solr/solr.in.sh

SOLR_HEAP="512m" => SOLR_HEAP="1024m"  
SOLR_TIMEZONE="UTC" => SOLR_TIMEZONE="GMT+8"

##PATCH##
	cd /home/hans_huang/solr_patch/
	sudo cp *.js  /opt/solr/server/solr-webapp/webapp/js/lib/


##資料匯入範本##
	sudo cp -a /opt/solr/example/example-DIH/solr/solr/conf /var/solr/data/isch_videos/conf
	sudo cp -a /opt/solr/example/example-DIH/solr/solr/conf /var/solr/data/isch_programs/conf
	sudo cp -a /opt/solr/example/example-DIH/solr/solr/conf /var/solr/data/isch_content/conf
	sudo cp -a /opt/solr/example/example-DIH/solr/solr/conf /var/solr/data/isch_users/conf


##安裝中文分詞套件##
	cd /home/hans_huang/solr_lib/
	sudo cp *.*  /opt/solr/server/solr-webapp/webapp/WEB-INF/lib/


##更新設定檔##
	cd /home/hans_huang/solr_conf/isch_videos
	sudo cp *.*  /var/solr/data/isch_videos/conf/

	cd /home/hans_huang/solr_conf/isch_programs
	sudo cp *.*  /var/solr/data/isch_programs/conf/

	cd /home/hans_huang/solr_conf/isch_content
	sudo cp *.*  /var/solr/data/isch_content/conf/

	cd /home/hans_huang/solr_conf/isch_users
	sudo cp *.*  /var/solr/data/isch_users/conf/


##更新安全性設定##
	cd /home/hans_huang/solr_security
	sudo cp jetty.xml webdefault.xml realm.properties /opt/solr/server/etc/


##設定修改:data-config.xml (sample)##
	<dataConfig>
		<dataSource type="JdbcDataSource" 
					driver="com.mysql.jdbc.Driver"
					url="jdbc:mysql://[IP]:3306/[Database]" 
					user="[帳號]" 
					password="[密碼]"/>
		<document>
			<entity name="[ENTITY]"  
					pk="id"
					query="select id,context from [table]"
					deltaImportQuery="SELECT id,context from [table] WHERE id='${dataimporter.delta.id}'"
					deltaQuery="SELECT id FROM [table] WHERE updated_at > UNIX_TIMESTAMP('${dataimporter.last_index_time}')" 
					deletedPkQuery="SELECT id FROM [table] WHERE updated_at > UNIX_TIMESTAMP('${dataimporter.last_index_time}')" >
				
				<field column="id" name="id"/>
				<field column="context" name="context"/>       
			</entity>
		</document>
	</dataConfig>


##設定修改:schema.xml##
根據 data-config.xml 匯入的欄位修改 schema.xml  
另外加入中文分詞分析器設定:

	<!-- mmseg4j-->
	<fieldtype name="textComplex" class="solr.TextField" positionIncrementGap="100">
		<analyzer>
			<tokenizer class="com.chenlb.mmseg4j.solr.MMSegTokenizerFactory" mode="complex" dicPath="dic"/>
		</analyzer>
	</fieldtype>
	
	<fieldtype name="textMaxWord" class="solr.TextField" positionIncrementGap="100">
		<analyzer>
			<tokenizer class="com.chenlb.mmseg4j.solr.MMSegTokenizerFactory" mode="max-word" />
		</analyzer>
	</fieldtype>
	
	<fieldtype name="textSimple" class="solr.TextField" positionIncrementGap="100">
		<analyzer>
			<tokenizer class="com.chenlb.mmseg4j.solr.MMSegTokenizerFactory" mode="simple" />
		</analyzer>
	</fieldtype>
	<!-- mmseg4j-->	

加入自訂分析器：

	<fieldType name="actorsDelimited" class="solr.TextField">
		<analyzer>
			<tokenizer class="solr.PatternTokenizerFactory" pattern="[、，；;,\s]\s*" />
			<!--<charFilter class="solr.PatternReplaceCharFilterFactory" pattern="[\s]" replacement="_"/>-->		
			<filter class="solr.SynonymFilterFactory" synonyms="jav_actors_synonyms_rev3.txt" ignoreCase="true" expand="true"/>
			<filter class="solr.LowerCaseFilterFactory"/>
		</analyzer>
	</fieldType>
	<fieldType name="idDelimited" class="solr.TextField">
		<analyzer>
			<tokenizer class="solr.PatternTokenizerFactory" pattern="[,]\s*" />
		</analyzer>
	</fieldType>


##設定修改:solrconfig.xml##

`<requestHandler name="/dataimport" class="solr.DataImportHandler">`  
修改為  
`<requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">`

`<str name="config">solr-data-config.xml</str>`  
修改為  
`<str name="config">data-config.xml</str>`

`<str name="df">text</str>`  
修改為  
`<str name="df">[預設查詢欄位 參考 schema.xml]</str>`


##安全性修改:webdefault.xml##
加入:  

	<security-constraint>
		<web-resource-collection>
			<web-resource-name>Solr</web-resource-name>
			<url-pattern>/*</url-pattern>
		</web-resource-collection>
		<auth-constraint>
			<role-name>search-role</role-name>
		</auth-constraint>
	</security-constraint>
	
	<login-config>
		<auth-method>BASIC</auth-method>
		<realm-name>Solr</realm-name>
	</login-config>


##安全性修改:jetty.xml##
加入:

	<Call name="addBean">
		<Arg>
			<New class="org.eclipse.jetty.security.HashLoginService">
				<Set name="name">Solr</Set>
				<Set name="config"><SystemProperty  name="jetty.home" default="."/>/etc/realm.properties</Set>
				<Set name="refreshInterval">0</Set>
			</New>
		</Arg>
	</Call>

##安全性修改:realm.properties##
帳號:密碼,角色名稱  

	username:password,search-role


#開啟防火牆##
	sudo ufw allow in 8983
	sudo ufw allow out 8983


##TEST##
	http://[SERVER]:8983/solr/
	http://[SERVER]:8983/solr/[CORE]/select?q=[KEYWORD]&wt=json&indent=true








