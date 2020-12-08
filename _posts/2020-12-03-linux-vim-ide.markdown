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

功能：补全结构体的成员以及类成员

下载地址：<http://www.vim.org/scripts/script.php?script_id=1520>

安装：在~/.vim目录下解开压缩包

tags生成命令：

```shell
ctags -R --C-kinds=+p --fields=+aS --extra=+q
```

**taglist**

下载地址：<https://github.com/yegappan/taglist>

配置

```
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

**echofunc**

功能：自动提示函数原型

下载地址：<http://www.vim.org/scripts/script.php?script_id=1735>

定义两个快捷键来切换函数的不同定义。

```
let g:EchoFuncKeyNext='<S-n>' 
let g:EchoFuncKeyPrev='<S-p>' 
```

**nerdtree**

nerdtree是一个用于浏览文件系统的树形资源管理。

下载地址：<https://github.com/scrooloose/nerdtree>

vimrc配置信息：

```
let NERDTreeWinPos='right'
let NERDTreeWinSize=30
map <F2> :NERDTreeToggle<CR>
```

配置之后可以使用`:NERDTree`或者配置的快捷键`F2`开启。


**pathogen**

```
pathogen一般作为vim新手的第一个插件，用来统一管理vim插件包，
```

官方解释：非常容易的管理你的 ‘runtimepath’ ，在实际项目中，vim-pathogen可以在它的私有文件夹下非常轻松的安装插件和管理运行时文件

**安装**

复制以下代码到你的终端

```
mkdir -p ~/.vim/autoload ~/.vim/bundle &&
curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
```

添加以下代码到你的.vimrc 文件中(文件位于用户的家目录下，如果不存在该文件，可以新建一个)

```
execute pathogen#infect()
syntax on
filetype plugin indent on
```

此时pathogen已经成功安装，可以安装以下插件测试下

```
cd ~/.vim/bundle &&
git clone https://github.com/tpope/vim-sensible.git

```
参考地址： <https://github.com/tpope/vim-pathogen>