---
title: ADMSFGC(我也不知道啥意思)
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

### 角色服务以及关联（简称架构）

{% asset_img structure.png This is an image %}

1. 客户端向Orthanc的Dicom服务器(8042端口)查询（使用WADO协议获取图像信息）

