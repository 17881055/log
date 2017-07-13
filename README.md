问题场景：
猿宏本地开发时一切正常，后来使用 Jenkins 发布到测试环境时，被测试发现Ant-design-mobile 的 Toast 控件（之后简称为Toast）出了样式的问题（图1）。排除发现，导致样式异样的真凶是结构中多了一个 **<span>** 标签，使AntD的样式出了问题。（图1），而官网正常的结构是图2 这样的
<img src="/uploads/default/original/1X/63b5c43b433280fdc2cc48a4e9279709b500bdf8.png" width="590" height="372">
图1
<img src="/uploads/default/original/1X/069ecbb125b91e8281d882a62c0f8c2bbb64eeac.png" width="590" height="309">
图2
----------

继续：
发现这个 **<span>** 标签是由 **rc-notification** （rc 是 antD的爸爸，antD 在 rc 的基础上二次开发） 这位大爷的兄弟 **rc-Animate** 生成的；
<img src="/uploads/default/original/1X/b004883f01c17272bf75f6ea33d5439caf048894.png" width="590" height="248">

<img src="/uploads/default/original/1X/14bb4f86f08458af6f411c66dd6a5bc38fbea82b.png" width="590" height="161">

<img src="/uploads/default/original/1X/2c41fcd4f6d7dae2ffe45b91a433718a43f8727a.png" width="590" height="205">

<img src="/uploads/default/original/1X/4f76c5740231505a7f6d1ca7e14f378969eb75bd.png" width="590" height="173">

----------

深入：
知道**rc-Animate** 是生产者，回到**rc-notification**调用生产者时发现 **rc-notification** 2.0.0 和 **rc-notification** 2.0.1 代码发生变化了 
<img src="/uploads/default/original/1X/695a17b36bf2fe7437c6c018d4d49f5d26833b6f.png" width="690" height="272">

----------


思考：
为什么猿宏本地开发时一切正常，使用 Jenkins 发布到测试环境就出问题呢？？？？ 原来 Jenkins 安装**node_modules**  的依赖环境是 **rc-notification** 2.0.1 ，而 猿宏本地的**node_modules** 是 **rc-notification** 2.0.0，坑！坑！坑！就在
AntD 的依赖引用上。使用了  " ~ 2.0.0"  。
<img src="/uploads/default/original/1X/767838c32287c8518faa7cfa5342748703b52603.png" width="312" height="499">

有朋友问这不是2.0.0么，怎么会下载 2.0.1 的。网上找了一图很好解析，原来 用了“~” 最后一位取最新版本。导致 Jenkins 
 更新时，刚好  *rc-notification** 也出了 2.0.1的版本。所以就引用了 *rc-notification** 2.0.1 。就它就它就是它！
<img src="/uploads/default/original/1X/fe2838bce4c3c0609fbe97d6f43988aff49646f3.png" width="528" height="390">

----------

解决：
把依赖写死 或者  使用 package-lock.json 锁死 ；
<img src="/uploads/default/original/1X/51289e79986afe71b67e7ce498fc3a742c4b6407.png" width="690" height="135">
<img src="/uploads/default/original/1X/119af85f96b70220fb9db568a3ceea5f2a324830.png" width="690" height="476">