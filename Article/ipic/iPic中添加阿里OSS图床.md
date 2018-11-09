[阿里云对象存储](https://www.aliyun.com/product/oss)（Object Storage Service，简称OSS），是阿里云对外提供的云存储服务。OSS适合存放任意文件类型，有很多朋友将其作为静态网站加速、或图床。本文就介绍下如何在 iPic 中添加阿里 OSS 图床。

# 在偏好设置中添加阿里云 OSS 图床

打开 `偏好设置`，进入 `图床` 页，选择添加 `阿里云 OSS`

![](http://twowaterimage.oss-cn-beijing.aliyuncs.com/2018-11-09-124310.jpg)

以下分别介绍各项的意义及如何配置：

*   `Bucket`
    *   即阿里云 OSS 中对应的 Bucket
    *   更多介绍，请参考下文中的 [创建阿里云 OSS 存储空间（即 Bucket）](https://toolinbox.net/iPic/AddAliOSS.html#%E5%88%9B%E5%BB%BA%E9%98%BF%E9%87%8C%E4%BA%91-OSS-%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%EF%BC%88%E5%8D%B3-Bucket%EF%BC%89)
*   `Access Key` 与 `Secret Key`
    *   这个稍微有点复杂，不熟悉的朋友可以看下文中的 [使用安全的 AccessKey](https://toolinbox.net/iPic/AddAliOSS.html#%E4%BD%BF%E7%94%A8%E5%AE%89%E5%85%A8%E7%9A%84-AccessKey)
*   `网址前缀`
    *   用于生成上传图片的完成 Http 路径，比如 _[http://ipic-test.oss-cn-hangzhou.aliyuncs.com](http://ipic-test.oss-cn-hangzhou.aliyuncs.com/)_
    *   （可选）如果你绑定了域名，可以使用绑定的域名
    *   更多介绍，请参考下文中的 [阿里云 OSS 域名管理](https://toolinbox.net/iPic/AddAliOSS.html#%E9%98%BF%E9%87%8C%E4%BA%91-OSS-%E5%9F%9F%E5%90%8D%E7%AE%A1%E7%90%86)

完成输入后，可以点击 **验证** 按钮。如果输入没有问题，则右侧会出现 **通过** 链接，点击后就可以看到测试时上传至又拍的图片。

除了上述基本的设置，阿里云 OSS 还支持几项高级设置。点击 `网址前缀` 右侧的高级设置图标，即可弹出如下设置界面：

![](http://twowaterimage.oss-cn-beijing.aliyuncs.com/2018-11-09-131517.jpg)

*   `网址前缀`
    *   和前文介绍的一致
*   `文件名前缀`
    *   可简单理解为阿里云 OSS 中的目录
    *   例如，如果希望 iPic 上传的图片都位于 `blog` 目录中，只要在这里输入 `blog`，所有上传的图片则均位于 `blog` 目录中，生成的图片链接类似于：_[http://ipic-test.oss-cn-hangzhou.aliyuncs.com/blog/pic.jpg](http://ipic-test.oss-cn-hangzhou.aliyuncs.com/blog/pic.jpg)_
*   `自定义文件名`
    *   支持以下三种文件名
    *   `纯文件名` 也即和上传时文件名相同，比如 `pic.jpg`
    *   `日期-文件名` 也即和之前的 iPic 相同，比如 `2016-06-16-pic.jpg`
    *   `随机` 比如 `jk8l1.jpg`，可以大大缩短链接长度
*   `网址后缀`
    *   可用于阿里云 OSS 中的自定义样式
    *   比如，在又拍中添加了名字为 `s` 的版本表示缩略图，间隔标识符为 `-`，则可以在 `网址后缀` 中输入 `-s`。这样之后每次上传图片生成的链接最后都会附上 `-s`，例如：_[http://ipic-test.oss-cn-hangzhou.aliyuncs.com/blog/pic.jpg-s](http://ipic-test.oss-cn-hangzhou.aliyuncs.com/blog/pic.jpg-s)_

最后，点击 **应用** 按钮进行保存。

# 阿里云 OSS 介绍

## 创建阿里云 OSS 存储空间（即 Bucket）

首先，需要 [开通 OSS 服务](https://help.aliyun.com/document_detail/31884.html)

然后，可参考此教程：[创建存储空间](https://help.aliyun.com/document_detail/31885.html)

创建后，打开该 Bucket，点击 `Bucket 预览`，在页面底部即可看到 `OSS 外网域名`，即是 iPic 图床配置中的 `网址前缀`

![](https://ps.toolinbox.net/9i2dn.png)

## 使用安全的 AccessKey

默认的 AccessKey 具体全部权限，从安全的角度，并不建议直接使用。而事实上，阿里云有更复杂、安全的访问控制服务 RAM (Resource Access Management) 这里简单介绍下如何创建仅能访问某一 Bucket 的 AccessKey.

### 新建授权策略

进入并开启 `访问控制` 面板

![](https://ps.toolinbox.net/edzro.png)

点击 `授权策略管理`，然后点击 `新建授权策略`

![](https://ps.toolinbox.net/uokdh.png)

搜索 oss，选择 `AliyunOSSFullAccess` 进入下一步

![](https://ps.toolinbox.net/o3qjm.png)

设置 `授权策略名称`，并填写如下内容（注意，需要将 `ipic-test` 替换为你实际的 Bucket 名称）：

![](https://ps.toolinbox.net/hn0vh.png)

```json
{
  "Statement": [
    {
      "Action": "oss:*",
      "Effect": "Allow",
      "Resource": [
        "acs:oss:*:*:ipic-test",
        "acs:oss:*:*:ipic-test/*"
      ]
    }
  ],
  "Version": "1"
}
```


最后，点击 `新建授权策略`

### 新建用户

依次点击 `用户管理` > `新建用户`

![](https://ps.toolinbox.net/pkshf.png)

填写登录名，注意勾选 `为该用户自动生成 AccessKey`，然后点击确定

![](https://ps.toolinbox.net/sycgg.png)

保存这里的 `AccessKeyID` 与 `AccessKeySecret`，分别对应 iPic 图床配置中的 `Access Key` 与 `Secret Key`

![](http://twowaterimage.oss-cn-beijing.aliyuncs.com/2018-11-09-131710.jpg)

在用户列表界面点击新创建用户对应的 `授权`

![](http://twowaterimage.oss-cn-beijing.aliyuncs.com/2018-11-09-131651.jpg)

搜索刚刚创建的策略名称，并点击 `>` 将其移至右侧，然后点击 `确定`


![](http://twowaterimage.oss-cn-beijing.aliyuncs.com/2018-11-09-131635.jpg)

![](http://twowaterimage.oss-cn-beijing.aliyuncs.com/2018-11-09-131625.jpg)

这样，就完成了新建用户及授权，接下来可以使用文章开头介绍的方法，使用该 AccessKey 连接阿里云 OSS 图床。

## 阿里云 OSS 图片处理服务

和七牛等图床一样，阿里云 OSS 也支持自定义图片样式，比如宽度最大为 500 px 的缩略图。需要注意的是，如果要使用自定义样式，网址前缀就要使用图片服务特定的网址，而非一般文件的网址。比如：

*   一般文件的网址：[http://ipic-test.oss-cn-hangzhou.aliyuncs.com](http://ipic-test.oss-cn-hangzhou.aliyuncs.com/)
*   图片服务特定网址：[http://ipic-test.img-cn-hangzhou.aliyuncs.com](http://ipic-test.img-cn-hangzhou.aliyuncs.com/)
*   事实上，也就是将标准链接中的 _oss_ 替换为 _img_

具体的，在打开该 Bucket 后，点击 `图片处理`，然后在右侧即可看到 `图床服务的域名`，将其设置为 iPic 图床配置中的 `网址前缀`，即可使用阿里云 OSS `样式管理` 中所定义的样式。

![](http://twowaterimage.oss-cn-beijing.aliyuncs.com/2018-11-09-131601.jpg)

更多介绍：[OSS 图片处理服务](https://help.aliyun.com/document_detail/31917.html)

## 阿里云 OSS 域名管理

除了使用标准的域名，也可以绑定自己的域名。
官方教程：[阿里 OSS 域名管理](https://help.aliyun.com/document_detail/31902.html)



