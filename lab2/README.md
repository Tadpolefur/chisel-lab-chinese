# 实验二：Chisel 组合逻辑电路

本实验课程将介绍使用 Chisel 语言描述组合逻辑电路的基本方法。
在本实验中，你将在一些预设的模块中添加组合逻辑电路的描述，然后在其上运行单元测试。
可选地，你还可以将你的电路综合到 FPGA 板上，并在 FPGA 板上测试它。

在执行前进行测试被称作“测试驱动开发（test driven development）”，
这在软件开发中很常见，但也是很好的硬件开发实践。
测试用例在本课程中将直接给出，而在后续课程中需你要自行编写。

本实验完成后，你将了解如何使用 Chisel 描述常见的组合逻辑，比如多路复用器、编码器、解码器、函数表等。

我们预设你已经下载了完整的实验材料，并将其放置在名为 `chisel-lab` 的文件夹中。

## 背景阅读

本实验大致基于 [Digital Design with Chisel](http://www.imm.dtu.dk/~masca/chisel-book.html) 的第二和第五章。

## 编译和测试组合逻辑电路

今天的实验课程主题是描述 Chisel 中选定的组合逻辑构建块。
我们为你的电路提供了测试代码，当你使用 `sbt test` 通过了所有测试时而不报任何错误时，便视为完成了所有练习。

使用如下步骤将实验二项目导入到 IntelliJ 中：

 * 启动 IntelliJ
 * 如果这是你第一次启动 IntelliJ，点击 *导入项目*，否则选择 *文件 - 新建 - 现有源中的项目*
 * 选择 *sbt*
 > 译注：该选项在“从外部模型导入项目”下
 * 移动到 `.../chisel-lab/lab1` 目录下并选择 `build.sbt` 文件，点击 *下一步*
 * 在 *项目 JDK* 中选择高于1.8的JDK版本
 * 点击 *创建*
 
### 多数决投票器

第一个练习是完成一个相对简单的多数决投票器。

Chisel 组件 `Majority` 位于 `lab2/src/main/scala/Majority` 文件夹中，双击打开它。

这个 Chisel 组件应实现三个信号（`a`, `b` 和 `c`）的多数决投票。
多数决意味着，电路输出的结果应该是输入信号中出现次数最多的值，例如：
如果 `a==1, b==0, c==1` ，输出的结果就应该是 `1` 。
参见 Dally 3.6 节的解决方案。
> 译注：这里的 Dally 应该是 William J . Dally 的教材 《Digital Design Using VHDL: A Systems Approach》，非开源书籍，请自行购买或借阅。

在 Intellij 窗口底部的终端中，运行以下命令：

```
sbt test
```

以编译和测试你的项目。

在“运行”窗口中，你会看到一些测试失败信息，类似于：

```
[info] Suites: completed 7, aborted 0
[info] Tests: succeeded 2, failed 5, canceled 0, ignored 0, pending 0
[info] *** 5 TESTS FAILED ***
```

我们为多数决电路设置了三个*测试用例*：

 1. 打印测试 `MajorityPrinter` ：简单打印出电路的逻辑表，对纠错很有用，但不适合自动化回归测试。
 1. 简单测试 `MajoritySimple` : 一个仅涵盖了部分案例的简单测试，对于默认实现来说很简单。
   这意味着测试往往不能保证百分百正确的解决方案。
 1. 全量测试 `MajorityFull`: 这是一个涵盖了所有输入可能性的详尽测试，也往往是最好的测试形式。
   然而，详尽的测试仅在非常简单的电路上有实现性。
   
你可以在终端窗口运行以下命令，执行指定的单元测试：

```
sbt "testOnly MajorityPrinter"
```

运行 `MajorityPrinter` 来查看逻辑表输出：

```
Logic table for Majority
  a     b     c   -> out
false false false -> 0
true false false -> 1
false true false -> 0
true true false -> 1
false false true -> 0
true false true -> 1
false true true -> 0
true true true -> 1
[info] MajorityPrinter:
[info] Majority print results
[info] - should pass
[info] Run completed in 1 second, 246 milliseconds.
[info] Total number of tests run: 1
[info] Suites: completed 1, aborted 0
[info] Tests: succeeded 1, failed 0, canceled 0, ignored 0, pending 0
[info] All tests passed.
```

这个输出显示了原始的代码执行情况，只将输入 `a` 的值简单复制到了输出端。
显然，这不是一个多数决电路。请修改 `Majority` 组件，实现多数决电路。
你可以使用逻辑表来调试，并在代码编写完成后，运行以下命令：

```
sbt "testOnly MajorityFull"
```

来验证你通过了本实验。

### 选做：生成硬件

在实验一中，你已经学会了如何生成在 FPGA 上运行的硬件；而本实验中，你将使用测试来运行你的组合逻辑电路。
此外，我们也可以在FPGA板上运行这些电路，并使用开关和 LED 灯来测试电路。

使用以下命令运行 `Majority` 应用以生成 Verilog 代码：

```
sbt run
```

如果项目中又不止一个应用，你需要选择运行的目标，这里选择 `Majority`。
和测试一样，你可以这样指定运行哪个应用：

```
sbt "runMain Majority"
```

创建一个 Xilinx Vivado 项目，使用 `Majority.v` 作为源文件，`majority.xdc` 作为约束文件，其中包含引脚定义。
综合并实现设计，创建比特流，配置 FPGA，并使用三个开关 `sw0`、`sw1` 和 `sw2` 测试设备。

虽说使用真实的硬件测试设计是否正常工作可信度高，但这种方法有两个缺陷：
（1）综合——即使是小型的设计——会耗费大量时间；
（2）无法脱离手动工作。
相对而言，使用 Chisel 编写的测试可以更快、更容易复现和自动化。

### 多路复用器（Mux）

![Mux](../figures/mux.svg)


多路复用器对不同的的输入进行选择，如上图是一个 2:1 多路复用器。
我们使用 `sel` 输入信号决定 输出端 `y` 连接到输入端 `a` 还是 `b`。
本案例中，我们假使当 `sel` 为 `0` 或 `false` 时，`a` 被选中，否则 `b` 被选中。

打开 `Mux2` 组件以完成多路复用器，你可以使用以下命令来测试你的实现：

```
sbt "testOnly Mux2Spec"
```

一种底层的解决方案是用布尔方程来描述多路复用器，形如 `(!sel & a) | (sel & b)` 。
这也是对的，你可以在 `Mux2` 中尝试，但易读性很差。
此外，布尔方程不能很好地处理多位值。

更好的解决方案是使用条件赋值，在 Chisel 中对应着 `when` 和 `.otherwise` 。
在 `the Chisel book` 的第五章查阅相关内容，并实现多路复用器。

由于多路复用是非常基础的操作，Chisel 提供了 `Mux` 组件。
使用 `Mux` 组件实现最终版的多路复用器。

非常*炫酷*的是，`Mux` 组件可以对任意复杂的数据结构进行多路复用，而不仅仅是位向量。
任何用户定义的数据类型都适用于 `Mux` 。

### 解码器

![Decoder](../figures/decoder.svg)

下一个练习是描述一个 2 比特解码器，以下是其测试方法：

```
sbt "testOnly DecoderSpec"
```

你可以在 `Decoder.scala` 中找到练习的框架，并补全其中缺失的语句。
使用 Chisel 的 `switch` 语句可能是最优雅的解决方案，也可以尝试其他有效解法。

### 加减法电路

接下来的练习中，你要构建一个小型的算术电路，它可以对两个无符号整数进行加减。
输入端 `selAdd` 决定两个数是相加还是相减（类似于一个多路复用器）。
测试方法如下：

```
 sbt "testOnly AddSubSpec"
```

你需要在 `AddSub` 文件中实现加减法电路。

### 最大值查找器

接下来的练习是搭建一个查找四个无符号整数中的最大值的电路。
测试方法如下：

```
 sbt "testOnly MaxFinderSpec"
```

你会在 `MaxFinder.scala` 中找到练习的框架。
你需要使用多路复用器来选择四个输入中的最大值，尝试使用 `Mux(sel, trueCase, falseCase)` 函数。
你每次使用 `Mux` 时都会新建一个多路复用器，需要几个多路复用器来找到最大值？
你可以使用 `>` 来比较两个数，这会创建什么样的电路，又需要多少个比较器呢？

#### 选做：提供最大值的索引

作为可选的练习，你可以扩展最大值查找器，使其提供最大值的索引。
例如，如果输入 `c` 是最大值，输出的索引应为 `2` （我们假定 `a` 的索引为 `0`，`b` 的索引为 `1`，`c` 的索引为 `2`，`d` 的索引为 `3`）。
这个功能需要什么额外的资源？你可以使用 `MaxFinderSpec` 中的现有测试来验证你的行为。
添加索引会带来一些不确定性：如果两个或多个输入有相同的最大值，输出的索引应该是哪个？