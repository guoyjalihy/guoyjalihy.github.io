---
layout: post
title:  "curl模拟POST请求.md"
date:   2019-5-21 11:11:01 +0800
categories: linux
---

###POST请求
```cmd
curl -g http://[2409:8028:5a01:202b::2000:43a]:80/performConfig/upload/performStats -X POST -H "Content-Type:application/json" -d '{["performIndexCode":"FSCEPHA01","performSpaceGra":"FSCEPFunction","statsTypeEnum":"TYPE_FIVE_MINUTE","performIndexNameCn":"发送文本消息请求数","performIndexNameEng":"SendTxtReq","statResult":"0.0","statRatio":"100.0","statTime":"1552531500000","createTime":"2019-03-14T10:50:00.003","updateTime":"2019-03-14T10:50:00.003"]}'
```
