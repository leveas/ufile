# 海外加速访问国内方案

## 背景

在海外访问国内US3存储服务，会有访问速度慢的问题。

## 应用场景

需要在海外访问国内的US3存储服务

## 方案优势

可以使海外用户稳定访问国内的US3存储服务

## 方案实施

### 使用PathX全球动态加速

PathX的使用文档见：[PathX文档](https://docs.ucloud.cn/pathx)

### 配置US3的自定义域名

1. 在PathX服务建立后会获取到一个PathX加速域名

![image](/images/pathx2.png)

![image](/images/pathx3.png)

2. 进入US3的国内可用区Bucket中，选择域名管理选项卡，绑定自定义域名，将PathX提供的加速域名填写到自定义域名中，CDN加速选择否。

![image](/images/pathx1.png)

之后在海外访问PathX的域名即可通过PathX的加速通道访问到国内的Bucket内容

## 注意事项

在配置了pathX加速域名后，只有在访问此加速域名时才会拥有加速效果

