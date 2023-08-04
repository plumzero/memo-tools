
显示二进制目标文件的符号表(常用于查找函数名称)

通过此命令对汇编后的目标文件或可执行文件进行查看。

对于解析 undefined symbol 编译错误(即程序要求调用某个函数，但该函数因为某种原因，未被链接到可执行程序中)，nm 命令非常有用。

通过对所有目标文件运行 nm 命令，就可定位缺失的符号是在哪里引用的，在哪里定义的。
```shell
    nm obj.o
    nm a.out
```