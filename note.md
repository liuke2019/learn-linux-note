# Linux 基础入门

课程地址：[实验楼](https://www.shiyanlou.com/courses/1)

## Linux 系统简介

---

## 基本操作及概念

**终端：** 在图形界面下，linux提供了终端模拟器（Terminal），本质上对应着linux的 /dev/tty 设备，多用户登录就是通过不同的 /dev/tty 设备完成。

创建一个名为 file 的文件：

```shell
$ touch file
```

进入一个名为 file 的目录：

```shell
$ cd file
```

查看当前所在目录：

```shell
$ pwd
```

### 快捷键

<kbd>tab</kbd> ：自动补全

<kbd>ctrl</kbd> + <kbd>c</kbd> ：强制终止

---

## 用户及文件权限管理

查看当前伪终端的用户：

```shell
$ who am i
```

查看当前登录的用户：

```shell
$ whoami
```
---

创建用户需要 root 权限，要用到 sudo 命令，使用 sudo 有两个条件：

- 知道当前登录用户的密码
- 当前用户属于 sudo 用户组

新建名为 lilei 的用户：

```shell
$ sudo adduser lilei
```

设置 shiyanlou 的用户密码：

```shell
$ sudo passwd shiyanlou
```

切换登录用户到 lilei：

```shell
$ su -l lilei
```

退出当前用户：

```shell
$ exit
```

或者 <kbd>ctrl</kbd> + <kbd>d</kbd>

---

**用户组：** Linux每个用户都有一个用户组，共享一些资源和权限。

查看 shiyanlou 的用户组：

```shell
$ groups shiyanlou
```

新建用户时，如果不指定用户组，会默认创建一个同名的用户组。

查看 shiyanlou 的 sudo 权限：

```shell
$ cat /etc/sudoers.d/shiyanlou
```

查看所有的用户组（sort 按字典序排序）：

```shell
$ cat /etc/group | sort
```

查看 shiyanlou 用户组：

```shell
$ cat /etc/group | grep -E "shiyanlou"
```

/etc/group 文件格式：

`group_name:password:GID:user_list`

将 lilei 添加到 sudo 用户组：

```shell
$ su shiyanlou
$ groups lilei
$ sudo usermod -G sudo lilei
$ groups lilei
```

---

删除 lilei 用户：

```shell
$ sudo deluser lilei --remove-home
```

---

使用较长格式列出文件：

```shell
$ ls -l
```

Linux 里面一切皆文件, 一个目录同时具有读权限和执行权限才可以打开并查看内部文件，而一个目录要有写权限才允许在其中创建其它文件。

显示除了 .（当前目录）和 ..（上一级目录）之外的所有文件，包括隐藏文件（Linux 下以 . 开头的文件为隐藏文件）：

```shell
$ ls -A
```

可以同时使用两个参数：

```shell
$ ls -Al
```

查看某一个目录的完整属性：

```shell
$ ls -dl <目录名>
```

显示所有文件大小，并以普通人能看懂的方式呈现：

```shell
$ ls -AsSh
```

---

查看文件属性：

```shell
$ ll file
```

在 shiyanlou 用户下，将文件 iphone6 的所有者从 lilei 变为 shiyanlou：

```shell
$ cd /home/lilei
$ ls iphone6
$ sudo chown shiyanlou iphone6
```

---

每个文件有三组权限：拥有者、所属用户组、其他用户。每个权限对应一个3位二进制数：rwx。

修改权限（方法1）：

```shell
$ chmod 600 iphone6
```

修改权限（方法2）：

```shell
$ chmod go-rw iphone6
```

其中：

|参数|含义|参数|含义|
|---|---|---|---|
|u|user|+|增加权限|
|g|group|-|删除权限|
|o|others|

---

## Linux 目录结构及文件基本操作

FHS（Filesystem Hierarchy Standard，文件系统层次结构标准），多数 Linux 版本采用这种文件组织形式，FHS 定义了系统中每个区域的用途、所需要的最小构成的文件和目录同时还给出了例外处理与矛盾处理。

FHS 定义了两层规范：

- 第一层是 / 下面的各个目录应该要放什么文件数据，例如 /etc 应该放置设置文件，/bin 与 /sbin 则应该放置可执行文件等等。
- 第二层针对 /usr 及 /var 这两个目录的子目录来定义。例如 /var/log 放置系统日志文件，/usr/share 放置共享数据等等。

|目录|含义|
|---|---|
|.|当前目录|
|..|上级目录|
|~|当前用户的home目录|

- 绝对路径，以根 / 目录为起点的完整路径，以你所要到的目录为终点。
- 相对路径，以当前目录 . 为起点，以你所要到的目录为终点。

创建名为 mydir 的空目录：

```shell
$ mkdir mydir
```

同时创建一个多级目录：

```shell
$ mkdir -p father/son/grandson
```

复制一个文件到指定目录：

```shell
$ cp file father/son/grandson
```

复制目录（递归复制）：

```shell
$ cp -r father family
```

删除文件：

```shell
$ rm file
```

删除目录(递归删除)：

```shell
$ rm -r family
```

移动文件 file 到 Documents 目录：

```shell
$ mv file Documents
```

重命名文件 file1 到 file2：

```shell
$ mv file1 file2
```

查看文件内容：

```shell
$ cat file
```

显示行号：

```shell
$ cat -n file
```

分页查看：

```shell
$ more file
```

查看最后10行：

```shell
$ tail file
```

查看最后1行：

```shell
$ tail -n 1 file
```

查看文件类型：

```shell
$ file filename
```

---

## 环境变量与文件查找

创建名为 tmp 的变量：

```shell
$ declare tmp
```

给变量赋值：

```shell
$ tmp=shiyanlou
```

读取变量的值：

```shell
$ echo $tmp
```

---

在所有的 UNIX 和类 UNIX 系统中，每个进程都有其各自的环境变量设置，且默认情况下，当一个进程被创建时，除了创建过程中明确指定的话，它将继承其父进程的绝大部分环境设置。

通常我们会涉及到的变量类型有三种：

- 当前 Shell 进程私有用户自定义变量，只在当前 Shell 中有效。
- Shell 本身内建的变量。
- 从自定义变量导出的环境变量。

与上述三种环境变量相关的命令：

|目录|含义|
|---|---|
|set|显示当前 Shell 所有变量，包括其内建环境变量（与 Shell 外观等相关），用户自定义变量及导出的环境变量。|
|env|显示与当前用户相关的环境变量，还可以让命令在指定环境中运行。|
|export|显示从 Shell 中导出成环境变量的变量，也能通过它将自定义变量导出为环境变量。|

比较几个文件的内容:

```shell
$ vimdiff env.txt export.txt set.txt
```

在当前进程的子进程有效则为**环境变量**。通常设为大写。

按变量的生存周期来划分，Linux 变量可分为两类：

- 永久的：需要修改配置文件，变量永久生效。
- 临时的：使用 export 命令行声明即可，变量在关闭 shell 时失效。

/etc/bashrc 存放 shell 变量。/etc/profile 存放环境变量。注意每个用户目录下的这个 .profile 只对当前用户永久生效。

当我们在 Shell 中执行一个命令时，系统就会按照环境变量 PATH 中设定的路径按照顺序依次到目录中去查找，如果存在同名的命令，则执行先找到的那个。

创建一个 Shell 脚本文件：

```shell
$ touch hello_shell.sh
$ gedit hello_shell.sh
$ chmod 755 hello_shell.sh
$ ./hello_shell.sh
```

创建一个 C 语言程序：

```shell
$ gedit hello_world.c
$ gcc -o hello_world hello_world.c
$ ./hello_world
```

添加自定义路径（绝对路径）到 PATH 环境变量（只在当前 Shell 有效）：

```shell
$ PATH=$PATH:/home/shiyanlou/mybin
```

在每个用户的 home 目录中有一个 Shell 每次启动时会默认执行一个配置脚本，以初始化环境，包括添加一些用户自定义环境变量等等。zsh 的配置文件是 .zshrc，Bash 的配置文件为 .bashrc 。它们在 etc 下还都有一个或多个全局的配置文件，不过我们一般只修改用户目录下的配置文件。

直接添加内容到 .zshrc 中：

```shell
$ echo "PATH=$PATH:/home/shiyanlou/mybin" >> .zshrc
```

\>> 表示将标准输出以追加的方式重定向到一个文件中，\> 是以覆盖的方式重定向到一个文件中。

删除一个环境变量：

```shell
$ unset temp
```

让环境变量立即生效：

```shell
$ source .zshrc
```

---

简单快速的搜索：只能搜索二进制文件 -b，man 帮助文件 -m，源代码文件 -s.

```shell
$ whereis who
```

快而全的搜索：通过 /var/lib/mlocate/mlocate.db 数据库查找，不过这个数据库也不是实时更新的，系统会使用定时任务每天自动执行 updatedb 命令更新一次，所以有时候刚添加的文件，它可能会找不到，需要手动执行一次 updatedb 命令。

```shell
$ sudo apt-get update
$ sudo apt-get install locate
$ sudo updatedb
$ locate /etc/sh
```

查找 /usr/share/ 下所有 jpg 文件：

```shell
$ locate /usr/share/\*.jpg
```

小而精的搜索：which 本身是 Shell 内建的一个命令，我们通常使用 which 来确定是否安装了某个指定的软件，因为它只从 PATH 环境变量指定的路径中去搜索命令。

```shell
$ which man
```

精而细的搜索：find 应该是这几个命令中最强大的。基本命令格式:

`find [path] [option] [action]`

例如：

```shell
$ sudo find /etc/ -name interfaces
```

---
