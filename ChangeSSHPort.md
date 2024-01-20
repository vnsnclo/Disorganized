### SSH 通过 443 端口连接 GitHub

今天 `clone` 仓库的时候发现 ssh 失效了，一直克隆失败超时，网络没问题，看了下官方文档设置了一下发现可以正常 `clone`、`push` 了。

GitHub 提供两种协议供 git 连接，ssh 和 https。理论上可以随意选择两者之一连接到 GitHub 上的代码仓库，无论是 `clone` 到本地，还是 `push` 到远程。但是我的网络环境 22 端口总是连接超时，而 22 端口正是 GitHub 提供 ssh 访问的端口号。可以换用 https 协议，但是每次都要输入账号密码验证，很是繁琐，所以为了方便，理想解决方式就是让 git 的 ssh 协议改用 22 以外的其他端口连接 GitHub。

---

一般在 clone GitHub 上的代码仓库时，可以看到 GitHub 提供了两种不同的链接：

```
git clone https://github.com/xxxx/xxxx.git	# 这是https
git clone git@github.com:xxxx/xxxx.git	# 这是ssh
```

第一个https协议一般不会有问题（只要能打开 GitHub），而第二个依赖 ssh 的正常工作。因为我的网络环境 22 端口的连接总超时，还有可能会被阻断。所以测试 GitHub 的 ssh 连接时会出现以下报错：

```
❯ ssh -T git@github.com
Cloning into 'xxxx'... 
ssh: connect to host github.com port 22: Connection timed out 
fatal: Could not read from remote repository. 

Please make sure you have the correct access rights and the repository exists.
```

而正常应该输出：

```
❯ ssh -T git@github.com
Hi xxxx! You've successfully authenticated, but GitHub does not provide shell access.
```

看下官方文档 [Using SSH over the HTTPS port](https://docs.github.com/zh/authentication/troubleshooting-ssh/using-ssh-over-the-https-port) ，可以发现 GitHub 在另一个域名（ssh.github.com）上提供了一个 443 端口的 ssh 服务。防火墙一般不会阻拦 443 端口（只要能浏览 GitHub 就能连上），使用这个命令进行测试：

```
❯ ssh -T -p 443 git@ssh.github.com
```

可能会问你：

```
The authenticity of host '[ssh.github.com]:443 ([xxx.xxx.xxx.xxx]:443)' can't be established.
xxxxxxx key fingerprint is 
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

这个消息是 ssh 首次连接到一个新的主机时的提示，它询问你是否确认连接。输入 `yes` 即可。

为了让 git 通过 443 端口用 ssh 访问 GitHub，我们为上述 ssh 连接方式设置一个别名。首先找到 ssh 的配置文件，路径一般是 `C:\Users\你的用户名\.ssh\config`，如果这个文件不存在的话创建一个，注意，这个文件没有后缀名。然后在其中增加以下内容：

```
Host github.com
    Hostname ssh.github.com
    Port 443
```

其中 `Host` 是别名，`HostName` 是实际的域名地址，`Port` 是端口号。当我在用 ssh 连接 github.com 时，实际访问的是 ssh.github.com，所以 `Host` 和 `HostName` 分别设置成这两个域名。

如此，ssh.github.com 就成了 github.com 的顶替。当 git 通过 ssh 协议试图访问 github.com 的时候，ssh 会发现它是 ssh.github.com 的别名，因此会用 443 端口实际连接到后者。这样就绕开了网络环境对 22 端口的限制。