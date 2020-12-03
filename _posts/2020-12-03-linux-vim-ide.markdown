---
layout: post
title: 打造VIM编辑器IDE
---

**安装vim、ctags、cscope**

```shell
sudo apt install vim exuberant-ctags cscope
```

**vim**

配置/etc/vim/vimrc或~/.vimrc

```shell
set nu # 设置行号
```

**ctags**

使用

```shell
ctags -R . # 生成tags
vim -t tag # tag
:ts # tag list
:tp # tag preview
:tn # tag next
ctrl-] # enter
ctrl-t # back
```

配置vimrc

```shell
set tags=path/tags
```

**cscope**

使用方法

```shell
find . -name "*.[h|c]" > cscope.files
cscope -bkq -i cscope.files
cscope -d 
# exit: ctrl-d
```

vim中常用命令

```shell
:cscope find s ---- 查找C语言符号，即查找函数名、宏、枚举值等出现的地方  
:cscope find g ---- 查找函数、宏、枚举等定义的位置，类似ctags所提供的功能  
:cscope find d ---- 查找本函数调用的函数  
:cscope find c ---- 查找调用本函数的函数  
:cscope find t ---- 查找指定的字符串  
:cscope find e ---- 查找egrep模式，相当于egrep功能，但查找速度快多了  
:cscope find f ---- 查找并打开文件，类似vim的find功能  
:cscope find i ---- 查找包含本文件的文 
```

**vim插件**

开启插件~/.vimrc

```shell
filetype plugin on
```

**omnicppcomplete**

下载地址：<http://www.vim.org/scripts/script.php?script_id=1520>

安装：在~/.vim目录下解开压缩包

tags生成命令：

```shell
ctags -R --C-kinds=+p --fields=+aS --extra=+q
```

**taglist**

下载地址：<https://github.com/yegappan/taglist>

配置

```shell
let Tlist_Show_One_File=1 "不同bai时显示多个文件du的zhitag，只显示当前文件的
let Tlist_Exit_OnlyWindow=1 "如果taglist窗口是最后一dao个窗口，则退出vim
let Tlist_Ctags_Cmd="/usr/bin/ctags" "将taglist与zhuanctags关联 ！
```

操作命令：

":TlistToggle"：打开左边的tag窗口，再输一次自动关闭，按ctrl+w可以在窗口之间进行切换

":TlistOpen"：Open and jump to the taglist window. 

":TlistClose"：Close the taglist window.

":TlistToggle"：Open or close (toggle) the taglist window.

在taglist中的一些操作：

: 跳转到光标处tag的源文件(就是回车键)

o : 打开新窗口，并跳转到光标处tag的源文件。

p : 与

t : 打开新tab并跳转到光标处tag的源文件。

: 显示光标处tag的函数原型

\+ : 打开tag组

\- : 收拢tag组

x : 最大化／还原

