---
layout: post
title:  "SpringBoot获取参数的多种方式."
date:   2019-1-11 18:41:01 +0800
categories: springboot
---

#### 一、@PathVariable

前端：  
```javascript
$.ajax({
    type:"get",
    url:"/json/privilege/list/"+roleId,
    dataType:"json",
    success:function (data) {
    },
    error:function () {
    }
});
```

后端：
```java
//完整版
public class PrivilegeJSONController {
    @RequestMapping(value = "/json/privilege/list/{roleId}")
	@ResponseBody
	public List<MenuVO> list(@PathVariable(value = "roleId", required = false)Long roleId){
		dosomething...
	}
}	
//简写版
public class PrivilegeJSONController {
    @RequestMapping(value = "/json/privilege/list/{roleId}")
	@ResponseBody
	public List<MenuVO> list(@PathVariable Long roleId){
		dosomething...
	}
}	

```

#### 二、@RequestParam
請求URL:http://ip:port/login?token=xxxxxxxx

後端解析
```java
@GetMapping("/remote/login")
    public String remoteLogin(@RequestParam("token") String token) {
        //do something    
    }
```

#### 三、@RequestBody

后端解析
```java
@PostMapping("/config/{type}/put")
    public String put(@PathVariable String type, @RequestBody APIConfig apiConfig) {
        //do something
    }
```