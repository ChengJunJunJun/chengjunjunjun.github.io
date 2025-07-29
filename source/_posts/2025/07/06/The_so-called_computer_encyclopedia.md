---
title: 所谓的计算机百科全书【主要是自己查字典】
date: 2025-07-06
last_modified: 2025-07-11
author: Cheng Jun
desc: 很多命令比较难记，需要不断的积累，这个教程主要是方便直接查询
tags: [进阶]
categories: [Technical sharing]
---

## 常见的类Unix命令

### mkdir

`-p` 参数的主要功能：
- **递归创建目录**：如果父目录不存在，会自动创建所有必需的父目录
- **忽略已存在的目录**：如果目录已经存在，不会报错

例如：
```bash
mkdir -p /home/user/documents/projects/myapp
```
这会创建整个目录路径，即使 `documents` 和 `projects` 目录原本不存在。

#### mkdir 命令的其他常用选项

**基本语法：**
```bash
mkdir [选项] 目录名
```

**常用参数：**

- `-m, --mode=模式`：设置目录权限
  ```bash
  mkdir -m 755 mydir
  ```

- `-v, --verbose`：显示详细信息
  ```bash
  mkdir -v newdir
  # 输出：mkdir: created directory 'newdir'
  ```

- `-p, --parents`：递归创建目录（前面已介绍）

- `--help`：显示帮助信息

- `--version`：显示版本信息

#### 实用示例

```bash
# 创建单个目录
mkdir myproject

# 创建多个目录
mkdir dir1 dir2 dir3

# 创建带权限的目录
mkdir -m 700 private_dir

# 递归创建并显示详细信息
mkdir -pv /tmp/test/deep/structure

# 创建多层目录结构
mkdir -p project/{src,docs,tests}/{main,utils}
```

最后一个例子会创建：
```
project/
├── src/
│   ├── main/
│   └── utils/
├── docs/
│   ├── main/
│   └── utils/
└── tests/
    ├── main/
    └── utils/
```

> 💡 `mkdir -p` 是日常使用中非常实用的命令，特别适合需要创建深层目录结构的场景。

---

### 系统管理命令

以下是常用的系统管理命令的详细解释和用法，适用于 Linux / macOS 等类 Unix 系统：

#### `df` — 查看磁盘空间使用情况

**描述**：显示文件系统磁盘空间的使用情况。

**常见用法：**

- `df`  
  显示所有挂载分区的磁盘使用情况（单位为 KB）
- `df -h`  
  **"human-readable"**：以 MB/GB 格式显示，更易读
- `df -T`  
  显示文件系统类型（如 ext4、xfs）

#### `du -sh` — 查看文件或目录大小

**描述**：显示文件或目录占用的磁盘空间。

**常见用法：**

- `du -sh /path/to/dir`  
  `-s`: 仅显示总和（不递归）  
  `-h`: 以 MB/GB 格式显示（人类可读）

**示例：**
```bash
du -sh .   # 查看当前目录占用的磁盘空间
```

#### `rsync` — 高效复制文件或目录（本地或远程）

**描述**：同步文件/目录，适用于备份。

**常见用法：**

- **本地同步：**
  ```bash
  rsync -avh source/ dest/
  ```
  - `-a`: 归档模式，保持权限、时间等
  - `-v`: 显示过程
  - `-h`: 人类可读格式（文件大小）

- **远程同步：**
  ```bash
  rsync -avz file.txt user@remote:/path/
  ```
  - `-z`: 启用压缩

#### `cat` — 查看文件内容、拼接文件

**描述**：打印文件内容到终端。

**常见用法：**

- `cat file.txt`  
  打印文件内容
- `cat file1 file2 > combined.txt`  
  拼接多个文件内容到一个新文件

#### `touch` — 创建新文件或修改时间戳

**描述**：创建空文件，或更新文件的访问/修改时间。

**常见用法：**

- `touch file.txt`  
  若不存在则创建，存在则更新其修改时间
- `touch -t 202507061200 file.txt`  
  设置为指定时间（年月日时分）

#### `su` — 切换用户

**描述**：以另一个用户身份运行 shell（默认是 root）。

**常见用法：**

- `su`  
  切换为 root 用户（需 root 密码）
- `su username`  
  切换到指定用户
- `su - username`  
  切换并加载该用户的环境（相当于登录该用户）

> ⚠️ 大部分现代系统推荐使用 `sudo` 替代 `su`。

#### `getent` — 查询系统数据库（如 passwd、group、hosts）

**描述**：统一接口查询系统账户、组、网络信息等。

**常见用法：**

- `getent passwd`  
  列出所有用户（类似查看 `/etc/passwd`）
- `getent group`  
  列出所有组
- `getent hosts google.com`  
  获取 DNS 解析结果

#### `ps` — 查看当前进程

**描述**：显示正在运行的进程。

**常见用法：**

- `ps`  
  显示当前 shell 下的进程
- `ps aux`  
  显示所有用户的所有进程（详细信息）：
  - `a`: 所有用户的进程
  - `u`: 显示用户名和更多信息
  - `x`: 包括没有控制终端的进程
- `ps -ef`  
  与 `ps aux` 类似的标准格式（System V 风格）

---

## 基础权限知识

### 什么是权限

`rwxr-xr-x` 是 Linux 和类 Unix 系统中用于描述文件或目录权限的一种符号表示形式，它包含 10 个字符，通常你在使用 `ls -l` 命令时会看到这样的输出，例如：

```bash
-rwxr-xr-x  1 user group 1234 Jul  6 10:00 somefile
```

#### 基本结构（共 10 个字符）

| 字符位置    | 含义               | 示例值                              |
|-----------|-------------------|-----------------------------------|
| 第 1 位    | 文件类型           | `-` 表示普通文件；`d` 表示目录；`l` 表示链接等 |
| 2~4 位    | 所有者（user）权限  | `rwx` 表示拥有读、写、执行权限           |
| 5~7 位    | 用户组（group）权限 | `r-x` 表示拥有读、执行权限              |
| 8~10 位   | 其他人（others）权限| `r-x` 表示拥有读、执行权限              |

#### 权限字符含义

每组三位权限字符分别表示：

| 字符 | 含义                                          |
|-----|---------------------------------------------|
| `r` | **read（读）权限**：可以读取文件内容或列出目录内容      |
| `w` | **write（写）权限**：可以修改文件内容或目录内容（包括创建、删除文件）|
| `x` | **execute（执行）权限**：可以执行文件（如脚本、程序）或进入目录 |
| `-` | **无此权限**                                   |

---

### 什么样的权限是安全的？

在 Linux 权限设置中，**最小权限原则**是安全的核心。以下是不同场景下的安全权限设置建议：

> **最小权限原则**：只给用户完成工作所需的最小权限，避免不必要的风险。

#### 1. 普通文件

```bash
# 个人文件：只有所有者可读写
chmod 600 private_file.txt      # rw-------

# 共享文件：所有者读写，组可读
chmod 640 shared_file.txt       # rw-r-----

# 公共只读文件
chmod 644 public_file.txt       # rw-r--r--
```

#### 2. 目录权限

```bash
# 个人目录：只有所有者访问
chmod 700 private_dir/          # rwx------

# 共享目录：所有者全权限，组可读执行
chmod 750 shared_dir/           # rwxr-x---

# 公共目录：所有人可读执行
chmod 755 public_dir/           # rwxr-xr-x
```

#### 3. 可执行文件

```bash
# 个人脚本
chmod 700 my_script.sh          # rwx------

# 系统工具
chmod 755 system_tool          # rwxr-xr-x
```

#### ⚠️ 危险的权限设置

避免使用这些权限：

```bash
# 非常危险 - 所有人都有完全权限
chmod 777 file                 # rwxrwxrwx

# 危险 - 所有人可写
chmod 666 file                 # rw-rw-rw-

# 危险 - 组和其他用户可写目录
chmod 757 directory            # rwxr-xrwx
```

#### ✅ 安全的权限设置原则

- 给文件 `644` 权限，目录 `755` 权限（一般情况）
- 敏感文件 `600` 权限，敏感目录 `700` 权限
- 避免使用 `777` 权限
- 定期审核和清理权限
- 使用专用用户而非 root
- 遵循最小权限原则

---

### chmod 和 chown 的用法

#### 命令说明

**递归更改 jyn 目录下所有文件和子目录的所有者**

```bash
chown -R jyn:jyn /home/data3/jyn
```

**参数解释：**
- `chown` = 更改所有者 (change owner)
- `-R` = 递归处理所有子目录和文件
- `jyn:jyn` = 用户名:组名 (将所有者和组都设为 jyn)

**再设置权限（如果需要）**

```bash
chmod -R 755 /home/data3/jyn
```

### 用rar解压文件
由于Mac系统没有自带rar解压软件，所以需要使用第三方软件。

要解压HUST-Master-Thesis.rar文件并保留完整路径结构，您可以使用`x`命令（Extract files with full path）：

```bash
rar x HUST-Master-Thesis.rar
```

这个命令会将RAR文件中的所有内容解压到当前目录下，并且保留文件的原始路径结构。

如果您想将文件解压到特定的文件夹中（例如创建一个同名的文件夹），您可以使用`-op`选项（Set the output path for extracted files）：

```bash
rar x HUST-Master-Thesis.rar -op"HUST-Master-Thesis"
```

这个命令会将RAR文件解压到名为"HUST-Master-Thesis"的文件夹中。

还有一种方法是先创建目标文件夹，然后在解压时指定路径：

```bash
mkdir HUST-Master-Thesis
rar x HUST-Master-Thesis.rar HUST-Master-Thesis/
```