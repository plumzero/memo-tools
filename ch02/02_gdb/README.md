
### gdb功能

- 设置断点(可以是条件表达式)
- 单步执行
- 查看变量
- 动态改变执行环境
- 分析崩溃程序产生的core文件

### 常用交互命令一览

gdb 在调试程序时执行的操作主要有 5 个，分别是启动程序、设置断点、查看信息、分步运行、改变环境。

gdb 常用的调试命令如下:

|    命令    | 缩写  |  功能         |
|:-----------|:-----|:-------------|
| list       | l    | 查看程序源代码 |
| break      | b    | 设置断点      |
| run        | r    | 运行程序      |
| print      | p    | 显示变量或表达式的值 |
| display    | disp | 跟踪某个变量，每次程序停下来都显示该变量的值 |
| continue   | c    | 程序继续执行，直到下一个断点 |
| step       | s    | 执行下一条语句，若该语句为函数调用，则进入该函数执行第一条语句后停止 |
| next       | n    | 执行下一条语句，若该语句为函数调用，则不进入该函数内部，直接执行函数，在函数的下一行语句处停止 |
| start      | st   | 开始执行程序，在 main 函数的第一条停止 |
| backstrace | bt   | 查看函数调用信息 |
| frame      | f    | 查看栈帧 |
| watch      | w    | 监视变量值的变化，一旦被监视变量的值发生变化，程序就暂停下来 |
| delete     | del  | 删除断点 |
| info       | i    | 描述程序状态 |
| setvar     |      | 设置变量的值 |
| file       |      | 加载需要调用的程序 |
| kill       | k    | 终止正在调试的程序 |
| quit       | q    | 退出GDB环境 |

[部分交互命令说明](部分交互命令说明.md)


### 快速使用

- 关于断点操作
  ```sh
    b 25                // 在 25 行处设置断点
    info break          // 查看断点信息
    dis 1               // 使断点 1 无效
    ena 1               // 使断点 1 有效
    del 1               // 删除断点 1
  ```

- 变量查看
  ```sh
    info local          // 查看本地变量表
    info args           // 查看输入参数
    disp avg            // 程序执行过程中，对 avg 进行实时观察
    undisp 1            // 取消对编号为 1 的成员的观察
  ```

- 对堆栈的操作
  ```sh
    bt                  // 堆栈回溯，即列举中堆栈帧
    f 1                 // 切换到第 1 帧堆栈
    up/down             // 在堆栈中上下移动
  ```

- 单步执行操作
  ```sh
    s
    n
    u
    c
    ret                 // 从当前函数立即抬，并给出返回值
    j 105               // 跳到第 105 行
  ```

- 对多线程的操作
  ```sh
    info threads        // 获得多线程列表
    thread 2            // 切换到线程 2
  ```
  
- 为参数指定别名
  ```sh
    set $vd = my_model->dataset->vector->data
    p *$vd@10
    set $ptr=&x[3]
    p *$ptr=8
    p *($ptr++)
  ```

### 利用 bt 对堆栈的追踪示例

很多程序分析技巧就是跳入堆栈中，从一个个的函数的堆栈中追踪因果关系。

示例伪码
```c
    // addresses.c
    agent_address(int) {}
    get_agents() {
        ...
        agent_address(int);
        ...
    }
    main() {
        ...
        get_agents();
        ...
    }
```
通过 backtrace 命令可以查看该程序堆栈帧如下:
```sh
    #0 0x00413bbe in agent_address (agent_number=312) at addresses.c:100
    #1 0x004148b6 in get_agents() at addresses.c:163
    #2 0x00404f9b in main(argc=1,argv=0x7fffffffe278) at addresses.c:227
```
堆栈顶端是 0 帧，帧号后的十六进制是一个地址，被调函数返回的时候，程序执行将返回这个地址。

地址后面是函数名、入参，以及当前执行到的源代码行号。
