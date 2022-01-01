# 免责声明
本项目仅用于安全自查，请勿利用文章内的相关工具与技术从事非法测试，如因此产生的一切不良后果与本项目无关


如果觉得还不错，请给本项目一个star

# AWVS13\14 api脚本
![awvs_config.ini](https://s4.ax1x.com/2022/01/01/T5TeoR.png)




## 2022年1月1号，新支持批量添加目标，仅扫描log4j漏洞
1-9-标签，标签可不输
![f16321066fa883e8c685ad99fd2c140](https://s4.ax1x.com/2022/01/01/T5T5XF.png)
![f16321066fa883e8c685ad99fd2c140](https://s4.ax1x.com/2022/01/01/T5HAa9.png)
## AWVS14，本版本支持log4j版本漏洞
推荐使用docker 
```
安装
docker pull xiaomimi8/awvs14-log4j-2022

启动
docker run -it -d -p 13443:3443 xiaomimi8/awvs14-log4j-2022

```

## 联动Xray 说明一下 
如果AWVS爬虫请求太多，此时发送给Xray，可能会占满Xray队列(max_length)，导致代理阻塞，由于Xray的阻塞，AWVS会导致爬虫超时，这个在Xray文档中有说明，所以在批量之前 ，尽可能把Xray的max_length的值设成很大

## 脚本功能
支持AWVS13的API接口

* 支持URL批量添加扫描
* 支持批量扫描apache-log4j漏洞
* 支持对批量url添加`cooKie`凭证进行爬虫扫描
* 支持结合被动扫描器进行配置扫描,如：`xray`,`w13scan`,`burp`等扫描器
* 支持一键删除所有任务
* 通过配置`awvs_config.ini`文件，支持自定义各种扫描参数，如:爬虫速度，排除路径(不扫描的目录),全局`cookie`,限制为仅包含地址和子目录
* 支持对扫描器内已有目标进行批量扫描，支持自定义扫描类型

## 使用教程

### awvs_config.ini请使用专业编辑器打开，记事本会改变原有格式，导致报错

#### 1、配置好当前目前的awvs_config.ini文件
![awvs_config.ini](https://github.com/test502git/awvs13_batch_py3/blob/master/add_log/config.png)


#### 2、使用Python3运行awvs_add_url-v2.0.py
![awvs_add_url](https://github.com/test502git/awvs13_batch_py3/blob/master/add_log/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200728190739.png)


现在就可以根据自己需求进行扫描吧

如
#### awvs13批量添加并设置仅爬虫，配置好cookie等参数，发送到xray扫描器扫描
![awvs_add_url](https://github.com/test502git/awvs13_batch_py3/blob/master/add_log/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200728204949.png)


## AWVS性能优化(防止AWVS宕机) 1核1G vps举例
#### 内存限制
如： 限制awvs 最多占用0.5G内存(按机器实际情况配置)

```docker update --memory 512m --memory-swap -1 awvs容器id```

#### CPU限制
如： 假机器有1核，下面是限制awvs最多使用0.5核，  那么主机最多cpu占用率不会超过50%(按机器实际情况配置)

```docker update --cpus 0.5 --memory-swap -1 awvs容器id```

#### 容器自启
主要防止意外情况主机的重启 后awvs不会自启

 ```docker update --restart=always awvs容器id ```

这样一套设置下来后再扫，awvs永不死机

