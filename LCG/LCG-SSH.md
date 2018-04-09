# LCG - Linux Configuration Guide - SSH
# SSH配置指引

## 在客户端机器上生成Key  
* 生成Key  
```

    ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/demo/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /demo/.ssh/id_rsa.
    Your public key has been saved in /demo/.ssh/id_rsa.pub.
    The key fingerprint is:
    4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a
    The key's randomart image is:
    +--[ RSA 2048]----+
    |          .oo.   |
    |         .  o.E  |
    |        + .  o   |
    |     . = = .     |
    |      = S = .    |
    |     o + = +     |
    |      . o + o .  |
    |           . o   |
    |                 |
    +-----------------+
```

## 在远程服务器上设置Public Key-CentOS
* 将生成的Public Key复制到远程服务器
`scp ~/.ssh/id_rsa.pub root@xxx.xxx.xxx.xxx:/root/.ssh/`  

* 设置权限  
`chmod 600  ~/.ssh/id_rsa.pub`  

* 添加Public Key  
`cd ~/.ssh`
`cat id_rsa.pub >> authorized_keys`

* 如果在执行上述操作前，authorized_keys不存在，则需执行如下命令以确保权限正确  
`chmod 600 authorized_keys`  
`chown root:root authorized_keys`
