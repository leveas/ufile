# 使用STS临时访问凭证访问US3

您可以通过STS服务给其他用户颁发一个临时访问凭证。该用户可使用临时访问凭证在规定时间内访问您的US3资源。临时访问凭证无需透露您的长期密钥，使您的US3资源访问更加安全。

## **前提条件**

已确保当前账号为被授予 `STSFullAccess` 权限的子账户或者主账户。关于为子用户/角色授权的具体步骤，请参见[为子账户用户授权](https://docs.ucloud.cn/uproject/user?id=%e4%b8%ba%e5%ad%90%e7%94%a8%e6%88%b7%e6%b7%bb%e5%8a%a0%e6%9d%83%e9%99%90)。

## **适用场景**

假设您是一个移动App开发者，希望使用US3服务来保存App的终端用户数据，并且要保证每个App用户之间的数据隔离。此时，您可以使用STS授权用户直接访问US3。

使用STS授权用户直接访问US3的流程如下：

![Untitled](/images/guide/STS整体流程图.png)

1. APP用户登录。APP用户和云账号无关，它是APP的终端用户，APP服务器支持APP用户登录。对于每个有效的APP用户来说，需要APP服务器能定义出每个APP用户的最小访问权限。
2. APP服务器请求STS服务获取一个安全令牌（SecurityToken）。在调用STS之前，APP服务器需要确定App用户的最小访问权限（用RAM Policy来自定义授权策略）以及凭证的过期时间。然后通过扮演角色（AssumeRole）来获取一个代表角色身份的安全令牌（SecurityToken）。
3. STS返回给APP服务器一个临时访问凭证，包括一个安全令牌（SecurityToken）、临时访问密钥（AccessKeyId和AccessKeySecret）以及过期时间。
4. APP服务器将临时访问凭证返回给App客户端，APP客户端可以缓存这个凭证。当凭证失效时，APP客户端需要向APP服务器申请新的临时访问凭证。例如，临时访问凭证有效期为1小时，那么APP客户端可以每30分钟向APP服务器请求更新临时访问凭证。
5. APP客户端使用本地缓存的临时访问凭证去请求US3 API。US3收到访问请求后，会通过STS服务来验证访问凭证，正确响应用户请求。

## ****步骤一：创建子用户****

1. 登录[控制台](https://console.ucloud.cn/uaccount/iam/user_manage)。
2. 在左侧导航栏，选择**用户管理**。
3. 单击邀请**用户**，并选择 "API访问”。
4. 输入**邮箱/用户名称**。
5. 然后单击**确定**。
6. 创建完成完成之后需要给改用户添加 `STSFullAccess` 权限

![Untitled](/images/guide/STS添加权限.png)

## **步骤二：创建用于获取临时访问凭证的角色**

1. 在左侧导航栏，选择**角色管理**。
2. 单击**创建角色**。
3. 在**创建角色**对话框，**角色名称**填写为RamOssTest。
4. 单击**完成**。角色创建完成后，单击**关闭**。

![Untitled](/images/guide/STS创建角色.png)

## **步骤三：为角色授予上传文件的权限**

1. 创建上传文件的自定义权限策略。

   1. 在左侧导航栏，选择**权限管理**>**权限策略**。

   2. 在**权限策略**页面，单击**创建权限策略**。

   3. 在**创建权限策略**页面，点击**授权语句**并选择**对象存储**，然后在Allow的新增类权限中选择PutFile的权限。具体配置示例如下。

      ![Untitled](/images/guide/STS添加PUT权限.png)

   4. 点击**确定。**

2. 为RAM角色RamOssTest授予自定义权限策略。

   1. 在左侧导航栏，选择**角色管理**。
   2. 在**角色**页面，找到目标RAM角色RamOssTest。
   3. 单击RAM角色RamOssTest页面的**添加权限**。
   4. 在**添加权限**页面下的**自定义策略**页签，选择已创建的自定义权限策略test。
   5. 单击**确定**。

## ****步骤四：获取临时访问凭证****

- 使用 REST API 

  使用子账户的API密钥调用****AssumeRole(获取扮演角色的临时身份凭证)，****请确保已为调用者授予STS的管理权限，当前API访问地址为`https://api.ucloud.cn`

  ****请求参数：****

  | 名称            | 类型   | 必填 | 描述                 | 示例值                            |
  | --------------- | ------ | ---- | -------------------- | --------------------------------- |
  | Action          | string | 是   | 动作：AssumeRole     | AssumeRole                        |
  | DurationSeconds | int    | 否   | 有效期。单位：秒。   | 43200                             |
  | PublicKey       | string | 是   | API公钥。            | ************                      |
  | RoleSessionName | string | 是   | 角色会话名称。       | alice                             |
  | RoleUrn         | string | 是   | 要扮演的RAM角色ARN。 | ucs:iam::55936045:role/RamOssTest |
  | Signature       | string | 是   | 签名。[签名方法](https://docs.ucloud.cn/api/summary/signature)               | ************                      |

  返回参数：

  | 名称            | 类型   | 描述                | 示例值                  |
  | --------------- | ------ | ------------------- | ----------------------- |
  | Message         | string | 状态消息。          | success                 |
  | SecurityToken   | string | 安全令牌。          | AAHoTYM******           |
  | AccessKeyId     | string | 访问密钥ID。        | STS.3ho71eJpBoyQvgD**** |
  | AccessKeySecret | string | 访问密钥。          | MwSLKHDpz72KsKBQ****    |
  | Expiration      | string | Token到期失效时间。 | 2023-07-11T20:15:58Z    |

  示例：

  ![Untitled](/images/guide/STS获取Token.png)

## ****步骤五：使用临时访问凭证上传文件至US3****

以Go SDK 为例，将本地`/data/`路径下的exampletest.txt文件上传至存储空间examplebucket下的src目录的示例代码如下：

```go
func PutFile() {
	config := &ufsdk.Config{
		PublicKey:  "STS.******",
		PrivateKey: "******",
		FileHost: "cn-bj.ufileos.com",
		BucketName: "examplebucket",
	}
	head := http.Header{}
	head.Set("SecurityToken", "AAFtszzpaIlV8y23F0T2tCpAJmtEZ6CpIjO4bCmCCU******")
	req, err := ufsdk.NewFileRequestWithHeader(config, head, nil)
	if err != nil {
		panic(err.Error())
	}
	err = req.PutFile("/data/exampletest.txt", "src/exampletest.txt", "")
	if err != nil {
		panic(err.Error())
	}
}
```
