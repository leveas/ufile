# 在URL中包含签名

除了使用Authorization Header，您还可以在URL中加入签名信息，以便将该URL转给第三方实现授权访问。

### 注意事项

- 使用在URL中签名的方式，会将授权的数据在过期时间内曝露在互联网上，请预先评估使用风险。
- US3不支持同时在URL和Header中包含签名。
- PUT和GET请求都支持在URL中签名。
- 您可以为PUT操作生成一个预签名的URL，该URL检查用户是否上传了正确的内容。SDK对请求进行预签名时，将计算请求正文的校验和，并生成包含在预签名URL中的MD5校验和。用户必须上传与SDK生成的MD5校验和相同的内容，否则操作失败。如果要验证MD5，只需在请求中增加Content-MD5头即可。

### 签名实现

* 签名示例

  ```https://*****.cn-bj.ufileos.com/us3-api.pdf?UCloudPublicKey=TOKEN_2368f919-0e7e-4e6c-affb-083fa53110e8&Signature=ExPEr%2FBkI8AQZ2RP87UAgn%2BwkCk%3D&Expires=1656487334```

* 参数说明

  | 名称                | 类型   | 是否必选 | 描述                                                         |
  | :------------------ | :----- | :------- | :----------------------------------------------------------- |
  | **UCloudPublicKey** | 字符串 | 是       | 指定URL签名中使用的UCloudPublicKey。                         |
  | **Expires**         | 数字   | 是       | Unix时间戳（自UTC时间1970年01月01号开始的秒数），用于标识该URL的超时时间。如果US3接收到该URL请求的时间晚于签名中包含的Expires参数时，则返回请求超时的错误码。例如，当前时间是1141889060，开发者希望创建一个60秒后自动失效的URL，则可以设置Expires时间为1141889120。 |
  | **Signature**       | 字符串 | 是       | 签名信息。格式如下：<br>```StringToSign = HTTP-Verb + "\n" + Content-MD5 + "\n" +    Content-Type + "\n" + Expires + "\n" +    CanonicalizedUCloudHeaders +    CanonicalizedResource```<br>* 所有US3支持的请求和各种Header参数，在URL中进行签名的算法和在Header中包含签名的算法类似。<br>* 生成URL中的签名字符串时，除了将Date参数替换为Expires参数外，仍包涵其他[签名]([API 签名算法 对象存储 US3_文档中心_UCloud中立云计算服务商](https://docs.ucloud.cn/ufile/api/authorization?id=文件管理签名算法))Header<br>* 使用URL签名时，US3会先验证请求时间是否晚于Expires时间，然后再验证签名。 |

