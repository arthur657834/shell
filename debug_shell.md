信号名	何时产生
EXIT	从一个函数中退出或整个脚本执行完毕
ERR	当一条命令返回非零状态时(代表命令执行不成功)
DEBUG	脚本中每一条命令执行之前

-x 是 set 命令中的一个选项，它用来进入跟踪方式，这样会显示出脚本执行每一条命令及其参数，它是脚本调试中的一个有用选项。它输出的被执行的命令行及参数前面会添加一个 "+" 号。实际上，这个 "+" 号就是内置变量 $PS4 的值，可以输出验证：
[beyes@localhost ~]$ echo $PS4
+

export PS4='+$LINENO: {${FUNCNAME[0]}} '
$LINENO 表示脚本执行到的当前行，这和 C语言 中的内置宏 __LINE__ 是一个道理。

$FUNCTION 也是一个内置变量，它相当于 C语言 中的内置宏 __func__ 。但 $FUNCTION 它比 __func__ 更强大，它实际上是一个数组变量，如同堆栈一般，在栈底的是脚本的顶层"main"，在栈顶的是当前所执行的函数，所以 $[FUNCTION[0]} 就是要显示出当前的执行函数。



ex1.sh
ERRTRAP()
{
  echo "[LINE:$1] Error: Command or function exited with status $?"
}
foo()
{
  return 1;
}
trap 'ERRTRAP $LINENO' ERR
abc
foo

ex2.sh
#!/bin/bash
trap 'echo “before execute line:$LINENO, a=$a,b=$b,c=$c”' DEBUG
a=1
if [ "$a" -eq 1 ]
then
   b=2
else
   b=1
fi
c=3
echo "end"

ex3.sh
DEBUG()
{
if [ "$DEBUG" = "true" ]; then
    $@　　
fi
}
a=1
DEBUG echo "a=$a"
if [ "$a" -eq 1 ]
then
     b=2
else
     b=1
fi
DEBUG echo "b=$b"
c=3
DEBUG echo "c=$c"


