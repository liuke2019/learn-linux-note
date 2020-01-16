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

| 参数 | 含义 | 参数 | 含义 |
| --- | --- | --- | --- |
|u|user|+|增加权限|
|g|group|-|删除权限|
|o|others|

---

