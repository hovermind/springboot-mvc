## Nexus Repository installation Tutorial (English)

1.Actuvate Command Line with Administrator's permission.

※the following website guides you how to get Administrator's permission on Windows OSes.
https://qiita.com/takuya0301/items/df6cde3bbaf9e13ef8f0


2.Move to the folder where "～\nexus-3.7.1-02-win64\nexus-3.7.1-02\bin\nexus.exe" exists. 

（cd ～\nexus-3.7.1-02-win64\nexus-3.7.1-02\bin\nexus.exe）

3.Insert following command into the Command Line.

nexus.exe /run

4.If the installation is successfully ended, following massage shall be desplayed. 

"Started Nexus Repository Manager 3.7.1-02"

5.Activate browser and put "localhost:8081" into the URL box and access to the web application on the localhost.

※Appendix
https://stackoverflow.com/questions/40545824/how-to-install-nexus-3-on-windows



## Nexus Repository　インストール手順 (日本語)

1.コマンドラインを管理者権限で起動します。

※Windows　OSでの管理者権限でのコマンドラインの開き方は下記のページ参照のこと。
https://qiita.com/takuya0301/items/df6cde3bbaf9e13ef8f0


2. "～\nexus-3.7.1-02-win64\nexus-3.7.1-02\bin\nexus.exe"ファイルのある場所へ移動します。
（cd ～\nexus-3.7.1-02-win64\nexus-3.7.1-02\bin\nexus.exe）


3.コマンドラインに下記のコマンドを入力します。

nexus.exe /run

4.インストールに成功した場合は、コマンドラインに下記のメッセージが出力されます。

"Started Nexus Repository Manager 3.7.1-02"

5.ブラウザを起動して、URLボックスに "localhost:8081" を入力してローカルホスト上のウェブアプリにアクセスします。

※参考資料
https://stackoverflow.com/questions/40545824/how-to-install-nexus-3-on-windows


## Maven setting
```
<settings>

  <mirrors>
    <mirror> <!--This sends everything else to /public --> 
    <id>nexus</id> 
    <mirrorOf>*</mirrorOf> 
    <url>http://localhost:8081/nexus/content/groups/public</url> 
    </mirror>
  </mirrors>
  
  <profiles>
  
    <profile>
    
      <id>nexus</id>
      <!--Enable snapshots for the built in central repo to direct -->
      <!--all requests to nexus via the mirror --> 
      <repositories>
      
        <repository>
          <id>central</id>
          <url>http://central</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
          <enabled>true</enabled></snapshots>
        </repository>
        
      </repositories>
      
      <pluginRepositories>
      
        <pluginRepository>
          <id>central</id>
          <url>http://central</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots> 
        </pluginRepository> 
        
      </pluginRepositories> 
      
    </profile> 
 
  </profiles> 
  
  <activeProfiles>
    <!--make the profile active all the time --> 
    <activeProfile>nexus</activeProfile> 
  </activeProfiles> 
  
</settings>
```

## project's pom.xml
```
<distributionManagement>
 
  <snapshotRepository> 
    <id>my-snapshots</id> 
    <name>My internal repository</name> 
    <url>http://localhost:8081/nexus/content/repositories/snapshots</url> 
  </snapshotRepository> 

  <repository> 
    <id>my-releases</id> 
    <name>My internal repository</name> 
    <url>http://localhost:8081/nexus/content/repositories/releases</url> 
  </repository> 

</distributionManagement>
```
