
## Atom 配置eslint
> 以目前最新的[1.19.0版](https://github.com/atom/atom/releases/tag/v1.19.0)为例

### 需要安装插件
  1. [linter](https://atom.io/packages/linter) ``atom-linter`` 基础插件
  2. [linter-eslint](https://atom.io/packages/linter-eslint) ``eslint`` 对应的插件
  3. [linter-ui-default](https://atom.io/packages/linter-ui-default) 作为 ``linter`` 的依赖会被自动安装
  4. [intentions](https://atom.io/packages/intentions) 同上，会被自动安装

---

##### 安装插件的方法
  1. 直接在Atom Package Manage 中通过搜索对应的包名来安装
  2. 使用 ``apm`` 命令来安装 ([没有apm命令请看这](#添加apm命令)):
     ```bash
	  apm install [<package_name>...]

	  # 更多用法请自行查看help
	  apm -h
	  ```
     > 1,2 这两种方法都需要有能翻墙的网络环境，不然安装容易失败([apm设置代理请看这](#apm使用代理))
  3. 直接去到对应插件的 ``Github repo``中下载源码包，解压并改名 ``(去掉'-master'等后缀)`` 放在 ``.atom/packages/`` 下，再进入该目录通过 ``npm`` 安装 **dependencies** 中的包，再重启 ``Atom``

---

##### 配置 linter-eslint

![linter-eslint-config](/uploads/default/original/1X/c7e52c0deb6452dab1a2508b3b1b729331e68f1e.jpg)


---

##### 添加apm命令
 - 请将 ``『你的Atom目录/resources/cli』`` 添加的环境变量 ``PATH`` 中

##### apm使用代理
 下面代理服务器地址以 ``http://127.0.0.1:1080/``为例
 ```bash
 apm config set http-proxy http://127.0.0.1:1080/
 apm config set https-proxy http://127.0.0.1:1080/
 apm config set strict-ssl false



----------




## Sublime Text 配置eslint
> 以目前最新版 [3126 (Windows 64 bit)](http://www.sublimetext.com/3) 为例
> ![sublime V](/uploads/default/original/1X/53e5b3c2f5ba08731da2119d3bd2b093089de83c.png)

#### 需要安装插件
  1. [Babel](https://packagecontrol.io/packages/Babel) 使用 `babel` 来解析代码(包括 `react` ), [装上这个请禁用自带的 `「JavaScript」` 语法](https://github.com/babel/babel-sublime#advanced-usage)
  2. [SublimeLinter](https://packagecontrol.io/packages/SublimeLinter) 必装,基础框架. [官方文档](http://www.sublimelinter.com/en/latest/index.html)
  3. [SublimeLinter-contrib-eslint](https://packagecontrol.io/packages/SublimeLinter-contrib-eslint) 基于 `SublimeLinter` 的 `eslint` 插件  (使用**本地 eslint**)
  4. [ESLint-Formatter](https://packagecontrol.io/packages/ESLint-Formatter) `eslint` 自动修复插件，(使用**本地 eslint**)
  5. [EditorConfig](https://packagecontrol.io/packages/EditorConfig) 非必须，推荐装上. 统一各种IDE/编辑器 编码/缩进/换行等 [官网](http://editorconfig.org/)

---

#### 配置
  - 推荐在 ``sublime`` 用户设置添加下面配置
   ![sublime config](/uploads/default/original/1X/a94866b7c5562eaccc925ec434096d2f57a32427.png)
	<br>
  - 配置 `SublimeLinter`
   ![sb111](/uploads/default/original/1X/f689a6c23248e5969a8fa000e69a3cf349e44b32.png)
  - 配置 `Eslint Formatter`
   ![eslint-fix](/uploads/default/original/1X/ffeeb056bfd0b284313887ec014625e55a138ffe.png)
---
##### 坑1
``SublimeLinter`` 读取文件时默认将所有 ``换行符`` 转为 <kbd>LF</kbd> <small>「`\n`」</small>; (暂时无解)
当 `eslint` 的 [linebreak-style](http://eslint.org/docs/rules/linebreak-style#options) 设为 `"windows"` 时,且源文件换行符确实为 <kbd>CRLF</kbd> <small>「`\r\n`」</small> 时还会报错.
**所以只能妥协在 `SublimeLinter` 中关闭这个规则**
 - [相关问题](https://forum.sublimetext.com/t/sublime-linter-eslint-jscs-falsy-detect-lineending-as-unix/25552/3)
 - [相关问题](https://github.com/roadhump/SublimeLinter-eslint/issues/143)

##### 坑2
`nodeJs` 当前工作目录 `process.cwd()` 会影响 `eslint` 的行为和某些规则(例如 [import/no-extraneous-dependencies](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-extraneous-dependencies.md))

- ```bash
   项目根目录
	├───src
	│   └──index.js
	└───.eslintignore
	```
	当在 `src/index.js` 时运行 `SublimeLinter`, `eslint` 无法获取 `.eslintignore`
	![sublime cwd BUG](/uploads/default/original/1X/9f1a28fbcccd14c3f57842daec888170ad3201c1.png)<br>
	还会导致 [import/no-extraneous-dependencies](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-extraneous-dependencies.md) <small>``(暂时发现这个)``</small> 这个规则路径匹配有问题
	虽然[文档](http://www.sublimelinter.com/en/latest/linter_settings.html?highlight=chdir#chdir)上提供 `${project}` 这个占位符来替换成当前项目路径,但实测过**无效**.
	**所以只能够`手动`将 ``chdir`` 这个值设为`当前项目的根路径`**



----------



## vscode 配置eslint
> 以[1.14.2版](https://github.com/atom/atom/releases/tag/v1.19.0)为例
> ![vscode V](/uploads/default/original/1X/5f959b3f8845065a81c41b2db90f0042bb8390a6.png)


### 需要安装插件
  - [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)


### 配置插件
打开`用户设置`:
![vsc eslint](/uploads/default/original/1X/4fdf42e000f540aee859c5d96a093abdcb011326.png)
<br>详细的配置说明请看[文档](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint#settings-options)



----------


## WebStrom配置eslint
然后在 WebStorm 中，打开设置（File>Setting或者Alt+F7），按路径进入 ESLint 的配置界面（Languages&Frameworks>JavaScript>Code Quality Tools>ESLint）。
或者 直接搜索 eslint 

开启 ESLint，并配置相应路径，配置文件默认使用.eslintrc。
<img src="/uploads/default/original/1X/f97b5308425721aa49dcb7f3a06b63201eebed33.png" width="490" height="244">




----------



## 特别感谢：  招振国
