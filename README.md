# “小红”操作系统设计和功能说明文档
![welcome](/Users/hongjiayong/Desktop/OsPic/xiaohong.png)
## 目录
<!-- MarkdownTOC -->

- [队员组成](#队员组成)
- [开发环境说明](#开发环境说明)
	- [开发系统环境](#开发系统环境)
	- [相关软件](#相关软件)
	- [开发语言](#开发语言)
	- [项目管理平台](#项目管理平台)
- [“小红”操作系统设计说明](#小红操作系统设计说明)
	- [系统概要](#系统概要)
		- [为什么叫“小红”](#为什么叫小红)
		- [操作系统设计思路](#操作系统设计思路)
		- [操作系统组成](#操作系统组成)
		- [操作系统亮点](#操作系统亮点)
		- [操作系统各部分简述](#操作系统各部分简述)
			- [内核](#内核)
			- [控制台](#控制台)
			- [文件系统](#文件系统)
			- [应用](#应用)
- [“小红”操作系统基本功能说明](#小红操作系统基本功能说明)
	- [功能汇总](#功能汇总)
	- [welcome](#welcome)
	- [clear](#clear)
	- [animation](#animation)
	- [ls](#ls)
	- [proc](#proc)
	- [help](#help)
	- [saolei](#saolei)
		- [操作说明](#操作说明)
	- [2048](#2048)
		- [操作说明](#操作说明-1)
	- [caculator](#caculator)
		- [操作说明](#操作说明-2)
	- [duihua](#duihua)
	- [print](#print)
	- [newfile](#newfile)
	- [read](#read)
	- [delete](#delete)
	- [edit+](#edit)
	- [edit](#edit-1)
	- [add](#add)
	- [move](#move)
	- [login](#login)
	- [loginout](#loginout)
- [“小红”操作系统主打功能详细说明](#小红操作系统主打功能详细说明)
	- [基于用户的二级文件系统](#基于用户的二级文件系统)
		- [例子](#例子)
	- [文件恢复和用户记录功能](#文件恢复和用户记录功能)
- [参考文献](#参考文献)
- [鸣谢](#鸣谢)

<!-- /MarkdownTOC -->




<h2 id="队员组成"> 队员组成</h2>
| 学号 | 姓名 | 备注 | 分工 | 分数比例 |
| :--------: | :--------:| :--: | :-------:|:----: |
| 1452822   |  洪嘉勇   |  队长 | 项目管理 二级文件系统编写 | 35% |
| 1453093   |   夏陈   |  队员 | 应用制作 系统调用等技术攻关 | 35% |
| 1452690   |   赵旭    | 队员  | 环境搭建 文档、PPT制作 | 30% |

<h2 id="开发环境说明"> 开发环境说明 </h2>
<h3 id="开发系统环境"> 开发系统环境 </h3>
	Ubuntu-16.04 32位
<h3 id="相关软件"> 相关软件 </h3>
	bochs
<h3 id="开发语言"> 开发语言 </h3>
- C语言
- 汇编

<h3 id="项目管理平台"> 项目管理平台 </h3>
**coding**
	
![coding](/Users/hongjiayong/Desktop/OsPic/coding1.png)
![coding](/Users/hongjiayong/Desktop/OsPic/coding2.png)

>由于失误没有创建公有项目 助教和老师可以通过 账号tj_island@163.com 密码hjy673773登录coding
在OS-Fighting中查看 

**另外在github上我们也有项目存放**<br/>
[github](https://github.com/Hjyheart/XHOS)

	

<h2 id="小红操作系统设计说明"> “小红”操作系统设计说明 </h2>
<h3 id="系统概要">系统概要 </h3>
<h4 id="为什么叫小红"> 为什么叫“小红” </h4>
	我们的队伍的两名核心队员姓的首字母分别为“X”和“H”，合起来为“XH”，我们觉得取名“小红”
	很接地气,我们的OS便由此得名
<h4 id="操作系统设计思路"> 操作系统设计思路 </h4>
	这次项目虽然老师和助教为我们制定了相应的难度评级，但我们认为既然时间这么充裕，我们没
	有必要强行按照老师和助教制定的难度评级制作我们的操作系统，我们能在ddl前加上什么功能就
	加上什么功能，这样的开发才有快感。所以最后我们的产品麻雀虽小五脏俱全。
	操作系统的GUI我们打算就用控制台，毕竟制作太过于复杂的GUI很花时间也超出了我们的能力范
	围。
	从0开始开发着实困难，还好我们有Orange提供的源码，针对它的缺陷，我们展开相应的分析，最
	后决定在它的不完善的文件系统上下功夫。Orange的文件系统只实现了文件最基本的操作，是一个
	一级的文件系统，而且不具有文件记忆功能，在操作完文件完成硬盘读写之后关机再次开机文件系统
	会默认没有任何文件，然而硬盘是已经操作过的，这是一个非常大的缺陷。我们通过努力，实现了
	基于用户的二级文件系统和用户记忆恢复功能。
	做完这些之后我们发现还有很多时间，我们并不能因此停下脚步，于是我们打算想办法让我们的操作
	系统显得更加有趣，于是我们又花了不少时间学习如何将应用装载到操作系统上。最后我们成功为操
	作系统装上了开机动画和2048、扫雷和一个简易计算器。
	最后我们又尝试着在系统调用中定义了一个属于我们自己的消息，并成功完善了它的消息传递机制。
	最终用户可以在我们的控制台上创建自己的用户，创建属于自己的文件，并进行最基本的文件操作。
	并可以打开相应的游戏进行娱乐。
	
<h4 id="操作系统组成"> 操作系统组成 </h4>
- boot（引导）
- kernel（内核）
- command（应用集）
- fs（文件系统）
- lib（可用代码库）
- include（头文件集）
- mm（内存调度系统）

<h4 id="操作系统亮点"> 操作系统亮点 </h4>
- 实现控制台操作
- **改写文件系统代码实现用户文件隔离的二级文件系统**
- **改写文件系统代码实现文件恢复和用户记录功能**
- 实现文件的增删改查基本操作
- 实现一套易用的命令行指令
- 2048游戏（应用）
- 扫雷（应用）
- 简易计算器（应用）

<h4 id="操作系统各部分简述"> 操作系统各部分简述 </h4>
<h5 id="内核"> 内核 </h5>
>超过128k的内核！！！

	我们能力有限，无法自己创造一个属于我们的内核。因此为了后续的开发，我们直接借鉴了
	Orange书后的代码，使用了它的简易内核。但在后续的编写中，我们的内核大小了超过了
	作者所设置的128k，我们不得不将内核进行了扩容。
<h5 id="控制台"> 控制台 </h5>
>与文件系统关联的控制台！！！

	控制台的实现我们在通读Orange的tty部分之后，成功实现了一个和文件系统相关联的控制
	台，通过在特殊文件中读写实现控制台的输入和输出。
<h5 id="文件系统"> 文件系统 </h5>
>二级文件系统&文件记忆恢复功能！！！

	Orange中的文件系统并没有文件夹之类的概念，是一个一级的文件系统。我们对文件系统进
	行了修改，使得文件系统变成了二级的文件系统，每一位用户在登录之后只能看到自己的文件
	从而实现了文件的隔离。
	与此同时，我们实现了文件系统的记忆功能，在再次开机之后，我们通过读取特殊文件将用户
	们之前的文件操作全部恢复，也就是说，我们的操作系统有文件记忆功能，这是Orange的源码
	所没有实现的。
<h5 id="应用"> 应用 </h5>
>酷炫的控制台游戏！！！

	我们通过努力在控制台上实现了2048和扫雷，通过手动输入移动方向和位置来操作，同时支持
	随时退出游戏，用户体验极佳。

<h2 id="小红操作系统基本功能说明"> “小红”操作系统基本功能说明 </h2>
<h3 id="功能汇总"> 功能汇总 </h3>
| 命令 | 参数 | 概述 | 是否需要登录 |
|:---:|:----:|:---:| :---: |
| welcome | - | 打印欢迎语句 | No |
| clear | - | 清屏并打印欢迎语句 | No |
| animation | - | 播放开机动画 | No |
| ls | - | 输出对应用户所属的文件列表 | Yes |
| proc | - | 输出当前所有进程的状态 | No |
| help | - | 输出所有可用命令 | No |
| saolei | - | 开始扫雷游戏 | No |
| 2048 | - | 开始2048游戏 | No |
| caculator | - | 打开简易计算器 | No |
| duihua | str【char * 】 | 打开用消息传递机制实现的打印功能 | No |
| print | str【char * 】 | 在控制台打印输入的语句 | No |
| newfile | filename【char * 】 content【char * 】 | 在当前用户下创建文件 并输入初始化内容 | Yes |
| read | filename 【char * 】 | 读取当前用户的文件 | Yes |
| delete | filename 【char * 】 | 删除当前用户下的某一文件 | Yes |
| edit | filename 【char * 】 content【char * 】 | 编辑当前用户下的某一文件 覆盖其内容 |  Yes |
| edit+ | filename 【char * 】 content 【char * 】  | 编辑当前用户下的某一文件 追加 | Yes |
| add | username 【char * 】 password 【char * 】 | 创建用户 | No |
| move | username 【char * 】password 【char * 】 | 删除用户 | No |
| login | username 【char * 】 password 【char * 】 | 登录 | No |
| loginout | - | 登出 | No |

<h3 id="welcome"> welcome </h3>
![welcome](/Users/hongjiayong/Desktop/OsPic/welcome.png)

	键入welcome后将输出欢迎语句和小组成员列表
	
<h3 id="clear"> clear </h3>
![clear](/Users/hongjiayong/Desktop/OsPic/clear.png)

	键入clear后将清屏并打印欢迎语句和小组成员列表
	
<h3 id="animation"> animation </h3>
![animation](/Users/hongjiayong/Desktop/OsPic/donghua.png)

	键入donghua后将执行开机动画打印蟠龙
	
<h3 id="ls"> ls </h3>
![ls](/Users/hongjiayong/Desktop/OsPic/ls.png)

	键入ls后将输出当前用户下的文件列表

<h3 id="proc"> proc </h3>
![proc](/Users/hongjiayong/Desktop/OsPic/proc.png)

	键入proc后将输出当前所有进程的状态
	
<h3 id="help"> help </h3>
![help](/Users/hongjiayong/Desktop/OsPic/help.png)

	键入help后将输出当前所有可用命令
	
<h3 id="saolei"> saolei </h3>
![saolei](/Users/hongjiayong/Desktop/OsPic/saolei.png)

	键入saolei后即可开始扫雷游戏
<h4 id="操作说明"> 操作说明 </h4>
	输入行列编号踩雷
	输入q退出游戏

<h3 id="2048"> 2048 </h3>
![2048](/Users/hongjiayong/Desktop/OsPic/2048.png)

	键入2048后即可开始2048游戏
<h4 id="操作说明-1"> 操作说明 </h4>
| 按键 | 功能 |
| :--: | :--: |
| W | 向上 |
| A | 向左 |
| S | 向下 |
| D | 向右 |
| Q | 退出游戏 |

<h3 id="caculator"> caculator </h3>
![caculator](/Users/hongjiayong/Desktop/OsPic/caculator.png)

	键入caculator即可打开简易计算器
<h4 id="操作说明-2"> 操作说明 </h4>
	仅支持二元整数的加减乘除运算
	键入q退出计算器
<h3 id="duihua"> duihua </h3>
![duihua](/Users/hongjiayong/Desktop/OsPic/对话.png)

	键入duihua和参数字符串就能通过消息传递的机制打印字符串
<h3 id="print"> print </h3>
![print](/Users/hongjiayong/Desktop/OsPic/print.png)

	键入print和参数字符串之后将会在控制台打印该字符串
<h3 id="newfile"> newfile </h3>
![newfile](/Users/hongjiayong/Desktop/OsPic/newfile.png)

	键入newfile和文件名之后将会在该用户目录下新建该文件，并初始化内容

![newfile](/Users/hongjiayong/Desktop/OsPic/fileexist.png)

	如果执行新建文件命令之后该文件存在的情况下将会给出文件存在的提示
	
<h3 id="read"> read </h3>
![read](/Users/hongjiayong/Desktop/OsPic/read.png)

	键入read和文件名之后将会在该用户目录下查找该文件，找到之后将其内容输出

![read](/Users/hongjiayong/Desktop/OsPic/readfail.png)

	如果未能查找到将会输出失败提示
	
<h3 id="delete"> delete </h3>
![delete](/Users/hongjiayong/Desktop/OsPic/delete.png)

	键入delete和文件名之后将会在该用户目录下查找该文件，找到之后将其删除，我们可以
	看到删除后再次读取文件已经找不到了

<h3 id="edit+"> edit+ </h3>
![edit+](/Users/hongjiayong/Desktop/OsPic/edit+.png)

	键入edit+和文件名及追加字符串之后将在原文件内容后追加内容
	
<h3 id="edit"> edit </h3>
![edit](/Users/hongjiayong/Desktop/OsPic/edit.png)

	键入edit和文件名及覆盖字符串之后将以该字符串覆盖原内容
	
<h3 id="add"> add </h3>
![add](/Users/hongjiayong/Desktop/OsPic/add.png)

	键入add和用户名及密码之后键入系统管理密码就可以新建用户，新建完毕之后就可以用用
	户名和密码进行登录
	
<h3 id="move"> move </h3>
![move](/Users/hongjiayong/Desktop/OsPic/move.png)

	键入move和用户名及密码之后键入管理员密码就可以删除相应用户，再次登录就失效了
	
<h3 id="login"> login </h3>
![login](/Users/hongjiayong/Desktop/OsPic/login.png)

	键入login和用户名及密码就可以执行登录操作

<h3 id="loginout"> loginout </h3>
![loginout](/Users/hongjiayong/Desktop/OsPic/loginout.png)

	键入loginout后就会执行登出操作，退后初始状态
	
<h2 id="小红操作系统主打功能详细说明"> “小红”操作系统主打功能详细说明 </h2>
<h3 id="基于用户的二级文件系统"> 基于用户的二级文件系统 </h3>
	每一位用户在注册成功之后会拥有自己的一个目录，在登录状态下创建的文件会生成在相应
	目录下。只有在登录之后才有权限操作属于自己的文件，用户之间的文件是相互隔离的。

<h4 id="例子"> 例子 </h4>
![example](/Users/hongjiayong/Desktop/OsPic/user1.png)

> 一. 新建两个用户hjy和xia 密码分别为hjy和xia

![example](/Users/hongjiayong/Desktop/OsPic/user2.png)

> 二. 登录hjy之后新建文件test和test1，执行ls后将会看到目录下有test和test1

![example](/Users/hongjiayong/Desktop/OsPic/user3.png)

> 三. 登录xia之后新建和hjy的同名文件test，内容与hjy的test不同，在执行read之后不会混淆

<h3 id="文件恢复和用户记录功能"> 文件恢复和用户记录功能 </h3>
![file](/Users/hongjiayong/Desktop/OsPic/file1.png)
>一. 登录xia，新建一个文件test，然后关闭操作系统

![file](/Users/hongjiayong/Desktop/OsPic/file2.png)
>二. 再次打开操作系统，可以直接登录xia，用户的信息还在，直接读取test可以看到文件都还在

<h2 id="参考文献"> 参考文献 </h2>
|书名|作者|出版社|
|:-:|:-:|:-:|
| 《Orange S:一个操作系统的实现》 | 于渊 | 电子工业出版社 |
| 《Linux内核源代码情景分析》 | 毛德操 / 胡希明 | 浙江大学出版社 |
| 《Linux命令行大全》 | William E.Shotts  | 人民邮电出版社 |

<h2 id="鸣谢"> 鸣谢 </h2>
**感谢助教和老师为这门课设所付出的辛勤和汗水！！**
	

