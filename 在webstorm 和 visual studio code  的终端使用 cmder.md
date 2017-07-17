**visual studio code**

文件>首选项>设置

在右边的编辑设置上写上

> "terminal.integrated.shell.windows": "C:\\WINDOWS\\sysnative\\cmd.exe",
> "terminal.integrated.shellArgs.windows": ["/K", "E:\\cmder\\vendor\\init.bat inject"]

“E:\cmder\ 是 cmder” 目录路径 换成电脑的 cmder 目录。

<img src="/uploads/default/original/1X/9fcaebad8d1da6a39cfd4f675753b559fffe3c86.png" width="690" height="401">




----------

**webstorm** 

settings > terminal > shell path, 填写下面内容

> "cmd.exe" /k ""%CMDER_ROOT%\vendor\init.bat""

CMDER_ROOT 是全局路径，如果没有设置，请自行先百度设置

<img src="/uploads/default/original/1X/fed24edef7e64a85d3246136fbf79e4bf9595d08.png" width="690" height="233">