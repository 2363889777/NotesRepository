# 登录token

参考：

1.解决定义在main.js中axios拦截器刷新实现bug：https://blog.csdn.net/lgyt240054/article/details/104413207

2.解决跨域请求自己添加请求头后端接收不到的bug:https://segmentfault.com/q/1010000012364132

## 一、业务需求

通过token实现用户登录状态监控，如果超过规定时间，则让用户重新进行登录

## 二、业务流程

![token的业务流程](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200525232013.png)

## 三、功能实现

### （一）需要了解的知识点

#### 1.前端如何自定义请求头

​	通过axios拦截器实现每个请求在请求头中携带token

##### 注意点：

​	1.如果直接在main.js中配置axios拦截器，会出现如下bug(刷新（F5）axios拦截器失效)

```js
import axios from "axios";
import router from "../router";

export const request = (config) => {
    return axios(config);
};


// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    // 判断是否存在token,如果存在将每个页面header添加token
    if (window.sessionStorage.getItem('accountToken')) {
        config.headers.common['token'] = window.sessionStorage.getItem('accountToken')
    }
    return config
}, function (error) {
    router.push('/login')
    return Promise.reject(error)
})
// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response
}, function (error) {
    // 对响应错误做点什么
    if (error.response) {
        switch (error.response.status) {
            case 401:
                router.push('/login');
        }
    }
    return Promise.reject(error)
})
```

![main.js调用axios拦截](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200525234647.png)

#### 2.后端如何处理跨域带自定义请求的请求头

![spring-mvc配置token拦截](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200525235629.png)

```java
package com.xuetang9.controller.sn.interceptor;

import com.xuetang9.BO.RedisDO;
import com.xuetang9.utils.LoggerUtils;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Date;

/**
 * @类: LoginInterceptor
 * @描述: 用户登录权限拦截器
 * @date: 2020/5/25
 * @author: Admin
 * @ver 1.0.0
 * @since JDK 1.8
 */
@CrossOrigin
public class LoginInterceptor implements HandlerInterceptor {
    /**
     * 另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），
     *  浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），
     *  从而获知服务端是否允许该跨域请求。服务器确认允许之后，才发起实际的 HTTP 请求。
     */
    private static final String OPTIONS = "OPTIONS";

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        LoggerUtils.debug("进行拦截。。。。");

        if (OPTIONS.equals(request.getMethod())){
            //跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站有权限访问哪些资源。
            // 另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），
            // 浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），
            // 从而获知服务端是否允许该跨域请求。服务器确认允许之后，才发起实际的 HTTP 请求。
            // 在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。
            // 参考：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS
            response.setStatus(HttpServletResponse.SC_OK);
            return true;
        }

        //打印请求地址
        LoggerUtils.info("请求地址为：{}",request.getRequestURI());

        //获取token
        String token = request.getHeader("token");

        //根据获取token
        if (token == null || token.isEmpty()) {
            //不存在，设置错误状态码
            response.setStatus(401);
        } else {
            //存在
            //判断是否过期
            if (RedisDO.isUserExpired(token)) {
                //过期，设置错误状态码
                response.setStatus(401);
            } else {
                //更新token时间
                RedisDO.recordUser(token,new Date());
                return true;
            }
        }
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```

