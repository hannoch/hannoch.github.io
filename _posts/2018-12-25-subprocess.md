---
layout: post
title:  "python3 获得shell的交互(subprocess)"
categories: subprocess
tags:  Python
author: Hannoch
---

# 使用Python subprocess设计一个函数
使用Python subprocess设计一个函数
要求：
1.函数的参数1是要执行的命令行，参数2是超时时间，单位为秒；
2.函数功能为执行参数1的命令行，不能阻塞；
3.设计函数的返回值，方便调用者。

'''python

#python3
# -*- coding: utf-8 -*-
import subprocess
import time
 
class TimeoutError(Exception):
	pass
 
def excuteCmd(cmd, timeout = 1):
	#src = raw_input(unicode("请输入源文件完整路径及文件名： ", 'utf-8').encode('gbk'))
	#dst = raw_input(unicode("请输入目标文件完整路径及文件名： ",'utf-8').encode('gbk'))
	#cmd = 'copy "%s" "%s"'%(src,dst)
	s = subprocess.Popen(cmd, shell = True)
	
	beginTime = time.time()
	secondsPass = 0
	
	while True:
		if s.poll() is not None:
			break
		secondsPass = time.time() - beginTime
		
		if timeout and timeout < secondsPass:
			s.terminate()
			print (u"超时!")
			return False
	
		time.sleep(0.1)
	return True
	
        #can't goto here
	#return s.returncode
	
 
#main function
if  __name__ == '__main__':
    
    try:
        print("test1")
        ret = excuteCmd("ping www.baidu.com",5)
        print(ret)
        print ("test2")
        ret = excuteCmd("echo start && ping -n 10 127.0.0.1 > nul && echo done ",3)
        print(ret)
    except TimeoutError as e:
        print(e)

'''

# Django 中应用思路

1、在模版中写一个表单form, 表单action 中填写路径。

2、根据表单action中的路径，通过urls 路由映射到视图views。

3、在views 中得到post 数据，用subprocess 调用执行*.py 代码，将返回值推送到模版。

4、希望脚本的输出能实时显示在页面则用ajax 或websocket
