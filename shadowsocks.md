# 科学上网：ubuntu16服务器上搭建shadowSocks服务
## 前言
<p>科学上网应该时必备技能，但是一些免费的方案如goagent等实在是不太稳定。购买自己的服务器，然后自己搭建梯子不失为一种较好的方案。在ubuntu服务器上搭建使用shadowsocks搭建自己的科学上网工具<br>shadowsocks由于政策的原因，在百度是搜索不到的。<p>
## shasowsocks服务器安装
更新软件源

```
sudo apt-get update
```
安装pip环境

```
sudo apt-get install python-pip
```
直接安装shadowsocks服务器

```
sudo pip install shadowsocks
```
运行shadowsocks服务器

```
sudo ssserver -p 8388 -k password -m rc4-md5 -d start
```
也可以使用配置文件进行配置。方法：创建/etc/shadowsocks.json文件，填入如下内容

```json
{
	"server":"0.0.0.0",
	"server_port":443,
	"local_address":"127.0.0.1",
	"local_port":1080,
	"password":"mypassword",
	"timeout":300,
	"method":"rc4-md5"
}
```
各字段含义如下

```
| 字段 |  含义 |
| :--- |  ----: |
| server | 服务器ip(IPV4/IPV6),正也将是服务端监听的ip地址 |
| server_port    | 服务器端口     |
| local_port    | 本地端端口     |
| password    | 用来加密的密码     |
| timeout    | 超时时间     |
| method    | 加密方法,可选择"bf-cfb","aes-256-cfb","des-cfb","rc4"等等     |

```
<b>Tips: </b> 加密方式推荐使用rc4-md5，因为rc4比aes速度快好几倍，如果用在路由器上会带来显著的性能提升。旧的rc4加密之所以不安全是因为shadowsocks在每个连接上重复使用key，没有使用iv。现在已经重新正确使用，可以放心使用，更多可以看issue<br>

<b>Tips:</b> 如果需要配置多个用户，可以这样设置

```json
{
    "server":"my_server_ip",
    "port_password": {
        "端口1": "密码1",
        "端口2": "密码2"
    },
    "timeout":300,
    "method":"rc4-md5",
    "fast_open": false
}
```
创建完毕后，赋予文件权限:

```
sudo chmod 777 /etc/shadowsocks.json
```
为了支持这些加密方式，你需要安装

```
sudo apt-get install python–m2crypto
```
然后使用配置文件在后台运行

```
sudo ssserver -c /etc/shadowsocks.json -d start
```
## 配置开机启动
编辑/etc/rc.local文件

```
sudo vi /etc/rc.local
```
在exit o 和一行的上边加入如下代码端

```
/usr/local/bin/ssserver -c /etc/shadowsocks.json
```
或者不用配置文件直接加入命令启动如下：

```
/usr/local/bin/ssserver -p 8388 -k password -m aes-256-cfb -d start
```
到此重启服务器后，会自动启动
## 安装和配置shadowsocks客户端
最新版本的的shadowsocks客户端可以从github上下载[github下载](https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)<br>
客户端配置及使用方法![alt](https://github.com/macfu/document/blob/master/shadowsocks.png)
