# 
`VM：Ubuntu 18.04 LTS`</
(SSH後於本機端設定)

### 代管主機資料搬移(適用無主機權限、無原部署檔的土法鍊鋼作法)
> 原伺服器webapps內資料夾下載(如有中文顯示問題，需修改語系或
> 需注意如原程式中有Hard Code原站點路徑的部份，應進行改寫</br>
> 如Tomcat資料夾路徑為 "/opt/apache-tomcat/webapps/myApps/"</br>
> 原站點為 "http://localhost:8080/myApps/"
1. `request.getScheme()` => `http`</br>
2. `request.getServerName()` => `localhost`</br>
3. `request.getServerPort()` => `8080`</br>
4. `request.getContextPath()` => `/myApps`</br>
5. `request.getRealPath("/")` => `/opt/apache-tomcat/webapps/myApps/`</br>
e.g. 也可以用如`${pageContext.request.contextPath}`的方式取得4.的頁面路徑

### 
```
