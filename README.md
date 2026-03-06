# Chisel in Digital Electronics 2 实验资料

本项目翻译自 [schoeberl/chisel-lab](https://github.com/schoeberl/chisel-lab) ，使用人工智能(AI)辅助翻译。

本仓库为丹麦技术大学(Technical University of Denmark, DTU)
[数字电子学 (02139)](http://www2.imm.dtu.dk/courses/02139/) 课程的配套实验资料。
尽管本实验是为上述课程定制的，但仍可以作为基于[Chisel](https://chisel.eecs.berkeley.edu/)的数字设计入门实验用。

本实验基于 Martin Schoeberl 的教材
[Digital Design with Chisel](http://www.imm.dtu.dk/~masca/chisel-book.html) 。

[![Book Cover](figures/cover-small.jpg)](http://www.imm.dtu.dk/~masca/chisel-book.html)

你可以通过如下方法使用该资料：
(1) 下载仓库的 `.zip` 文件；
(2) 克隆仓库到本地，并在本地完成实验；
(3) Fork 仓库，并克隆到本地，以便参与贡献；
或者 (4) 直接在 GitHub 上在线浏览。

最好在你的本地计算机上安装所有工具，参见 [Setup.md](Setup.md)。

我们在 [FAQ.md](FAQ.md) 中收集并解答了一些了常见问题。

**贡献**：我们欢迎以 `pull request` 形式参与贡献，即使是修复一个小的拼写错误也是如此。

## 实验概览

以下是所有实验课及作业的概览。自动售货机 (Vending Machine) 的相关实验请与助教确认通过。

 * [实验一：你好，Chisel](lab1)
 * [实验二：Chisel 组合逻辑电路](lab2)
 * [实验三：Chisel 组件和小型时序逻辑电路](lab3)
 * [实验四：简单测试器](lab4)
 * [实验五：七段译码器和测试](lab5)
 * [实验六：多路复用七段显示器](lab6)
 * [实验七：使用外部组件：串行端口](lab7)
 * [实验十：测试自动售货机](lab10)
 * [实验十一/十二：自动售货机](vending)

## 资源

 * [Setup.md](Setup.md) 安装说明
 * [Basys3_Master.xdc](Basys3_Master.xdc) 引脚定义和时钟约束文件
 * [IntelliJ for Scala](https://docs.scala-lang.org/getting-started-intellij-track/getting-started-with-scala-in-intellij.html)
 * [Vivado WebPACK](https://www.xilinx.com/products/design-tools/vivado/vivado-webpack.html)
 * [The Chisel Jupyter Notebooks](https://mybinder.org/v2/gh/freechipsproject/chisel-bootcamp/master)
 Scala 语言和 Chisel 的在线入门教程。
 * [The Chisel Cookbook](https://github.com/freechipsproject/chisel3/wiki/Cookbook) 大型的 Chisel 常见问题解答和介绍。
 * [Basys 3](https://reference.digilentinc.com/reference/programmable-logic/basys-3/start?redirect=1)