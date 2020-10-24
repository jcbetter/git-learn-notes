**常用 Linux 命令**
[toc]

### `apt`

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
  升级软件包，升级前先删除需要更新软件包

- `show [软件包名]`
  显示软件包具体信息（版本号，安装大小，依赖关系等等）。

- `autoremove`
  清理不再使用的依赖和库文件。

- `purge [软件包名]`
  移除软件包及配置文件。

- `search [关键字]`
  查找软件包命令。