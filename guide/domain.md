# 域名管理

文件（Object）上传至存储空间（Bucket）后，US3会自动生成文件URL，用户可以直接通过文件URL访问该文件。若用户希望通过自己的域名或CDN访问US3上的文件，需要将自己的域名或CDN域名绑定至文件所在的存储空间。
>- 实际生产环境推荐您绑定自定义域名创建 CDN 加速。

## 绑定自定义域名
>**前提条件**：
>- 确认自定义域名已经完成备案（若未进行备案，域名会被管局停止解析，导致业务不可用），备案相关请访问 [备案专区](https://www.ucloud.cn/site/beian/index.html)；
>- 并已在DNS服务商处***配置CNAME***至US3的存储空间域名。

1. 登陆对象存储控制台。
2. 单击**单地域空间管理**，然后单击目标Bucket。
3. 在顶部导航栏，选择**域名管理**，单击**绑定自定义域名**。
![](/images/guide/绑定自定义域名1.png)
    - CDN加速：CDN加速选项在UCDN产品页加速和管理，点击确定后将前往UCDN产品页面继续完成操作，跳转到 UCDN 产品页面继续完成基础配置和高级配置。UCDN加速相关请参考[UCDN创建加速域名](https://docs.ucloud.cn/ucdn/quick/create)。
4. 在域名列表找到需要证书托管的域名，单击右侧**证书托管**，证书来源选择如下：
    - **从USSL导入**：选择已托管在USSL证书管理平台的证书
    ![](/images/guide/证书托管-ussl导入.png)
    - **从本地导入**：按照公钥（public.crt）、私钥（private.key）格式上传文件
    ![](/images/guide/证书托管-本地导入.png)
    - **手动输入**：按照公钥、私钥pem格式输入
    ![](/images/guide/证书托管-手动输入.png)
5. 通过自定义域名访问US3文件
绑定自定义域名后可直接通过HTTP协议访问文件，文件URL的格式为http://YourDomainName/ObjectName 。如果用户需要通过HTTPS协议访问文件，需完成步骤4的证书托管。
> 举例：目标存储空间examplebucket中存放了exampleobject.jpg的文件，自定义域名为www.demo.com ，且已完成证书托管，此时您可以使用https://www.demo.com/exampleobject.jpg 访问目标文件。

## 解绑自定义域名
当用户的自定义域名不再使用时，可以手动解除域名绑定。

1. 如果目标存储空间绑定的自定义域名启用了CDN加速服务，用户需要修改CDN加速服务的源站信息，使自定义域名不再指向US3域名。具体操作，请参见[停用UCDN服务](https://docs.ucloud.cn/ucdn/quick/stop)。
2. 在顶部导航栏，选择**域名管理**，在域名列表找到需要解绑的域名，单击右侧**解绑**，弹窗**确定**后即可完成。
![](/images/guide/域名解绑.png)

