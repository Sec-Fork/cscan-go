# Cscan-Go 介绍

## 免责声明：
本网络安全工具仅用于提供技术支持，不涉及任何应用或商业行为。用户在使用本网络安全工具的过程中，不得以任何方式损害他人的合法权益。

该工具的运行仅依赖用户提供的信息，并不包括任何违反相关法律法规的内容。用户在使用本网络安全工具时，必须确保其提供的信息合法、有效、真实可靠，否则可能会产生不利后果。

本软件提供的服务仅供参考，不构成任何责任。用户在使用本网络安全工具时，应自行承担有关安全风险。我们并不对使用者使用本工具所涉及的任何技术服务承担任何义务或责任，无论此类技术服务是否有任何损失和/或损害。

## 简介：
Cscan-Go 一款主要用来平替 cscan(python版) 的，本人也很喜欢使用 cscan，但是无奈 python 版有一些局限性，在内网横向的时候，如果想要把 cscan 传到内网，就要考虑环境的问题了，所以有了 cscan 的 go 版本！  

本程序目前刚开始开发，所以有些 cscan 原版的功能不是很全，但是我会把 cscan 原版的全部功能都加进去的，然后再加一些自己觉得好用的小细节和功能点，在今后的hw中，方便红队队员们使用。  

最后，感谢 cscan 作者带给我的灵感！

🎉🎉🎉！
## 使用方法： 
```shell
# 对 IP 或 IP 段 进行扫描（可以同时对多个段进行扫描）
./cscan -i "192.168.1.1, 192.168.2.1-50, 10.0.0.1/16"

# 对 IP 文件内的 IP 或 IP 段 进行扫描（ipfile.txt 文件中的格式可以看下面的示例！）
./cscan -l "ipfile.txt"

# 指定端口，并加上默认端口，对 IP 段进行扫描（支持端口范围添加，
# 而且不用担心指定的端口与默认端口重复，因为本工具会自动去重！）
./cscan -i "192.168.1.1/24" -dp "1-1000, 8000-10000, 23333"

# 指定端口，禁用默认端口，对 IP 段进行扫描（同上，就参数不一样，使用方法都一样）
./cscan -i "192.168.1.1/24" -fp "1-1000, 8000-10000, 23333"
```

### ipfile.txt 格式示例：
```text
192.168.1.1
192.168.1.2
192.168.3.1-50
10.2.1.1/16
172.18.1.1/24
```

### 参数介绍：
```text
-i  ==> 需要扫描的 IP、IP段 或 IP范围，如：192.168.1.1, 192.168.1.1/24, 192.168.1.1-20（仅支持 "," 逗号分隔）

-l  ==> 需要导入的 IP 文件

-t  ==> 设置最大线程数（默认：100）

-v  ==> 显示并打印所有细节

-f  ==> filter 过滤保留需要的状态码，并打印输出（支持 "," 逗号 " " 空格 "；" 分号分隔）

-o  ==> 导出检测的结果，目前支持的格式：txt、csv。如：-o outfile.txt、-o outfile.csv

-dp ==> default ports 指定端口，然后加上默认端口一块扫描（支持 "," 逗号 " " 空格 "；" 分号分隔）

-fp ==> forbid ports 指定端口扫描，禁用默认端口（支持 "," 逗号 " " 空格 "；" 分号分隔）

-timeout    ==> 设置超时时长，单位：秒（默认：7s）

-proxy  ==> 设置代理，如：socks5://localhost:1080 或 http://localhost:8080

-debug  ==> Debug 模式，开启后显示所有的日志信息
```
#### 默认端口：
```text
80 81 82 83 84 85 86 87 89 88 443 8443 7001 7080 7090 8000 8008 
8888 8070 8080 8081 8082 8083 8084 8085 8086 8087 8088 8089 8090 
8161 9001 9090 9443 21 22 445 1100 1433 1434 1521 3306 3389 3399 
6379 8009 9200 11211 27017 50070
```

## 版本信息：

### v1.1.0 🐱
#### 新版本迭代实现的功能：
1. 扫描完端口之后，对于开放的端口，检测是否有 web 服务（并且检测到的结果终端输出显示的花里胡哨）
2. 只输出指定状态码的扫描结果
3. 实现导出功能，支持 txt、csv 两种格式
#### 新版本改动：
1. 考虑到使用场景大部分是在内网，所以缩短了 timeout 的超时时间，由原来的 10s 修改为 7s（这将大大提高扫描效率，如果网络连通性不好，可以自行修改）
2. 默认 50 的线程数修改为 100


### v1.0.0 🐶
#### 作为初始版本，主要支持的功能有：
1. 基本的端口扫描
2. 支持 IP、IP段、c段，三种格式的扫描方式
3. 端口输入支持范围输入
4. 导入 IP 文件同样支持 IP、IP段、c段，三种格式的扫描方式
5. 支持 socks5、http 代理，方便挂代理扫描，或者穿透到内网扫描
#### 后续准备添加的功能有（画饼）：
1. 支持导出输出结果（因为暂时功能太少了，所以还没写导出，不过这个很简单，很快就能写好）
2. 扫描完端口之后，对于开放的端口，检测是否有 web 服务
3. 对于一些常用的端口，如：3306，1433等，进行简单的弱口令爆破（放心，密码不会太多！）
4. 暂时想不到了，等想到了再补充...