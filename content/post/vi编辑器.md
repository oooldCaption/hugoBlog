---
title: VI编辑器
date: 2018-09-18T18:08:26+08:00 
draft: false
tags: ['巧篆']
categories: ['奇巧淫技']
---

vi(vim)是上Linux非常常用的代码编辑器，很多Linux发行版都默认安装了vi(vim)。vi(vim)命令繁多但是如果使用灵活之后将会大大提高效率。vi是“visual interface”的缩写，vim是vi IMproved(增强版的vi)。在一般的系统管理维护中vi就够用，如果想使用代码加亮的话可以使用vim
<!-- more -->
基本上vi可以分为三种状态，分别是命令模式（command mode）、插入模式（Insert mode）和底行模式（last line mode），各模式的功能区分如下：

*   命令模式（command mode） 控制屏幕光标的移动，字符、字或行的删除，移动复制某区段及进入插入模式、底行模式下。
    
*   插入模式（Insert mode） 只有在插入模式下，才可以做文字输入，按ESC键可回到命令模式。
    
*   底行模式（last line mode） 将文件保存或退出vi，也可以设置编辑环境，如寻找字符串、列出行号。
    

不过一般我们在使用时把vi简化成两个模式，就是将底行模式也算入命令模式。

### 打开/保存/关闭文件

*   vi filename //打开filename 文件
*   :w //保存文件
*   :w filename //保存文件到 filename 文件
*   :q //推出编辑器, 且不保存
*   :q! //推出编辑器, 且不保存
*   wq //推出编辑器. 且保存文件

### 插入文本或行(命令模式下使用, 执行后进入插入模式, esc 退出插入模式)

*   a //在当前光标位置的右边添加文本
*   i //在当前光标位置的左边添加文本
*   A //在当前行的末尾位置添加文本
*   I //在当前行的开始处添加文本(非空字符的行首)
*   O //在当前行的上面新建一行
*   o //在当前行的下面新建一行
*   R //替换(覆盖)当前光标位置及后面的若干文本
*   J //合并光标所在行及下一行为一行(依然在命令模式)

### 移动光标(命令模式下)

*   vi可以直接用键盘上的光标来上下左右移动，但正规的vi是用小写英文字母 h 、 j 、 k 、 l ，分别控制光标左、下、上、右移一格。
*   按 Ctrl+b ：屏幕往后移动一页。
*   按 Ctrl+f ：屏幕往前移动一页。
*   按 Ctrl+u ：屏幕往后移动半页。
*   按 Ctrl+d ：屏幕往前移动半页。
*   按数字 0 ：移到当前行的开头。
*   按 G ：移动到文章的最后。
*   按 $ ：移动到光标所在行的行尾。
*   按 ^ ：移动到光标所在行的行首。
*   按 w ：光标跳到下个字的开头。
*   按 e ：光标跳到下个字的字尾。
*   按 b ：光标回到上个字的开头。
*   按 #l ：光标往后移的第#个位置，如：5l,56l .

### 删除,恢复字符或者行(命令模式下)

*   x ：每按一次，删除光标所在位置的后面一个字符。
*   #x ：删除光标所在位置的后面#个字符，例如， 6x 表示删除光标所在位置的后面6个字符。
*   X ：每按一次，删除光标所在位置的前面一个字符。
*   #X ：删除光标所在位置的前面#个字符，例如， 20X 表示删除光标所在位置的前面20个字符。
*   dd ：删除光标所在行。
*   #dd ：从光标所在行开始删除#行。

### 搜索 (命令模式)

*   /xiaosu //向光标下搜索xiaosu字符串
*   ?xiaosu //向光标上搜索xiaosu字符串
*   n //向下搜索前一个搜素动作
*   N //向上搜索前一个搜索动作

### 跳转 (命令模式)

*   n+ //向下跳n行
*   n- //向上跳n行
*   nG //跳到行号为n的行
*   G //跳至文件的底部

### 设置行号 (命令模式)

*   :set nu //显示行号
*   :set nonu //取消显示行号

### 复制/粘贴 (命令模式)

*   yy //将当前行复制到缓存区，也可以用 “ayy 复制，”a 为缓冲区，a也可以替换为a到z的任意字母，可以完成多个复制任务。
*   nyy //将当前行向下n行复制到缓冲区，也可以用 “anyy 复制，”a 为缓冲区，a也可以替换为a到z的任意字母，可以完成多个复制任务。
*   yw //复制从光标开始到词尾的字符。
*   nyw //复制从光标开始的n个单词。
*   y^ //复制从光标到行首的内容。
*   y$ //复制从光标到行尾的内容。
*   p //粘贴剪切板里的内容在光标后，如果使用了前面的自定义缓冲区，建议使用”ap 进行粘贴。
*   P //粘贴剪切板里的内容在光标前，如果使用了前面的自定义缓冲区，建议使用”aP 进行粘贴。

### 替换(vi命令模式下使用)

*   :s/old/new //用new替换行中首次出现的old
*   :s/old/new/g //用new替换行中所有的old
*   :n,m s/old/new/g //用new替换从n到m行里所有的old
*   :%s/old/new/g //用new替换当前文件里所有的old
