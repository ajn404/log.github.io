---
title: 毕设
tags:
 - 毕业设计过程
 - 前端架构之路
---
### 关于开发环境搭建

```shell
yarn
npm run dev :默认datasource为DCM4CHEE
```

### 关于部署

```
Orthanc:
客户端
服务端
数据库
```

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

```
