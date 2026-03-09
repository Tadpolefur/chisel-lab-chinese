# 工具安装指南

本文档介绍了如何在 Linux/Ubuntu 和 Windows 上安装所有工具，如下是所要安装的工具一览：

 * [版本高于 8 但不高于 21 的 Java OpenJDK](https://adoptopenjdk.net/)
 * [sbt](https://www.scala-sbt.org/)
 * [IntelliJ](https://www.jetbrains.com/idea/download/) （免费的社区版）
 * Vivado（如 2023.1 版本）
   * Windows: https://www.xilinx.com/member/forms/download/xef.html?filename=Xilinx_Unified_2023.1_0507_1903_Win64.exe
   * Linux: https://www.xilinx.com/member/forms/download/xef.html?filename=Xilinx_Unified_2023.1_0507_1903_Lin64.bin
 * [GTKWave](http://gtkwave.sourceforge.net/)
 * make, git（使用命令行，或者带用户界面的Git客户端，见下文详述）

## Ubuntu 虚拟机

最简单的安装所有工具的方法是使用如下链接提供的 Ubuntu 虚拟机：

 * [Ubuntu VM](https://patmos-download.compute.dtu.dk/de2lab.ova)

使用用户名 ```de2lab``` 和密码 ```de2lab```登录。你需要安装免费的 [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
来运行该虚拟机。注意，虚拟机文件**非常大**，需要下载43GB的文件，而解压后的虚拟机大约占用77GB的空间。

使用 Virtual Box 的 *File* >> *Import Appliance...* 选项以解压虚拟机镜像，然后启动虚拟机。使用虚拟机窗口顶端的 *devices* >> *USB* 选项并选择菜单中的 FGPA 以将你的 FPGA 通过 USB 连接到虚拟机。

## Chisel

Chisel 是 Scala 语言的一个库，而 Scala 是运行在 Java 虚拟机 (JVM) 上并使用 Java库的一种语言。因此，你需要在你的计算机上安装
[版本高于 8 但不高于 21 的 Java OpenJDK](https://adoptopenjdk.net/)。

你需要安装 Scala 构建工具 [sbt](https://www.scala-sbt.org/) 以使用命令行构建 Chisel 项目，
请注意安装 sbt 也可以使 IntelliJ 构建流程变得更简单。

[IntelliJ](https://www.jetbrains.com/idea/download/) 是一个不错的 Chisel/Scala 编辑器，首次启动时需要在“下载推荐插件（Download featured plugins）”处下载 Scala 插件。

## Vivado

Vivado 是 Xilinx 公司为 Basys3 FPGA 开发板提供的综合工具，其免费版本可从以下链接下载：
https://www.xilinx.com/products/design-tools/vivado/vivado-webpack.html

Vivado is the synthesize tool from Xilinx for the Basys3 FPGA board.
The WebPACK edition is freely available at:
https://www.xilinx.com/products/design-tools/vivado/vivado-webpack.html

 * 下载 [Vivado WebPACK](https://www.xilinx.com/products/design-tools/vivado/vivado-webpack.html)
   * 你需要注册 Xilinx
   * 为了节省空间，可以不选择除 Artix-7 之外的其他设备
 * 对于 Linux，可运行安装程序 ```bash Xilinx...```
   * 参见 [Digilent 安装说明](https://reference.digilentinc.com/vivado/installing-vivado/start)
 * 根据上述说明安装 USB 驱动程序和 Digilent 板文件

## Ubuntu/Linux

这是我为 DE 2 实验室准备的 Ubuntu 虚拟机的安装日志。这些说明可能对你设置 Linux 系统有所帮助。

 * 安装 Ubuntu 18.04 LTS，最大磁盘空间设置为 80 GB，内存设置为 4 GB。
 * uid: de2lab, pwd: de2lab
 * 设置时间和时区，这对后续的安装至关重要！
 * 进入 `设置-电源` 设置 `黑屏：永不`，`隐私：自动` 和 `锁定：关闭` 
 * 将 Chisel 书籍复制到桌面上
   * 这是我当前的快照
 * 在主目录下安装 Vivado
   * 安装接线驱动程序
   * 获取 Digilent 开发板定义文件
 * 使用以下指令安装 Java JDK 和其他工具：
   * ```sudo apt install openjdk-8-jdk git make gtkwave```
 * 按照 [sbt 下载](https://www.scala-sbt.org/download.html) 的说明安装 sbt
 * 安装 IntelliJ 和 Scala 插件，并在收藏夹中创建快捷方式
 * 在桌面上创建 GtkWave 的快捷方式

除了手动运行 ```apt``` 命令之外，你也可以运行提供的 ```setup.sh``` 脚本。

## Windows 10

 * 按上述说明安装 Vivado 和 Digilent 开发板文件
 * 安装 [版本高于 8 但不高于 17 的 Java OpenJDK](https://adoptopenjdk.net/)
 * 安装 [sbt](https://www.scala-sbt.org/)
 * 安装 [IntelliJ](https://www.jetbrains.com/idea/download/)
   * 社区版
   * 创建桌面快捷方式
 * 启动 IntelliJ 完成安装
   * 如果你喜欢的话，选择浅色 UI 主题
   * 在推荐插件中选择安装 Scala 插件
   * 导入项目时，选择你安装的 JDK
     * 在项目 JDK 选择 *New*
     * 选择 *JDK*
     * 选择你的 OpenJDK 8 安装路径，一般在类似 `C:\Program Files\AdoptOpenJDK\jdk-8.0.232.09-hotspot\` 的位置
 * 下载 [GTKWave 二进制文件](https://sourceforge.net/projects/gtkwave/files/)
   * 选择与 `gtkwave-{release number}-bin-win32` 匹配的最新版本
   * 解压下载的 `.zip` 文件到任意目录
   * 在 `gtkwave\bin\` 目录运行 `gtkwave.exe`
   * 在桌面创建快捷方式
 * 把 [Chisel Book](http://www.imm.dtu.dk/~masca/chisel-book.html) 复制到桌面上
 * 安装 [git 客户端](https://git-scm.com/download/win)
   * 如果你对 git 还不熟悉，可以参考以下链接了解 git 的工作流程和版本控制系统的优势。[1](https://www.youtube.com/watch?v=SWYqp7iY_Tc), [2](https://www.freecodecamp.org/news/what-is-git-and-how-to-use-it-c341b049ae61/)。注意绝大多数 git 教程强调使用命令行，但是这并不意味着必须如此。存在很多优秀的图形化 git 客户端，例如 [Github Desktop](https://desktop.github.com/) 和 [Fork](https://fork.dev/)。

实验中的第一个联系可以验证安装的正确性。或者你可以在 `Windows PowerShell` 中快速测试以下指令：

```
javac
sbt
```

## Intel 芯片架构的 Mac

Vivado 在 macOS 上不可用，但是 Chisel 在 Mac 上运行良好。你可以在你的 Mac 设备上进行设计的模拟，然后使用如 Ubuntu 等虚拟机来运行 Vivado 进行综合。

 * 安装 [版本高于 8 但不高于 17 的 Java OpenJDK](https://adoptopenjdk.net/)
 * 使用 `brew install sbt` 安装 sbt
 * 安装 [GTKWave](http://gtkwave.sourceforge.net/)
   * 对于 macOS 14，可以按照以下方式安装：
   * `brew install --HEAD randomplum/gtkwave/gtkwave`
   * 参见 [这个问题](https://github.com/gtkwave/gtkwave/issues/250)
 * 安装 [IntelliJ](https://www.jetbrains.com/idea/download/)
   * 社区版
   * 创建桌面快捷方式
 * 在 IntelliJ 中完成安装
   * 如果你喜欢的话，选择浅色 UI 主题
   * 在推荐插件中选择安装 Scala 插件
   * 导入项目时，选择你安装的 JDK
     * 在项目 JDK 选择 *New*
     * 选择 *JDK*
     * 选择你的 OpenJDK 安装路径

## Arm 芯片架构的 Mac (M1, M2, 和 M3)

 * 参考 [MacM1.md](MacM1.md) 在 Arm 架构芯片的 Mac 上安装 Vivado。

## 常见问题
请参考 [FAQ](FAQ.md)
