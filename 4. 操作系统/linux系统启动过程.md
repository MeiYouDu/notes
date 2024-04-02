# linux启动时我们会看到许多启动信息。
Linux系统的启动过程并不是大家想象中的那么复杂，其过程可以分为5个阶段：

1. 内核的引导。
2. 运行 init。
3. 系统初始化。
4. 建立终端。
5. 用户登录系统。
## 内核引导
当计算机打开电源后，首先是BIOS开机自检，按照BIOS中设置的启动设备（通常是硬盘）来启动。
操作系统接管硬件以后，首先读入 /boot 目录下的内核文件。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649738686972-9cf6d8cc-75e9-4a52-9738-6f4aba50e8b0.png#clientId=u8cc1d210-fb3a-4&from=paste&id=u3a2cfcfa&originHeight=135&originWidth=602&originalType=url&ratio=1&rotation=0&showTitle=false&size=3299&status=done&style=none&taskId=u172e0b5a-9786-4186-8735-fffcfd56f5e&title=)
## 运行init
init 进程是系统所有进程的起点，你可以把它比拟成系统所有进程的老祖宗，没有这个进程，系统中任何进程都不会启动。
init 程序首先是需要读取配置文件 /etc/inittab。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649738687126-ddec128f-bca0-4a46-be3c-4fca77dfaea3.png#clientId=u8cc1d210-fb3a-4&from=paste&id=u13021470&originHeight=131&originWidth=596&originalType=url&ratio=1&rotation=0&showTitle=false&size=6988&status=done&style=none&taskId=u6de9f990-e6d7-40d1-8936-126b0ef4404&title=)
### 运行级别
许多程序需要开机启动。它们在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）。
init进程的一大任务，就是去运行这些开机启动的程序。
但是，不同的场合需要启动不同的程序，比如用作服务器时，需要启动Apache，用作桌面就不需要。
Linux允许为不同的场合，分配不同的开机启动程序，这就叫做"运行级别"（runlevel）。也就是说，启动时根据"运行级别"，确定要运行哪些程序。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649738687136-917ba8e1-c054-47b9-a74d-5060d9d39a60.png#clientId=u8cc1d210-fb3a-4&from=paste&id=u98403d83&originHeight=114&originWidth=589&originalType=url&ratio=1&rotation=0&showTitle=false&size=8612&status=done&style=none&taskId=ufa049ef4-9145-432b-ab49-d90f7041847&title=)
Linux系统有7个运行级别(runlevel)：

- 运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
- 运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
- 运行级别2：多用户状态(没有NFS)
- 运行级别3：完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
- 运行级别4：系统未使用，保留
- 运行级别5：X11控制台，登陆后进入图形GUI模式
- 运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动
## 系统初始化
在init的配置文件中有这么一行： si::sysinit:/etc/rc.d/rc.sysinit 它调用执行了/etc/rc.d/rc.sysinit，而rc.sysinit是一个bash shell的脚本，它主要是完成一些系统初始化的工作，rc.sysinit是每一个运行级别都要首先运行的重要脚本。
它主要完成的工作有：激活交换分区，检查磁盘，加载硬件模块以及其它一些需要优先执行任务。
l5:5:wait:/etc/rc.d/rc 5
这一行表示以5为参数运行/etc/rc.d/rc，/etc/rc.d/rc是一个Shell脚本，它接受5作为参数，去执行/etc/rc.d/rc5.d/目录下的所有的rc启动脚本，/etc/rc.d/rc5.d/目录中的这些启动脚本实际上都是一些连接文件，而不是真正的rc启动脚本，真正的rc启动脚本实际上都是放在/etc/rc.d/init.d/目录下。
而这些rc启动脚本有着类似的用法，它们一般能接受start、stop、restart、status等参数。
/etc/rc.d/rc5.d/中的rc启动脚本通常是K或S开头的连接文件，对于以 S 开头的启动脚本，将以start参数来运行。
而如果发现存在相应的脚本也存在K打头的连接，而且已经处于运行态了(以/var/lock/subsys/下的文件作为标志)，则将首先以stop为参数停止这些已经启动了的守护进程，然后再重新运行。
这样做是为了保证是当init改变运行级别时，所有相关的守护进程都将重启。
至于在每个运行级中将运行哪些守护进程，用户可以通过chkconfig或setup中的"System Services"来自行设定。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649738687262-f31affd3-9720-42d3-9804-003502383bb4.png#clientId=u8cc1d210-fb3a-4&from=paste&id=ud24d7bc6&originHeight=206&originWidth=592&originalType=url&ratio=1&rotation=0&showTitle=false&size=11960&status=done&style=none&taskId=ufd53fe7b-b986-4009-b62f-93b38030e74&title=)
