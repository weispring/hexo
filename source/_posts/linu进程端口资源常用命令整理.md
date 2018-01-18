---
title: linu进程端口资源常用命令整理
date: 2018-01-06 09:58:41
tags: [linux]
categories: 笔记
---

# 进程常用命令
## 查看进程
ps  查看所有进程 常用参数 ps aux  ，ps -le
-a 显示一个终端的所有进程，除了会话引线
-u 显示进程的归属用户和内存适用情况
-x 显示没有控制终端的进程
-l 显示更加详细的信息
-e 显示所有进程
<!-- more -->
结果字段意思  
USER  所属用户      
PID  进程的唯一id 
%CPU  消耗cpu的百分比 
%MEM  消耗内存的百分比
STAT  进程状态 START  运行开始时间   
TIME   占用cpu总时间  执行的命令
VSZ(占用的虚拟内存)    单位都是kb
RSS（占用的真是内存）  单位都是kb
TTY，该进程是在那个终端中运行的。TTY1-TTY7是本地终端，TTY1-TTY6是字符界面终端，TTY7是图形界面终端，TTY/0-255是远程终端

常用 ps -ef | grep 程序名 

选项含义如下：
ps:将某个进程显示出来
-A 　显示所有程序。 
-e 　此参数的效果和指定"A"参数相同。
-f 　显示UID,PPIP,C与STIME栏位。 
grep命令是查找
中间的|是管道命令 是指ps命令与grep同时执行

字段含义如下
UID PID PPID C STIME TTY TIME CMD
各相关信息的意义：
UID 程序被该 UID 所拥有
PPID 则是其上级父程序的ID
C CPU 使用的资源百分比
STIME 系统启动时间

## 查看进程树 
pstree  显示进程树，以父子进程的关系显示，查看统一个服务有启动了几个进程特别方便
-u 显示所属用户
-p 显示pid

## 杀死进程
杀死一个进程 kill -9 pid
杀死一组进程 killall 进程名 

# 端口查看命令 

## netstat   查看端口是否开放
-a (all)显示所有选项，默认不显示LISTEN相关
-t (tcp)仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化成数字。
-l 仅列出有在 Listen (监听) 的服務状态
-p 显示建立相关链接的程序名
-r 显示路由信息，路由表
-e 显示扩展信息，例如uid等
-s 按各个协议进行统计
-c 每隔一个固定时间，执行该netstat命

## telnet 探测端口是否可以访问到
telnet  ip port

# 查看系统资源

## 查看内存适用情况
free -[b|k|m|g] 

## 查看硬盘使用情况
df -[b|k|m] 
