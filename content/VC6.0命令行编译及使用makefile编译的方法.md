Title: VC6.0命令行编译及使用makefile编译的方法
Date: 2014-12-27 
Category: websites 
Tags: Windows 
Slug: Windows
Author: tianruoxuanhe
Summary:

##VC6.0命令行编译及使用makefile编译的方法

转载、来源：http://blog.csdn.net/cs_21cn/article/details/8015596

VC6是挺经典的一个IDE，但有时编译比较慢。作为一个典型的程序员，我总想让电脑多做点事，自己少点事。编译软件也一样，又是能够执行后不管了，让程序自己慢慢编译，人可以做其他事情，或者直接写成批处理，自动的一个个慢慢执行编译就好了。所以一直想探索一下vc6环境怎么使用命令行编译，以及vc6怎么使用makefile编译。

今天终于找到办法！虽然还停留在初浅的层面，但还是可以做到命令行自动编译了。所以写下来既是分享，也供自己以后查阅。

###一、VC6命令行编译
VC6对应的可执行文件是msdev，在命令行窗口（以下简称cmd）中输入msdev /? 回车就能看到msdev的命令行参数帮助了（如果提示未知命令，那么请使用vc6安装目录下的VCVARS32.BAT设置环境）。帮助信息如下：

F:\VC_Code\maintest>msdev/?

Usage:

  MSDEV [myprj.dsp|mywksp.dsw]  - load project/workspace

        [<filename>]            - load source file

        /?                      - display usageinformation

        /EX <macroname>         - execute a VBScript macro

        /OUT <filename>         - redirect command line output to afile

        /USEENV                 - ignoretools.options.directories settings

        /MAKE [<target>] [...]  - build specified target(s)

              [<project> -<platform> <configname>]

              [[<project>|ALL] -[DEBUG|RELEASE|ALL]]

              /CLEAN            - delete intermediate files butdon't build

              /REBUILD          - clean and build

              /NORECURSE        - don't build dependent projects

 

下面逐个参数论述：

[myprj.dsp|mywksp.dsw]  - load project/workspace

[<filename>]            - load source file

/?                      - display usage information

/EX<macroname>         - execute aVBScript macro

/USEENV                 - ignoretools.options.directories settings

这几个选项跟本文想论述的内容关系不大，也比较好懂，有兴趣的人自己动手试一下就知道了。

/OUT<filename>         - redirectcommand line output to a file 把输入信息重定向到文件中，可以跟编译指令结合，保存编译过程的输出信息，有利于确认编译成功与否，特别是使用批处理批量编译时。

/MAKE 就是主角了，就是编译指令了。下面以几个示例来说明吧：

####（1）编译工作区文件（dsw）的项目

msdev maintest.dsw/make "all - win32 debug" /out f:\result.txt

maintest.dsw 工作区文件

/make 编译指令

all 编译所有项目（project），如果工作区有多个项目，也可以指定只编译特定项目，可以使用记事本打开dsw文件，可以看到里面包含的各个项目文件。

win32 平台（platform），vc6运行基本也就是win32平台了，所以一般该参数可以省略。

debug 编译设置项（configname），可以在DEBUG|RELEASE|ALL选择

/out f:\result.txt 重定向输出，把编译过程的信息输出到f:\result.txt中。

所以上面的命令行的意思就是：把maintest.dsw工作区里的所有项目编译debug版的产品，并把编译过程的信息输出到f:\result.txt中。

据此可以容易理解下面几个命令行的意思吧：

msdev maintest.dsw /make "all - win32release"  –所有项目编译release版

msdev maintest.dsw /make "all -all"  --所有项目编译debug和release版

msdev maintest.dsw /make "maintest -release" –编译工作区中的maintest项目的release版

####（2）编译项目文件（dsp）的项目

msdev maintest.dsp/make "all - win32 debug" /out f:\result.txt

maintest.dsp 项目配置文件

其他参数都跟编译工作区文件（dsw）的项目一样，注意其他的all，一直保持all就行了，因为项目配置文件里面也就只包含一个项目了。

/CLEAN            - delete intermediate files butdon't build

/REBUILD          - clean and build

跟图形化VC6 IDE中的BUILD->CLEAN和BUILD->REBUILD ALL意义是一样的，这里就不赘述了。

###二、VC6使用makefile编译
VC6 IDE带的makefile编译程序是nmake，也可以在cmd中输入nmake /?获取详细的帮助信息，因为我自己没有一一使用过，所以也就不一一论述了。下面只提供一个使用的方法：

（1）在VC6图形化界面上打开一个项目，然后执行Project->Export Makefile，会在项目的目录下生成mak和dep两个文件，这就是项目配置的makefile了。

（2）然后使用如下命令行编译了：

NMAKE /f "maintest.mak"CFG="maintest - Win32 Debug" /y /d

NMAKE /f "maintest.mak"CFG="maintest - Win32 Release" /y /d

注意这里的CFG="maintest- Win32 Debug"区分大小写的，而msdev的/make "all - win32 release"是不区分大小写的。

如果你有兴趣继续探索，可以使用记事本打开dsp和mak文件看一下。