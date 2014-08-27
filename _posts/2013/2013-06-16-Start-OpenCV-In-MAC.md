---
layout: post
title: Start with OpenCV in MAC OS X
categories:
- OpenCV
tags:
- OpenCV
- MAC
- Homebrew
---

## Processing
> 在网上看了很多的教程，都是基于Unix的系统，OS X和Linux在安装openCV的时候都会用到cmake的指令。在Linux系统里，基本上用的是apt-get的软件包，直接集成在系统内，所以没什么大的差别。在OS X的环境下cmake需要自己安装。一般来说安装方法分为三种：1.直接安装；2.Macports; 3.Homebrew .第一种方法本人没有亲验过。我的mac原来安装过macports，但是按网上的说法，homebrew在某些方面更加好，于是我卸载了macports, 安装了homebrew, 然后就安装cmake, 和OpenCV.就当一切OK时，我试了第一个程序就编译出了问题。本人小白一个，不太懂linux编译的原理，然后google了各种答案，决定删除homebrew又切换到macports. 可是macports也是有同样的问题。此时真是郁闷无比，经历过多次折腾，又鬼使神差的卸载macports换到homebrew, 并想降低openCV的版本。没想到2.4.3竟然无法安装，神奇的是2.4.5却一切正常！刚开始编译不能通过的代码也能成功运行了, 女神lena成功现身，终于长舒一口气。

所以我最后的方法是用homebrew进行cmake的安装进而安装openCV，本教程也将按照该方法展开。

### Macport的卸载
打开terminal, 输入以下命令：

```
sudo port -f uninstall installed
sudo rm -rf \\
/opt/local \\
/Applications/DarwinPorts \\
/Applications/MacPorts \\
/Library/LaunchDaemons/org.macports.\* \\
/Library/Receipts/DarwinPorts\*.pkg \\
/Library/Receipts/MacPorts\*.pkg \\
/Library/StartupItems/DarwinPortsStartup \\
/Library/Tcl/darwinports1.0 \\
/Library/Tcl/macports1.0 \~/.macports
```
### Homebrew的安装（与卸载）
首先要保证安装了ruby, mac自带，所以不用担心，接下来在terminal中输入：

```
ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"
```

即可完成homebrew的安装。
如果像我这样折腾来又折腾去免不了装几次卸载几次的，以下为卸载的代码：

```
cd `brew --prefix`
rm -rf Cellar
brew prune
rm `git ls-files`
rm -r Library/Homebrew Library/Aliases Library/Formula Library/Contributions
rm -rf .git
rm -rf \~/Library/Caches/Homebrew
```

### Homebrew的检查以及环境变量的设置
安装完homebrew，先进行升级：

```
brew update
```
然后按照提示要先运行一下brew doctor来检查，一般说来没有问题就继续安装步骤。

### Cmake的安装
```
brew install cmake
```

### OpenCV的安装
可以直接从brew集成的软件包下载最新版本：

```
brew install opencv
```
直接安装完成。或者从sourceforge直接下载：
[http://sourceforge.net/projects/opencvlibrary/][1]

下载后解压进入该文件夹，新建一个空的文件夹release，进入该文件夹，编译安装opencv，使用命令如下：

```
 cd openCV2.4.5
 mkdir release
 cd release
 cmake -G "Unix Makefiles" ..
 make
 sudo make install
```
安装好的lib文件存放在*“/usr/local/lib”*文件夹，h文件存放在*“/usr/local/include”*.

[1]:	http://sourceforge.net/projects/opencvlibrary/