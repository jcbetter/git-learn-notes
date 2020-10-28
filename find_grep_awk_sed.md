### linux 四剑客
[toc]

### 1. `find`

#### 1.1 基本使用

语法：
```linux
find [目录] [选项]
```

选项：

- `-name`：按文件名查找。
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -name "hello.txt"
  tmp/hello.txt
  ```

- `-iname`：按文件名查找，忽略大小写。
  ```linux
  
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -iname "hello.txt"
  tmp/HELLO.TXT
  tmp/hello.txt
  ```

- `type`：按文件类型查找。
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -type d    # 目录类型
  tmp/
  tmp/books
  tmp/contacts

  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -type f    # 一般文件类型
  tmp/calcu.cpp
  tmp/config.cpp
  tmp/FreeRTOSv10.4.1.zip
  tmp/HELLO.TXT
  tmp/hello.txt
  tmp/hi.txt
  tmp/linux-5.6.13.tar.xz
  tmp/pi.py
  tmp/server.go
  tmp/style.html
  tmp/test.sh
  tmp/test.txt
  ```

- `-size`：按照文件大小查找。
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -size 1k     # 文件长度等于 1k
  tmp/hello.txt
  tmp/test.sh
  tmp/test.txt

  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -size -1k    # 文件长度小于 1k
  tmp/calcu.cpp
  tmp/config.cpp
  tmp/HELLO.TXT
  tmp/hi.txt
  tmp/pi.py
  tmp/server.go
  tmp/style.html

  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -size +1k    # 文件长度大于 1k
  tmp/
  tmp/books
  tmp/contacts
  tmp/FreeRTOSv10.4.1.zip
  tmp/linux-5.6.13.tar.xz
  ```

- `-mtime`：按修改时间查找。
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -mtime 1     # 1 天前的一天内
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -mtime +1    # 1 天之前
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -mtime -1    # 1 天以内
  tmp/
  tmp/books
  tmp/calcu.cpp
  tmp/config.cpp
  tmp/contacts
  tmp/FreeRTOSv10.4.1.zip
  tmp/HELLO.TXT
  tmp/hello.txt
  tmp/hi.txt
  tmp/linux-5.6.13.tar.xz
  tmp/pi.py
  tmp/server.go
  tmp/style.html
  tmp/test.sh
  tmp/test.txt
  ```

- `-perm`：根据文件权限查找。
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -perm 755
  tmp/
  tmp/books
  tmp/contacts
  tmp/FreeRTOSv10.4.1.zip
  tmp/linux-5.6.13.tar.xz
  tmp/test.sh
  tmp/test.txt
  ```

#### 1.2 对搜索结果进行处理

语法：
```linux
find [目录] [选项] -exec [命令] \;
```

- 删除搜索结果。
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -name "test.txt"
  tmp/test.txt
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -name "test.txt" -exec rm {} \;
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -name "test.txt"
  jincheng@LAPTOP-VHI28MJ8:~$
  ```

- 复制搜索结果。
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -iname "hello.txt"
  tmp/HELLO.TXT
  tmp/hello.txt
  jincheng@LAPTOP-VHI28MJ8:~$ find tmp/ -iname "hello.txt" -exec cp {} tmp/books/ \;
  jincheng@LAPTOP-VHI28MJ8:~$ ls tmp/books/
  HELLO.TXT  hello.txt
  ```


### 2. `grep`

grep（global search regular expression and print out the line）是一种强大的文本搜索工具。它使用正则表达式搜索文本，并把匹配的行打印出来。

#### 2.1 基本语法

命令：
```linux
grep [选项] [模式] [文件名]
```

选项：
- `-n`：显示行号
- `-c`：显示匹配的行数
- `-o`：输出匹配部分
- `-v`：反向查找并输出
- `-i`：忽略大小写
- `-w`：只显示全字匹配的结果
- `-m [特定值]`：限制匹配的结果数量为特定值
- `-l`：列出文件内容匹配的文件名
- `-r`：对目录递归地搜索
- `-A [行数]`：显示匹配的行及其之后的若干行
- `-B [行数]`：显示匹配的行及其之前的若干行
- `-C [行数]`：显示匹配的行及其前后的若干行
- `-E`：使用扩展的正则表达式

模式：常规的正则表达式，加上下面介绍的特殊规则表达式。

#### 2.2 特殊规则表达式

除正则表达式外，grep 命令还支持下列规则：
- `[[:alpha:]]`：字母
- `[[:digit:]]`：数字
- `[[:alnum:]]`：字母和数字
- `[[:upper:]]`：大写字母
- `[[:lower:]]`：小写字母
- `[[:punct:]]`：标点符号
- `[[:space:]]`：空白字符

#### 2.3 实例

```linux
# 文件内容

jincheng@LAPTOP-VHI28MJ8:~$ cat file
Talk is cheap, show me the code.
Practise makes perfect.
Where there is a will, there is a way.
Good good study, day day up.
You say hi hi hi, Mr.Black.

# 搜索以 w 开头的单词

jincheng@LAPTOP-VHI28MJ8:~$ grep "\<w" file
Where there is a will, there is a way.

# 搜索包含 a 的行数

jincheng@LAPTOP-VHI28MJ8:~$ grep -c "a" file
5

# 从标准输出搜索以 . 结尾的单词

jincheng@LAPTOP-VHI28MJ8:~$ echo I love coding. | grep -o -E "[a-z]+\."
coding.
```

### 3. `sed`

`sed` 是利用脚本来处理文本文件。

#### 3.1 基本语法

工作模式：
- `标准输出 | sed [选项] "模式 命令"`
- `sed [选项] '模式 命令' 文件名`

选项：
- `-n`：只输出模式匹配的行
- `-e`：默认选项，直接在命令行进行脚本编辑
- `-f`：指定脚本文件
- `-r`：支持扩展正则表达式
- `-i`：直接修改文件内容

模式：
- `特定行号`：特定行
- `起始行，结束行`：范围内的行
- `特定行号, +数量`：特定行及其后一定数量的行
- `模式串`：匹配模式串的行
- `模式串1, 模式串2`：从匹配模式串1的行开始，到匹配模式串2的行结束
- `特定行号, 模式串`：从特定行开始，到匹配模式串的行结束
- `模式串, 特定行号`：从匹配模式串的行开始，到特定行结束

其中，模式串的格式为：`分界符 内容 分界符` 。分界符可以是任意一堆相同的字符，通常使用 `/` 。

命令：
- 增：
  - `a`：在当前行的下一行添加内容
  - `i`：在当前行的上一行添加内容
  - `r`：在当前行的下一行添加指定文件中的内容
  - `w`：将匹配的行写入到外部文件中
- 删：
  - `d`：删除当前行
- 改：
  - `s/old/new/`：将行内第一个 old 替换为 new
  - `s/old/new/n`：将行内第 n 个 old 替换为 new
  - `s/old/new/g`：将行内所有的 old 替换为 new
  - `s/old/new/ig`：将行内所有的 old 替换为 new ，匹配 old 时忽略大小写
- 查：
  - `p`：打印当前行
  - `=`：打印行号

#### 3.2 引用

- `&`：引用模式匹配的整个串
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ cat file
  Talk is cheap, show me the code.
  Practise makes perfect.
  Where there is a will, there is a way.
  Good good study, day day up.
  You say hi hi hi, Mr.Black.

  # 将所有以 e 结尾的单词首尾加 *
  
  jincheng@LAPTOP-VHI28MJ8:~$ sed 's/\b\w*e\b/*&*/g' file
  Talk is cheap, show *me* *the* *code*.
  *Practise* makes perfect.
  *Where* *there* is a will, *there* is a way.
  Good good study, day day up.
  You say hi hi hi, Mr.Black.
  ```

- `\1`：引用模式匹配第一个左括号 `(` 对应的右括号 `)` 内的部分串
  ```linux
  jincheng@LAPTOP-VHI28MJ8:~$ cat file
  Talk is cheap, show me the code.
  Practise makes perfect.
  Where there is a will, there is a way.
  Good good study, day day up.
  You say hi hi hi, Mr.Black.

  # 将所有以 s 开头的单词首字母大写

  jincheng@LAPTOP-VHI28MJ8:~$ sed 's/\bs\(\w*\)/S\1/g' file
  Talk is cheap, Show me the code.
  Practise makes perfect.
  Where there is a will, there is a way.
  Good good Study, day day up.
  You Say hi hi hi, Mr.Black.
  jincheng@LAPTOP-VHI28MJ8:~$
  ```

#### 3.3 实例

```linux
# 文件内容

jincheng@LAPTOP-VHI28MJ8:~$ cat file
Talk is cheap, show me the code.
Practise makes perfect.
Where there is a will, there is a way.
Good good study, day day up.
You say hi hi hi, Mr.Black.

# 将 will 改写成 WILL

jincheng@LAPTOP-VHI28MJ8:~$ sed "s/will/WILL/g" file
Talk is cheap, show me the code.
Practise makes perfect.
Where there is a WILL, there is a way.
Good good study, day day up.
You say hi hi hi, Mr.Black.

# 删除以 W 开头的行

jincheng@LAPTOP-VHI28MJ8:~$ sed "/^W/d" file
Talk is cheap, show me the code.
Practise makes perfect.
Good good study, day day up.
You say hi hi hi, Mr.Black.

# 在第 5 行后面加上一行

jincheng@LAPTOP-VHI28MJ8:~$ sed  "5a new line" file
Talk is cheap, show me the code.
Practise makes perfect.
Where there is a will, there is a way.
Good good study, day day up.
You say hi hi hi, Mr.Black.
new line

# 将以 s 开头的单词首字母大写

jincheng@LAPTOP-VHI28MJ8:~$ sed "s/\bs\(\w*\)/S\1/g" file
Talk is cheap, Show me the code.
Practise makes perfect.
Where there is a will, there is a way.
Good good Study, day day up.
You Say hi hi hi, Mr.Black.
new line

# 将第 3-5 行写入新文件中

jincheng@LAPTOP-VHI28MJ8:~$ sed -n "3,5w search.res" file
jincheng@LAPTOP-VHI28MJ8:~$ cat search.res
Where there is a will, there is a way.
Good good study, day day up.
You say hi hi hi, Mr.Black.
```



### 4. `awk`

awk 是一种处理文本文件的语言，是一个强大的文本分析工具。