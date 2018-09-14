---
layout: post
title:  "Ajax学习笔记"
categories: Ajax
tags:  Ajax Jquery 异步 同步
author: Hannoch
---

* content
{:toc}


AJAX:Asynchronous Javascript And XML.

AJAX不是新的编程语言，而是一种使用现有标准的新技术。AJAX是在不重新加载整个页面的情况下，与服务器交换数据并异步更新部分网页的艺术。
# 一、传统javascript使用ajax方式：
XMLHttpRequest对象：所有现代浏览器均支持XMLHttpRequest对象，XMLHttpRequest用于在后台与服务器交换数据。
## 1）生成XMLHttpRequest对象：
```javascript
<script type="text/javascript" >
        function CreateXMLHTTP() {
            var objXmlHttp;
            // 检测MSXMLHTTP版本，为了兼容IE各个版本
            var activeKey = new Array("MSXML2.XMLHTTP.5.0", "MSXML2.XMLHTTP.4.0", "MSXML2.XMLHTTP.3.0", "MSXML2.XMLHTTP", "Microsoft.XMLHTTP");
            if (window.ActiveXObject) {
                for (var i = 0; i < activeKey.length; i++) {
                    try {
                        objXmlHttp = new ActiveXObject(activeKey[i]);
                        if (objXmlHttp != null)
                            return objXmlHttp;
                    }
                    catch (error) {
                        throw new Error("您的浏览器版本过低，请更新浏览器");
                    }
                }
            }
            else if (window.XMLHttpRequest) {
                objXmlHttp = new XMLHttpRequest();
            }
            return objXmlHttp;
        }
    </script>
```
## 2）XMLHttpRequest对象向服务器发送请求：
```javascipt
objXmlHttp.open(method,url,async);
objXmlHttp.send();
```
> **method**：请求的类型有GET货POST;
> **url**:文件在服务器上的位置；
> **async**:true（异步）或false(同步).

 
使用GET还是POST？

与post相比，get更简单也更快，并且在大部分情况下都能用。然而，在以下情况中，请使用post请求：

 1. 无法使用缓存文件（更新服务器上的文件或数据库）。
 2.  向服务器发送大量数据（post没有数据量的限制）。
 3. 发送包含未知字符的用户输入时，post比get更稳定可靠。

## 3）服务器响应：

如需获得来自服务器的响应，使用XMLHttpRequest对象的responseText或responseXML属性。

例如：
```javascript
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;

xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++)
  {
  txt=txt + x[i].childNodes[0].nodeValue + "<br />";
  }
document.getElementById("myDiv").innerHTML=txt;
```
## 4）客户端响应事件onreadystatechange
XMLHttpRequest对象的三个重要属性：

> **onreadystatechange**:存储函数（或函数名），每当readystate属性改变时，就会调用该函数。
> **readystate**：有5个状态，0：请求未初始化，1：服务器连接已建立，2：请求已接收，3：请求处理中，4：请求已完成，且响应已就绪。
> **status**:有200：“OK”，404：未找到页面。

 
跟进javascript使用ajax的步骤编写一具体实例：
```
<script type="text/javascript" >
        function GetSendData() {
            document.getElementById("divTip").innerHTML = "正在加载中";
            var strSendUrl = "b.html?date=" + Date();
            CreateXMLHTTP();
            objXmlHttp.open("GET", strSendUrl, true);
            objXmlHttp.onreadystatechange = function () {
                if (objXmlHttp.readyState == 4) {
                    if (objXmlHttp.status == 200) {
                        document.getElementById("divTip").innerHTML = objXmlHttp.responseText;
                    }
                }
            }

            objXmlHttp.send(null);
        }
    </script>
```
# 二、ajax 一般流程总结

```javascript
<script>
//1.创建一个XMLHttpRequest类型的对象—相当于打开了一个浏览器
var xmlhttp;
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
//2.打开与一个网址之间的连接—相当于在地址栏输入访问地址
xmlhttp.open('GET','./time.php')
//3.通过连接送一次请求—相当于回车或者点击访问发送请求
xmlhttp.send(nul1)
//4.指定xmlhttp状态变化事件处理函数—相当于处理网页呈现后的操作
xmlhttp.bnreadystatechange=function(){
//通过xmlhttp的readyState判断此次请求的响应是否接收完成
if(this.readyState ===4){
//通过xmlhttp的responseText获取到响应的响应体
console.log(this)
</script>

```



# 三、jQuery中ajax技术

## 1、jQuery下load方法

在jQuery中使用load方法可以轻松获取异步数据的功能，语法格式如下：
```
<script type="text/javascript" >
        $(function () {
            $("#Button2").click(function () {
                $("#divTip2").load("b.html");
            })
        })
    </script>
```
## 2、getJSON方法

虽然load方法可以很快的加载数据到页面上，但是有时需要对获取的数据进行处理，如果将load方法获取的数据内容进行遍历，也可以进行数据的处理，但这样获取的内容必须先插入页面中，然后才能进行，因此它的执行效率不高。为了解决此问题，采用另一种较为轻量级的数据交互格式json文件格式。其调用语法格式为：

    $.getJSON(url,[data],[callback])

具体调用实例：
首先创建一个.json格式的文件UserInfo.json:
```json
[
    {
        "name":"张三",
        "sex":"男",
        "email":"zhangsan@163.com"
    },
    {
        "name":"李四",
        "sex":"女",
        "email":"lisi@163.com"
    }
]
```
使用getJSON方法进行调用：

```javascript
<script type="text/javascript" >
        $(function () {
            $("#Button1").click(function () {
                $.getJSON("UserInfo.json", function (data) {
                    $("#divTip").empty();
                    var strHtml = "";
                    $.each(data, function (infoIndex, info) {
                        strHtml += "姓名：" + info["name"] + "<br>";
                        strHtml += "性别：" + info["sex"] + "<br>";
                        strHtml += "邮箱：" + info["email"] + "<br>";
                    })
                    $("#divTip").html(strHtml);
                })
            })
        })
    </script>
```
## 3、getScript方法
直接实例说明，创建一个文件UserInfo.js:
```
var data = [
    {
        "name": "张三",
        "sex": "男",
        "email": "zhangsan@163.com"
    },
    {
        "name": "李四",
        "sex": "女",
        "email": "lisi@163.com"
    }
];

    var strHtml = "";
    $.each(data, function () {
        strHtml += "姓名：" + this["name"] + "<br>";
        strHtml += "性别：" + this["sex"] + "<br>";
        strHtml += "邮箱：" + this["email"] + "<br>";
    })
    $("#divTip").html(strHtml);
```
调用方法：
```
<script type="text/javascript" >
        $(function () {
            $("Button1").click(function () {
                $.getScript("UserInfo.js");
            })
        })
    </script>
```
 
## 4、jQuery中异步加载xml文档

具体实例：创建一个UserInfo.xml文件

```
<?xml version="1.0" encoding="utf-8" ?>
<Info>
  <User id="1">
    <name>张三</name>
    <sex>男</sex>
    <email>zhangsan@163.com</email>
  </User>
  <User id="2">
    <name>李四</name>
    <sex>女</sex>
    <email>lisi@163.com</email>
  </User>
</Info>
```
调用方法：

```
<script type="text/javascript" >
        $(function () {
            $("Button1").click(function () {
                $.get("UserInfo.xml", function (data) {
                    $("#divTip").empty();
                    var strHtml = "";
                    $(data).find("User")
                        .each(function () {
                            var $strUser = $(this);
                            strHtml += "姓名：" + $strUser.find("name").text() + "<br>";
                            strHtml += "性别：" + $strUser.find("sex").text() + "<br>";
                            strHtml += "邮箱：" + $strUser.find("email").text() + "<br>";
                        })
                        $("#divTip").html(strHtml);
                })
            })
        })
    </script>
```
 

## 5、$.get()

jQuery中异步加载xml数据中使用$.get()函数，除此之外，$.get()函数还可以实现数据的请求。具体实例说明：

创建一个页面UserInfo.aspx:
```
<%@ Page Language="C#" ContentType="text/html" ResponseEncoding="gb2312" %>

<%
    string strName = System.Web.HttpUtility.UrlDecode(Request["name"]);
    string strHtml = "<div>";
    if (strName == "张三")
    {
        strHtml += "姓名：张三<br>";
        strHtml += "性别：男<br>";
        strHtml += "邮箱：zhangsan@163.com<br>";
    }
    else if (strName == "李四")
    {
        strHtml += "姓名：李四<br>";
        strHtml += "性别：女<br>";
        strHtml += "邮箱：lisi@163.com<br>";
    }

    strHtml += "</div>";
    Response.Write(strHtml);
     %>
```
使用$.get()进行调用：
```
<script type="text/javascript" >
        $(function () {
            $("#Button1").click(function () {
                $.get("UserInfo.aspx",
                { name: encodeURI($("#txtName").val()) },
                function (data) {
                    $("#divTip")
                    .empty()
                    .html(data);
                })
            })
        })
    </script>
```
 

## 6、$.post()

　　带参数向服务器发出数据请求，全局函数$.post()和$.get()在本质上没有太大的区别，不同的是Get方式不适合传递数据量较大的数据，同时get请求的历史信息也会保存在浏览器的缓存中，有一定的安全风险。而以post方式向服务器发送数据请求，则不存在这两个方面的不足。

具体实例：创建一个页面UserInfo2.aspx

```
<%@ Page Language="C#" ContentType="text/html" ResponseEncoding="gb2312" %>

<%
    string strName = System.Web.HttpUtility.UrlDecode(Request["name"]);
    string strSex = System.Web.HttpUtility.UrlDecode(Request["sex"]);
    string strHtml = "<div>";
    if (strName == "张三" && strSex == "男")
    {
        strHtml += "姓名：张三<br>";
        strHtml += "性别：男<br>";
        strHtml += "邮箱：zhangsan@163.com<br>";
    }
    else if (strName == "李四" && strSex == "女")
    {
        strHtml += "姓名：李四<br>";
        strHtml += "性别：女<br>";
        strHtml += "邮箱：lisi@163.com<br>";
    }

    strHtml += "</div>";
    Response.Write(strHtml);
     %>
```
使用$.post()进行调用：

```
<script type="text/javascript" >
        $(function () {
            $("#Button1").click(function () {
                $.post("UserInfo2.aspx",
                { name: encodeURI($("#txtName").val()), 
                  sex:encodeURI($("#selSex").val())
                },
                function (data) {
                    $("#divTip")
                    .empty()
                    .html(data);
                })
            })
        })
    </script>
```
 

## 7、$.ajax()

$.ajax()方法是jQuery中最底层的方法，该方法不仅可以方便的完成$.get(),$.post()方法，还可以关注更多的细节。

首先列举一简单实例：

创建一页面：login.aspx

```
<%@ Page Language ="C#" ContentType="text/html" ResponseEncoding="gb2312"%>
<%
    string strName = System.Web.HttpUtility.UrlDecode(Request["txtName"]);
    string strPass = System.Web.HttpUtility.UrlDecode(Request["txtPass"]);
    bool isLogin = false;

    if (strName == "admin" && strPass == "admin")
    {
        isLogin = true;
    }
    Response.Write(isLogin);
     %>
```
创建一前台界面：

```
<div>
                <div id="divError"></div>
                <div>名称：<input id="txtName" type="text"/></div>
                <div>密码：<input id="txtPass" type="text" /></div>
                <div>
                    <input id="btnLogin" type="button" value="登录" class="btn" />&nbsp;&nbsp;
                    <input id="btnReset" type="button" value="取消" class="btn" />
                </div>
            </div>
```
 

使用$.ajax()方法进行调用：

```
<script type="text/javascript" >
        $(function () {
            $("#btnLogin").click(function () {
                var strName = encodeURI($("#txtName").val());
                var strPass = encodeURI($("#txtPass").val());
                $.ajax({
                    url: "login.aspx",
                    dataType: "html",
                    data: { txtName: strName,
                        txtPass: strPass
                    },
                    success: function (strValue) {
                        if (strValue == "True") {
                            $("#divError")
                                    .show().html("操作提示，登录成功");
                        }
                        else {
                            $("#divError")
                                    .show().html("用户名或密码错误");
                        }
                    }
                })
            })
        })
    </script>
```
 

$.ajax()方法中的参数有：url，type,data,dataType,beforeSend,complete,success,error,timeout,global,async,cache.它们所代表的具体意义，可以查些资料，此处就不一一赘述了。
$.ajaxSetup()设置全局Ajax。
## 8、ajax中全局事件：

ajaxComplete(callback),ajaxError,ajaxSend,ajaxStart,ajaxStop,ajaxSuccess.

ajax全局事件可以绑定在页面的任何一个元素中。实例：
```
$("#divTip").ajaxStart(function(){
   $(this).html("正在处理中...") ;
})
``` 

## 9、综合运用：

　　此次介绍它和后台系统的交互过程。编写一个函数，其中使用ajax向后台传递数据，并从后台获取数据，并显示出来。testMethod5函数中，id为13，后台process方法得到id=13后，将字符串heightstr复制78，然后传递到前台testMethod函数中并显示出来。
　　
前台函数testMethod5：
```
<script type="text/javascript" >
        function testMethod5() {
            var id = 13;
            var age = 46;
            var address = "china";
            $.ajax({
                type: "POST",
                url: "Test.aspx/Process",
                data: {
                    "id": id,
                    "age": age,
                    "address": address
                },
                dataType: "json",
                success: function (jsonData) {
                    var result = jsonData[0]["result"];
                    if (result == "1") {
                        var getValue = jsonData[0]["height"];
                        alert(getValue);

                    } else {
                        alert("false");
                    }
                }
            });
        }
    </script>
```
简单：
```
<div>
        <input type="button" id="Button1" value="btn3" onclick ="testMethod5()" />
    </div>
``` 
后台代码编写，其中使用反射技术完成ajax前后台交互。

```
namespace OSCEWEB.UI
{
    public partial class Test : System.Web.UI.Page
    {
        private const string PAGE_PATH_INFO = "/UI/Test.aspx";//URL访问路径
        private const string ASSEMBLY_NAME = "OSCEWEB";//程序集名称
        private const string CLASS_NAME = "OSCEWEB.UI.Test";//类名

        protected void Page_Load(object sender, EventArgs e)
        {
            if (Request.Params["PATH_INFO"].StartsWith(PAGE_PATH_INFO + "/"))
            {
                Response.End();
            }
        }

        protected override void OnInit(EventArgs e)
        {
            string pathInfo = Request.Params["PATH_INFO"];
            if (pathInfo.StartsWith(PAGE_PATH_INFO + "/"))
            {
                string[] nameList = pathInfo.Substring(PAGE_PATH_INFO.Length + 1).Split('/');
                if (nameList.Length < 1)
                {
                    Response.End();
                    return;
                }

                try
                {
                    Assembly assembly = Assembly.Load(ASSEMBLY_NAME);
                    Type type = assembly.GetType(CLASS_NAME);
                    MethodInfo method = type.GetMethod(nameList[0], System.Reflection.BindingFlags.NonPublic | System.Reflection.BindingFlags.Instance);
                    method.Invoke(this, null);
                }
                catch (Exception ex)
                {
                    Response.End();
                    return;
                }
            }
        }

        private void Process()
        {
            string idStr = Request.Form["id"];//从前台获取值
            int id = Convert.ToInt32(idStr);
            string heightStr;
            if (id == 13)
            {
                heightStr = "78";
            }
            else
            {
                heightStr = "96";
            }

            StringBuilder jsonStringBuilder = new StringBuilder(string.Empty);
            jsonStringBuilder.Append("[{");
            jsonStringBuilder.Append("\"result\":\"1\",");
            jsonStringBuilder.Append("\"height\":\"" + heightStr + "\"");//传递到前台
            jsonStringBuilder.Append("}]");

            Response.Write(jsonStringBuilder .ToString ());
        }


    }
}
```
 笔停此处，如有更深层次的理解，再行更新。

补充：

上文提到ajax调用页面，那页面是如何处理ajax请求的呢？实例中使用了反射技术，在平时的项目操作时更多的是使用一般处理程序，下面简单介绍下一般处理程序：

一般处理程序，一般处理程序是什么呢？ 一般处理程序实际上就是一个处理程序类。对于ASP.NET网站来说，网站最常见的处理结果就是HTML网页，生成网页的工作通常使用拓展名为ASPX的web窗体来完成。对于处理结果不是HTML的请求，都可以通过一般处理程序完成，例如：RSS、Feed、XML、图片等。一般处理程序是ASP.NET网站中最为简单、高效的处理程序，在处理返回类型不是HTML的请求中有着重要的作用。

```
public void ProcessRequest(HttpContext context)
        {
            context.Response.ContentType = "text/plain";
            var param = context.Request["act"];
            context.Response.Write(param);
        }
```
HttpContext类：封装有关个别HTTP请求的所有HTTP特定的信息，又叫上下文。 看到这个解释，我觉得有些抽象，Http特定信息具体又是什么？看了下备注：

为继承 IHttpModule 和 IHttpHandler 接口的类提供了对当前 HTTP 请求的 HttpContext 对象的引用。该对象提供对请求的内部 Request、Response 和 Server 属性的访问。

**生命周期**：从用户发送请求开始到服务器处理完请求并生成内容返回到客户端为止。针对每个不同用户的请求,服务器都会创建一个新的HttpContext实例直到请求结束,服务器销毁这个实例.

**HttpContext的作用**：处理请求的属性如：request，response，server等。其实我们在开发asp.net页面的时候，可以直接使用request等，那为什么要通过HttpContext类来访问呢？因为request等这些可以在aspx页面的代码中直接使用，但是在IHttpModule或IHttpHandler中就不能直接使用。HttpContext类对Request、Response、Server等进行了封装，保证在整个请求周期内都可以随时随地地调用。

**HttpContext其它功能**：HttpContext还可以处理CacHe，HttpContext.Item等，在其生命周期内可以存储一些临时数据，方便随时使用。当用户发送某个Http请求，我们可以通过HttpContext进行截获，查看里面包含了哪些请求的信息，然后可以进行一系列的操作，比如说切换到其他的页面，这个时候，可以重组请求的数据满足新页面的要求。即：即使不在page页面中，也可以通过HttpContext的Current属性来获取当前的web状态。


















