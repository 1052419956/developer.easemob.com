---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: push
layout: docs
---
## Group

### group properties


* title -- the display name of group
* path

    group name, can't be modified after create. API Service use this property to unique identify a group, the path can contains slash, this allows you to create group hierarchies.
    
### create a new group

    POST /:org/:app/groups {"path":"dev", "title":"the developer team"}    
    
