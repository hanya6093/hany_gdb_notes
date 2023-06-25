# hany_gdb_notes

个人笔记，后续不断添加并更新



## 启动调试程序

### 启动调试程序文件

`gdb program`  program - 可执行文件

### 调试正在运行的进程

`gdb -p pid`

`gdb attach pid` 

<img src=".\README.assets\image-20230625152525892.png" alt="image-20230625152525892" style="zoom: 50%;" />

### 退出调试程序

`quit` 

### 运行调试程序

`r - restart`  ：重新启动程序,从程序的起始位置开始执行。这会重新执行初始化代码,重置变量的值等

`c - continue` ：继续程序,继续从当前断点往后执行。这不会重新执行初始化代码,变量的值也不会重置

`n - next` ：执行一行源程序代码，此行代码中的函数调用也一并执行。相当于 `Step Over (单步跟踪)` 

`s - step` ：执行一行源程序代码，如果此行代码中有函数调用，则进入该函数，相当于 `Step Into (单步跟踪进入)` 

## 设置断点

断点表示期望程序运行停止的位置，设置断点后，程序运行到断点处，停止运行，以便观察程序的当前状态

### 断点相关命令

<img src=".\README.assets\image-20230625153154874.png" alt="image-20230625153154874" style="zoom:67%;" />

<img src=".\README.assets\image-20230625153351556.png" alt="image-20230625153351556" style="zoom: 50%;" />

## 打印变量

### 打印变量主要命令

`p(print) 变量名` 

`x addr` 

#### 格式化输出变量

<img src=".\README.assets\image-20230625154432081.png" alt="image-20230625154432081" style="zoom:67%;" />

#### 数组打印相关设置

- 数组默认打印200个
- `set print elements numberelems(打印的个数) ` 改变打印个数
- 不限制打印个数 `set print elements 0`
- 打印数组的范围 `print *(arr + 起始地址)@num` 
- 打印数组内容的同时显示数组下标 `set print array-indexes on` 

### 内存打印

`x/nfu addr` 

以 f 格式打印从 addr 开始的 n 个长度单元为 u 的内存值

<img src=".\README.assets\image-20230625155349109.png" alt="image-20230625155349109" style="zoom:67%;" />

## 调试程序方法

<img src=".\README.assets\image-20230625155935816.png" alt="image-20230625155935816" style="zoom:67%;" />

## watch命令

`watch expression`  命令用于监控某个*表达式*的值是否发生变化

- *表达式*：1、变量  2、内存值得变化

- 监控的值发生变化，gdb 会中断程序执行并停在对应的断点处，观察对应的值的变化是否符合预期

`info watch` 查看当前 watch 的信息

<img src=".\README.assets\image-20230625164559883.png" alt="image-20230625164559883" style="zoom: 50%;" />
