# 简介

Julia 是一个面向科学计算的高性能动态高级程序设计语言。语法和其他科学计算编程语言类似，易于学习。Julia提供了数字精度特性和分布式并行运行方式，并拥有丰富的数学计算函数库。

> [julialang.org](http://julialang.org/)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/julia/logo.png)

# 使用

## 启动Julia REPL

```console
$ docker run -it --rm julia
```

## 将你本地的Julia脚本在容器中运行

```console
$ docker run -it --rm -v "$PWD":/usr/myapp -w /usr/myapp julia julia script.jl arg1 arg2
```

# 授权

查看[授权](http://julialang.org/)
