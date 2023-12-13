![image](https://github.com/lorraineya/ufile/assets/111561221/da381fde-6b8f-45d3-8904-16cd088c11ce)

# 文件管理

选择指定存储空间，在右侧操作中单击“文件管理”按钮进入文件管理页。
![image](/images/guide-management/首个文件管理.png)

在文件管理列表页，可查看文件的基本信息，如文件名、ETag、文件大小、存储类型、MIME-TYPE、更新时间等。
![image](/images/guide-management/文件管理列表.png)

单击文件管理右上角的设置按钮，可以自定义文件列表。

![image](/images/guide-management/自定义列表.png)

## 上传文件

在文件管理页单击“上传文件”，弹窗中可以设置路径前缀、对象标签以及选择存储类型。
> 其中“继承Bucket”是指以Bucket存储类型为准，如需了解存储类型相关说明，请参见[存储类型](https://docs.ucloud.cn/ufile/introduction/storage_type)

![image](/images/guide-management/上传文件2.png)

## 修改 MIME-Type

直接单击“MIME-Type”下对应文件的修改按钮，或者勾选文件后单击“修改MIME-Type”即可完成修改。

![image](/images/guide-management/修改MIME-Type.png)
![image](/images/guide-management/修改MIME-Type2.png)

## 修改存储类型

直接单击“操作”下的“···”选择“修改存储类型”即可完成修改。

![image](/images/guide-management/修改存储类型.png)
![image](/images/guide-management/修改存储类型2.png)

## 删除文件

**单个文件删除**：直接单击“操作”下的“···”选择“删除”即可。

![image](/images/guide-management/单个文件删除.png)

**批量文件删除**：勾选待删除文件后单击“删除”即可。但需注意目前仅支持空目录删除，删除后将不可恢复。

![image](/images/guide-management/批量删除1.png)
![image](/images/guide-management/批量删除2.png)

## 下载文件

**单个文件下载**：直接单击“操作”下的“下载”即可。

![image](/images/guide-management/单个文件下载.png)

**批量文件下载**：勾选待下载文件后单击“下载”即可，下载可能存在延迟敬请谅解。但需注意选中的文件中如果包含归档/解冻的文件，请先完成解冻后再进行下载。

![image](/images/guide-management/批量下载.png)
![image](/images/guide-management/批量下载2.png)

## 重命名文件

在同一个存储空间内对文件进行重命名操作，实际上是通过Copy接口将源文件拷贝至目标文件，然后通过DeleteFile接口删除源文件来实现。

> 重命名注意事项：
> 1. 文件存储类型为低频且存储未满规定时长，则会产生存储不足规定时长容量费用。价格详情请参见[产品价格](https://docs.ucloud.cn/ufile/bill/billing)；
> 2. 文件存储类型为归档时，不支持重命名操作；
> 3. 重命文件名长度必须在1-1024字节之间，且不能包含“/”，不允许重名文件名；
> 4. 文件大小超过100MB，请使用命令行工具cli、US3 SDK或US3 API等方式，通过UploadPartCopy接口进行重命名操作。
>

将鼠标悬停在目标文件上，然后单击重命名图标，对文件进行重命名。重命名时，文件名称需包含后缀。

![image](/images/guide-management/重命名文件.png)

## 获取地址

**单个文件获取URL**：直接单击“操作”下的“···”选择“获取URL”，弹窗内可以设置传输协议（HTTPS或HTTP）、过期时间（5分钟、30分钟、12小时或自定义）、自有域名，单击”Copy“可复制该URL。

![image](/images/guide-management/单个获取url.png)
![image](/images/guide-management/单个获取url2.png)

**批量文件获取URL**：勾选待导出URL文件后单击“导出URL列表”，弹窗内可以设置传输协议（HTTPS或HTTP）、过期时间（5分钟、30分钟、12小时或自定义）、自有域名，确认资源后即可导出URL列表。

![image](/images/guide-management/批量导出url.png)
![image](/images/guide-management/批量导出url2.png)


## 设置HTTP头

文件（Object）的 HTTP 头部（元数据头部 ）是服务器以 HTTP 协议传送 HTML 资料到浏览器前所送出的字符串。通过修改 HTTP 头部（元数据头部），可以改变页面的响应形式，或者传达配置信息，例如修改缓存时间。修改对象的 HTTP 头部不会修改对象本身。
对象元数据包括有两种元数据：HTTP标准属性（HTTP Header）和用户定义元数据（User Meta）。

**HTTP标准属性（HTTP Header）**
为上传至存储空间中的每个文件保留如下HTTP标准属性。

| **名称**            | **描述**                   | **示例**                        |
| :------------------ | :------------------------- | :------------------------------ |
| MIME-Type           | 文件的 MIME 信息           | image/jpeg                      |
| Content-Encoding    | 文件的编码格式             | gzip                            |
| Content-Language    | 声明Object内容使用的语言。 | zh-CN                           |
| Content-Disposition | MIME 协议的扩展            | attachment;filename="fname.ext" |
| Cache-Control       | 文件的缓存机制             | no-cache;max-age=200            |
| Expires             | 用来控制缓存的失效日期     | Wed, 21 Oct 2015 07:28:00 GMT   |


**用户自定义元数据（User Meta）**
用户可以在上传文件（Object）时，为文件添加自定义元数据（User Meta），用于标识文件的用途或属性等。

- 一个Object可以有多个自定义元数据，但所有的自定义元数据总大小不能超过8 KB。
- 自定义元数据是一组键值对，元数据名称必须以X-Ufile-Meta-开头。
- 调用GetFile或者HeadFile接口时，将在HTTP头部返回自定义元数据。

  

直接单击“操作”下的“···”选择“设置HTTP头”即可完成修改。

![image](/images/guide/设置header头-文件列表.png)
![image](/images/guide/设置http头-界面.png)


