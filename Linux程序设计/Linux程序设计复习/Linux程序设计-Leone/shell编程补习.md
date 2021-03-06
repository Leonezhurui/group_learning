# shell编程补习

## 如何引用变量？

```shell
a=1
echo ${a}
```

注意：

1. 赋值语句`a=1`，等号两边不能有空格！(有空格就编译不过)
2. 变量引用使用`${}`，大括号！



## 如何使用for循环？

```shell
for i in 
do
	#body
done
```

1-100for循环的几个实现：

1. `for i in {1..100}`

2. ```shell
   for i in `seq 1 100`
   ```

3. ```shell
   for ((i=1;i<=100;i++))
   ```

   







## 如何使用if循环？

if的结构就是：

```shell
if 条件表达式
then 
	#主体
elif 条件表达式
then
	#主体
fi
```



```shell
i=1
if [${i}==1]
then
    echo ${i}
fi
```

结果：`Solution.sh: line 4: [1==1]: command not found`。if条件判断中的condition判断不了，因为`[]`两边没有空格。

注意：

1. 判断条件中的condition`[]`两边必须要有空格！





接下来我们来看一下条件表达式的使用：

三种条件表达式：`[condition]`、`[[condition]]`、`((condition))`



`[[ condition1]] && || [[condition2]]`多条件协同判断，数学运算符号（+、-、*、/、%），if [[ "x$1" == "x" ]]，相比[ ]是针对**字符串表达式**的加强版；

(( condition ))中支持比较表达式（<、<=、>、>=、=、==、！=）数学运算符号和其他符号，相比[ ]是**针对数字表达式的加强版**。



那我们现在重新来看if的条件表达式：

三种条件表达式在ifyuju中都可以使用，但是有差异：

1. `if(($i>1))`：括号两边不需要空格，且可以使用数学运算符（<、<=、>、>=、=、==、！=）
2. `[]`和`[[]]`类似。`if[ $i \> 1 ]`和`if[ $i -lt 1 ]`：括号两遍需要空格，并且并不支持所有的数学运算符，最好使用(-eq, -ne, -lt, -le, -gt, -ge)



`$[]`和`$(())`是一样的，都是**进行数学运算**的。支持`+ - * / %`（“加、减、乘、除、取模”）



## read用法





## 判断符号

整数比较

```shell
-eq 等于,如:if [ "$a" -eq "$b" ]   
-ne 不等于,如:if [ "$a" -ne "$b" ]   
-gt 大于,如:if [ "$a" -gt "$b" ]   
-ge 大于等于,如:if [ "$a" -ge "$b" ]   
-lt 小于,如:if [ "$a" -lt "$b" ]   
-le 小于等于,如:if [ "$a" -le "$b" ]   
<   小于(需要双括号),如:(("$a" < "$b"))   
<=  小于等于(需要双括号),如:(("$a" <= "$b"))   
>   大于(需要双括号),如:(("$a" > "$b"))   
>=  大于等于(需要双括号),如:(("$a" >= "$b")) 
```



字符比较

```shell
字符串比较：
=                         等于           if [ "$a" = "$b" ]
==                        与=等价
!=                        不等于         if [ "$a" = "$b" ]
<                         小于，在ASCII字母中的顺序：
                          if [[ "$a" < "$b" ]]
                          if [ "$a" \< "$b" ]         #需要对<进行转义
>                         大于
-z                        字符串为null，即长度为0
-n                        字符串不为null，即长度不为0

```





## 补充命令

### bc

ash Shell 内置了对整数运算的支持，但是并不支持浮点运算，而 **Linux bc 命令可以很方便的进行浮点运算**，当然整数运算也不再话下。

bc 甚至可以称得上是一种编程语言了，它**支持变量、数组、输入输出、分支结构、循环结构、函数**等基本的编程元素，所以 Linux 手册中是这样来描述 bc 的：

An arbitrary precision calculator language

翻译过来就是“**一个任意精度的计算器语言**”。

在终端输入`bc`命令，然后回车即可**进入 bc 进行交互式**的数学计算。在 Shell 编程中，我们也可以通过**管道和输入重定向来使用 bc**。



bc内置变量

| 变量名      | 作 用                                                        |
| ----------- | ------------------------------------------------------------ |
| scale       | 指定精度，也即小数点后的位数；默认为 0，也即不使用小数部分。 |
| ibase       | 指定输入的数字的进制，默认为十进制。                         |
| obase       | 指定输出的数字的进制，默认为十进制。                         |
| last 或者 . | 表示最近打印的数字                                           |





在shell中要计算浮点数，则推荐使用bc；有小数点精确要求的则需要使用如下方法：

```shell
n=$(echo 'scale=5; 3/7'|bc -l|xargs printf "%.3f")
echo $n
```

由于bc的scale参数是将要保留位数后面的小数直接省去，所以如果有小数点精确要求，要将scale放大，并**使用printf来消除scale的影响**。



## 实战

hackerrank shell 2

输出1-100中的所有奇数

```shell
for i in `seq 1 100`
do
	if (($i%2 == 1))
	then
		echo $i
	fi
done
```



hackerrank shell Arithmetic Operations

```shell
#!/bin/bash
read line
echo "scale=5; $line" | bc -l | xargs printf "%.3f"
```

