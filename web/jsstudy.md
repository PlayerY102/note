# javascript study note

## 基础语法

* script标签可以在html内嵌入js。sr导入外部js  
    ```js
    <script src=""></script>
    ```
* function  
    ```js
    function 函数名(参数){  
        功能；
    }；  
    ```
* button标签  
    ```js
    <button type="button" onclick="函数名()">button的内容</button>
    ```
* getElement  
    ```js
    <p id="demo">
    JavaScript 能改变 HTML 元素的内容。
    </p>
    <script>
    function myFunction()
    {
        x=document.getElementById("demo");  // 找到元素
        x.innerHTML="Hello JavaScript!";    // 改变内容
    }
    </script>
    <button type="button" onclick="myFunction()">点击这里</button>
    ```
  * 可改变很多属性  
    element.src  
    element.style.color  
    element.value
* 输出  
    使用 window.alert() 弹出警告框。
    ```js
    <script> 
    window.alert(5 + 6); 
    </script> 
    ```
    使用 document.write() 方法将内容写到 HTML 文档中。
    ```js
    <script> 
    document.write(Date()); 
    </script> 
    ```
    使用 innerHTML 写入到 HTML 元素。
    ```js
    <p id="demo">我的第一个段落</p> 

    <script> 
    document.getElementById("demo").innerHTML = "段落已修改。"; 
    </script> 
    ```
    使用 console.log() 写入到浏览器的控制台。f12调出console。
    ```js
    <script>
    console.log(1);
    </script>
    ```
* 语法  
    使用关键字var来定义变量  
  * 数组  
    ```js
    var cars=new Array();
    cars[0]="Saab";
    cars[1]="Volvo";
    cars[2]="BMW";
    ```
    或者
    ```js
    var cars=["Saab","Volvo","BMW"];
    ```
  * 对象  
    ```js
    var person={
    firstname : "John",
    lastname  : "Doe",
    id        :  5566
    };
    ```
    寻址方式：  
    ```js
    name=person.lastname;
    name=person["lastname"];
    ```
  * Undefined Null  
    undefined这个值表示变量不含有值
    可以通过将变量设置为null来清空变量。  
    ```js
    cars=null;
    person=null;
    ```
  * 声明变量类型
    ```js
    var carname=new String;
    var x=      new Number;
    var y=      new Boolean;
    var cars=   new Array;
    var person= new Object;
    ```
* 函数  
    函数可以嵌套在其他函数中定义，这样可以访问它们被定义时所处的作用域中的任何变量。  
    return 返回值可选  
  * 作用域  
    在 JavaScript 函数内部声明的变量（使用 var）是局部变量，所以只能在函数内部访问它。  
    在函数外声明的变量是全局变量，网页上的所有脚本和函数都能访问它。  
    局部变量会在函数运行以后被删除。  
    全局变量会在页面关闭后被删除。  
    如果您把值赋给尚未声明的变量，该变量将被自动作为全局变量声明。  
    ```js
        carname="Volvo";
    ```  
    在 HTML 中, 全局变量是 window 对象: 所有数据变量都属于 window 对象。  
    ```js
    <script>
    myFunction();
    document.getElementById("demo").innerHTML =
        "我可以显示 " + window.carName;
    function myFunction() 
    {
        carName = "Volvo";
    }
    </script>
    ```

## 事件

* 事件是可以被JavaScript侦测到的行为。  
    ```js
    <some-HTML-element some-event='some JavaScript'>
    <some-HTML-element some-event="some JavaScript">
    ```
    常见的HTML事件  
    ```js
    onchange    HTML 元素改变  
    onclick     用户点击 HTML 元素  
    onmouseover     用户在一个HTML元素上移动鼠标  
    onmouseout      用户从一个HTML元素上移开鼠标  
    onkeydown       用户按下键盘按键  
    onload      浏览器已完成页面的加载
    ```
* 错误处理  
    ```js
    try
    {
    //在这里运行代码
    }
    catch(err)
    {
    //在这里处理错误
    }
    ```
    err为一个对象，有message属性，错误信息。  
    throw语句可以自定义错误信息。
    ```js
    <script>
    function myFunction()
    {
    try
    { 
    var x=document.getElementById("demo").value;
    if(x=="")    throw "empty";
    if(isNaN(x)) throw "not a number";
    if(x>10)     throw "too high";
    if(x<5)      throw "too low";
    }
    catch(err)
    {
    var y=document.getElementById("mess");
    y.innerHTML="Error: " + err + ".";
    }
    }
    </script>
    ```
* DOM  
* window  
    浏览器对象模型 (BOM) 使 JavaScript 有能力与浏览器"对话"。  
    所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。  
    全局变量是 window 对象的属性。  
    全局函数是 window 对象的方法。  
    甚至 HTML DOM 的 document 也是 window 对象的属性之一。  
    ```js
    window.document.getElementById("header");
    document.getElementById("header");  
    ```
* 本地存储  
    本地存储建议用chrome.storage而不是普通的localStorage。  
    需要声明storage权限，有chrome.storage.sync和chrome.storage.local2种方式可供选择。  
    chrome.storage.sync可以跟随当前登录用户自动同步，这台电脑修改的设置会自动同步到其它电脑，很方便，如果没有登录或者未联网则先保存到本地，等登录了再同步至网络。  
    ```js
    // 读取数据，第一个参数是指定要读取的key以及设置默认值
    chrome.storage.sync.get({color: 'red', age: 18}, function(items) {
        console.log(items.color, items.age);
    });
    // 保存数据
    chrome.storage.sync.set({color: 'blue'}, function() {
        console.log('保存成功！');
    });
    ```