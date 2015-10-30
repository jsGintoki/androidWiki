一些前置条件

Homebrew - MAC 下的包管理软件，如果不了解请自行搜索,这样主要是用来完成ffmpge命令的编译安装 (如果你使用linux系统，用其他的包管理系统效果是一样的，比如apt-get）
Android 4.4 系统以上 - 这里会用到Android4.4系统下的 screenrecord 命令
Android SDK - 主要是需要其工具包里的adb 命令用来和手机设备的shell交互,SDK的安装可以参考这篇文章《打造一个全命令行的Android构建系统》
首先我们需要通过homebrew 安装ffmpge ，在这里我假设你已经在使用homebrew管理mac的软件依赖了，终端下敲入下面的命令。(为了确保安装成功最好在VPN环境下，因为某些安装包的依赖可能在墙外的，我自己是在VPN状态下安装成功的)

brew install ffmpeg

接下来我们可以尝试用Android4.4 下的 "screenrecord" 录制一段屏幕录像，下面的命令用于开启屏幕录制，按下回车命令我们就可以在手机屏幕上进行操作了，录制完毕直接 ctrl+c 这时候会在手机的 /sdcard目录下生成一个叫做"demo.mp4"的视频文件。

adb shell screenrecord /sdcard/demo.mp4

经过多次尝试，我准备使用600x800的分辨率，控制10秒的时长，主要是从视频质量和大小进行考量，视频源的大小和质量同时也会影响到接下来gif生成的质量(gif图片的大小最好控制在1M以下).

adb shell screenrecord /sdcard/demo.mp4  --size 600x800 --time-limit 10

由于生成的屏幕录像视频存在于手机本身的 SD卡目录下，而ffmpge命令是我PC中的命令，所以还需要把"demo.mp4" 复制到PC中，使用下面的命令。

adb pull /sdcard/demo.mp4

ffmpeg生成gif的基本用法如下:

ffmpeg -t <视频时长> -ss  -i <视频文件>  demo.gif

接下来我们用ffmpeg命令就可以生成一个10妙的git动态图。

ffmpeg -t 10 -ss 00:00:00 -i demo.mp4 demo.gif


来自：http://www.jianshu.com/p/307369f50a3f
