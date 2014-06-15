---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: push
layout: docs
---
## Asset

Asset在API Service中主要是用来存储二进制文件的, 例如照片, 视频和音频等, 但是asset并不只限于二进制文件, 也可以用来模拟一个文件系统.

### 创建目录(folder)

例如, 你有一些照片想要存储进API Service当中, 为了方便管理, 还想把这些照片放到一个目录当中, 那么, 需要现在API Service中创建一个folder

    POST /:org/:app/folders {request body}
    
    例如 post /folders {"name":"照片","owner":"2bbc8b8a-b22f-11e2-ac3f-c97fdfc6de31","path":"/images"}
    
其中, request body中有一些必须的属性:

* name 此目录的名称 
* owner 此目录的所有者 -- User UUID
* path 此目录的路径, 绝对路径


POST的返回结果中会包含此folder的UUID (35618fba-b8af-11e2-ba5e-9176b8e27aa2), 这个在以后会用到的.


### 创建文件(asset)
接下来就该为每个照片来创建一个asset了, 例如, 有一个照片名称叫做 _my_image.jpg_, 可以通过这样的方法来创建一个asset

    POST /:org/:app/assets {request body}
    例如 post /assets {"name":"照片1.jpg","owner":"2bbc8b8a-b22f-11e2-ac3f-c97fdfc6de31","path":"/images/a1.jpg"}
    
其中, request body需要的一些必须的属性包括:

* name 此目录的名称 
* owner 此目录的所有者 -- User UUID
* path 此目录的路径, 绝对路径

还有一个可选的属性, 如果这个属性没有设置的话,那么API Service会去监测上传上来的文件来判断此文件的类型.

* content-type 存储的文件的类型, 例如 "images/jpeg"

这个POST同样会返回一个UUID (64c51a5a-b8b0-11e2-bc5d-99b2f605e77f)用来唯一标识此Asset.

### 链接目录和文件

接下来就是把这个文件(asset)添加到之前创建好的目录中了

    POST /:org/:app/folders/:folder_uuid/assets/:asset_uuid
    例如 post /folders/35618fba-b8af-11e2-ba5e-9176b8e27aa2/assets/64c51a5a-b8b0-11e2-bc5d-99b2f605e77f
    
    返回结果如下, 可以看到metadata.path给出了此asset的路径
    注意, 也可以直接访问asset而不使用folder
    
    {
      "action": "post",
      "application": "09621090-affb-11e2-ac08-773f1211b6de",
      "params": {},
      "path": "/folders/35618fba-b8af-11e2-ba5e-9176b8e27aa2/assets",
      "uri": "http://localhost:8080/test-organization/test1-app/folders/35618fba-b8af-11e2-ba5e-9176b8e27aa2/assets",
      "entities": [
        {
          "uuid": "64c51a5a-b8b0-11e2-bc5d-99b2f605e77f",
          "type": "asset",
          "name": "照片1.jpg",
          "created": 1368107851893,
          "modified": 1368107851893,
          "owner": "2bbc8b8a-b22f-11e2-ac3f-c97fdfc6de31",
          "path": "/images/a1.jpg",
          "metadata": {
            "path": "/folders/35618fba-b8af-11e2-ba5e-9176b8e27aa2/assets/64c51a5a-b8b0-11e2-bc5d-99b2f605e77f"
          }
        }
      ],
      "timestamp": 1368108005446,
      "duration": 101,
      "organization": "test-organization",
      "applicationName": "test1-app"
    }
        
### 获取目录或文件

获取目录

    GET /:org/:app/folders/:folder_uuid
    
获取文件

    GEt /:org/:app/assets/:assets_uuid
    
    
注意, 这里是获取的目录或者文件的元信息, 例如

    /folders/35618fba-b8af-11e2-ba5e-9176b8e27aa2
     
    {
      "action": "get",
      "application": "09621090-affb-11e2-ac08-773f1211b6de",
      "params": {
        "_": [
          "1368108289401"
        ]
      },
      "path": "/folders",
      "uri": "http://localhost:8080/test-organization/test1-app/folders",
      "entities": [
        {
          "uuid": "35618fba-b8af-11e2-ba5e-9176b8e27aa2",
          "type": "folder",
          "name": "照片",
          "created": 1368107342891,
          "modified": 1368107342891,
          "owner": "2bbc8b8a-b22f-11e2-ac3f-c97fdfc6de31",
          "path": "/images2",
          "metadata": {
            "path": "/folders/35618fba-b8af-11e2-ba5e-9176b8e27aa2",
            "connections": {
              "assets": "/folders/35618fba-b8af-11e2-ba5e-9176b8e27aa2/assets"
            }
          }
        }
      ],
      "timestamp": 1368108290171,
      "duration": 16,
      "organization": "test-organization",
      "applicationName": "test1-app"
    }     

### 上传实际的文件内容到Asset

注意, 文件的实际内容是和Asset分开存放的

上传文件是通过往asset的data POST数据的方式

    POST /:org/:app/assets/:assets_uuid/data {binary-data}
    
其中, binary-data是要上传文件的二进制内容, 使用cURL的方法的话, 可以这样来做

    POST  --data-binary "@src/resources/my-image.jpg" -H
    "Content-Type: application/octet-stream"
    ‘http://api.easemob.com/my-org/my-app/assets/9501cda1-2d21-11e2-b4c6-02e81ac5a17b/data’    
          
          
### 下载文件内容

    GET /:org/:app/assets/:asset_uuid/data
    
修改的话, 则可以使用PUT方法              
