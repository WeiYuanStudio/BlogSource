---
title: 在Manjaro上使用Pacman安装docker时遇到的问题
date: 2019-08-17 16:32:33
thumbnail:
categories:
 - 踩坑
tags:
 - Linux
 - pacman
 - docker
 - python lib
---
今天想在自己的笔记本上安装一个docker环境用于练习，结果发生了错误。


在印象中，我之前在这台笔记本安装过docker，也有可能只是在另一台Chrome Book 安装过而已，我已经记不清了。


但是今天必须要把这个安装问题解决掉才行

<!--more-->

# 问题描述

我试图安装docker的时候发生了错误

```
[adam@mibookpro ~]$ sudo pacman -S docker docker-compose
resolving dependencies...
looking for conflicting packages...

Packages (24) bridge-utils-1.6-3  containerd-1.2.7-1  python-asn1crypto-0.24.0-2  python-bcrypt-3.1.7-1  python-cached-property-1.5.1-2  python-cffi-1.12.3-1  python-cryptography-2.7-1
              python-docker-4.0.2-1  python-docker-pycreds-0.4.0-1  python-dockerpty-0.4.1-4  python-docopt-0.6.2-5  python-jsonschema-3.0.2-1  python-paramiko-2.6.0-1  python-ply-3.11-2
              python-pyasn1-0.4.6-1  python-pycparser-2.19-1  python-pynacl-1.3.0-1  python-pyrsistent-0.15.4-1  python-texttable-1.6.2-1  python-websocket-client-0.56.0-1  python-yaml-5.1.1-1
              runc-1.0.0rc8-1  docker-1:19.03.1-1  docker-compose-1.24.1-1

Total Download Size:    64.68 MiB
Total Installed Size:  318.34 MiB

:: Proceed with installation? [Y/n] Y
:: Retrieving packages...
 bridge-utils-1.6-3-x86_64                                                                          15.5 KiB  0.00B/s 00:00 [###########################################################################] 100%
 python-ply-3.11-2-any                                                                              73.5 KiB  1565K/s 00:00 [###########################################################################] 100%
 python-pycparser-2.19-1-any                                                                       163.9 KiB  3.20M/s 00:00 [###########################################################################] 100%
 python-cffi-1.12.3-1-x86_64                                                                       207.3 KiB  11.9M/s 00:00 [###########################################################################] 100%
 python-asn1crypto-0.24.0-2-any                                                                    159.6 KiB  12.0M/s 00:00 [###########################################################################] 100%
 python-cryptography-2.7-1-x86_64                                                                  334.0 KiB  1358K/s 00:00 [###########################################################################] 100%
 python-pyasn1-0.4.6-1-any                                                                         105.0 KiB  14.6M/s 00:00 [###########################################################################] 100%
 runc-1.0.0rc8-1-x86_64                                                                              2.2 MiB  7.16M/s 00:00 [###########################################################################] 100%
 containerd-1.2.7-1-x86_64                                                                          22.8 MiB  6.44M/s 00:04 [###########################################################################] 100%
 docker-1:19.03.1-1-x86_64                                                                          37.5 MiB  5.61M/s 00:07 [###########################################################################] 100%
 python-cached-property-1.5.1-2-any                                                                 10.3 KiB  0.00B/s 00:00 [###########################################################################] 100%
 python-docopt-0.6.2-5-any                                                                          21.5 KiB  1653K/s 00:00 [###########################################################################] 100%
 python-yaml-5.1.1-1-x86_64                                                                        173.5 KiB  7.37M/s 00:00 [###########################################################################] 100%
 python-texttable-1.6.2-1-any                                                                       16.1 KiB  0.00B/s 00:00 [###########################################################################] 100%
 python-websocket-client-0.56.0-1-any                                                               57.0 KiB  5.56M/s 00:00 [###########################################################################] 100%
 python-docker-pycreds-0.4.0-1-any                                                                   8.9 KiB  0.00B/s 00:00 [###########################################################################] 100%
 python-bcrypt-3.1.7-1-x86_64                                                                       30.0 KiB  4.18M/s 00:00 [###########################################################################] 100%
 python-pynacl-1.3.0-1-x86_64                                                                       76.9 KiB  7.51M/s 00:00 [###########################################################################] 100%
 python-paramiko-2.6.0-1-any                                                                       246.2 KiB   459K/s 00:01 [###########################################################################] 100%
 python-docker-4.0.2-1-any                                                                         171.4 KiB  10.5M/s 00:00 [###########################################################################] 100%
 python-dockerpty-0.4.1-4-any                                                                       19.0 KiB  0.00B/s 00:00 [###########################################################################] 100%
 python-pyrsistent-0.15.4-1-x86_64                                                                  86.0 KiB  14.0M/s 00:00 [###########################################################################] 100%
 python-jsonschema-3.0.2-1-any                                                                      90.4 KiB  8.83M/s 00:00 [###########################################################################] 100%
 docker-compose-1.24.1-1-any                                                                       191.2 KiB  9.34M/s 00:00 [###########################################################################] 100%
(24/24) checking keys in keyring                                                                                            [###########################################################################] 100%
(24/24) checking package integrity                                                                                          [###########################################################################] 100%
(24/24) loading package files                                                                                               [###########################################################################] 100%
(24/24) checking for file conflicts                                                                                         [###########################################################################] 100%
error: failed to commit transaction (conflicting files)
python-yaml: /usr/lib/python3.7/site-packages/_yaml.cpython-37m-x86_64-linux-gnu.so exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__init__.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/__init__.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/composer.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/constructor.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/cyaml.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/dumper.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/emitter.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/error.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/events.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/loader.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/nodes.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/parser.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/reader.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/representer.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/resolver.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/scanner.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/serializer.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/__pycache__/tokens.cpython-37.pyc exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/composer.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/constructor.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/cyaml.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/dumper.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/emitter.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/error.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/events.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/loader.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/nodes.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/parser.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/reader.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/representer.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/resolver.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/scanner.py exists in filesystem
python-yaml: /usr/lib/python3.7/site-packages/yaml/serializer.py exists in filesystem
python-yaml: pacman -Qo 文件的完整路径tokens.py exists in filesystem
Errors occurred, no packages were upgraded.
```

这种包管理器炸毛了，真的是一点头绪都没有，每次看见滚命令行，但是事实上对包管理器安装软件包时，背后发生了什么一无所知。这个报错看起来像是某些文件已经存在导致了某些问题？

# 初步分析

使用`pacman -Qs docker`命令发现并没有安装过docker的迹象。这可能的确是我记错了

搜索了网上相关的资料，并没有发现和我有类似问题的用户。难道是某次滚包的时候留下了历史遗留问题吗。

故查阅世界上最好的Linux文档 ———— Arch Wiki。找到了和[Pacman相关的词条](https://wiki.archlinux.org/index.php/Pacman)

移步至**升级时遇到问题: "file exists in filesystem"(conflicting files)!**

照葫芦画瓢的使用了`pacman -Qo 文件的完整路径`命令，用于查找该目录隶属于何软件包，我上面报错路径是`/usr/lib/python3.7/site-packages/yaml/`，使用该命令查找该路径的时候发现没有任何包是隶属于该目录，但是查找上级目录时候就能找到一堆包。

再一看使用命令安装docker的时候，需要安装的依赖包里面含有**python-yaml**这个包。而且本地包管理器里面并没有这个包，却又有应该属于这个包的目录`/usr/lib/python3.7/site-packages/yaml/`和文件**_yaml.cpython-37m-x86_64-linux-gnu.so**

初步怀疑是**python-yaml**这个lib早期是隶属于python的某个其他包。当初安装的时候就顺便被安装上了，在后期的升级中，该lib被分离出来作为一个单独的包。而包管理器升级的时候又没有将旧的文件移除，导致无法再次安装**python-yaml**这个包，进而导致包管理器无法安装需要依赖**python-yaml**的docker。

于是我先保守起见，移走相关的文件。将可能有关**python-yaml**安装有关的文件移走。接着再次执行安装docker，不出意外，安装成功了。

# 证实猜想

再来查看刚才移走的python lib yaml的目录，发现已经重新补上了。使用包管理器的`pacman -Qo /usr/lib/python3.7/site-packages/yaml/`

```
[adam@mibookpro yaml]$ sudo pacman -Qo /usr/lib/python3.7/site-packages/yaml/
[sudo] password for adam: 
/usr/lib/python3.7/site-packages/yaml/ is owned by python-yaml 5.1.1-1
```

现在看来，该目录已经隶属于**python-yaml**这个包了