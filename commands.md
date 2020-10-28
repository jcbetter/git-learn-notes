**常用 Linux 命令**
[toc]

### 1. `apt`

语法：
```linux
apt [选项] [命令]
```

选项：

- `-y`：安装过程的提示全部选择 yes 。
- `-q`：不显示安装过程。

命令：

- `install [软件包名]`
  安装指定软件包。

- `remove [软件包名]`
  删除软件包。

- `update`
  列出所有可更新的软件清单。

- `upgrade [软件包名]`
  升级指定软件包。

- `upgrade`
  升级所有软件包。

- `list --installed`
  列出所有已安装的软件包。

- `list --upgradeable`
  列出所有可升级的软件包。

- `list --all-versions`
  列出所有已安装的软件包版本信息。

- `full-upgrade`
  升级软件包，升级前先删除需要更新软件包。

- `show [软件包名]`
  显示软件包具体信息（版本号，安装大小，依赖关系等等）。

- `autoremove`
  清理不再使用的依赖和库文件。

- `purge [软件包名]`
  移除软件包及配置文件。

- `search [关键字]`
  查找软件包命令。


### 2. `tar`

在压缩比率上： `tar.bz2` > `tgz` > `tar`
在耗费时间上： `tar.bz2` > `tgz` > `tar`

因此，Linux 下对于占用空间与耗费时间的折衷多选用 `tgz` 格式，不仅压缩率较高，而且打包、解压的时间都较为快速，是较为理想的选择。

#### 2.1 `tar`

`tar` 只是对文件进行打包，即归档处理，不做压缩（压缩率极低）。解压时，也只是将归档文件释放出来。

**1. 归档**

命令：
```linux
tar -cvf [归档后文件名] [被归档文件名]
```

解释：
- `-c`：create ，创建一个归档文件
- `-v`：verbose ，显示归档文件的进程
- `-f`：file ，后借被处理的档案名

效果：
```linux
jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ tar -cvf test.tar FreeRTOS
FreeRTOS

# 比较两个文件的大小

jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ ls -al
total 129600
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:34 .
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:26 ..
-rwxr-xr-x 1 jincheng jincheng 65762958 Oct 24 09:26 FreeRTOS
-rw-r--r-- 1 jincheng jincheng 65771520 Oct 24 09:34 test.tar
```

**2. 释放**

命令：
```linux
tar -xvf [归档文件名] -C [释放路径]
```
`-C [释放路径]` 可以省略，默认释放到当前路径。

解释：
- `-x`：extract ，从归档文件中提取文件

效果：
```linux
jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ tar -xvf test.tar
FreeRTOS

# 比较两个文件的大小

jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ ls -al
total 128640
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:42 .
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:26 ..
-rwxr-xr-x 1 jincheng jincheng 65762958 Oct 24 09:26 FreeRTOS
-rw-r--r-- 1 jincheng jincheng 65771520 Oct 24 09:34 test.tar
```

#### 2.2 `tar.gz`

`tar.gz` 又写作 `tgz` ，是 Linux 下使用非常普遍的一种压缩方式。

**1. 压缩**

命令：
```linux
tar -zcvf [压缩后文件名] [被压缩文件名]
```

解释：
- `-z`：gzip ，通过 gzip 压缩的形式对文件进行归档。

效果：
```linux
jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ tar -zcvf test.tgz FreeRTOS
FreeRTOS

# 比较两个文件的大小

jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ ls -al
total 187840
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 10:08 .
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:26 ..
-rwxr-xr-x 1 jincheng jincheng 65762958 Oct 24 09:26 FreeRTOS
-rw-r--r-- 1 jincheng jincheng 65771520 Oct 24 09:34 test.tar
-rw-r--r-- 1 jincheng jincheng 60414403 Oct 24 10:08 test.tgz
```

**2. 解压**

命令：
```linux
tar -zxvf [归档文件名] -C [释放路径]
```
`-C [释放路径]` 可以省略，默认释放到当前路径。

效果：
```linux
jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ tar -zxvf test.tgz
FreeRTOS

# 比较两个文件的大小

jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ ls -al
total 187840
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 10:12 .
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:26 ..
-rwxr-xr-x 1 jincheng jincheng 65762958 Oct 24 09:26 FreeRTOS
-rw-r--r-- 1 jincheng jincheng 65771520 Oct 24 09:34 test.tar
-rw-r--r-- 1 jincheng jincheng 60414403 Oct 24 10:08 test.tgz
```

#### 2.3 `tar.bz2`

这是一种压缩率非常高的压缩方法，但是耗时也非常久。

**1. 压缩**

命令：
```linux
tar -jcvf [压缩后文件名] [被压缩文件名]
```

解释：
- `-j`：bzip2 ，通过 bzip2 压缩的形式对文件进行归档。

效果：
```linux
jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ tar -jcvf test.tar.bz2 FreeRTOS
FreeRTOS

# 比较两个文件的大小

jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ ls -al
total 246272
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 10:17 .
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:26 ..
-rwxr-xr-x 1 jincheng jincheng 65762958 Oct 24 09:26 FreeRTOS
-rw-r--r-- 1 jincheng jincheng 65771520 Oct 24 09:34 test.tar
-rw-r--r-- 1 jincheng jincheng 59772144 Oct 24 10:18 test.tar.bz2
-rw-r--r-- 1 jincheng jincheng 60414403 Oct 24 10:08 test.tgz
```

**2. 解压**

命令：
```linux
tar -jxvf [归档文件名] -C [释放路径]
```
`-C [释放路径]` 可以省略，默认释放到当前路径。

效果：
```linux
jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ tar -jxvf test.tgz
FreeRTOS

# 比较两个文件的大小

jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ ls -al
total 246272
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 10:20 .
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:26 ..
-rwxr-xr-x 1 jincheng jincheng 65762958 Oct 24 09:26 FreeRTOS
-rw-r--r-- 1 jincheng jincheng 65771520 Oct 24 09:34 test.tar
-rw-r--r-- 1 jincheng jincheng 59772144 Oct 24 10:18 test.tar.bz2
-rw-r--r-- 1 jincheng jincheng 60414403 Oct 24 10:08 test.tgz
```


### 3. `gz`、`zip`、`rar`

**1. `gz`**

压缩：
```linux
gzip [被压缩文件名]
```

命令执行后会在被压缩文件名后缀上 `.gz` 。

解压：
```linux
gunzip [压缩文件名]
```

命令执行后会去掉压缩文件名的后缀 `.gz` 。

**2. `zip`**

压缩：
```linux
zip [压缩后文件名] [被压缩文件名]
```

解压：
```linux
unzip [压缩文件名]
```

**3. `rar`**

压缩：
```linux
rar a [压缩后文件名] [被压缩文件名]
```

解压：
```linux
rar -x [压缩文件名]
```

**三种压缩方式比较**

```linux
jincheng@LAPTOP-VHI28MJ8:~/tmp/test$ ls -al
total 234624
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 10:54 .
drwxr-xr-x 1 jincheng jincheng     4096 Oct 24 09:26 ..
-rwxr-xr-x 1 jincheng jincheng 65762958 Oct 24 09:26 FreeRTOS
-rwxr-xr-x 1 jincheng jincheng 60414242 Oct 24 10:41 FreeRTOS.gz
-rw-r--r-- 1 jincheng jincheng 52293093 Oct 24 10:52 test.rar
-rw-r--r-- 1 jincheng jincheng 60414381 Oct 24 10:47 test.zip
```


### 4. `ls`


### 5. `head`

语法：
```linux
head [参数] [文件名]
```

参数：
- `-n [行数]`：等价于 `-[行数]` ，显示文件的前几行
- `-c [行数]`：显示文件的前几个字节

实例：
```linux
# 文件内容

jincheng@LAPTOP-VHI28MJ8:~$ cat file
Talk is cheap, show me the code.
Practise makes perfect.
Where there is a will, there is a way.
Good good study, day day up.
You say hi hi hi, Mr.Black.

# 显示文件前 2 行

jincheng@LAPTOP-VHI28MJ8:~$ head -n 2 file
Talk is cheap, show me the code.
Practise makes perfect.

# 显示文件前 2 行

jincheng@LAPTOP-VHI28MJ8:~$ head -2 file
Talk is cheap, show me the code.
Practise makes perfect.

# 显示文件前 10 个字节

jincheng@LAPTOP-VHI28MJ8:~$ head -c 10 file
Talk is ch
```


### 6. `tail`

语法：
```linux
tail [参数] [文件名]
```

参数：
- `+[行数]`：显示从特定行到文件尾部
- `-n [行数]`：等价于 `-[行数]` ，显示文件的最后几行
- `-c [行数]`：显示文件的最后几个字节

实例：
```linux
# 文件内容

jincheng@LAPTOP-VHI28MJ8:~$ cat file
Talk is cheap, show me the code.
Practise makes perfect.
Where there is a will, there is a way.
Good good study, day day up.
You say hi hi hi, Mr.Black.

# 显示文件第 2 行至末尾

jincheng@LAPTOP-VHI28MJ8:~$ tail +2 file
Practise makes perfect.
Where there is a will, there is a way.
Good good study, day day up.
You say hi hi hi, Mr.Black.

# 显示文件最后 2 行

jincheng@LAPTOP-VHI28MJ8:~$ tail -2 file
Good good study, day day up.
You say hi hi hi, Mr.Black.

# 显示文件最后 10 个字节

jincheng@LAPTOP-VHI28MJ8:~$ tail -c 10 file
Mr.Black.
```


### 7. `cat`

cat（concatenate）命令用于连接文件并打印到标准输出设备上。

语法：
```linux
cat [选项] [文件名]
```

选项：
- `-n`：从 1 对文件进行编号，包括空白行
- `-b`：从 1 对文件进行编号，空白行除外

实例：
```linux
# 文件内容

jincheng@LAPTOP-VHI28MJ8:~$ cat file
Talk is cheap, show me the code.

Practise makes perfect.

Where there is a will, there is a way.

Good good study, day day up.

You say hi hi hi, Mr.Black.

# 编号，包括空白行

jincheng@LAPTOP-VHI28MJ8:~$ cat -n file
     1  Talk is cheap, show me the code.
     2
     3  Practise makes perfect.
     4
     5  Where there is a will, there is a way.
     6
     7  Good good study, day day up.
     8
     9  You say hi hi hi, Mr.Black.

# 编号。空包行除外

jincheng@LAPTOP-VHI28MJ8:~$ cat -b file
     1  Talk is cheap, show me the code.

     2  Practise makes perfect.

     3  Where there is a will, there is a way.

     4  Good good study, day day up.

     5  You say hi hi hi, Mr.Black.

# 将文件备份到其他文件中

jincheng@LAPTOP-VHI28MJ8:~$ cat file > file.bak

# 查看备份文件

jincheng@LAPTOP-VHI28MJ8:~$ cat file.bak
Talk is cheap, show me the code.

Practise makes perfect.

Where there is a will, there is a way.

Good good study, day day up.

You say hi hi hi, Mr.Black.
```


### 8. `echo`

echo 是 shell 的指令，用于字符串的输出。

用法：
- `echo [字符串]`：显示一个字符串
- `echo -e [字符串]`：`-e` 选项开启转义，显示一个带有转义字符的字符串
- `echo "Everything is just good!" > hello.txt`：将结果定向至文件
- echo \`ls /tmp/\`：显示命令执行的结果


### 9. `ps`

ps（process status）命令用于显示当前进程的状态。

语法：
```linux
ps [选项]
```

选项：
- `-u [用户名]`：显示指定用户的进程信息
- `-e`：显示所有的进程
- `-f`：显示较为详细的信息
- `-au`：显示更详细的进程信息
- `-aux`：显示包含其他使用者的更为详细的进程信息

实例：
```linux
# 显示指定用户进程

jincheng@LAPTOP-VHI28MJ8:~$ ps -u jincheng
  PID TTY          TIME CMD
    9 tty1     00:00:00 bash
   53 tty1     00:00:00 tail
  252 tty1     00:00:00 ps

# 显示所有进程

jincheng@LAPTOP-VHI28MJ8:~$ ps -e
  PID TTY          TIME CMD
    1 ?        00:00:00 init
    8 tty1     00:00:00 init
    9 tty1     00:00:00 bash
   53 tty1     00:00:00 tail
  247 tty1     00:00:00 ps

# 显示较为详细的信息

jincheng@LAPTOP-VHI28MJ8:~$ ps -f
UID        PID  PPID  C STIME TTY          TIME CMD
jincheng     9     8  0 08:11 tty1     00:00:00 -bash
jincheng    53     9  0 08:14 tty1     00:00:00 tail - v-1 file
jincheng   248     9  0 09:12 tty1     00:00:00 ps -f

# 显示更为详细的信息

jincheng@LAPTOP-VHI28MJ8:~$ ps -au
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         8  0.0  0.0   8936   224 tty1     Ss   08:11   0:00 /init
jincheng     9  0.0  0.0  16916  3688 tty1     S    08:11   0:00 -bash
jincheng    53  0.0  0.0  13992   864 tty1     T    08:14   0:00 tail - v-1 file
jincheng   249  0.0  0.0  17384  1924 tty1     R    09:12   0:00 ps -au

# 显示所有进程较为的详细信息 【常用】

jincheng@LAPTOP-VHI28MJ8:~$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 08:11 ?        00:00:00 /init
root         8     1  0 08:11 tty1     00:00:00 /init
jincheng     9     8  0 08:11 tty1     00:00:00 -bash
jincheng    53     9  0 08:14 tty1     00:00:00 tail - v-1 file
jincheng   250     9  0 09:12 tty1     00:00:00 ps -ef

# 显示包含其他使用者更为详细的信息 【常用】

jincheng@LAPTOP-VHI28MJ8:~$ ps -aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0   8936   316 ?        Ssl  08:11   0:00 /init
root         8  0.0  0.0   8936   224 tty1     Ss   08:11   0:00 /init
jincheng     9  0.0  0.0  16916  3688 tty1     S    08:11   0:00 -bash
jincheng    53  0.0  0.0  13992   864 tty1     T    08:14   0:00 tail - v-1 file
jincheng   251  0.0  0.0  17648  2052 tty1     R    09:12   0:00 ps -aux
```


### 10 `kill`

kill 命令可将指定的信息送至程序，用于删除执行中的程序或工作。预设的信息为 SIGTERM(15) ，可将指定程序终止。若仍无法终止该程序，可使用 SIGKILL(9) 信息尝试强制删除程序。

语法：
```linux
kill -[信息编号] [PID]
```

信息编号：可使用 `kill -l` 列出所有信息编号及其名称。
- `1`：SIGHUP ，重新加载进程
- `9`：SIGKILL ，杀死一个进程
- `15`：SIGTERM ，正常停止一个进程

实例：
```linux
# 查看进程信息

jincheng@LAPTOP-VHI28MJ8:~$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 08:11 ?        00:00:00 /init
root         8     1  0 08:11 tty1     00:00:00 /init
jincheng     9     8  0 08:11 tty1     00:00:00 -bash
jincheng    53     9  0 08:14 tty1     00:00:00 tail - v-1 file
jincheng   262     9  0 09:33 tty1     00:00:00 ps -ef

# 杀死一个进程

jincheng@LAPTOP-VHI28MJ8:~$ kill 53
[1]+  Terminated              tail - v-1 file

# 查看进程信息

jincheng@LAPTOP-VHI28MJ8:~$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 08:11 ?        00:00:00 /init
root         8     1  0 08:11 tty1     00:00:00 /init
jincheng     9     8  0 08:11 tty1     00:00:00 -bash
jincheng   263     9  0 09:34 tty1     00:00:00 ps -ef
```


### 11. `chmod`

chmod（change mode）命令是控制用户对文件权限的命令。

语法：
```linux
chmod [选项] [权限模式] [文件名]
```

选项：
- `-v`：显示权限变更的详细信息
- `-R`：以递归的方式对目录下的所有文件进行相同的权限变更

权限模式：
- 符号模式：
  - `u` 文件所有者、`g` 群组用户、`o` 其他用户、`a` 所有用户
  - `+` 增加权限、`-` 取消权限、`=` 设定权限
  - `r` 读、`w` 写、`x` 执行
- 八进制模式：
  - `r` 4、`w` 2、`x` 1

实例：
```linux
# 列出文件详细信息

jincheng@LAPTOP-VHI28MJ8:~/test$ ls -l
total 0
-rw-r--r-- 1 jincheng jincheng 157 Oct 26 09:50 file

# 为文件所有者加上执行权限

jincheng@LAPTOP-VHI28MJ8:~/test$ chmod u+x file
jincheng@LAPTOP-VHI28MJ8:~/test$ ls -l
total 0
-rwxr--r-- 1 jincheng jincheng 157 Oct 26 09:50 file

# 修改文件权限为指定值

jincheng@LAPTOP-VHI28MJ8:~/test$ chmod -v 644 file
mode of 'file' changed from 0744 (rwxr--r--) to 0644 (rw-r--r--)
```


### 12. `chown`

chown 命令用于改变文件的所有者，需要 root 权限才能执行。

语法：
```linux
chmod [选项] [用户:群组] [文件名]
```

选项：
- `-v`：显示详细的处理信息
- `-c`：显示更改部分的信息
- `-R`：以递归的方式对目录下的所有文件进行相同处理

实例：
```linux
# 列出文件信息
jincheng@LAPTOP-VHI28MJ8:~/test$ ls -l
total 0
-rw-r--r-- 1 jincheng jincheng 157 Oct 26 09:50 file

# 改变文件所有者

jincheng@LAPTOP-VHI28MJ8:~/test$ sudo chown root file
jincheng@LAPTOP-VHI28MJ8:~/test$ ls -l
total 0
-rw-r--r-- 1 root jincheng 157 Oct 26 09:50 file

# 改变文件群组

jincheng@LAPTOP-VHI28MJ8:~/test$ sudo chown :root file
jincheng@LAPTOP-VHI28MJ8:~/test$ ls -l
total 0
-rw-r--r-- 1 root root 157 Oct 26 09:50 file

# 改变文件所有者和群组

jincheng@LAPTOP-VHI28MJ8:~/test$ sudo chown jincheng:jincheng file
jincheng@LAPTOP-VHI28MJ8:~/test$ ls -l
total 0
-rw-r--r-- 1 jincheng jincheng 157 Oct 26 09:50 file
```

