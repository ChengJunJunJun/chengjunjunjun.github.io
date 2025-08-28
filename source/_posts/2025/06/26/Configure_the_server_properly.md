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