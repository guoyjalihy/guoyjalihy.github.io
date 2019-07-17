---
layout: post
title:  "Springboot+Thymeleaf"
date:   2019-04-24 9:31:01 +0800
categories: springboot
---

#### pom.xml增加依赖
```xml
<!--主依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
<!--正式开发一般都会用到layout,引用下面的jar包，具体layout可以参考git@github.com:guoyjalihy/portal.git-->
<dependency>
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>
</dependency>
```

####  Controller
```java
@Controller
public class LoginController {

    private final Logger logger = LoggerFactory.getLogger(this.getClass());

    @Autowired
    private UserService userService;

    /**
     * 登录跳转
     *
     * @param model
     * @return
     */
    @GetMapping("/login")
    public String loginGet(Model model) {
        return "login";
    }

    /**
     * 登录
     *
     * @param userVO
     * @param model
     * @param httpSession
     * @return
     */
    @PostMapping("/login")
    @OperationLog(operationType = OperationType.LOGIN, content = "登入")
    public String loginPost(UserVO userVO, Model model, HttpSession httpSession) {
        UserVO user = userService.findByNameAndPassword(userVO.getUsername(), userVO.getPassword());
        if (user != null) {
            user.setIp(WebUtils.getRequest().getRemoteAddr());
            httpSession.setAttribute("user", user);
            userService.saveOrUpdate(user);
            return "redirect:home";
        } else {
            model.addAttribute("error", "用户名或密码错误，请重新登录！");
            return "login";
        }
    }


    /**
     * 登出
     *
     * @param httpSession
     * @return
     */
    @GetMapping("logout")
    @OperationLog(operationType = OperationType.LOGOUT, content = "注销")
    public String logout(HttpSession httpSession) {
        httpSession.removeAttribute("user");
        httpSession.removeAttribute("menus");
        httpSession.removeAttribute("roles");
        httpSession.removeAttribute("currMenu");
        return "login";
    }
}

```
#### html
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">

<head>

<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />

<title>INSPINIA | Login</title>

<link th:href="@{/css/bootstrap.min.css}" rel="stylesheet" />
<link th:href="@{/font-awesome/css/font-awesome.css}" rel="stylesheet" />

<link th:href="@{/css/animate.css}" rel="stylesheet" />
<link th:href="@{/css/style.css}" rel="stylesheet" />

<!-- Sweet Alert -->
<link th:href="@{/css/plugins/sweetalert/sweetalert.css}" rel="stylesheet"/>
    
</head>

<body class="gray-bg">

	<div class="middle-box text-center loginscreen animated fadeInDown">
		<div>
			<div>
				<h2 class="logo-name">PORTAL</h2>
			</div>
			<h3>Welcome to PORTAL</h3>
			<p>Login in. To see it in action.</p>
			<form class="m-t" role="form" action="login" method="post">
				<div class="form-group">
					<input type="text" class="form-control" placeholder="username" name="username" required="" />
				</div>
				<div class="form-group">
					<input type="password" class="form-control" placeholder="password" name="password" required="" />
				</div>
				<button type="submit" class="btn btn-primary block full-width m-b">Login</button>
				<a th:href="@{register}" href="#"><small>Forgot password?</small></a>
				<p class="text-muted text-center">
					<small>Do not have an account?</small>
				</p>
				<a class="btn btn-sm btn-white btn-block" th:href="@{register}" href="register.html">Create an account</a>
			</form>
			<p class="m-t">
				<small>Inspinia we app framework base on Bootstrap 3 &copy; 2018</small>
			</p>
		</div>
	</div>

	<!-- Mainly scripts -->
	<script th:src="@{/js/jquery-2.1.1.js}"></script>
	<script th:src="@{/js/bootstrap.min.js}"></script>
	<!-- Sweet alert -->
    <script th:src="@{/js/plugins/sweetalert/sweetalert.min.js}"></script>

	
	<script th:inline="javascript">
		var error = [[${error}]];
		$(document).ready(function () {
			if(error!=null){
				 swal({
		         	title : "温馨提示",
		         	text : error
		      	});
			}
		});

	</script>
</body>

</html>

```
