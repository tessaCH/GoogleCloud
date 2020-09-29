# GoogleCloud
`VM：Ubuntu 18.04 LTS`</
(SSH後於本機端設定)

### Java 8 安裝及設定
參考文章: https://www.cloudbooklet.com/how-to-install-java-on-google-cloud-ubuntu-18-04-lts/

#### 更新安裝元件到目前版本
```
sudo apt update
```
#### 安裝OpenJDK版 Java 8
```
sudo apt install openjdk-8-jdk
```
##### 安裝完畢，確認已安裝Java版本
```
java -version
```
> openjdk version "1.8.0_265"</br>
> OpenJDK Runtime Environment (build 1.8.0_265-8u265-b01-0ubuntu2~18.04-b01)</br>
> OpenJDK 64-Bit Server VM (build 25.265-b01, mixed mode)</br>

##### 安裝完畢，確認已安裝Javac版本
```
javac -version
```
> javac 1.8.0_265

##### 確認Java版本及路徑
```
sudo update-alternatives --config java
```
> There is only one alternative in link group java (providing /usr/bin/java): </br>
> /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java</br>
> Nothing to configure.</br>
(OpenJDK 8 的路徑為 `/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java`)

#### 以nano編輯器開啟環境設定文件
```
sudo nano /etc/environment
```
在文件最下方加入此行(設定JAVA_HOME的路徑為上述OpenJDK的路徑)</br>
`JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java"`</br>
按下`Ctrl+X`->`Y`->`Enter`以儲存並離開nano編輯器

##### 確認環境設定中JAVA_HOME的設定值
```
source /etc/environment
echo $JAVA_HOME
```
> /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
