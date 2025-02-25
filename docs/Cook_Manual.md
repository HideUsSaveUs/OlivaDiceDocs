# Mirai和XQ的框架食用指北

> *世界是属于每一个人的。要创造一个充满逻辑并尊重每一个人的世界。*    
> *——《[Новый Элемент Расселения](https://ru.wikipedia.org/wiki/%D0%9D%D0%BE%D0%B2%D1%8B%D0%B9_%D1%8D%D0%BB%D0%B5%D0%BC%D0%B5%D0%BD%D1%82_%D1%80%D0%B0%D1%81%D1%81%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F)》A.D.1960 Москва*

![DIXE(OLIVADICE)](_static/DIXE_OLIVADICE.jpg)
#### 前言：
为节约版面篇幅   
以下统称`青果掷骰核心机器人`为`骰娘 `  
本篇仅适用于`青果掷骰核心机器人`的骰娘，   
但理论上一样可以适用与其他核心的骰娘，   
如果您在阅读完本文档后，以本文档描述的方法去使用其他核心的骰娘，出现了问题，本食用指北概不负责。   

本篇仅介绍Mirai和XQ的框架登录和初步设置  
Mirai两种，XQ一种，共三种  
看完以后你可以学会的是：   
1. XQ的额外设置  
2. 怎么设置自动登录  
3. 怎么设置master  
4. 怎么放图片和声音等  
5. 怎么更新骰娘  
6. 各个框架使用的协议与注意事项  
7. 骰娘不回复消息的方法   
8. 骰娘被屏蔽了的方法   
9. 怎么放牌堆、Mod和Lua脚本文件，以及识别方法   
10. 怎么迁移到另一个框架或者另一台电脑上去
11. 下一步去哪里看教程  
12. 常见术语定义
--------

## 1.**XQ的额外设置**
XQ框架由于杀毒软件会误报  
然后运行着就直接给你删了  
所以需要在杀毒软件那设置排除  
具体设置就是**整个框架的文件夹**都排除  
怎么设置的话...  
由于杀毒软件有各种各样的  
这里只介绍win10自带的`Microsoft Defender`  
**迈克菲**，360等杀毒软件请自行解决  
 - 打开`设置`
 - 打开`更新和安全`
 - 点击左侧的`windows安全中心`   
 有可能会因为太窄变成只有`盾牌图标`  
 - 点击右侧的`打开windows安全中心`   
 - 然后右边会出现扫描历史记录或者安全概览等  
 - 点击左边的`病毒威胁和防护`菜单栏   
 - 点击右边下面的`“病毒和威胁防护”设置`选项   
 当然有可能是`管理设置`  
 - 点击下面的`排除项`下的`添加或删除排除项`  
 - 点击`添加排除项`  
 - 点击`文件夹`  
 - 将`整个骰娘整合包解压出来的文件夹`设置信任  
 - 完毕  

------

## 2.**怎么设置自动登录**  
#### Mirai篇:  
首先你应该有一个整合包  
然后解压出来看一下  
有config/console/autologin.yml的   
是新版或者M2版  
有config.txt的是原始版  
根据文件里面的内容查看你用的是哪一个版本  

**Mirai2**

* 在你的骰娘目录下  
如果用的是整合包  
你应该可以看见如下文件路径和文件  
`config\Console\AutoLogin.yml`  
如果不出意外，里面有

  ```yaml
  accounts: 
    - # 账号, 现只支持 QQ 数字账号
      account: 123456
      password: 
        # 密码种类, 可选 PLAIN 或 MD5
        kind: PLAIN
        # 密码内容, PLAIN 时为密码文本, MD5 时为 16 进制
        value: pwd
      # 账号配置. 可用配置列表 (注意大小写):
      # "protocol": "ANDROID_PHONE" / "ANDROID_PAD" / "ANDROID_WATCH"
      configuration: 
        protocol: ANDROID_PHONE
  ```

* Mirai2有两种不同的设置自动登录的方式   
你可以从里面任选一种进行尝试   
    * **第一种：直接改文件**   
修改`account: `后面的数字`123456`，那是骰娘的qq号   
修改`value: `后面的文字`pwd`，那是骰娘的密码   
修改`protocol: `后面的`ANDROID_PHONE`为`ANDROID_PAD`   
那是设备协议   
然后保存，回到骰娘包的最顶层   
双击运行`start by jre.bat`启动骰娘   
一般来说会提示你扫码登录或者是滑动验证码登录
      > **注意：不要直接用记事本打开修改！会出错！**  
      最好使用`notepad++`,[`visual studio code`](https://code.visualstudio.com/)等工具进行编辑
      
  * **第二种：通过指令设置**   
首先运行`start by jre.bat`，你会发现出现了一个命令提示符的窗口   
输入以下三条命令   
    ```
    autologin clear
    autologin add 123456789 abcdefghi
    autologin setconfig 123456789 protocol ANDROID_PAD
    ```
    `123456789`是你的骰娘`qq号`   
    `abcdefghi`是你的骰娘`密码`   
    * NOTE：这里的第三条可以不输入，不过代价就是你无法同时使用手机登录
    * 以上三条指令的作用分别是：   
      * 清理模板示例
      * 设置自动登录账号
      * 设置登录协议
  
    * 当设置完成后，你需要等待**两分钟**让设置生效才能继续关闭窗口（m2无法立即保存你的自动登录设置）   
    所以趁着这个时间你可以输入   
    `login 123456789 abcdefghi`   
    来完成你的骰娘账号的第一次登录   
    * 一般来说会需要你扫描二维码或者是滑动滑块进行账号验证   
    当你完成了这些步骤以后一般来说两分钟也已经到了   
    * 当你下次登录的时候就只要直接打开   
    `start by jre.bat`就好了

> **注意：**   
`Mirai2`整合包需要***Chrome***作为***默认浏览器***才能完成设置   

 * 无法验证的手动方法：   
 如果Mirai2提示   
 “当前上网环境异常，清更换网络环境或在常用设备上登录或稍后再试。”    
 那么你需要的就是手动完成验证了    
手动的话一般来说任何基于`Chrome内核`的浏览器都可以    
比如说`Firefox`，`Chrome`，`Edge`等等   
当然推荐使用`Chrome`作为默认浏览器    
因为本教程默认使用`Chrome`浏览器作为范例    
请务必不要使用IE浏览器，否则造成的一切问题请自行解决    
 1. 请修改`OlivaDice.bat`内的    
`jre\bin\java -cp`    
为    
`jre\bin\java -Dmirai.slider.captcha.supported -cp`    
简称在`-cp`前添加` -Dmirai.slider.captcha.supported`    
2. 删除`plugins`文件夹下的
`mirai-login-solver-selenium-1.0-dev-16-all.jar`
3. 重启`OlivaDice.bat`
之后会跳出一个浏览器窗口，里面有一个滑块验证码   
    >* 有大概率打开网页的时候是一片空白    
    遇到这种情况请尝试使用手机qq打开浏览器跳出的链接    
    可以跳过4-8步直接进入9或者10    
    此操作方法来自最近的新骰主    
    ——2021年8月12日20补充   
4. 先别急着滑，对着浏览器按F12这个按键    
切换到`Network`标签   
不会的话去`百度`，`谷歌`等等任何你想得到的搜索引擎里搜索
`浏览器的F12怎么使用`
当学会了再继续，或者换成XQ
5. 滑动滑块通过验证   
6. 你会看见下方的`Network`里出现了一个名为
`cap_union_new_verify`的项目    
左键点击这个项目    
7. 在右侧点击`Preview`    
你会看见`ticket: "balabalbalablabalabal"`   
右键`ticket`，选择`Copy Value`    
8. 黏贴至Mirai2跳出来的文字输入框内   
这时Mirai2会再次跳出一个窗口    
9. 点击设备锁验证   
10. 手机扫码验证，然后关闭弹出的窗口即可    
11. [资料辅助验证为唯一方式的情形，请尝试使用QQ内嵌浏览器打开链接，或尝试本Issues所提到的其它方法。](https://github.com/mamoe/mirai/issues/1297)


* 三种协议分别是
  * `ANDROID_PHONE` 手机协议
  * `ANDROID_PAD` 平板协议
  * `ANDROID_WATCH` 手表协议
  这里比较推荐的就是pad协议或者watch协议

------

**Mirai新版**  

* 在你的骰娘目录下  
如果用的是整合包  
你应该可以看见如下文件路径和文件  
`config/console/autologin.yml`  
如果不出意外，里面有  

  ```yaml
  plainPasswords:  
      123456789:abcdefghi  
  md5Passwords:  
      xxxxxxxxxxxxxxxxxxxxxx  
  ```
* 123456789换成你的骰娘qq  
* abcdefghi换成你的骰娘密码   
  中间的冒号记得别删，这样就可以自动登录了  

**Mirai原始版**  

* 在你的骰娘目录下  
如果用的是整合包  
你应该可以看见如下文件路径和文件  
`config.txt`  
如果不出意外，里面有  

  ```bash
  DEBUG
  NOUPDATE
  #在----------下面可以添加需要在每次启动时输入得指令
  #请注意，指令部分中#并不起效，miraiOK会原样输入到console
  例如:
  login 123456789 TestMiraiOK
  say 655057127 MiraiOK_published!
  ----------
  ```  

  这样一段话  
  在下面一行输入  
  `login 123456789 abcdefgh`  
  再按一个回车，然后保存即可  

#### XQ篇:  
* XQ的自动登录只要你登录上了就可以了  

------

## 3.**怎么设置master**

#### Mirai篇:
* 当骰娘登录以后  
你右键猫猫头  
此时会跳出一个指令框  
最下面应该会有  

|Dice!
|----
|关于
* 选中`Dice！`  
往右可以看见`Master模式切换`  
点下去，如果你能看见`Master模式已开启`  
那么你就可以私聊你的骰娘发送.master来设置master了  
或者你也可以选择`综合管理`，这里就不多介绍了  


#### 注意:
master权限可以执行cmd指令    
具体指令为:.master cmd 你想执行的指令    
示例:
```
 .system cmd mshta vbscript:CreateObject("Wscript.Shell").popup("这是一个弹窗",7,"title",64)(window.close)
```
所以请不要随意将master权限交给他人    
毕竟cmd可以做的事情还是很多的

#### XQ篇:
  * XQ请选中`插件扩展`栏  
  然后`右键`CQXQ插件  
  点击`设置插件`  
  点击`Dice!`  
  点击右边的`菜单`  
  可以看见`Master模式切换`  
  点下去，如果你能看见`Master模式已开启`  
  那么你就可以私聊你的骰娘发送.master来设置master了  
  或者你也可以选择`综合管理`，这里就不多介绍了  

------

## 4.**怎么放图片和声音等**
#### Mirai:  
图片放在你的骰娘包里的`\jre\bin\data\image`下面  
声音放在你的骰娘包里的`\jre\bin\data\record`下面  
#### XQ:  
图片放在你的骰娘包里的`data\image`下面  
声音放在你的骰娘包里的`data\record`下面  

------

## 5.**怎么更新骰娘**
#### Mirai2:
关闭框架/卸载骰娘插件    
然后替换`\data\MiraiNative\plugins`目录下的对应dll和json文件    
然后重启框架/重载骰娘插件并启用    
推荐直接关了替换了再开    
以防指令输入错误    
指令卸载并加载的方法示例:    
```
> npm list
共加载了 1 个 Mirai Native 插件
Id：0 标识符：com.w4123.dice 名称：OlivaDice(DIXE) 版本：2.5.2CHAOS4.Oliva.1.2.12 状态：已启用 已加载
> npm unload 0
2021-06-07 18:53:14 I/MiraiNative: 插件 "com.w4123.dice" (com.w4123.dice.dll) (ID: 1) 已被卸载，返回值为 0 。


这时候你就可以替换对应的dll和json文件了
替换完以后看下面

> npm load com.w4123.dice.dll
2021-06-07 18:53:17 I/MiraiNative: 插件 com.w4123.dice.dll 已被加载，返回值为 0 。
> npm list
共加载了 1 个 Mirai Native 插件
Id：1 标识符：com.w4123.dice 名称：OlivaDice(DIXE) 版本：2.5.2CHAOS4.Oliva.1.2.12 状态：已启用 已加载
> npm enable 1
插件 com.w4123.dice 已启用。
```
#### Mirai新版:
关闭框架/卸载骰娘插件  
然后替换`\data\MiraiNative\plugins`目录下的对应dll和json文件  
然后重启框架/重载骰娘插件并启用  
#### Mirai原始版:
关闭框架/卸载骰娘插件  
然后替换`plugins\MiraiNative\plugins`目录下的对应dll和json文件  
然后重启框架/重载骰娘插件并启用  
#### XQ:  
关闭框架/卸载骰娘插件  
然后替换`CQPlugins`目录下的对应dll和json文件  
然后重启框架/重载骰娘插件并启用  

------

## 6.**各个框架使用的协议与注意事项**
**Mirai框架**使用QQ HD协议  
即安卓平板版  
所以可以再开着骰娘的同时使用手机QQ登录或者使用电脑端登录  
**XQ框架**使用PC端协议  
所以只能在手机QQ上登录  
或者在手机QQ HD上登录  
即:两个都是手机  
注:别想了，那个TIM指的是PC端TIM（笑）

------

## 7. **骰娘不回复消息了怎么办?**  
#### 看看后台有没有回复?  
1. 如果后台日志有回复  
   * 你的骰娘被屏蔽了  
2. 如果后台日志没回复  
   * 重启试试  

------

## 8.**骰娘被屏蔽了怎么办?**  
首先要明确一点  
任何一个骰娘都有被屏蔽的几率  
只是多与少，我们能做的只有降低几率  

#### 以下是玄学方法  
1. 改个密码  
   * 纯粹玄学  
2. QQ会员  
   * 这个方法有一定帮助，但是不要迷信  

#### 以下是常用方法  
* 等级挂高一点，屏蔽几率就小一点  
  * 这个是最简单的方法，关闭所有插件挂个几天就好了  
  * XQ有加速，建议使用XQ进行挂机  
* 附体水群  
  * 结合上面的一起食用效果更佳  

------

## 9.怎么放牌堆和Mod和Lua脚本文件

#### 牌堆和Mod篇

* 怎么识别你下载的是牌堆还是mod

用记事本打开你下载的json文件  
你会发现里面有一大堆的文字  
通常来说这些都是用utf-8编码写的所以不会乱码  
你只要观察开头的几行就好了  

* 牌堆的文件内容格式一般是
  ```json
  {
    "_祈愿内容1": [
        "a1",
        "a2",
        "a3",
        "a4"
    ],
    "_祈愿内容2": [
        "b1",
        "b2",
        "b3",
        "b4"
    ],
    "_祈愿抽取词条": [
        "::5::{_祈愿内容1}",
        "::20::{_祈愿内容2}"
    ],
    "祈愿1":["::5::随着你的祈愿,你抽取到了{_祈愿抽取词条}"]
  }
  ```
* mod文件的内容格式一般是
  ```json
  {
    "mod": "mod名称",
    "author": "mod作者",
    "brief": "简介",
    "comment": "具体介绍",
    "helpdoc": {
        "具体帮助词条1": "帮助词条内容",
        "具体帮助词条2": "帮助词条内容",
        "具体帮助词条3": "帮助词条内容"
    }
  }
  ```

#### 牌堆、Mod、Lua脚本文件的安装必要条件

当你成功登录你的骰娘账号以后  
你会发现在文件夹内出现了  
`Dice+你的骰娘qq号`的文件夹   
这个文件夹就是安装的必要条件   

#### 牌堆的安装方法

* 在里面新建一个名为`publicdeck`的文件夹   
  然后就可以在里面放你的牌堆了   
* 完整的牌堆文件路径就是`Dice000000000\publicdeck\牌堆.json`   

#### mod的安装方法

* 在里面新建一个名为`mod`的文件夹   
  然后就可以在里面放你的mod了   
* 完整的mod文件路径就是`Dice000000000\mod\牌堆.json`  

#### 安装完毕以后提示找不到牌堆怎么办
指令重载骰娘
  ```
  ->.system load
  <-0000-00-00 00:00:00 读取C:\xxxxxx\Dice000000000\PublicDeck\中的X个文件, 共Y个条目
  读取C:\xxxxxx\Dice000000000\plugin\中的A个文件, 共B个条目
  扩展配置读取完毕√
  
  ->已手动加载数据√
  ```

#### Lua脚本文件的安装方法
* 在里面新建一个名为`Plugin`的文件夹   
在`Plugin`文件夹内新建一个`lib`文件夹   

* 第三方库的安装   
在`Dice000000000\Plugin\lib\`路径下放置第三方库   
举例：`Dice000000000\Plugin\lib\dkjson.lua`   

* Lua脚本文件的安装   
在非`Dice000000000\Plugin\`路径下放置Lua脚本文件   
举例：`Dice000000000\Plugin\Test.lua`   
或者：`Dice000000000\Plugin\Lua\Test.lua`   
或者：`Dice000000000\Plugin\ovo\Test.lua`   
放在哪里都一样，只要在`Dice000000000\Plugin`路径下的非`Dice000000000\Plugin\lib\`路径下放置即可   

## 10.怎么迁移到另一个框架或者另一台电脑
准备工作:   
先输入`.system save`保存一下骰娘的各种设置   
虽然现在已经是十分钟自动保存一次了   
不过多保存一次不会错   

接下来就是迁移了   
简单点的就是整个框架文件夹压缩   
复制到新电脑，解压，运行   
XQ记得删掉原有登录重新登录一遍   
因为XQ的登录记录在不同的电脑上不通用   

复杂点的话   
复制   
`dice+qq号文件夹`   
各种图片语音等等   
到新的电脑/框架上去   

如果自己有其他更多的比如说引用到的程序啊之类的自行解决   
这里只是提供一个基础的教学   
然后复制到新的电脑/框架上去    
最后登录    

如果是`Mirai`->`Mirai`额外复制`device.json`文件   
如果是`Mirai`->`XQ`就按照上面的进行   
如果是`XQ`->`Mirai`最好找找看之前有没有保存的`device.json`文件，否则你得重新过一遍第二节的验证了   
如果是`XQ`->`XQ`记得删掉原有的qq记录重新登录一遍，`XQ`的登录记录在不同的电脑不适用   
如果是其他框架请自行对照解决   

## 11.初出茅庐

现在你已经完成了当骰主的新手阶段   
试着去看骰主手册、进阶手册、喧闹手册吧！ 

## 12.常见术语定义
* 后台  
你运行骰娘以后的那个会刷新出来一大堆花花绿绿的字的黑色窗口  
在你使用第二章:怎么设置自动登录  
采取第二种方法手动输入指令的那个窗口  
当你关掉这个窗口就相当于关闭了骰娘

* 后台日志    
后台上面显示的内容(拥有一段时间内的聊天记录)   
或者在bots\123456789\logs内的文件所显示的内容(只有运行记录)   

* 骰娘    
为实现在聊天工具内跑团而存在的一种，同时被额外拟人化的机器人   
绝大部分仅面向跑团用户群体开放  


> *By NULL&仑质*
