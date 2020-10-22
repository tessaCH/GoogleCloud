# GoogleCloud
`VM：Ubuntu 18.04 LTS`</
(SSH後於本機端設定)

### Tomcat 9 安裝及設定
參考文章: https://www.digitalocean.com/community/tutorials/install-tomcat-9-ubuntu-1804/

#### 新增名為`tomcat`的專用群組(基於安全考量，需設定一組非root權限的身份供Tomcat服務執行用)
```
sudo groupadd tomcat
```
#### 新增名為`tomcat`的使用者，權限目錄為`/opt/tomcat`，設定shell為`/bin/false`(禁止登錄該帳戶)
```
sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
```

##### 至Tomcat官網複製Tomcat 9的tar.gz載點連結 `https://tomcat.apache.org/download-90.cgi`->Full documentation <a href="https://downloads.apache.org/tomcat/tomcat-9/v9.0.39/bin/apache-tomcat-9.0.39-fulldocs.tar.gz">tar.gz</a> (pgp, sha512)
##### 下載至`/tmp`資料夾
```
cd /tmp
curl -O https://downloads.apache.org/tomcat/tomcat-9/v9.0.39/bin/apache-tomcat-9.0.39.tar.gz
```
##### 新增`/opt/tomcat`資料夾，並將上述檔案解壓縮至該資料夾
```
sudo mkdir /opt/tomcat
sudo tar xzvf apache-tomcat-*tar.gz -C /opt/tomcat --strip-components=1
```
##### 移至`/opt/tomcat`資料夾
```
cd /opt/tomcat
```
##### 將`/opt/tomcat`的所有權設定給`tomcat`群組
```
sudo chgrp -R tomcat /opt/tomcat
```
##### 將`conf`資料夾內容存取與執行權限設定給`tomcat`群組
```
sudo chmod -R g+r conf
sudo chmod g+x conf
```
##### 將`tomcat`用戶設定為`webapps`、`work`、`temp`和`logs`資料夾的擁有者
```
sudo chown -R tomcat webapps/ work/ temp/ logs/
```

#### 確認Java的安裝位置(`JAVA_HOME`設置路徑)
```
sudo update-java-alternatives -l
```
>java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
即此`JAVA_HOME`的位置為`/usr/lib/jvm/java-1.8.0-openjdk-amd64`

##### 在systemd中新增service檔，用於Tomcat服務
##### 於`/etc/systemd/system`資料夾中，以nano新增並開啟`tomcat.service`檔
```
sudo nano /etc/systemd/system/tomcat.service
```
將下方文字貼上(設定JAVA_HOME的路徑為上述路徑)</br>
按下Ctrl+X->Y->Enter以儲存並離開nano編輯器
```
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```
#### 重新載入systemd的daemon(系統服務)，以套用`tomcat.service`服務
```
sudo systemctl daemon-reload
```
#### 啟用Tomcat服務
`sudo systemctl start tomcat` 或 `sudo service tomcat start`
#### 確認Tomcat服務狀態
`sudo systemctl status tomcat` 或 `sudo service tomcat status`
#### 重啟Tomcat服務
`sudo systemctl restart tomcat` 或 `sudo service tomcat restart`
#### 停用Tomcat服務
`sudo systemctl stop tomcat` 或 `sudo service tomcat stop`

#### 修改Tomcat預設連接埠、支援中文檔名
`sudo nano /opt/tomcat/conf/server.xml`
在`<Service name="Catalina">`往下約十行，找到如下方文字。</br>
1.變更預設連接埠： 將`port="8080"`中的8080改為小於2的16次方的其他數字</br>
2.下載支援中文檔名： 在`redirectPort="8443"`後方加一行`URIEncoding="UTF-8"`
```
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```
#### 開啟Tomcat管理頁面Manager App
`sudo nano /opt/tomcat/conf/tomcat-users.xml`
在`<tomcat-users `往下方的第三塊註解文字中，有`<role rolename="tomcat"/>`等文字，去掉註解或在無註解的地方加入
```
  <role rolename="manager-gui" />
  <user username="使用者名稱" password="密碼" roles="manager-gui"/>
```
