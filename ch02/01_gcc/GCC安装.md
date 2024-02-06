

### 安装

配置
```s
  cd /usr/local/src/gcc-build
  ../gcc-8.4.0/configure --prefix=/usr/local/gcc --with-mpc=/usr/local/gcc-depend/mpc --with-mpfr=/usr/local/gcc-depend/mpfr --with-isl=/usr/local/gcc-depend/isl --with-gmp=/usr/local/gcc-depend/gmp --disable-multilib --enable-checking=release --enable-languages=c,c++ --disable-bootstrap --with-system-zlib
```
这里使用的 zlib 是系统环境上的，而不是 gcc 目录下自带的包，这也是推荐的做法。

编译
```s
  make -j4
```

在安装之前，可以对其进行测试(但可以不必强制):
```s
  ulimit -s 32768
  make -k test
```

```s
  make install
```

### 问题处理

make 时报找不到库错误:
```s
  /gcc/cc1: error while loading shared libraries: libisl.so.15: cannot open sh
```

解决办法是动态指定库路径编译:
```s
  LD_LIBRARY_PATH=/usr/local/gcc-depend/isl/lib make
```

(非并行)编译时长大约 1.5 个小时。

在有些机器上编译时可能还会出现类似于如下的错误:
```s
  configure: error: source directory already configured; run "make distclean" there first
  make[1]: *** [configure-mpfr] Error 1
  make[1]: Leaving directory `/usr/local/src/gcc-build'
  make: *** [all] Error 2
  
  configure: error: source directory already configured; run "make distclean" there first
  make[1]: *** [configure-isl] Error 1
  make[1]: Leaving directory `/usr/local/src/gcc-build'
  make: *** [all] Error 2
```
这个不妨将 `/usr/local/src/gcc-8.4.0` 中的文件清理一下(如删除 mpfr, isl 等)，或者直接还原回原来的状态，再试一下。

其他错误可以从 config.log 中查看原因分析。
