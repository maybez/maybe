
## 简介
本文主要介绍如何从Qt源码来编译安装Qt.

## 编译所需工具

- Qt下载地址:[Qt](http://download.qt.io/archive/qt/)
- ActivePerl下载地址:[ActivePerl](https://www.activestate.com/activeperl) (可能需要翻墙)
- python下载地址:[python](https://www.python.org/downloads/)
- Bison,Flex下载地址:[Bison](https://sourceforge.net/projects/winflexbison/files/win_flex_bison-2.5.5.zip/download)
- GPerf下载地址:[GPerf](https://sourceforge.net/projects/gnuwin32/files/gperf/3.0.1/gperf-3.0.1.exe/download?use_mirror=nchc&download=)

**以上材料下载后需要设置环境变量PATH.然后需要重启或者重新登录windows**

## 编译所需第三方库

### ICU

用来做国际化,如果加上它们会导致发布程序时要带上ICU的dll.几十M.所以一般不带,从5.3开始默认就是不带它的.也可以自己搞ICU库,去掉不必要的字符集,从而缩小ICU本身的大小.我们这里不要搞ICU,不管它了..

### ANGLE:
和openGL相关.用来把OpenGL ES 2.0API转换为DirectX 11 或者 DirectX 9的调用.QtQuick2需要OpenGL2.1或者更高的版本的图形驱动.Windows下的显卡驱动支持的是DirectX,默认带的OpenGL版本非常低(OpenGL1.1),基本不可用.要解决这个问题有两种方式:

1. 确保安装了支持OpenGL 2或者以上版本的显卡驱动.
2. 使用Angle将DirectX封装一下,模拟出一套OpenGL接口.

不同的配置说明:

- -opengl desktop :使用本机安装的OpenGL驱动,不使用ANGLE
- -opengl es2 -no-angle:
- -opengl es2 -angle:
- -opengl es2 -angle-d3d11:- 
- -opengl dynamic : 从Qt5.5开始,官方默认此选项.Qt自己决定使用什么样的OpenGL.灵活性最高的,推荐配置.
- -opengl dynamic -static :
- -opengl -no-opengl 

## 使用到的脚本

### vcvars32.bat 
这是在visual studio 提供的一个脚本,用来SET一系列vs编译环境相关的环境变量.可以在vs安装目录里找到它:
C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin.

### zh.bat
   
	@echo off
	call vcvars32.bat
	if not exist qt-5.9-build mkdir qt-5.9-build
	cd /d qt-5.9-build
	../qt-everywhere-opensource-src-5.9.0/configure -platform win32-msvc2015 -prefix D:\Qt\zh-qt\qt-5.9 -debug -release -nomake examples -opensource -opengl dynamic
	nmake
	nmake install
	
## configure 选项

### 目录组织
- 源码目录:用来存放从官网下载的源代码
- 构建目录:用来存储构建相关文件(如MakeFile,obj文件,其他中间文件)
- 安装目录:用来安装二进制库文件的位置.


最好把这些目录相互区分开,这样从同一份源码使用不同配置进行不同构建时就比较方便,源码会一直保持干净整洁,多个构建之间不会相互影响.

    mkdir qt-build
	cd /d qt-build
	../qt-source/configure -prefix ../qt-install
	nmake
	nmake install

其中选项-prefix用来指明安装目录.

选项-developer-build 表示构建是用来做开发的,不是用来发布的.它会比标准构建包含有更多的导出符号,编译时也会使用更高的编译警告等级.

### 包含和排除Qt模块

通过使用Configure可以在Qt构建中包含或排除指定模块.但是要记住:许多模块是依赖其他模块而构建的.所以要特别注意依赖问题.

### 排除一个模块
选项-sikp 用来指定要从Qt构建中排除的模块.



	

