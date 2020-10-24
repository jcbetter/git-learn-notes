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


### 3 `gz`、`zip`、`rar`

**1. `gz`**

压缩：
```linux
gzip [被压缩文件名]
```

命令执行后会在被压缩文件名后缀上 `.gz` 。

解压：
```linux
gunzip -d [压缩文件名]
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
