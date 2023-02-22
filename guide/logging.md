

# 日志管理

用户在访问 US3 文件时，会产生大量的访问日志，可以开启日志管理功能。开启后该空间的访问日志，会以小时为单位，按照固定的命名，存储到用户指定的空间中。请求日志会尽最大可能传输到您的桶中，不过可能会出现少量的日志延迟或丢失。  

## 设置日志存储

选择对应空间，在右侧操作中点击日志管理按钮  
![](/images/guide/日志管理1-1.png)

点击开启日志存储功能按钮  
![](/images/guide/日志管理1-2.png)

选择日志存储空间和填写日志前缀，日志存储空间默认为当前空间，日志前缀默认`ufile-log/`，前缀可以为空。
例：填写前缀为`ufile-log/2019/`，日志生成后将被存储在该填写的目录下。  

## 日志命名规则

```\<TargetPrefix\>\<SourceBucket\>-YYYY-MM-DD-HH-UniqueString.csv```

命名规则字段说明:  

|字段|说明|
|----|----|
|TargetPrefix|由用户指定，表示存储访问日志记录的名称前缀，可以为空|
|YYYY-MM-DD-HH|分别是该日志被创建时的阿拉伯数字的年、月、日、小时|
|UniqueString|为系统生成的位字符串，用于唯一标识该 Log 文件|

存储空间访问日志的名称例子如下： 
```ufile-log/SourceBucket-2018-10-22-08-537928a6-8be3-462c-a93c-b531028fe3fb.csv```

## 日志格式和示例

日志以csv格式提供，日志首行为日志头，对应下面日志正文每一列的含义。  
日志头如下:  
```Action,Bucket,Key,Host,UserAgent,RemoteIP,Referer,HttpStatus,TimeStamp,CostTime,ErrCode,ErrMsg,IsUCDN,FileSize,StorageClass,CreatedTime,RequestSize,TokenPublicKey,SessionId```  


### 字段描述
|字段|示例|说明|
|----|----|----|
|Action|LIST_OBJECTS|请求的接口|
|Bucket|my-bucket|请求的存储桶|
|Key|example-object|请求的对象名称|
|Host|example-bucket.cn-bj.ufileos.com|请求访问的目标域名|
|UserAgent|curl/7.77.0|HTTP的User-Agent头|
|RemoteIP|192.168.0.1|请求者的IP|
|Referer|https://console.ucloud.cn/|请求的HTTP Referer标头|
|HttpStatus|200|HTTP状态码|
|TimeStamp|1654653684|请求的时间戳|
|CostTime|13|请求耗时(ms)|
|ErrCode|0|请求错误码(0为没有报错)|
|ErrMsg|success|请求错误信息(success为请求成功)|
|IsUCDN|false|是否使用了UCDN|
|FileSize|1024|文件大小(B),非GET请求时此值为0|
|StorageClass|STANDARD|文件存储类型：STANDARD表示标准存储；IA表示低频访问存储；ARCHIVE表示归档存储。非GET请求时此值为UNKNOWN|
|CreatedTime|1654653681|文件创建时间,非GET请求时此值为0|
|RequestSize|4096|本次上传/下载请求时的大小, 仅在执行PutFile, PostFile, UploadPart, GetFile四种上传下载请求时有意义|
|TokenPublicKey|Token_xxxx_xxx_xxx|请求使用的token公钥|
|SessionId|d0e27b98-6d78-4a8d-ac3c-9dd3e3e7b405|请求的session id|

## 日志示例
``` bash
Action,Bucket,Key,Host,UserAgent,RemoteIP,Referer,HttpStatus,TimeStamp,CostTime,ErrCode,ErrMsg,IsUCDN,FileSize,StorageClass,CreatedTime,RequestSize,TokenPublicKey,SessionId
LIST_OBJECTS,example-bucket,,example-bucket.cn-bj.ufileos.com,Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML like Gecko) Chrome/102.0.5005.61 Safari/537.36,10.75.220.2,https://console.ucloud.cn/,200,1654653684,13,0,success,false,0,UNKNOWN,0, 0, Token_ABCDEFG, d0e27b98-6d78-4a8d-ac3c-9dd3e3e7b405
OPTIONS,example-bucket,,example-bucket.cn-bj.ufileos.com,Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML like Gecko) Chrome/102.0.5005.61 Safari/537.36,10.75.220.2,https://console.ucloud.cn/,200,1654653684,0,0,success,false,0,UNKNOWN,0, 0, Token_ABCDEFG, d0e27b98-6d78-4a8d-ac3c-9dd3e3e7b405
OPTIONS,example-bucket,,example-bucket.cn-bj.ufileos.com,Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML like Gecko) Chrome/102.0.5005.61 Safari/537.36,10.75.220.2,https://console.ucloud.cn/,200,1654653681,0,0,success,false,0,UNKNOWN,0, 0, Token_ABCDEFG, d0e27b98-6d78-4a8d-ac3c-9dd3e3e7b405
MPUT_DATA,example-bucket,2022-05-26-17-00000000.csv,example-bucket.ufileos.com,Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML like Gecko) Chrome/110.0.0.0 Safari/537.36,106.75.220.2,https://console.ucloud.cn/,200,1676257320,303,0,success,false,0,UNKNOWN,0,325,Token_xxxxxxxxxxxx,9bbff734-3555-4d29-a75b-e4221f1a3e9c
```
