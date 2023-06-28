# hany_gdb_notes

个人笔记，后续不断添加并更新

## 编译程序

`g++ -g -wall program.cpp -o program` 

`-g` 表示包含调试信息

`-wall`  显示所有的警告信息

注意：`-g` 选项的作用是在可执行文件中加入源代码的信息，比如可执行文件中第几条机器指令对应源代码的第几行，但并不是把整个源文件嵌入到可执行文件中，所以在调试时必须保证 gdb 能找到源文件。  

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

`start` ：程序开始运行，停在异地行

`r - restart`  ：重新启动程序,从程序的起始位置开始执行。这会重新执行初始化代码,重置变量的值等

`c - continue` ：继续程序,继续从当前断点往后执行。这不会重新执行初始化代码,变量的值也不会重置

`n - next` ：执行一行源程序代码，此行代码中的函数调用也一并执行。相当于 `Step Over (单步跟踪)` 

`s - step` ：执行一行源程序代码，如果此行代码中有函数调用，则进入该函数，相当于 `Step Into (单步跟踪进入)` 

`until` ：跳出循环

`finish` ：跳出函数体（只有函数体中没有断点才能跳出）

## 设置断点

断点表示期望程序运行停止的位置，设置断点后，程序运行到断点处，停止运行，以便观察程序的当前状态

### 断点相关命令

<img src=".\README.assets\image-20230625153154874.png" alt="image-20230625153154874" style="zoom:67%;" />

<img src=".\README.assets\image-20230625153351556.png" alt="image-20230625153351556" style="zoom: 50%;" />

## 打印变量

### 打印变量主要命令

`p(print) 变量名` 打印变量值

`ptype 变量名` 打印变量类型

`x addr` 

#### 格式化输出变量

<img src=".\README.assets\image-20230625154432081.png" alt="image-20230625154432081" style="zoom:67%;" />

#### 数组打印相关设置

- 数组默认打印200个
- `set print elements numberelems(打印的个数) ` 改变打印个数
- 不限制打印个数 `set print elements 0`
- 打印数组的范围 `print *(arr + 起始地址)@num` 
- 打印数组内容的同时显示数组下标 `set print array-indexes on` 

#### 其他操作

`set var 变量名=变量值` (直接设置变量的值)

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

## 自动变量操作

`display 变量名` (自动打印指定变量的值)

`i/info display` 展示设置的自动变量

`undisplay 编号` 移除自动变量

## 查看当前文件代码

`list/l`  (从默认位置显示)

`list/l 行号`  （从指定的行显示）

`list/l 函数名` （从指定的函数显示）

`list/l 文件名:行号` 

`list/l 文件名:函数名` 

`show list/listsize`  查看设置的值

`set list/listsize 行数`  设置显示的行数

## 设置程序参数/获取参数

`set args xx xx` 

`show args`

## 多进程调试

多进程是 gdb 默认只跟踪一个进程

- 设置调试父进程或者子进程：`set follow-fork-mode parent/child` 
  - 查看调试选项 `info follow-fork-mode` 
- 设置调试模式：`set detach-on-fork on/off` 
  - 默认为 on ，表示调试当前进程的时候，其他进程继续运行，如果为 off ，调试当前进程时，其他进程在 fork 处被 GDB 挂起
  - 查看参数：`info detach-on-fork` 
- 查看调试的进程：`info inferiors` 
- 切换当前调试的进程：`inferior id`  
  - id 为查看调试进程时的 id
- 使进程脱离 GDB 调试：`detach inferiors id` 
- 移除某个进程： `remove inferiors id` 
- 杀死某个进程：`kill inferiors id`
