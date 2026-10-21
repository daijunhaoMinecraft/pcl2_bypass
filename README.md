# PCL 2 去除版本下载等限制方法

## 前言
![Image](/pcl/PixPin_2026-10-20_20-05-15.png)<br/>

在 **2026年10月20日 04:41**, PCL 的作者发布了一篇动态, 其中主要内容是 PCL 2 因为 **"中国式家长"** 的原因导致**PCL 内的版本下载、整合包选择、整合包安装等功能无法正常使用**

## 解决方法

### 方法 1: 下载旧版本并禁用更新
下载 PCL 2 提交: [5ef81e](https://github.com/Meloong-Git/PCL/tree/5ef81e490ae46b1100d61e33d0183094c7a80ca8) 中的文件 ([最新正式版.zip](https://github.com/Meloong-Git/PCL/blob/5ef81e490ae46b1100d61e33d0183094c7a80ca8/%E6%9C%80%E6%96%B0%E6%AD%A3%E5%BC%8F%E7%89%88.zip)) [直接下载](https://github.com/Meloong-Git/PCL/raw/5ef81e490ae46b1100d61e33d0183094c7a80ca8/%E6%9C%80%E6%96%B0%E6%AD%A3%E5%BC%8F%E7%89%88.zip)
下载完成后将文件中的 Plain Craft Launcher 2.exe 解压:
![Image](/pcl/PixPin_2026-10-20_20-11-51.png)

文件校验:
```
E:\PCL2>certutil -hashfile "Plain Craft Launcher 2.exe" sha256
SHA256 的 Plain Craft Launcher 2.exe 哈希:
6b6e1260cb5a1d6a303855f1c60cab3d0bc1aebba7e40a964180188b271e7386
CertUtil: -hashfile 命令成功完成。

E:\PCL2>
```

打开 Plain Craft Launcher 2.exe 程序, 随后关闭, 随后目录会有一个名为 "PCL" 的文件夹, 点开并使用记事本编辑其中的文件: Setup.ini
其中将 **SystemSystemUpdate** 的值修改成 3 即可禁用更新
![Image](/pcl/PixPin_2026-10-20_20-14-55.png)

之后打开即可正常使用

---

### 方法 2: 修改新版本 PCL 2 文件
#### 准备工具
 - [Dnspy 软件](https://github.com/dnSpyEx/dnSpy/releases)
 - [PCL 2.14.0.0 正式版](https://github.com/Meloong-Git/PCL/blob/5ef81e490ae46b1100d61e33d0183094c7a80ca8/%E6%9C%80%E6%96%B0%E6%AD%A3%E5%BC%8F%E7%89%88.zip)
 - [PCL 2.99.0.0 正式版](https://github.com/Meloong-Git/PCL/blob/6b9a50df911c9d68b69b24c979efba477fb14b67/%E6%9C%80%E6%96%B0%E6%AD%A3%E5%BC%8F%E7%89%88.zip)
#### 操作方法
首先下载 [Dnspy 软件](https://github.com/dnSpyEx/dnSpy/releases) 并将工具解压
![dnspy Release](/pcl/PixPin_2026-10-20_20-23-15.png)

然后打开工具

![dnspy 界面](/pcl/PixPin_2026-10-20_20-27-03.png)

随后下载 [PCL 2.99.0.0 正式版](https://github.com/Meloong-Git/PCL/blob/6b9a50df911c9d68b69b24c979efba477fb14b67/%E6%9C%80%E6%96%B0%E6%AD%A3%E5%BC%8F%E7%89%88.zip) 并解压出里面的 Plain Craft Launcher 2.exe 文件, 然后将 Plain Craft Launcher 2.exe 文件脱动到 Dnspy 中

![image](/pcl/PixPin_2026-10-20_20-27-45.png)

点击左上角的 编辑(E) -> Find String References in Module

![image](/pcl/PixPin_2026-10-20_20-28-46.png)

随后找到 String References 窗口, 确保左侧选中的是 Plain Craft Launcher 2, 然后点击刷新按钮, 之后输入字符串 "为遵守相关法律法规" 然后将会弹出相关地方的字符串

![image](/pcl/PixPin_2026-10-20_20-31-14.png)

首先解决版本部分的问题, 双击 Referrer 中带有 PageDownloadInstall 的列表, 将会跳转到相对应的代码处
随后右键函数, 选择分析

![image](/pcl/PixPin_2026-10-20_20-35-25.png)

此时双击进入 LoaderInit 函数
![image](/pcl/PixPin_2026-10-20_20-36-11.png)

对着 LoaderInit 函数右键编辑 IL 命令
![image](/pcl/PixPin_2026-10-20_20-39-27.png)

删除代码段: ldnull 的下面, ldarg.0 的上面 部分
![image](/pcl/PixPin_2026-10-20_20-40-18.png)

右键 ldnull 段, 并选择 "在选取后添加指令(C)"
![image](/pcl/PixPin_2026-10-20_20-41-21.png)

找到 ldsfld 操作码
![image](/pcl/PixPin_2026-10-20_20-42-44.png)

其中在右侧选择字段: ModDownload.DlClientListLoader
![image](/pcl/PixPin_2026-10-20_20-44-31.png)

之后点击确定
![image](/pcl/PixPin_2026-10-20_20-50-06.png)

更改完应当是这样
![image](/pcl/PixPin_2026-10-20_20-50-56.png)

下一步: 处理整合包部分的问题
回到 String References 窗口, 选择 ModModpack.ModpackInstall(), 任意一个选择任意一个即可
然后下载 [PCL 2.14.0.0 正式版](https://github.com/Meloong-Git/PCL/blob/5ef81e490ae46b1100d61e33d0183094c7a80ca8/%E6%9C%80%E6%96%B0%E6%AD%A3%E5%BC%8F%E7%89%88.zip) 并将 Plain Craft Launcher 2.exe 解压出来然后拖入到 dnspy 中, 然后选择 PCL.ModModpack 部分

![image](/pcl/PixPin_2026-10-20_21-03-49.png)

右键 ModpackInstall() **不带传参的** 选择 编辑 IL 指令(S)
![image](/pcl/PixPin_2026-10-20_21-05-28.png)

全选然后复制
![image](/pcl/PixPin_2026-10-20_21-06-48.png)

回到受限制的函数部分, 编辑 IL 命令
![image](/pcl/PixPin_2026-10-20_21-07-42.png)

全选并移除指令
![image](/pcl/PixPin_2026-10-20_21-08-30.png)

然后 Ctrl+V 粘贴之前复制的指令, 然后点击确定
![image](/pcl/PixPin_2026-10-20_21-09-17.png)

然后修改 ModpackInstall(string File, string InstanceName = null, string Logo = null) 函数 **带传参的**, 之后重复以上操作

![image](/pcl/PixPin_2026-10-20_21-12-54.png)

此时这里有不一样的, 点击 异常处理程序
![image](/pcl/PixPin_2026-10-20_21-14-38.png)

修改成旧版本程序这样, 然后点击确定
![image](/pcl/PixPin_2026-10-20_21-15-23.png)

之后修改成功, 如果有看到明显的 switch 分支那么即为修改成功, 这时候点击左上角 文件(F) -> 保存模块(M)
![image](/pcl/PixPin_2026-10-20_21-18-55.png)

然后直接点击 确定 <br/>
![image](/pcl/PixPin_2026-10-20_21-20-04.png)

## 效果展示
这里主要展示修改新版本 PCL 2 文件的效果

![image](/pcl/PixPin_2026-10-20_21-23-45.png)
![image](/pcl/PixPin_2026-10-20_21-23-56.png)

修改版 PCL 2 下载地址: [Link](https://openlist.theconsole.top/openlist/share/PCL2/2.99.0.0) 请注意文件校验

Edit in 2026年10月20日 星期二 21:48:28
