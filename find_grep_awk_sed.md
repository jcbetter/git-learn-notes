**linux 四剑客**
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