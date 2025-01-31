# CTF 动态靶机模板

对于各个模板的详细适用情况，请见各情景的文件夹内的README

本仓库内的Docker容器模板均支持
- `$GZCTF_FLAG`
- `$DASCTF`
- `$FLAG`

三种动态flag部署方式，支持GZCTF、CTFd、安恒DASCTF等支持Docker动态部署题目靶机的平台

**有问题请开issue，好用请点star，有问题的话欢迎通过 [CTF-Archives售后快速服务群](http://qm.qq.com/cgi-bin/qm/qr?_wv=1027&k=KFamhBpmURTZpndhc0MI7_1l3a6Xezrf&authKey=Yenwm7%2B%2F%2FT%2BtSXCSyr%2B7fYS47Ot0MwFqesH4HOLT8ZADE2e9XO6AS96HQvjxh%2B%2BG&noverify=0&group_code=894957229) 联系维护人员寻求帮助**

## 请注意，此仓库内的模板仅在Linux环境（linux/amd64）下进行测试并保证可用性，如果为windows（windows/amd64）或者macos（linux/arm）等其他架构，不保证可用性😔

## 关于每个模板内的文件内容

这里以 `crypto-python_3.8-no_socket` 模板为例

```plaintext
.
├── docker
│   └── docker-compose.yml
├── Dockerfile
├── README.md
├── service
│   └── docker-entrypoint.sh
└── src
    └── main.py
```
`docker` 文件夹内存放与docker有关的文件，如 `docker-compose.yml` 文件，内部已经设置好了端口转发和测试用flag，便于测试容器环境

`Dockerfile` 为docker容器编译文件，用于设计docker容器，可在其中设置换源、增添软件包等等

`service` 文件夹内存放着与服务有关的文件，如 `docker-entrypoint.sh` 用于定义容器的入口点

`src` 文件夹内存放着题目的项目源码，也可以是pwn题目的二进制文件，即为题目的相关文件

`config` 文件夹内存放着容器内服务相关的配置文件，如 `nginx` 的配置文件等等

### 其他文件说明

`flag.php` 用于直接查看flag文件的测试文件，访问可直接查看当前题目根目录下的flag文件，如果文件不存在则会输出error

`shell.php` 一句话木马，用于测试web容器稳定性

## About no_socket with crypto

`no_socket`指的是源代码中没有引入`socket`相关的库，当希望达到的效果是类似于当用户通过特定端口连接到靶机时，就运行python代码，并将代码的运行界面转发给用户。如果已经引入了`socket`相关的库，请直接使用如`python app.py`这类语句启动python程序，并让程序自行监听特定端口

## 关于软件源换源

环境中涉及软件包处理的情形，如apt、yum，均已换源为ustc源，如不处于中国大陆网络环境/启用了全局代理环境，请自行修改相关换源语句，避免由于还原带来的负优化

## 关于容器无限重启，看日志发现sh文件错误

常见以下报错：
```shell
/docker-entrypoint.sh: line 2: $'\r': command not found
/docker-entrypoint.sh: line 26: syntax error: unexpected end of file
```
这是因为Windows与Linux文件编码在换行的操作不一样，导致Linux的shell无法解析脚本文件。解决方案如下
```shell
sed -i ""s/\r//"" docker-entrypoint.sh
```
即通过正则匹配，直接替换掉 `\r` 字符，不过此方案不一定能完全解决问题

建议直接在linux下执行 `git clone` 操作，或者直接从github下载zip版本的源码，避免一些奇奇怪怪的编码问题

请注意，`sed`指令在`unix（macos）`下的预期执行效果与`linux`下的预期执行效果不同

## A little advertisement

某 [Randark-JMT](https://github.com/Randark-JMT) 可以无偿为CTF平台搭建、题目打包提供一定帮助，欢迎联系😘

## 参考与鸣谢

[https://github.com/CTFTraining](https://github.com/CTFTraining)

感谢**glzjin-赵总**和**mozhu1024-陌竹**师傅们的项目，根据上述仓库，此项目才有了雏形，感谢他们为CTF事业做出的巨大贡献

[qsnctf / qsnctf_base_docker_images 青少年CTF基础Docker镜像](https://github.com/qsnctf/qsnctf_base_docker_images)

感谢**末心**师傅对相关模板作出的建议与努力
