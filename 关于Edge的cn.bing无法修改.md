**一般来说，首次进win前都会默认选择区域为中国，加上网络环境，所以首次打开edge后默认配置的bing的域名为cn.bing.com，以%s代替查询的URL是{bing:cnBaseURL}search?q=%s…，这是国内版的域名。因为这个默认的搜索引擎没办法删除，URL也没办法更改，但是有人有需求为new bing，也就是bing.com这个顶级域名搜索出来的东西，纵使清除cookie后使用代理重新打开edge，访问bing.com这个域名缓存cookie，但只要下一次还从地址栏走访问，默认引擎还是cn.bing.com域名，并且这次访问会覆盖你原来缓存的new bing的cookie。你也可以添加使用新的搜索引擎为bing的顶级域名，但是这样做地址栏搜索联想效果会失效，不完美。扩展也能解决这个问题，但太臃肿，强迫症不适合，所以也不太友好。**

---

#### 最好的解决办法就是让Edge默认的搜索引擎从最开始就是bing的顶级域名：

1. 修改 windows 的区域和 microsoft 账户的区域为非中国。
2. 删除Edge的 UserData 文件夹（清除浏览器缓存，历史记录，密码和cookie，做好同步），路径一般是:
    ```
    C:\Users\UserName\AppData\Local\Microsoft\Edge
    ```
3. 然后全局代理重新进入Edge，这时候bing的默认搜索引擎就会变为bing的顶级域名，也就是bing.com而不是cn.bing.com，观察URL也会变为{bing:BaseURL}search?q=%s…，搜索联想功能也正常。即使没有代理打开也不会篡改。

#### 这个方法非常完美，瑕疵就是没有网络环境的情况下搜索会同google一样无响应，不会自动跳转cn.bing，只能手动cn.bing或者baidu。但既然都这样操作了，相信也没有这个顾虑，瑕疵也就形同虚设了。