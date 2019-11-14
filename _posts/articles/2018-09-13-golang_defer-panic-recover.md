---
layout: post
title: "Golang Defer Panic Recover用法(译)"
excerpt: "golang 非常规控制流" 
categories: articles
tags: ['golang', 'control flow, defer, panic, recover']
comments: true
share: true
image:
  feature: so-simple-sample-image-2.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

Go has the usual mechanisms for control flow: if, for, switch, goto. It also has the go statement to run code in a separate goroutine. Here I'd like to discuss some of the less common ones: defer, panic, and recover.

Golang有通常的控制流机制: if, for, switch, goto. 它也含有go语句以在单独的goroutine中运行代码. 这里我想讨论一下不太常见的方式: defer, panic, 和recover. 

A defer statement pushes a function call onto a list. The list of saved calls is executed after the surrounding function returns. Defer is commonly used to simplify functions that perform various clean-up actions.

defer语句将函数调用推入列表中.这个用来存储调用的列表会在周围函数返回之后执行. defer通常被用于简化执行各种清理动作的功能. 

For example, let's look at a function that opens two files and copies the contents of one file to the other:

例如, 让我们看看一个函数打开两个文件并复制一个文件的内容到另一个文件中. 

```golang
func CopyFile(dstName, srcName string) (written int64, err error) {
    src, err := os.Open(srcName)
    if err != nil {
        return
    }

    dst, err := os.Create(dstName)
    if err != nil {
        return
    }

    written, err = io.Copy(dst, src)
    dst.Close()
    src.Close()
    return
}
```

This works, but there is a bug. If the call to os.Create fails, the function will return without closing the source file. This can be easily remedied by putting a call to src.Close before the second return statement, but if the function were more complex the problem might not be so easily noticed and resolved. By introducing defer statements we can ensure that the files are always closed:

它可以工作, 但是这里有一个bug. 如果调用os.Create失败, 这个函数会返回而不关闭source文件. 这可以简单地通过在第二个return语句之前调用src.Close解决这个问题, 但是如果该函数更加复杂的话,该问题可能不会那么容易被觉察和解决.通过引入defer语句, 我们可以确保文件始终会被关闭. 

```golang
func CopyFile(dstName, srcName string) (written int64, err error) {
    src, err := os.Open(srcName)
    if err != nil {
        return
    }
    defer src.Close()

    dst, err := os.Create(dstName)
    if err != nil {
        return
    }
    defer dst.Close()

    return io.Copy(dst, src)
}
```

Defer statements allow us to think about closing each file right after opening it, guaranteeing that, regardless of the number of return statements in the function, the files will be closed.

Defer语句允许我们考虑每一个文件打开之后关闭它, 保证不论retrun语句在函数中的数量, 文件都会被关闭. 

The behavior of defer statements is straightforward and predictable.

Defer语句的行为是直接和可预测的．　

There are three simple rules:

这里有三条简单的规则: 

1.A deferred function's arguments are evaluated when the defer statement is evaluated.

延迟函数的参数在defer语句评估时进行评估．　

In this example, the expression "i" is evaluated when the Println call is deferred. The deferred call will print "0" after the function returns.

在这个例子中，"i"表达式在延迟Println调用时被评估．　这个延迟调用会在函数返回之后打印"0". 

```golang
func a() {
    i := 0
    defer fmt.Println(i)
    i++
    return
}
```

2.Deferred function calls are executed in Last In First Out order after the surrounding function returns.

延迟函数调用会在周围函数返回之后按照Last In First Out(后进先出)顺序执行．　

This function prints "3210":

这个函数打印"3210":

```golang
func b() {
    for i := 0; i < 4; i++ {
        defer fmt.Print(i)
    }
}
```

3.Deferred functions may read and assign to the returning function's named return values.

延迟函数会读取并分配给返回函数的指定返回值．　

In this example, a deferred function increments the return value i after the surrounding function returns. Thus, this function returns 2:

在这个例子中，　延迟函数在周围函数返回之后增加了返回值i．　因此，　这个函数返回２：　

```golang
func c() (i int) {
    defer func() { i++ }()
    return 1
}
```

This is convenient for modifying the error return value of a function; we will see an example of this shortly.

修改一个函数的返回值会变得很便捷．　我们将很快看到一个例子．　

Panic is a built-in function that stops the ordinary flow of control and begins panicking. When the function F calls panic, execution of F stops, any deferred functions in F are executed normally, and then F returns to its caller. To the caller, F then behaves like a call to panic. The process continues up the stack until all functions in the current goroutine have returned, at which point the program crashes. Panics can be initiated by invoking panic directly. They can also be caused by runtime errors, such as out-of-bounds array accesses.

Panic是一种内置的函数用来停止普通工作流并开始paniking.当函数F调用panic, F停止执行，　F中其他延迟函数会被正常执行，　然后F返回给它的调用者．　对于调用者，　F充当一个panic调用．　该过程持续进行，　直到当前goroutine中的所有函数返回，　此时这个程序crashes.Pnics可以在直接调用panick时被初始化．　他们也可以被runtime错误所引起，　例如超出范围的数组访问．　

Recover is a built-in function that regains control of a panicking goroutine. Recover is only useful inside deferred functions. During normal execution, a call to recover will return nil and have no other effect. If the current goroutine is panicking, a call to recover will capture the value given to panic and resume normal execution.

Recover是一种内置函数用来重新控制a panicking goroutine.Recover只是在延迟函数中有用．　在正常执行期间，　一个recover的调用会返回nil和没有任何影响．　如果当前goroutine是在panicking，　recover调用会获取给到panic的值并且恢复到正常执行．　

Here's an example program that demonstrates the mechanics of panic and defer:

下面是一个例子程序用来演示panic和defer机制．　

```golang
package main

import "fmt"

func main() {
    f()
    fmt.Println("Returned normally from f.")
}

func f() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered in f", r)
        }
    }()
    fmt.Println("Calling g.")
    g(0)
    fmt.Println("Returned normally from g.")
}

func g(i int) {
    if i > 3 {
        fmt.Println("Panicking!")
        panic(fmt.Sprintf("%v", i))
    }
    defer fmt.Println("Defer in g", i)
    fmt.Println("Printing in g", i)
    g(i + 1)
}
```

The function g takes the int i, and panics if i is greater than 3, or else it calls itself with the argument i+1. The function f defers a function that calls recover and prints the recovered value (if it is non-nil). Try to picture what the output of this program might be before reading on.

函数g接受整数i, 并当i的值大于３时panic, 否则它调用它自己伴随着参数i+1.函数f延迟了调用recover并打印recovered值(非空)的函数．　尝试描绘程序可能的输出在继续读之前．　

The program will output:

```
Calling g.
Printing in g 0
Printing in g 1
Printing in g 2
Printing in g 3
Panicking!
Defer in g 3
Defer in g 2
Defer in g 1
Defer in g 0
Recovered in f 4
Returned normally from f.
```

If we remove the deferred function from f the panic is not recovered and reaches the top of the goroutine's call stack, terminating the program. This modified program will output:

如果我们删掉f函数中的延迟函数，　panic不会被恢复并且直达goroutine的调用栈，　终止该程序．　这样修改的程序的输出是：　

```
Calling g.
Printing in g 0
Printing in g 1
Printing in g 2
Printing in g 3
Panicking!
Defer in g 3
Defer in g 2
Defer in g 1
Defer in g 0
panic: 4
 
panic PC=0x2a9cd8
[stack trace omitted]
```

For a real-world example of panic and recover, see the json package from the Go standard library. It decodes JSON-encoded data with a set of recursive functions. When malformed JSON is encountered, the parser calls panic to unwind the stack to the top-level function call, which recovers from the panic and returns an appropriate error value (see the 'error' and 'unmarshal' methods of the decodeState type in decode.go).

对于真是环境的panic和recover案例，　查看Go标准库的json包．　它使用一组循环的函数解码JSON编码的数据．　当malformed JSON发生，　

The convention in the Go libraries is that even when a package uses panic internally, its external API still presents explicit error return values.

在Go库中约定即使内部包使用panic，　外部API仍然显示明显的返回错误．　

Other uses of defer (beyond the file.Close example given earlier) include releasing a mutex:

其他defer使用包括释放mutex.

```golang
mu.Lock()
defer mu.Unlock()
printing a footer:

printHeader()
defer printFooter()
and more.
```

In summary, the defer statement (with or without panic and recover) provides an unusual and powerful mechanism for control flow. It can be used to model a number of features implemented by special-purpose structures in other programming languages. Try it out.

总之，　defer语句( 不论是否包含panic和recover)提供了一套特别地／有力地控制流机制．　它可以模拟其他编程语言中特殊目的实现的功能．

By Andrew Gerrand

> Source:   
>  https://blog.golang.org/defer-panic-and-recover
