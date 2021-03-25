---
title: 胆道肿瘤精准辅助决策系统
tags:
 - 毕业设计过程
 - 前端架构之路
---
### 关于(进度)

- [ ] PACS {% post_link Orthanc%}
 - [ ]  客户端
  - [ ] 服务端
  - [ ] 数据库

### 角色服务以及关联（架构）

#### 存储（注释）

{% asset_img store.png 存储流程 %}

```
1. 客户端向Orthanc的Dicom服务器(8042端口)查询（使用WADO协议获取图像信息）

2. 客户端接收Orthanc的Dicom服务器的相应

3. 客户端的处理：

   获取图像的名称,大小和视图(view)。

   然后生成带注释的ROI(兴趣区域)的点的坐标(x,y).

   图像的名称,大小和视图,坐标和病变类型被转换成一个JSON格式的数据，并为下一步做好准备.

   (JSON是一种数据交换格式，便于机器解析和生成，也便于人们读写)

4. 客户端用HTTP POST请求将JSON数据发送到服务器端(端口:5000)。

   这是使用Ajax技术完成的，该技术能够异步发送和接收来自服务器的数据，而不会影响现有页面的显示和行为。

   在服务器端，使用POST方法创建了“存储-注释”REST  API，它接收JSON数据(在URL:http://localhost:5000/store-annotation中)并执行下一个过程。

5. Python语言提供REST API,首先由JSON格式生成数据,

   然后，使用带注释的ROI点的坐标和图像大小来建立带注释图像的真实值(groundtruth)

   这是用奇妙的OpenCV函数fillPoly()完成的

   该API能够建立任何数量的带注释的等高线

   图像的真实值(groundtruth)以png格式保存在本地存储器中，原始名称和大小以及所有注释信息以CSV格式保存，以便以后研究人员使用

6. 所有带注释的信息都以JSON格式存储在MongoDB数据库中，因此之后可以在客户端的OHIF查看器中再次使用它进行可视化。

7. 最后，在最后一步中，REST API用存储过程已成功执行的消息来响应客户端。
```

#### 处理（注释）

{% asset_img Intelligent.png 处理流程 %}

```
1.
  客户端使用WADO协议从Orthanc dicom服务器(端口:8042)查询以获取dicom图像
  
2. 
  客户端从Orthanc dicom服务器检索响应。
  
3. 
  客户端从dicom图像中获取像素数据，并从dicom标签中获取图像的高度和宽度。
  从像素数据构建图像需要有图像尺寸。
  因为在javascript中，检索到的图像像素数据看起来像一行中的数组对象，而不是numpy。
  然后，OHIF查看器将像素数据和图像大小转换为JSON字符串。
  
4. 
  客户端(端口:3000)使用Ajax通过HTTP POST请求
  (URL:HTTP://localhost:5000/segmentation)
   向服务器端发送JSON数据，以便能够为用户获得预测结果并可视化。
   
5. 
   在服务器端，新创建的REST API接收JSON数据并执行以下操作:
    1. 首先，使用整形生成并构建16位图像。然后，16位图像被转换为8位，并在[0，255]之间重新缩放。
 
    2. 转换后的图像大小调整为U-Net网络的输入大小(256x256)。
    
    3. 预先训练的U-Net深度学习模型照常运行(即我们如何测试它的预测)，
    	(1)测试生成器为keras模型生成图像，
    	(2)加载网络和模型权重，
    	(3)预测生成器给出分割概率图)。
       之后，一个API清除一个keras模型的会话。
       重要的是清除一个会话，否则在下一次预测中，它可能会由于keras张量而给出一个错误。
    4.然后，对图像的分割概率图应用阈值(0.5)。
      如果在一个分割的轮廓内有小的黑洞，就用闭合的形态变换去除。
      分割后的图像被重新调整到原始尺寸，分割后的轮廓点的坐标使用openCV查找轮廓功能生成
      
6. 
  用户需要在客户端(网络应用程序)可视化分割图像的轮廓点。最后，API将轮廓点返回给OHIF查看器进行可视化。

```

### 关于（技术）

|概述| 做这个项目能体验一把前端的发展史 |
| ---- | ---- |
| html,css,js,python,ci,cd,git | npm(node),webpack,gulp,react,svg,proptypes,classnames,lerna,vtk.js,cornerstone.js |
| **js** |      |
| react | https://reactjs.org/ |
| classnames | http://jedwatson.github.io/classnames/ |
| react生命周期 | [react的生命周期](https://www.jianshu.com/p/b331d0e4b398) <br>[详解React生命周期(包括react16最新版)](https://www.jianshu.com/p/514fe21b9914)<br>[流程图](https://upload-images.jianshu.io/upload_images/5287253-80e1623c694bcf36.png)<br> |
|  |  |

