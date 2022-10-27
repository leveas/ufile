# 图片样式


US3支持将多个图片处理参数封装在一个样式（Style）内批量处理图片，可以快速满足复杂的图片处理需求，在控制台支持创建样式，编辑样式以及删除样式等操作，目前仅支持新加坡地域，其他地域若有需要，请联系技术技术支持。

### 创建样式
1. 登陆US3管理控制台。
![image](/images/pic/us3.png)
2. 在单地域空间管理，单击目标Bucket。
![image](/images/pic/bucket.png)
3. 选择数据处理>图片处理，然后单击添加样式。
![image](/images/pic/image_addstyle.png)
4. 在添加样式面板配置样式，可以使用可视化编辑和代码编辑两种方式添加样式：
![image](/images/pic/add_style.png)
![image](/images/pic/add_style_1.png)
- 可视化编辑：通过可视化界面选择需要的图片处理功能，如基本处理、图片尺寸、图片效果、水印等。
- 代码编辑：使用API代码编辑图片处理功能，格式为iopcmd=action1&parame=value1|iopcmd=action2&parame=value2...。
5. 输入样式名称，点击确定即可


### 编辑样式
![image](/images/pic/style_edit.png)



### 删除样式
![image](/images/pic/style_delete.png)




## 注意事项
- 每个存储空间Bucket下最多能创建50个样式，这些样式仅适用于该Bucket下的图片
- 可视化编辑尚未完全支持所有的样式功能，所以代码编辑后再次进入编辑的时候，可视化编辑的结果可能会丢失
- 当选择编辑框里面的参数，若图片没有加载成功，打开开发者模式检查对应返回错误，修改对应参数。