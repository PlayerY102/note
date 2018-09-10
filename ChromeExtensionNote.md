### ChromeExtension
* 开发调试
    * Chrome插件没有严格的项目结构要求，要保证本目录有一个mainfest.json即可  
    勾选开发者模式既可以文件夹形式直接加载插件，否则只能安装.crx格式文件。  
    Chrome要求插件必须从它的chrome应用商店安装，所以其实我们可以把crx文件解压，然后通过开发者模式直接加载。  
    开发中，代码有任何改动都必须重新加载插件，只要在插件管理页按下ctr+R即可。
* 核心介绍
    * mainfest.json  
    这是一个chrome插件最重要也是必不可少的文件，用来配置所有和插件相关的配置，必须放在根目录。  
    其中，mainfest_version,name,version必不可少的，description和icons是推荐的。  
    
    * content-scripts  
    chrome插件中向页面注入脚本的一种形式（可以包括css),借助content-scripts我们可以实现通过配置的方式轻松向指定页面注入js和css  
    content-scripts和原始页面共享DOM,但是不共享JS,如果访问页面JS,只能通过injected js来实现。  
    content-scripts不能访问绝大部分chrome.xxx.api,除了下面这4种：  
    chrome.extension(getURL,inIncongnitoContext,lastError,onRequest,sendRequest)  
    chrome.i18n  
    chrome.runtime(connect,getMainfest,getURL,id,onConnect,onMessage,sendMessage)  
    chrome.storage  

    * background  
    后台，是一个常驻的界面，它的生命周期是插件种所有类型页面中最长的，它随着游览器的打开而打开，随着游览器的关闭而关闭，所以通常把需要一直运行的，启动就运行的，全局的代码放在background里面。  
    background的权限非常高，几乎可以调用所有的chrome扩展API，而且它可以无限制跨域，也就是可以跨域访问任何网站而无需要求对方设置CORS。  
    配置中，background可以通过page指定一张网页，也可以通过scripts直接指定一个JS,Chrome会自动为这个JS生成一个默认的网页。  
    * event-pages  
    在配置文件上，它与background的唯一区别就是多了persistent参数。  
    它的生命周期是：在被需要时加载，在空闲时关闭。  
    * popup是点击browser_action或者page_action图标时打开的一个小窗口网页，焦点离开网页就立即关闭。  
    popup可以包含任意你想要的HTML内容，并且会自适应大小。可以通过default_popup字段来指定popup页面，也可以调用setPopup()方法。
    * injected-script  
    content-script有一个很大的缺陷，也就是无法访问页面中的JS,虽然它可以操作DOM,但是DOM却不能调用它，也就是无法在DOM中通过绑定事件的方式调用content-script中的代码。
    * homepage_url  
    开发者或者插件主页设置。


