#  SpringMVC拦截器有哪些方法？多个拦截器执行时方法调用的顺序？


## 自定义一个拦截器

定义自己的拦截器实现HandlerInterceptor,进行方法的重写。

![拦截器](http://www.bcoder.top/img/interview/45.png)


在springmvc配置文件中进行配置：

![拦截器](http://www.bcoder.top/img/interview/46.png)

## 拦截器的主要方法：

### preHandle方法

preHandle(request, response, Object handler) ：该方法在处理器方法执行之前执行。其返回值为 boolean，若为 true，则紧接着会执行处理器方法，且会将 afterCompletion()方法放入到一个专门的方法栈中等待执行。

### postHandle方法
postHandle(request, response, Object handler, modelAndView) ：该方法在处理器方法执行之后执行。处理器方法若最终未被执行，则该方法不会执行。由于该方法是在处理器方法执行完后执行，且该方法参数中包含 ModelAndView，所以该方法可以修改处理器方法的处理结果数据，且可以修改跳转方向。

### afterCompletion方法
afterCompletion(request, response, Object handler, Exception ex) ：当 preHandle()方法返回 true 时，会将该方法放到专门的方法栈中，等到对请求进行响应的所有工作完成之后才执行该方法。即该方法是在中央调度器渲染（数据填充）了响应页面之后执行的，此时对 ModelAndView 再操作也对响应无济于事。


## 拦截器执行顺序

### 单拦截器执行顺序

![拦截器](http://www.bcoder.top/img/2017.05.30/138.png)


换一种一表现方式，也可以这样理解：


![拦截器](http://www.bcoder.top/img/2017.05.30/139.png)


### 多拦截器执行顺序


![拦截器](http://www.bcoder.top/img/2017.05.30/148.png)





