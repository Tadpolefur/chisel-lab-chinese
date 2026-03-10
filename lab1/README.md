# 实验一：你好，Chisel

本实验课会使用闪烁LED灯的案例，展示 Chisel 工具链的使用和设计工作流的运行方式。

完成试验后，你将对使用 Chisel 编码的硬件设计所用到的编辑和编译工具有全面的了解。
你将能够综合该设计，并配置 FPGA 板。

我们假设你已经从 GitHub 下载了完整的实验材料，并将其放置在文件夹 `chisel-lab` 中。

## 背景阅读

本实验大致基于 [Digital Design with Chisel](http://www.imm.dtu.dk/~masca/chisel-book.html) 的第一章。

## 探索和编译 Hello World 组件

在 IntelliJ 中使用以下步骤导入实验一项目文件：
 * 启动 IntelliJ
 * 如果这是你第一次启动 IntelliJ，点击 *导入项目*，否则选择 *文件 - 新建 - 现有源中的项目*
 * 选择 *sbt*
 > 译注：该选项在“从外部模型导入项目”下
 * 移动到 `.../chisel-lab/lab1` 目录下并选择 `build.sbt` 文件，点击 *下一步*
 * 在 *项目 JDK* 中选择高于1.8的JDK版本
 * 点击 *创建*

第一次导入项目可能需要一些时间，因为 Scala 和 Chisel 文件需要下载，请耐心等待。

> 如果你已经打开了一个 IntelliJ 项目，你可以使用：
> *文件 - 新建 - 现有源中的项目*

然后在项目导航器中依次选择 *lab1 - src - main - scala - Hello*，双击打开 ```Hello```。

这是一个完整的 Chisel 组件，包括生成一个较慢的逻辑时间，以便利用 Basys3 开发板的 100 MHz 时钟来驱动 LED 以 1 Hz 的频率闪烁。
不要被 20 行这么长的代码吓到，你很快就能理解其中的细节。今天的实验主题是让工具流运行到配置 FPGA 板的阶段。

在 IntelliJ 窗口的底部打开终端，输入以下命令编译并运行项目：

```
sbt run
```
在 *运行* 窗口可以看到以下内容：

```
[info] Running HelloMain 
Hello World, I will now generate the Verilog files!
[info] [0.001] Elaborating design...
[info] [2.205] Done elaborating.
Total FIRRTL Compile Time: 916.6 ms
[success] Total time: 22 s, completed Jul 22, 2019 11:09:25 AM
```

这是 Chisel 编译器和运行时的输出。为了验证程序是否运行，我们使用著名的 “Hello World” 开头输出了一条友好的欢迎信息。
如果你的设计存在错误，就会在此窗口看到错误信息。

运行 Chisel 程序会生成 Verilog 文件（`Hello.v`），我们将使用它来为 FPGA 设计综合。
这个文件的内容并不重要，但是你如果感兴趣，可以在 IntelliJ 中打开它。

**命令行界面替代方案：**

如果你不喜欢用 IDE，也可以在 `shell` 或 `终端` 中使用命令行操作。
使用你选择的文本编辑器打开 `.../lab1/src/main/scala/Hello.scala` 中的 `Hello.scala` 文件，然后在命令行中输入以下内容便可轻松编译并运行 *Hello World* 组件：

```bash
sbt run
```
**结束命令行界面替代方案**

## 使用 Vivado 综合和配置 FPGA 板


我们使用 Xilinx 的 [Vivado](https://www.xilinx.com/products/design-tools/vivado/vivado-webpack.html) 工具来综合我们的设计，并配置 Basys3 开发板。

这一过程在上一学期的《A digital circuit design flow guide  using VHDL and Xilinx Vivado
targeting a Digilent BASYS 3 FPGA board》中有详细描述，请使用该文档。以下仅为简单总结：

### Vivado 项目创建

 * 打开 Vivado
 * 点击 *Create Project*
 * 点击 *Next*
 * 选择一个名称和位置，然后点击 *Next*
   * 可以将你的项目放在 `chisel-lab/lab1` 目录下
 * 点击 *Next* ，然后选择 *RTL Project*
 * 在下一个对话框中点击 *Add Files* 并导航到 `Hello.v` 文件并添加它
 * 在 *Add Constraints* 对话框中点击 *Add Files* 并选择
   `Basis3Hello.xdc` 文件，然后点击 *OK*
   * 对于接下来的实验，你需要编辑 `Basis3Hello.xdc` 文件，以配合 Basys3 主板的约束文件
 * 点击 *Next*
 * 在 *Default Part* 对话框中选择 *Boards* 和 *Basys3*，然后点击 *Next*
 * 点击 *Finish* 创建项目

### 综合和配置 FPGA 板

只需几步便可将我们精彩的 *Hello World* 设计综合并配置到 Basys3 开发板。

 * 将 Basys3 开发板连接到你的电脑的 USB 接口
 * 在 *Project Manager* 底部点击 *Generate Bitstream* 开始综合、实现和生成比特流
   * 这个过程可能需要经过分钟级的时间
 * 在 *Open Hardware Manager* 下配置 FPGA 板
   * 打开硬件管理器后，打开目标，Auto Connect
   * 点击 *Program Device* 进行设备编程

你应该会看到 LED 开始以 1 Hz 的频率闪烁。

**恭喜！你已经用 Chisel 构建了你的第一个数字设计**

在这漫长的初始化过程之后，下一次设计流程的运行应该会很顺利。
试着改变常量 `CNT_MAX` 为一个稍小的值，比如将100000000改为50000000，来改变闪烁频率。
再次运行 IntelliJ 中的 Chisel 代码，并重新综合和配置 Vivado 板。LED 应该现在以另一种频率闪烁。
他变得更快还是更慢了，是什么频率？

### 无 FPGA 板模拟

如果你没有可用的 FPGA 板，也可以运行闪烁 LED 的仿真。
为了避免进行 100000000 次这个量级的的时钟周期仿真，在 `Hello.scala` 中把这一行

```
  val CNT_MAX = (100000000 / 2 - 1).U;
```
改成这样，
```
  val CNT_MAX = (50000 / 2 - 1).U;
```
然后使用以下命令运行仿真：
```
sbt test
```
你会在终端看到*模拟*的闪烁 LED 输出。

