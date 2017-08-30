一直有看到GitBook 的相关的书籍，想搭建一个属于自己的空间，用来做一些知识的搜集和整理，花了一个下午的时间搭建GitBook Editor 以及发布Git Book 到github上 整理下操作的步骤。

网上的教程很多，我也是考网上的教程，自己尝试然后搭建完成的。

本教程是针对Windows用户而言

GitBooks 是使用markDown的语法 必要知识

[5分钟内学会使用markdown编辑器](http://www.jianshu.com/p/d6fb260df77b)

# 1、搭建环境准备

安装本地GitBook Editor 需要准备 相应的环境和工具

GitBook 的源码是使用Node.jss实现的，因此需要安装Node.js的环境，以及准备GitBook Editor的安装版本

Node.js 下载 官网地址：[https://nodejs.org/en/](https://nodejs.org/en/) 选择相应的版本进行下载 [安装过程 ](http://www.runoob.com/nodejs/nodejs-install-setup.html)

下载G[itBook Editor Windows](https://www.gitbook.com/editor/)

完成相应环境的安装

详细的安装过程可以参考 [windows 系统下GitBook环境搭建与使用教程](http://www.jianshu.com/p/cf4989c20bd8)

注意:建议先选择在GitBook 上新建书籍，然后打开本地GitBook Editor  使用同一个账号进行登录 可以看到自己的的书籍 然后在本地编辑 同步即可

这个版本安装检测gitbook安装成功是**gitbook -V **

# 2、同步gitbooks上的书籍到GitHub上

登录自己的GitBooks 选择 settings

![](/assets/Screenshot of wwh %28@wwh%29 on GitBook · GitBook.jpg)

进入左侧的personal settings  选择GitHub

**install github integration** 以及**link github account**  即可

登录自己的github 在github中新建一个仓库 用来存放gitbooks

github 上仓库新建好之后   返回gitbooks  选择需要同步的书籍 如下图

![](/assets/Screenshot of GitHub - resource · GitBook.jpg)选择好同步的仓库即可 简历book和github仓库的绑定关系

在gitbook  Editor 编辑器主页的选项**book** 中选择** repository settings**中填入github 中存放gitbook的仓库地址即可

详细的安装步骤可以参考教程

##### [『极简教程』gitbook写书，同步github](http://www.jianshu.com/p/feae28bb3111)

##### [利用 gitbook 与 webhook 做一个展示前端项目的平台](http://www.jianshu.com/p/2a1cf8891d5f)

##### [Gitbook简易教程](https://segmentfault.com/a/1190000005859901)

##### [GitBook平台发布教程](http://brucewar.me/2016/12/05/GitBook平台发布教程/)



