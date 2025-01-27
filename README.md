# HexDnsEchoT

---

该工具由[https://github.com/sv3nbeast/DnslogCmdEcho](https://github.com/sv3nbeast/DnslogCmdEcho)修改而来。

## 背景

在一次日常渗透过程中，发现了一台机器可以利用jndi命令执行，但是无法注入内存马，也无法直接上线，所以想到了[https://github.com/sv3nbeast/DnslogCmdEcho](https://github.com/sv3nbeast/DnslogCmdEcho)这一工具来获取一些机器的信息，但是在使用过程中，发现该工具使用的DNS为dig.pm，但是dig.pm经过一些修改后变得时好时坏，无法接收到目标机器的请求，而且使用该工具需要执行两个python文件，非常的难受，因此产生了修改该工具的想法。

## 修改内容

* 将dig.pm修改为ceye.io，增加接收范围
* 添加自定义参数，不再需要在文件中修改
* 合并HexDnsEcho与CommandGen两个文件，使用更加方便
* 添加获取上一次命令执行结果的功能
* 兼容zsh

## 使用

```bash
usage: HexDnsEchoT.py [-h] [-d DNSURL] [-t TOKEN] [-lt LASTFINISHTIME] [-f FILTER] [-m MODEL]

options:
  -h, --help            show this help message and exit
  -d DNSURL, --dnsurl DNSURL
                        ceye dnslog
  -t TOKEN, --token TOKEN
                        ceye token
  -lt LASTFINISHTIME, --lastfinishtime LASTFINISHTIME
                        the lastfinisgtime
  -f FILTER, --filter FILTER
                        dns filter
  -m MODEL, --model MODEL
                        recent result
```



```bash
 python3 HexDnsEchoT.py -d YourCeye.ceye.io -t ceyeToken
```

![image-20230319185120315](https://github.com/A0WaQ4/HexDnsEchoT/blob/main/img/image-20230319185120315.png)

![image-20230319185219861](https://github.com/A0WaQ4/HexDnsEchoT/blob/main/img/image-20230319185219861.png)

复制输出的命令，在目标机器上执行

![image-20230318021456697](https://github.com/A0WaQ4/HexDnsEchoT/blob/main/img/image-20230318021456697.png)

DNS获取到请求，进行解密，获取机器信息

![image-20230318021542090](https://github.com/A0WaQ4/HexDnsEchoT/blob/main/img/image-20230318021542090.png)

在linux上也可以执行获取结果

![image-20230318021732464](https://github.com/A0WaQ4/HexDnsEchoT/blob/main/img/image-20230318021732464.png)

执行成功后自动开启新的filter，无需重新执行直接进行下一步命令执行

![image-20230319185425908](https://github.com/A0WaQ4/HexDnsEchoT/blob/main/img/image-20230319185425908.png)

有时会出现目标机器的命令未执行完成，但是已经获取到了一部分结果，可以使用以下命令再次获取结果，本命令已经输出在上次的执行结果中，可直接复制使用

```shell
python3 HexDnsEchoT.py -d yourceye.ceye.io -t ceyetoken -f filterstr -lt "上次命令执行的时间" -m GR
```

![image-20230319185814165](https://github.com/A0WaQ4/HexDnsEchoT/blob/main/img/image-20230319185814165.png)

![image-20230319185941334](https://github.com/A0WaQ4/HexDnsEchoT/blob/main/img/image-20230319185941334.png)

## 总结

在遇到工具无法使用的时候不要放弃，多想想办法就可以解决
