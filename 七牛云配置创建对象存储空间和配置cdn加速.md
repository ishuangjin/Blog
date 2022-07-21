

## 创建对象存储空间

1. 访问[七牛云官网](https://www.qiniu.com/)，注册登录，然后进行实名认证，实名认证后才有免费空间使用额度
2. 访问[新建存储空间链接](https://portal.qiniu.com/kodo/bucket?shouldCreateBucket=true)进入创建页面

![image-20220718174300580](http://cache.ishuangjin.cn/img/202207181743943.png)

3. 输入存储空间名称，因为我们后面要用来做CDN加速以及图床，所以访问控制选择为**公开**

   ![image-20220718175205203](http://cache.ishuangjin.cn/img/202207181752569.png)

4. 保存后就能在空间管理中看到刚刚创建好的存储空间了

## 配置CDN加速

1. 创建创建对象存储空间后，点击空间名称--》域名管理--》自定义CDN加速域名--》绑定域名

   ![image-20220718175942596](http://cache.ishuangjin.cn/img/202207181759869.png)

2. 进入域名配置项页面后，注意以下配置，具体可参考图片

   - 加速域名填写二级域名例如`cache.ishuangjin.cn`
   - 通信协议选择`HTTP`，*~~当然如果你是付费用户并且能折腾证书配置那当我没说~~*
   - 使用场景因为我用来当做图床，所以选择`图片小文件`
   - 源站配置选择`七牛云存储`
   - 缓存时间选择`自定义`--》`使用推荐配置`

   ![image-20220718180556673](http://cache.ishuangjin.cn/img/202207181805081.png)

3. 打开你的DNS解析，腾讯云服务器可以打开[腾讯云DNS解析](https://console.cloud.tencent.com/cns)创建一个二级域名，例如

## 防盗链



