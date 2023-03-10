# FABFM_Fix-Automatic-Brightness-Failure-of-MacBookPro
## 一、功能描述  
&emsp;&emsp;修复2014中期款MacBook Pro进入系统时自动亮度失效的问题。  
&emsp;&emsp;Fix the problem of automatic brightness failure when the 2014 mid MacBook Pro enters the system.  
## 二、初始故障描述  
&emsp;&emsp;根据统计，大多数MacBook Pro mid 2013~2014，以及部分2015版本，存在升级BigSur 11.2.1及更新版本（目前是11.7.4）后，开机或重启后，系统会失去对屏幕的自动亮度控制，以及手动亮度控制。  
&emsp;&emsp;但是安装双系统，发现Windows10不存在亮度不可控的问题，简单来说就是Windows下亮度控制一切正常。  
## 三、故障分析  
&emsp;&emsp;驱动加载异常。  
## 四、解决办法
### （1）睡眠解决：  
&emsp;&emsp;点击睡眠，再立刻唤醒，可暂时恢复亮度控制；  
### （2）开合盖子：  
&emsp;&emsp;盖上盖子，再立刻打开，可暂时恢复亮度控制；  
### （3）终端息屏：  
&emsp;&emsp;终端息屏再唤醒，可暂时恢复亮度控制；  
### （4）封装app：  
&emsp;&emsp;将息屏和唤醒指令打包进app，开机自动执行指令，可永久解决亮度控制。  
&emsp;&emsp;注意其中（1）~（3），不具备自动化，每次开机都需要手动操作，很繁琐；而（4）是自动化操作，相对便捷。  
## 五、具体细节  
### （1）代码分析。  
&emsp;&emsp;pmset displaysleepnow，此为立刻息屏；  
&emsp;&emsp;caffeinate -u -t s，为维持高性能状态s秒，执行此命令会强制开启屏幕显示，可用来亮屏；  
&emsp;&emsp;sleep s，为等待s秒。  
&emsp;&emsp;PS.注意息屏与亮屏中间，必须留有足够的时间间隔，否则后者不会被执行！  
&emsp;&emsp;所以正确的指令应为：pmset displaysleepnow;sleep s;caffeinate -u -t s;（s表示时间，单位为秒，推荐5s），示例如下：  
&emsp;&emsp;例：pmset displaysleepnow;sleep 5;caffeinate -u -t 5;   
&emsp;&emsp;我这里说明一下sleep的时长，为了减少亮灭切换对LCD显示器的伤害，时间间隔建议至少为5s；对于caffeinate，建议5~10s为佳，低于5s可能在开机自动化时反应不过来会出现额外黑屏时间。  
### （2）创建自动化。  
&emsp;&emsp;在macOS自带的自动操作app中创建应用程序，从实用工具选出终端shell，添加至程序；  
&emsp;&emsp;再将（1）中的代码写入shell编辑框，保存为app；  
&emsp;&emsp;为上述步骤生成的app开启辅助功能权限，然后添加开机自启动，即可。  
&emsp;&emsp;下次重启，或者开机，登录进入系统时，该app就会立刻按指令关闭并重新开启显示器，恢复一切亮度控制。  
