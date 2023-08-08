---
title: dwm
description: dwm
date: 2023-06-11 15:47:18
tags:
- dwm
categories:
- tools
swiper_index:
---


## init


首先 clone 下 dwm 的官方源码

```bash
git clone https://git.suckless.org/dwm
```



### config.mk

将以下需要调用的库函数等进行正确的指定  

```c
X11INC = /usr/X11R6/include
X11LIB = /usr/X11R6/lib
```

> 更改为以下指定目录  


在 archlinux 上，需要的 `X11` 库函数一般在以下目录中

```c
X11INC = /usr/include/X11
X11LIB = /usr/include/X11
```


### config.h

在 dwm 目录下执行以下指令来生成 `config.h` 文件（根据 `config.def.h` 文件生成）  

```bash
sudo make clean install
```

接着删除 `config.def.h` 文件，以后都使用 `config.h` 来代替  

---

之后对 `config.h` 来进行习惯上的配置  


#### 字体配置

指定 dwm 用于显示的字体和大小  

```c
static const char *fonts[]          = { "SauceCodePro Nerd Font Mono:size=16" };
static const char dmenufont[]       = "SauceCodePro Nerd Font Mono:size=16";
```
> fonts 用于指定 dwm 的字体  
> dmenufont 用于指定 dmenu 的字体，这里顺便指定以下  




#### 更改 MODKEY

MODKEY 用于 dwm 的一些组合键的前缀  

```c
#define MODKEY Mod4Mask
```

这里将 `MODKEY` 从原来的 `Mod1Mask`（即 Alt 键），改为 `Mod4Mask`（即 Win 键）  




## patches

在 dwm 的目录下建议一个目录 `patches` 用于存放补丁

如何打补丁？  
在 dwm 根目录下执行以下命令：  

```bash
patch -p1 < patches/需要打的补丁
```

---

在开始打补丁之前，建议先对 dwm 进行版本控制，万一有些补丁打完出现问题自己不能解决，可以回滚之前的状态  

`.gitignore` 加入以下内容  
```gitignore
dwm
patches
*.o
*.orig
*.rej
```


### autostart

此补丁建议使用旧的, 因为旧的简单，新的加了太多的判断因素  

补丁名称：`dwm-autostart-20161205-bb3bd6f.diff`

在打完补丁之后，对 `dwm.c` 进行更改  
将 `autostart` 补丁提供的函数 `runAutostart` 内容更改成以下：

```c
void
runAutostart(void) {
	system("cd ~/scripts; ./autostart.sh &");
}
```
> 这里的意思是：执行文件 `~/scripts/autostart.sh`  



### alpha

补丁名称：`dwm-alpha-20230401-348f655.diff`

一般不会出错，直接打就行


### status2d

这个补丁需要根据需求进行选择，见官方  
这里我只需要普通的，并且并不准备使系统托盘常驻于我的 dwm bar 上面  

补丁名称：`dwm-status2d-6.3.diff`

这个补丁打完需要注意，由于刚刚打的 `alpha` 补丁更改了一些与绘制相关的函数，加入的 `alpha` 自身需要的参数  
所以打完补丁可能没有出错，但是编译时会报错，需要根据 `alpha` 补丁的内容来对函数参数进行相应的添加  

---

此版本的补丁报错函数为：`drw_clr_create` 和 `drw_scm_create`  

对于 `drw_clr_create`：  

需要根据 `alpha` 补丁来确定参数加在哪里  

我这里是在此函数的最后一个参数缺失，我加了一个 `alpha[0][1]`  
> 这里两处进行了修改  


对于 `drw_scm_create`：  

需要根据 `alpha` 补丁来确定参数加在哪里  

我这里是在此函数的倒数第二个参数缺失  

注意此函数在 `dwm.c` 中有两处：  

* 第一处不再 for 循环中，添加倒数第二个参数 `alpha[0]`
* 第二处在 for 循环中，补丁已经添加了参数，不需要修改了


### awesomebar

`dwm-awesomebar-20230431-6.4.diff`

### hide vcant tags

`dwm-hide_vacant_tags-6.3.diff `


### scratchpad

`dwm-scratchpad-20221102-ba56fe9.diff`


###  focusadjacenttag

`dwm-focusadjacenttag-6.3.diff`


### rotatestack

`dwm-rotatestack-20161021-ab9571b.diff`


### viewontag

`dwm-viewontag-20210312-61bb8b2.diff`


### pertag

`dwm-pertag-20200914-61bb8b2.diff`


### vanitygaps

`dwm-vanitygaps-6.2.diff`


### noborder

`dwm-noborderfloatingfix-6.2.diff`


### doublepressquit

`dwm-doublepressquit-6.3.diff`

打完补丁之后，需要在 dwm 头部引入 `time.h` 头文件  


### fullscreen

`dwm-fullscreen-6.2.diff`
