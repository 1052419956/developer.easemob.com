---
title: 易用户
description: EM用户体系
category: emuser
layout: docs
---

 
#  组织机构管理 #

## 组织机构的数据结构

      { 
          "username" : "这个是用户的primarykey. 必须是英文字母数字下划线横线的组合，规则如下："^[a-z0-9_-]{3,15}$",
          "nick" : "用户昵称，可以为中文。",
          "avator" : "头像在图片服务器上的路径",
          "signature" : "个性签名",
          "sex" : "性别. F|M|U.不区分大小写",
          "community":{            //用户所属的社区。为一个数组。一个用户可以属于多个社区
          	"communityName":"863520001",
	  	"communityNick":"天泰城",
	        "building":"2"            //用户所属的社区的楼号。   
           }
          "mobile" : "绑定的手机号"
      }

## 1. 创建部门 ##



## 2.2 删除部门  ##

## 2.3 修改部门  ##     

## 2.3 获取完整的组织机构树  ##

## 2.4 修改员工部门属性  ##

## 按部门推送 ##