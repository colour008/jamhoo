---
title: 前端学习--Vue安装(npm方法)🍄
date: 2023-02-2 18:00:56
updated: 2024-11-30 18:00:00
tag: [前端,Vue]
categories: [编程,Vue]
cover: 
description: 前端学习--Vue安装(npm方法)🍄
swiper_index: 
sticky:  
---

---

# 前言

首先，先列出我们接下来需要的东西：

- node.js环境（npm包管理器）
- vue-cli 脚手架构建工具
- cnpm npm的淘宝镜像

# 一、安装[node.js](https://nodejs.org/zh-cn/)

从node.js官网下载并安装node，安装过程很简单，一直点下一步就ok了，安装完之后,输入`node -v` 命令，查看node的版本，若出现相应的版本号，则说明你安装成功了。安装成功后，运行代码结果如下（示例）：

![图1](https://bu.dusays.com/2024/12/29/6770cbf207968.png)

npm包管理器，是集成在node中的，所以安装了node也就有了npm,直接输入 `npm -v` 命令，显示npm的版本信息。安装成功后，运行代码结果如下（示例）：

![图2](https://bu.dusays.com/2024/12/29/6770cbf32668b.png)

到目前为止，node的环境已经安装完成，npm 包管理器也有了，由于有些npm资源被屏蔽或者是国外资源的原因，经常会导致npm安装依赖包的时候失败，所以我们还需要npm的国内镜像----cnpm。

# 二、安装cnpm

在命令行中输入 `npm install -g cnpm --registry=http://registry.npm.taobao.org` ，然后等待，没报错表示安装成功。安装成功后，运行代码结果如下（示例）：

![图3](https://bu.dusays.com/2024/12/29/6770cbf45880c.png)

完成之后，我们就可以用cnpm代替npm来安装依赖包了。如果想进一步了解cnpm的，查看淘宝npm镜像官网。

# 三、安装vue 脚手架构建工具

在命令行中运行命令，最新稳定版 `npm install -g @vue/cli`，然后等待安装完成。

是否安装成功：`vue -V`，安装成功后，运行代码结果如下（示例）：

![图4](https://bu.dusays.com/2024/12/29/6770cbf5781b2.png)

# 四、创建vue工程项目

### （一）基于命令行的方式创建vue项目

```perl
vue caeate project-name
```

### （二）基于图形化界面的方式创建vue项目

```perl
vue ui
```

### （三）开始创建项目

1.在终端中输入`vue create vue_project_01`，按下回车开始创建项目；

2.此时会出现两个选项询问你需要安装哪些功能：

第一项是默认方式创建vue项目，第二项是手动选择功能创建vue项目；

按键盘↑或↓来选择；

选择手动模式；

3.此时让你手动选择需要安装的功能 按空格键选择 选中的功能前面会带有*号 选择完毕之后按回车进入下一步操作；

4.如果选择了vue-router的话会弹出一个选项，问你是否需要安装历史模式的路由，我选否因为要用哈希模式的路由；

5.又弹出一个提示让你选择Eslint的语法版本，选择标准版本就行；

6.又又弹出一个提示框问你什么时候进行es的语法校验，选择lint on save；

7.又又又弹出一个提示问你这些工具的配置文件怎么创建，第一个选项是创建单独的配置文件，第二个选项是把这些配置文件都放到package.json中；

8.又又又又弹出一个提示问你要不要把刚才所做的那些设置保存为一个模板供后续创建其他项目的时候使用；

9.漫长的等待。。。

正在初始化项目的基本结构、创建一些模板，下载一些包。。。。

10.创建完成后提示我们进入到项目的根目录中，并且运行`npm run serve` 把项目运行起来；

11.在浏览器中输入http://localhost:8080/#/ 就可以访问到你创建的第一个vue项目了。

# 五、注意事项

工程项目初始化完成后，运行`npm run serve`时，可能会提示vue无法加载文件C:\Users\Administrator\AppData\Roaming\npm\vue.project-name，因为在此系统上禁止运行脚本。

这是因为在此系统上禁止运行脚本……解决办法如下：

1.管理员身份运行PowerShell（命令提示符，来源于Linux的命令提示符也叫Shell）：

![图5](https://bu.dusays.com/2024/12/29/6770cbf6c1c0c.png)

2、执行：set-ExecutionPolicy RemoteSigned （签名或运行这些脚本）：

![图6](https://bu.dusays.com/2024/12/29/6770cbf7cb244.png)

3.重新运行`npm run serve`，会成功运行。

4.vue打包命令

对当前vue项目进行打包的命令如下: `npm run build`，打包完成,会输出Build complete并且在vue项目中会生成一个名字为dist的打包文件。