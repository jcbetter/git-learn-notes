### `shell`
[toc]


### 1. 初识

#### 1.1 第一个 shell 脚本

test.sh ：

```shell 
#!/bin/bash

echo "Hello,World"
```

`#!` 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。

#### 1.2 运行 shell 脚本的两种方法

**1. 作为可执行程序执行**

```linux
jincheng@LAPTOP-VHI28MJ8:~$ chmod u+x test.sh
jincheng@LAPTOP-VHI28MJ8:~$ ./test.sh
Hello,World
```

**2. 作为解释器参数执行**

```linux
jincheng@LAPTOP-VHI28MJ8:~$ /bin/bash test.sh
Hello,World
```


### 2. 变量

#### 2.1 变量

定义变量时，变量名不加 `$` ，并且赋值号两边不能有空格，使用变量时需要在变量名前加 `$` ，并最好用 `{}` 将变量名包含起来。变量命名规则与一般标识符规则相同。

```shell
#!/bin/bash

num=123

echo $num      # 123，正确但不推荐
echo ${num}    # 123，总是将变量名用{}包含起来

name="mike"
echo "My name is ${name}."    # My name is mike.
```

**只读变量**

使用 `readonly` 命令可将变量定义为只读变量，只读变量的值不能被改变。

```shell
#!/bin/bash

num=123
readonly num
num=456         # test.sh: line 5: num: readonly variable
```

**删除变量**

使用 `unset` 命令删除变量，变量删除后不能继续使用，`unset` 不能删除只读变量。

```shell
#!/bin/bash

age=18
unset age
echo ${age}    # 不报错，但没有输出

url="www.baidu.com"
readonly url
unset url      # test.sh: line 9: unset: url: cannot unset: readonly variable
```

#### 2.2 字符串

字符串即可以使用 `''` 包含，也可以使用 `""` 包含。

- 单引号字符串中不能使用变量；内部可出现成对 `''` ，作为字符串拼接使用
- 双引号字符串中可以使用变量、转义字符。

```shell
#!/bin/bash

info='I love you!'
echo ${info}    # I love you!
info='I 'love' you!'
echo ${info}    # I love you!

msg="I want to tell you: ${info}"
echo ${msg}     # I want to tell you: I love you!
```

**拼接字符串**

成对使用 `''` 或 `""` 进行字符串拼接。

```shell
#!/bin/bash

name="mike"                                                               msg_1="hello,${name}!"                                                    msg_2="hello,"${name}

echo ${msg_1} ${msg_2}    # hello,mike! hello,mike!                       

msg_3='hello,${name}!'
msg_4='hello,'${name}'!'                                                  

echo ${msg_3} ${msg_4}    # hello,${name}! hello,mike!
```

**获取字符串长度**

在字符串变量名前加 `#` 获取字符串长度。

```shell
#!/bin/bash

str="abcde"

echo ${#str}    # 5
```

**提前子字符串**

下面实例从字符串中提取：第 2 个到第 5 个字符。（下标从 0 开始）

```shell
#!/bin/bash
str="abcdefghijkl"

{str:1:4}    # bcde 
```

**查找子字符串**

```shell
#!/bin/bash
str="abcdefghijkl"

echo `expr index "$str" "f"`    # 6
```

#### 2.3 数组

bash 支持一维数组（不支持多维数组），并且没有限定数组的大小，且下标从 0 开始。

**定义数组**

使用 `()` 表示数组，数组元素用空格分割。使用下标访问数组元素，使用 `@` 获取数组所有元素。

```linux
数组名=(值1 值2 ... 值n)
```
例如：

```shell
#!/bin/bash

arr_1=(1 2 3 4 5)

arr_2=(
6
7
8
9
)

echo ${arr_1[0]} ${arr_1[1]}    # 1 2
echo ${arr_2[0]} ${arr_2[1]}    # 6 7
echo ${arr_1[@]}                # 1 2 3 4 5
```

**获取数组长度**

获取数组长度的方法与获取字符串长度的方法相同。

```shell
#!/bin/bash

arr=(1 2 3 4 5)
echo ${#arr[@]}    # 5
echo ${#arr[*]}    # 5
```

#### 2.4 注释

使用 `#` 注释单行，使用 `:<<EOF EOF` 进行多行注释。

```shell
#!/bin/bash

:<<EOF
This is test file.
create on 10/26 2020.
EOF

name="mike"
age=18
# city="Beijing"

echo "Hello,I am ${name}, I am ${age} years old."    # Hello,I am mike, I am 18 years old.
```


### 3. 传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取第 n 个参数的格式为：`$n` 。`$0` 为执行的文件名，参数标号从 1 开始。

```shell
#!/bin/bash

echo "文件名为：$0"
echo "第一个参数：$1"
echo "第二个参数：$2"
```

运行脚本：
```linux
jincheng@LAPTOP-VHI28MJ8:~$ /bin/bash test.sh hello world
文件名为：test.sh
第一个参数：hello
第二个参数：world
```

**特殊参数**

- `$#`：传递到脚本的参数个数
- `$*`：以一个单字符串显示所有向脚本传递的参数
- `$@`：将每个参数均用引号包含并显示
- `$$`：脚本运行的当前PID
- `$?`：显示最后命令的退出状态。0 表示无错，其他任何值都表明有错误。

实例：
```shell
#!/bin/bash

echo "参数数量为：$#"    # 2
echo "所有参数为：$*"    # hello world
echo "所有参数为：$@"    # hello world
echo "脚本进程号：$$"    # 465
echo "退出状态为：$?"    # 0
```

`$*` 与 `$@` 的区别：
```shell
#!/bin/bash

for item in "$*"; do
    echo ${item}
done

for item in "$@"; do
    echo ${item}
done
```

脚本运行结果：
```linux
hello world
hello
world
```


### 4. 运算符

shell 和其他编程语言一样，支持多种运算符，包括：
- 算术运算符
- 关系运算符
- 布尔运算符
- 字符串运算符
- 文件测试运算符

原生 bash 不支持简单的数学运算，但是可以通过其他命令来实现，常见的有 `awk` 和 `expr` 。expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

例如：
```shell
#!/bin/bash

val=`expr 1 + 2`
echo ${val}        # 3
```

一点注意：在 expr 后的运算中，运算数与运算符之间要有空格，否则是不正确的。

#### 4.1 算数运算符

```shell
#!/bin/bash

a=10
b=20

echo `expr $a + $b`     # 30
echo `expr $a - $b`     # -10
echo `expr $a \* $b`    # 200
echo `expr $a / $b`     # 0
echo `expr $a % $b`     # 10

if [ $a == $b ];then
    echo "a equals to b"
fi

if [ $a != $b ];then
    echo "a diff from b"    # a diff from b
fi
```

#### 4.2 关系运算符

bash 的关系运算符只支持数字，不支持字符串，除非字符串的值是纯数字。

- `-eq`：等于
- `-ne`：不等于
- `-gt`：大于
- `-lt`：小于
- `-ge`：大于等于
- `-le`：小于等于

#### 4.3 布尔运算符

- `!`：非
- `-o`：或
- `-a`：与

注意例子中的 `[]` 数量。
```shell
#!/bin/bash

if [ 1 -lt 2 -a 3 -gt 2 ];then
    echo "pass"
fi
```

#### 4.4 逻辑运算符

- `$$`：与
- `||`：或

注意例子中的 `[]` 数量。
```shell
if [[ 1 -lt 2 && 3 -gt 2 ]];then
    echo "pass"
fi
```

#### 4.5 字符串运算符

- `=`：检查两个字符串是否相等
- `!=`：检查两个字符串是否不等
- `-z`：检查字符串长度是否为 0
- `-n`：检查字符串长度是否不为 0
- `$`：检查字符串是否不为空

```shell
#!/bin/bash

a="abc"
b="efg"

if [ $a = $b ];then
        echo "a与b相同"
fi

if [ $a != $b ];then
        echo "a与b不相同"
fi

if [ -z $a ];then
        echo "a的长度为0"
fi

if [ -n $a ];then
        echo "a的长度不为0"
fi

if [ $a ];then
        echo "a不为空"
fi
```

#### 4.6 文件测试运算符

- `-d [文件名]`：检查文件是否为目录文件
- `-f [文件名]`：检查文件是否为普通文件
- `-r [文件名]`：检查文件是否可读
- `-w [文件名]`：检查文件是否可写
- `-x [文件名]`：检查文件是否可执行
- `-s [文件名]`：检查文件大小是否大于 0
- `-e [文件名]`：检查文件是否存在

```shell
#!/bin/bash

if [ -r ./test.sh ];then
    echo "./test.bash 可读"
fi

:<<EOF
./test.bash 可读
EOF
```


### 5. `echo` 命令

echo 是用于字符串输出的命令。

用法：
- `echo [字符串]`：显示一个字符串
- `echo -e [字符串]`：`-e` 选项开启转义，显示一个带有转义字符的字符串
- `echo "Everything is just good!" > hello.txt`：将结果定向至文件
- echo \`ls /tmp/\`：显示命令执行的结果


### 6. `printf` 命令

语法：
```linux
printf "[格式]" 参数
```

格式：
- `%d`：十进制整数
- `%f`：浮点数
- `%s`：字符串
- `%c`：字符

实例：
```shell
#!/bin/bash

printf "%-5d,%-5.2f,%-5s,%-5c\n" 123 1.234 abc d    # 123  ,1.23 ,abc  ,d
```

上面的程序段中，`-` 表示左对齐，默认为右对齐；`5` 表示显示宽度；`.2` 表示保留两位小数。


### 7. `test` 命令

#### 7.1 数值测试

数值测试主要是基于 6 个比较运算符（`-eq`、`-ne`、`-gt`、`-lt`、`-ge`、`-le`）进行的。

```shell
#!/bin/bash

if test 2 -gt 1;then
    echo "pass"    # pass
fi
```

利用 `$[]` 可以进行基本的算数运算。

```shell
#!/bin/bash

if test $[1+2] -gt 1;then
    echo "pass"    # pass
fi
```

#### 7.2 字符串测试

字符串测试主要是基于 4 个字符串运算符（`-z`、`-n`、`=`、`!=`）进行的。

```shell
#!/bin/bash

a="abc"
b="efg"

if test $a = $b;then
    evho "pass"    # pass
fi
```

#### 7.3 文件测试

文件测试主要是基于文件运算符进行的。

```shell
#!/bin/bash

if test -e ./test.sh;then
    evho "exist"    # exist
fi
```


### 8. 流程控制

#### 8.1 `if else`

有一点需要注意，shell 中的 else 分支可以没有，但是不能为空。

格式 1：
```shell
if [条件]
then 
    [命令]
else
    [命令]
fi
```

格式 2：
```shell
if [条件]
then 
    [命令]
elif [条件]
then
    [命令]
else
    [命令]
fi
```

#### 8.2 `for`

格式：
```shell
for [条目] in [条目1] ... [条目n]
do
    [命令]
done
```

或者：
```shell
for [条目] in [条目1] ... [条目n]; do [命令]; done
```

实例：
```shell
#!/bin/bash

for ch in "I love coding."
do    
    printf "%s\n" ${ch}
done

:<<EOF
jincheng@LAPTOP-VHI28MJ8:~$ /bin/bash test.sh
I
love
coding.
EOF
```

#### 8.3 `while`

格式：
```shell
while [条件]
do
    [命令]
done
```

实例：
```shell
#!/bin/bash

int=1

while (( $int<=5 ))
do
    echo $int
    let int++
done

:<<EOF
jincheng@LAPTOP-VHI28MJ8:~$ /bin/bash test.sh
1
2
3
4
5
EOF
```

注意 `()` 的数量。上面的程序段用到了 `let` 命令，它用于执行一个或多个表达式，变量计算中不需要加上 `$` 来表示变量。

while 循环可用于读取键盘信息，按下 `Ctrl + D` 退出循环。
```shell
#!/bin/bash

echo -n "input your name:"    # 不换行输出

while read name
do
    echo "Hello, ${name}!"
done
```

**无限循环**

格式：
```shell
while :
do
    [命令]
done
```

或者：
```shell
for (( ; ; ))
```

#### 8.4 `until`

与 while 相反，until 运行到条件满足时停止。

格式：
```shell
until [条件]
do
    [命令]
done
```

实例：
```shell
#!/bin/bash

a=0

until [ ! &a -lt 5 ]
do
    echo $a
    a=`expr $a + 1`
done

:<<EOF
jincheng@LAPTOP-VHI28MJ8:~$ /bin/bash test.sh
0
1
2
3
4
EOF
```

#### 8.5 `case`

case 为多选语句。若值与模式匹配，则执行相匹配的命令。若无一模式与值匹配，则执行 * 后的命令。

格式：
```shell
case [值] in
[模式1])
    [命令]
    ;;
[模式2])
    [命令]
    ;;
[模式n])
    [命令]
    ;;
*)
    [命令]
    ;;
esac
```

实例：
```shell
#!/bin/bash

read num

case num in
1)
    echo "Input 1"
2)
    echo "Input 1"
3)
    echo "Input 1"
*)
    echo "Input others"
esac
```

#### 8.6 跳出循环

使用 `break` 和 `continue` 跳出循环。


### 9. 函数

格式：
```shell
function [函数名]() {
    [命令]
    return [整数]
}
```

最简单的形式：
```shell
[函数名] {
    [命令]
}
```

在这种最简单的形式下，其实就是讲一组命令打包起来，放在一起使用而已。

在函数中，同样可以使用传递参数时的特殊参数，用于对参数进行操作。

实例：
```shell
#!/bin/bash

function Sum() {
    sum=`expr $1 + $2`
    return ${sum}
}

Sum $1 $2

echo "${1} + ${2} equals to ${?}."    # 通过 $? 获取函数的返回值

:<<EOF
jincheng@LAPTOP-VHI28MJ8:~$ /bin/bash test.sh 2 3
2 + 3 equals to 5.
EOF
```


### 10. 输入/输出重定向

大多数 UNIX 系统命令从终端接受输入并将所产生的输出发送回​​到终端。通常情况下，这个标准输入就是你的键盘，标准输出就是你的屏幕。

命令：
- `[命令] > [文件名]`：将输出重定向到文件
- `[命令] < [文件名]`：将输入重定向到文件
- `[命令] >> [文件名]`：将输出以追加的方式重定向到文件
- `[文件描述符] > [文件名]`：将文件描述符对应的文件重定向到另一文件
- `[文件描述符] >> [文件名]`：将文件描述符对应的文件以追加的方式重定向到另一文件
- `[输出文件1] >& [输出文件2]`：将输出文件1和输出文件2合并
- `[输入文件1] <& [输入文件2]`：将输入文件1和输入文件2合并
- `<<[标记]`：将开始标记和结束标记之间的内容作为输入

通常，文件描述符 `0` 是标准输入（STDIN），`1` 是标准输出（STDOUT），`2` 是标准错误输出。

#### 10.1 输出重定向

实例：
```shell
# 列出文件

jincheng@LAPTOP-VHI28MJ8:~$ ls
GitTest  SoftWare  file  learn-notes  test  test.sh  tmp

# 将 ls 的输出重定向到文件 ls.res 中

jincheng@LAPTOP-VHI28MJ8:~$ ls > ls.res

# 列出文件

jincheng@LAPTOP-VHI28MJ8:~$ ls
GitTest  SoftWare  file  learn-notes  ls.res  test  test.sh  tmp

# 查看文件

jincheng@LAPTOP-VHI28MJ8:~$ cat ls.res
GitTest
SoftWare
file
learn-notes
ls.res
test
test.sh
tmp
```

输出重定向在文件不存在时会首先创建文件，在文件存在时会覆盖文件内容。若不想文件内容被覆盖，可以使用 `>>` 进行重定向。

```shell
# 列出文件

jincheng@LAPTOP-VHI28MJ8:~$ ls
GitTest  SoftWare  file  learn-notes  ls.res  test  test.sh  tmp

# 以追加的方式进行重定向

jincheng@LAPTOP-VHI28MJ8:~$ ls >> ls.res

# 查看文件

jincheng@LAPTOP-VHI28MJ8:~$ cat ls.res
GitTest
SoftWare
file
learn-notes
ls.res
test
test.sh
tmp
GitTest
SoftWare
file
learn-notes
ls.res
test
test.sh
tmp
```

#### 10.2 输入重定向

实例：
```shell
# 查看文件

jincheng@LAPTOP-VHI28MJ8:~$ cat file
Talk is cheap, show me the code.

Practise makes perfect.

Where there is a will, there is a way.

Good good study, day day up.

You say hi hi hi, Mr.Black.

# 将文件重定向到命令
# wc -l 用于统计输入文件的总行数

jincheng@LAPTOP-VHI28MJ8:~$ wc -l < file
9
```

典型用法：
```shell
# 希望将标准错误输出信息全部保存到文件中

[命令] 2 >> filename
```

实例：
```shell

```

#### 10.3 Here Document

Here Document 是 shell 中一种特殊的重定向方式，用来将输入重定向到一个交互式 shell 脚本或程序。

格式：
```shell
[命令] << [定界符]
    [文本]
[定界符]
```

它的作用是将两个定界符之间的内容作为输入传递给命令。

实例：
```shell
jincheng@LAPTOP-VHI28MJ8:~$ wc -l << EOF
> line1
> line2
> lline3
> EOF
3
```

#### 10.4 `/dev/null` 文件

当我们不希望某个命令的执行结果显示在屏幕上时，我们可以将其重定向到 `/dev/null` 。`/dev/null` 是一个特殊的文件，写入它的内容都会被丢弃。

实例：
```shell
jincheng@LAPTOP-VHI28MJ8:~$ ls > /dev/null
jincheng@LAPTOP-VHI28MJ8:~$
```


### 11. 文件包含

shell 可以不包含外部脚本。

格式：
```shell
. [文件名]    # 点号和文件名之间有一个空格

# 或者

source [文件名]
```

实例：
```shell
# test1.sh 脚本内容

jincheng@LAPTOP-VHI28MJ8:~$ cat test1.sh
name="Mike"

# test.sh 脚本内容

jincheng@LAPTOP-VHI28MJ8:~$ cat test.sh
#!/bin/bash

source test1.sh

echo "My name is ${name}."

# test.sh 包含 test1.sh

jincheng@LAPTOP-VHI28MJ8:~$ /bin/bash test.sh
My name is Mike.
```