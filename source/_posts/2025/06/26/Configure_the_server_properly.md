---
title: AI方向Linux（ubuntu）配置参考
date: 2025-06-26
last_modified: 2025-06-26
author: Cheng Jun
desc: 本教程适合AI方向国内服务器的一些配置，目前主要是方便于python的运行。（GPU服务器）
tags: [Linux, 进阶]
categories: Technical sharing
---
### 注意
- 请大家在看这个教程之前，先看一下[刘啸宇的教程](https://gist.github.com/lxysl/1e589e626658e30eb757323b8a0ae826)。本教程只是一个补充和拓展。
- 服务器的维护离不开大家的支持，我们都是在相互学习，相互帮助。本教程会不定时更新。


### 常见服务器维护命令

1. 查看系统所有用户和查看系统所有组

```bash
cat /etc/passwd
cat /etc/group
```

2. getent 用来 Get entries from administrative database.
3. 查看用户详细信息

```bash
id username
```

4. 在具有sudo权限的用户下运行这个脚本用来查看每一个用户的具体信息

```bash
printf "%-12s %-35s %-5s %-6s\n" "用户" "组" "UID" "权限"
printf "%-12s %-35s %-5s %-6s\n" "----" "----" "---" "----"
for user in $(awk -F: '$3 >= 1000 && $1 != "nobody" {print $1}' /etc/passwd); do
    uid=$(id -u $user)
    groups=$(id -Gn $user | tr ' ' ',')
    
    # 最简单的判断：只要sudo -l有实际输出就认为有权限
    sudo_output=$(sudo -l -U $user 2>/dev/null)
    if [ -n "$sudo_output" ] && [ "$sudo_output" != "User $user is not allowed to run sudo on $(hostname)." ]; then
        # 进一步检查输出内容
        if echo "$sudo_output" | grep -qE "(ALL|NOPASSWD|may run)"; then
            sudo_perm="有"
        else
            sudo_perm="无"
        fi
    else
        sudo_perm="无"
    fi
    
    printf "%-12s %-35s %-5s %-6s\n" "$user" "$groups" "$uid" "$sudo_perm"
done
```

5. 修改密码

```bash
# 修改当前用户密码
passwd
# 修改其他用户密码
sudo passwd username
```

6. 检查ssh服务和ssh服务状态

```bash
# 查看 SSH 服务状态
sudo systemctl status ssh
# 或者
sudo systemctl status sshd

# 查看 SSH 进程
ps aux | grep ssh
# 或者
pgrep -l ssh

# 查看端口 22 是否被监听
sudo netstat -tlnp | grep :22
# 或者使用 ss 命令
sudo ss -tlnp | grep :22
# 或者
sudo lsof -i :22

#重新加载 systemd 配置：
sudo systemctl daemon-reload
# 启动 SSH 服务
sudo systemctl start ssh
# 停止 SSH 服务
sudo systemctl stop ssh
# 重启 SSH 服务
sudo systemctl restart ssh
# 设置开机自启
sudo systemctl enable ssh
```

7. 常见apt命令

```bash
apt update
apt list --upgradable
apt upgrade -y
apt autoremove -y
apt autoclean
```

8. 添加用户

```bash
sudo adduser username
# 在某一个组把用户删除
sudo gpasswd -d _username _groupename
sudo deluser _username _groupename
# 删除主组
sudo groupdel _groupename
# 删除用户及其主目录
sudo userdel -r _username
```

9. 文件或者文件夹操作

```bash
# 移动文件或者文件夹
mv [选项] 源文件/文件夹 目标路径
      -i ：覆盖文件前提示确认（interactive）。
      -f ：强制覆盖，不提示（force）。
      -n ：不覆盖已有文件。
      -v ：显示过程（verbose）。

# 创建文件
touch file.txt

# 创建文件夹
mkdir mydir
mkdir -p parent/child/grandchild # 递归创建多级目录

# 删除文件
rm file.txt
rm file1.txt file2.txt # 删除多个文件

#删除文件夹
rmdir mydir # 删除空文件夹
rm -r mydir # 删除非空文件夹
      -r ：递归删除目录及其内容
      -f ：强制覆盖，不提示（force）。
```

### conda 的安装
1. 在使用服务器之前，建议大家学一点vim的常见用法，没有界面的服务器使用vim的频率会很高。我更推荐使用现在最新的nvim，晚上有大量的教程。这里推荐几个教程：[MIT missing semester](https://missing-semester-cn.github.io/2020/editors/)。如果习惯使用vs code等IDE，只需要知道几个简单的用法就足够了。
2. 我们组的服务器自带的有conda，你只需要把以下的代码复制到.bashrc中，然后source一下，就可以使用conda了。(如果不成功，可以继续阅读一下[刘啸宇的教程](https://gist.github.com/lxysl/1e589e626658e30eb757323b8a0ae826))
```bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/opt/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/opt/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/opt/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="/opt/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

3. 这样就可以使用pip安装包了，这里提供一个加载清华镜像源的一键命令，可以极大的加快安装包的速度。（注意，在安装pytorch的时候，一定要注意安装正确的版本，不然你会遇到很多问题）。
    - 一键命令
    ```bash
    mkdir -p ~/.pip && echo -e "[global]\nindex-url = https://pypi.tuna.tsinghua.edu.cn/simple\ntrusted-host = pypi.tuna.tsinghua.edu.cn" > ~/.pip/pip.conf
    ```
    此命令将创建或覆盖 ~/.pip/pip.conf，配置为：
    ```ini
    [global]
    index-url = https://pypi.tuna.tsinghua.edu.cn/simple
    trusted-host = pypi.tuna.tsinghua.edu.cn # 用于规避 SSL 警告
    ```
    该配置对你 当前用户下的所有 Conda 环境中运行的 pip 都会生效。

### wandb 的使用
1. wandb是用于记录和可视化训练过程的工具，但是国内不能访问wandb，需要使用代理。



