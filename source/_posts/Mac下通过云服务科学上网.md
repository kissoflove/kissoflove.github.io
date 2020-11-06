---
title: Mac下通过云服务科学上网
---
用某歌云服务搭建自己的云服务平台

## 1.申请某歌云服务
- 申请条件  
拥有双币信用卡（Master/VISA等） 
可以访问外网的环境（先用下别人的梯子）

- 2 开始注册  
[谷歌云](https://cloud.google.com)

- 参考  
[免费|申请谷歌云服务器](https://zhuanlan.zhihu.com/p/60993816)  
[免费申请谷歌云账号](https://zhuanlan.zhihu.com/p/96026350)

- 注册成功  
![注册成功](/images/blog/ggc/001.png)
<!-- <p><img src="/images/blog/ggc/001.png" width="300" height="200"><p> -->

## 2.VPC网络
- 配置防火墙规则
- 创建静态IP  
  > 可以参考“1.申请某歌云服务”中的链接

## 3.VPC网络计算引擎(compute engine)
- 创建基于静态IP的VM实例
  > 可以参考“1.申请某歌云服务”中的链接

## 4.SSH登录
- 网页登录(自带)  
- 本地登录  
  - 环境：Mac终端自带ssh
  - 生成ssh秘钥（网上可查如何生成，如果是常用git的人，用之前生成的ssh秘钥就行），上传到计算引擎(compute engine)
    - 元数据  
      ![元数据](/images/blog/ggc/002.png)
    - SSH 秘钥 -> 修改  
      ![秘钥](/images/blog/ggc/003.png)
    - 添加  
      ![添加秘钥](/images/blog/ggc/004.png) 
    - 输入 -> 保存  
      ![保存秘钥](/images/blog/ggc/005.png)
    - 登录  
      在终端输入
      ``` bash 
      # ssh 所添加秘钥的用户名@VM的外部IP
      ssh Kobe@35.124.13.20
      ```
## 5.在VM安装V2Ray
- 在网页或终端进入VM都行，然后输入：  
``` bash 
# 先进入root
sudo -i
```
``` bash 
# 安装
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```
- 安装完成  
  ![安装](/images/blog/ggc/006.png)
- 查看配置  
``` bash 
cat /usr/local/etc/v2ray/config.json
```
- 修改配置
``` bash 
vi /usr/local/etc/v2ray/config.json
```
输入“i”进入编辑状态，接着copy下面的内容：
``` bash 
{
	"inbounds": [{
		"port": 23581,
		"protocol": "vmess",
		"settings": {
			"clients": [{
				"id": "ceb793e6-49cf-25d8-e4de-ae542e62748e",
				"level": 1,
				"alterId": 64
			}]
		}
	}],
	"outbounds": [{
		"protocol": "freedom",
		"settings": {}
	}, {
		"protocol": "blackhole",
		"settings": {},
		"tag": "blocked"
	}],
	"routing": {
		"rules": [{
			"type": "field",
			"ip": ["geoip:private"],
			"outboundTag": "blocked"
		}]
	}
}
```
然后按“esc”键，输入“:wq”保存
> 配置里的“port”，“id”，“level” 和 “alterId”会在后续客户端配置用到。
- 启动  
  ![启动](/images/blog/ggc/007.png)
- 参考  
1.[fhs-install-v2ray](https://github.com/v2fly/fhs-install-v2ray)  
2.[V2Ray服务端安装和配置](https://www.lazylr.com/lazyl-jc/120.html)

## 6.使用V2Ray客户端（Mac版）
- 配置
  - 前面配置的VM IP（外部）  
    ![进入](/images/blog/ggc/008.png)
  - “port”，“id”，“level” 和 “alterId”  
    ![配置](/images/blog/ggc/009.png)  
- 使用
  - 开启(On)
  - 模式一般选 “Pac Mode”

## 7.登录某歌
  ![google](/images/blog/ggc/010.png) 
