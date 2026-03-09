# Digital Electronics 2 疑难解答

## Chisel

以下是一些常见的 Chisel 结构问题。

### 位提取

你可以这样从一个总线 `x` 中提取一个导线的子集：
    
    x(n, m)

该操作会提取总线 `x` 的第 `n` 到第 `m` 位导线，包括 `n` 和 `m` 。请注意，`n` 必须大于或等于 `m`，否则 Chisel 会抛出难以理解的错误。

### 缺失的 `.W`

在定义具有特定宽度的常量时，可能出现遗漏 `.W` 指示符的错误。例如：
`1.U(32)` 并不会定义一个 32 位宽的表示 `1` 的常量；
表达式中的 `(32)` 会被解释为从第 32 位进行位提取，导致整个式子的结果为单比特常量 `0`。这可能不是程序员的本意。

> 译注：定义 32 位宽的表示 `1` 的常量的写法应该是 `1.U(32.W)`。


## 错误信息

### 数据流方向检查失败

``` 
firrtl.passes.CheckFlows$WrongFlow:  @[cmdx.sc xx:xx]: [module xxxx]  Expression _T is used as a SinkFlow but can only be used as a SourceFlow.
```

不能绑定输出总线的**部分**导线，在一个表达式中，只能**全部**绑定或不绑定。一种解决方法是声明一个总线，并在其中绑定部分导线，然后再将这个总线绑定到输出总线。


### Scala 类型匹配错误

```
scala.MatchError: List(UInt\<x\>(0) ... UInt\<x>(y)) (of class scala.collection.immutable.$colon$colon)
```
该报错往往表明你声明枚举（Enum）变量时使用了错误的元素数量指示。

### 无法重新分配给只读对象

该错误往往在试图声明总线的一个子集时发生。例如：
```scala
class someClass extends Module {
  val io = IO(new Bundle {
    val y = Output(2.W)
})

// 下为非法的 Chisel3 代码
io.y(0) := 0.U
io.y(1) := 1.U
}
```
可以使用*提取和拼接*解决该问题，参考[SO post](https://stackoverflow.com/questions/40950073/chisel-3-assignment-to-bit-range)。

另一种方法是将你的总线解包为一个布尔类型向量，对其进行修改，然后重新打包为总线。参见 [the Chisel Cookbook](https://github.com/freechipsproject/chisel3/wiki/Cookbook#how-do-i-do-subword-assignment-assign-to-some-bits-in-a-uint) 中 *How do I do subword assignement?* 一节。

### 理解非常长的堆栈跟踪

你有时候会在错误发生时遇到非常长的堆栈跟踪信息，这往往意味着错误产生于 Scala ，而非 Chisel 专有。在这种超长堆栈跟踪信息中，绝大多数错误信息会被缩进；从下往上翻阅，直到遇到如下格式的信息：

```
Exception in thread "main" ...
    (lots of indented messages 许多缩进的信息)
Caused by: (THE REAL ERROR MESSAGE 真正的错误信息)
    (more indented messages 更多缩进的信息)
```

在折叠的错误信息的第二部分可能能找到用蓝色高亮的链接，这些链接会指向代码中发生错误的部分。

### 数组索引越界异常（IndexOutOfBoundsException）

在进行上文提到的“对向量进行子集指派”操作时可能发生：

```scala
// 非法代码，请勿复制
val myVec := Wire(Vec(1, UInt(16.W)))
myVec(2) := 0.U 
```

当使用 `myVec(2)` 访问上述向量时，Chisel 会试图选取 `myVec` 向量中的第三个元素，而非 `myVec(1)` 元素的第二位。

### 枚举（Enum）

有限状态机（FSM）状态的 `Enum(n)` 若包含数值错误，将产生冗长的错误日志。

## IntelliJ

### 我的文件列表消失了
在 IntelliJ 的最左侧，点击 "Project" 重新打开文件视图。

### 我的终端消失了

选择 `视图 > 工具窗口 > 终端` 或者按 `Alt + F12` 重新打开终端。
又或者，可以直接使用 `sbt shell` 指令。注意在 `sbt shell` 中执行的指令**不**应该包含 `sbt` 前缀（例如，应该输入 `run` 而不是 `sbt run` 并回车确认）。

## Vivado

### 比特流生成失败（Bitstream Generation failed）

该错误在 `.xdc` 文件缺失引脚分配时产生。在警告/错误（warning/error）中查找蓝色文本以了解哪些引脚未分配。

### 无法连接 Basys3 板

按照[本链接](https://www.xilinx.com/support/answers/59128.html)的指导修复。确保你以管理员身份打开 shell / 命令提示符，并进入了正确的文件夹，操作不需要额外加任何参数。


