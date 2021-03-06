# How-to-compile-LCM
总结LCM（Lightweight Communications and Marshalling）的编译方法。

由于工作需求使用到开源项目LCM（Lightweight Communications and Marshalling），用于传输车辆信息至PC端，
在使用过程中发现LCM使用简单、可靠性高，适应于我工作程序的调试。
LCM官网（http://lcm-proj.github.io/） 对于在linux、与windows等系统中编译都做了讲述，但我发现在linux环境下编译lcm库很容易，
但是在windows环境下却出现了一些问题，在网上的资料也不多，现根据自己的经验对在windows环境下编译LCM库做一些讲解：
###################################################################

1.下载gilb，LCM依赖于gilb. 
=
我下的是gilb2.20.5（32位），下载地址为：http://ftp.gnome.org/pub/gnome/binaries/win32/glib/2.20/
glib与gilb-dev都要下载，然后合并两个文件夹。
glib分32位与64位的，请下载对应的版本。

2.用vs打开lcm-1.3.1\WinSpecific\LCM.sln.
=
我使用的是LCM1.3.1版本，好像现在又更新了新的版本，但是编译方式应该没有改变。

3.添加附加包含目录、附加依赖项、附加库目录.  
=
3.1添加附加包含目录：  
添加环境变量GLIB_PATH=glib的路径，如GLIB_PATH=E:\gilb  
在vs附加包含目录中添加$(GLIB_PATH)\include\glib-2.0与$(GLIB_PATH)\lib\glib-2.0\include  
3.2添加附加依赖项  
在vs附加依赖项中添加：glib-2.0.lib  
    gthread-2.0.lib  
3.3添加附加库目录  
在vs附加库目录中添加：$(GLIB_PATH)\lib

4.添加动态库  
=
可在vs中调试->环境中添加path=dll的路径（如E:\gilb\bin），调用动态库。  
最简单的方式为将所下载的glib文件夹中的bin文件内的所有文件复制到所debug文件中或者release文件夹中（取决于调试模式）；  
其实所需要的是libglib-2.0-0.dll与libgthread-2.0-0.dll两个.dll文件。  

5.随后即可编译通过。
=

######################################################################
######################################################################

`注意事项`：
=

***在刚使用LCM的过程中，发现有时候网线直连能通，无线却不通；有时网线直连不通，无线却通；更有可能不紧直连不通无线也不通。
经我的调查，无意中发现，这是因为lcm的组播239.255.76.67没有加到相对应的网卡驱动中，比如我们电脑都会装一些虚拟机，这些虚拟机会产生一些
网卡，这个lcm的组播地址经常会加入到这些网卡中，致使lcm通信故障。我没有研究出相对应的解决方法，只使用了一种最傻的操作方法：将除了需要用的
网卡之外的网卡全部禁用（比如将除了无线网卡之外的网卡全部禁用，这样就能将lcm的组播就会自动的加入到无线网卡中）。倘若大家有更好的方法，方便的
话请分享与我。

***此外还需注意的就是在通过路由器无线传输的时候，对每次发的包的大小应做大小的限制，以我的经验是：每次发的包要小于100K，不然会出现大量丢包的
情况；在用有线传输貌似没有这个限制，只要小于官网说的2M即可（具体自测）。

现提供我自己编译的一些LCM动态库文件供大家使用。

本文档是结合网上现有资料总结出来的，如有雷同与侵权之处，请及时联系我删除，谢谢！
