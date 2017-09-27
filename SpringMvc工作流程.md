---
title: SpringMvc工作流程
date: 2017-09-12 18:14:53
tags:
---

## SpringMVC核心流程,主要在doDispatch方法中

*  1、用户发送请求，被SpringMvc的DispatcherServlet捕获到
*  2、DispatcherServlet对获取到的URL进行解析，得到统一定位符URI，然后根据URI调用HandlerMapping获得Handler对象以及跟handler相关的拦截器，最后以HandlerExecutionChain对象形式返回。
*  3、DispatcherServlet根据返回的Handler选择合适的HandlerAdapter，如果找到了HandlerAdapter,在之前就会执行preHandler方法
*  4、提取request中的模型数据，填充Handler入参，开始执行Handler
    * 在填充的过程中，SpringMvc会做一些操作：
        * HttpMessageConvert ： 将消息转换成一个对象，将对象转换成指定的响应信息。
        * 数据转换： 比如将String转换成Integer等
        * 数据格式化： 日期格式化等
        * 数据验证： 验证数据的有效性，包括长度、类型等等，验证结果存储到BindingResult或者Error中。
*  5、Handler执行完成后，像DispatcherServlet返回一个ModelAndView。
*  6、根据返回的ModelAndView选择一个合适的ViewResolver返回给DispatcherServlet
*  7、ViewResolveModel和View来渲染视图
*  8、将渲染结果返回给客户端

## 处理关键
* DispatcherServlet的HandlerMapping集合中根据请求的URL匹配每一个handlerMapping对象中的某个handler,匹配成功后将会返回这个handler的处理连接handlerExecutorChain对象，而这个对象中包括多个handlerInterceptor。
* HandlerInterceptor中包含三个方法
    * preHandler，在Handler执行之前调用。
    * postHandler，在Handler执行之后调用。
    * afterComplete，在view渲染完成后，dispatchServlet返回之前调用。
* 当preHandler返回false时，请求将在执行afterComplete后直接返回，不会执行handler。    

## 首先有几个类需要声明,ModelAndView、HandlerExecutionChain、HandlerMapping、HandlerMethod、HandlerAdapter。

* 1、HandlerMethod(org.springframework.web.method.HandlerMethod)，这个类为中存放了某个bean对象和该bean对象的某个要处理的Method对象。
* 2、HandlerMapping,作用为通过request对象，获取对应的HandlerMethod对象。
* 3、HandlerExecutionChain作用为通过加入Interceptor拦截器包装HandlerMapping返回的HandlerMethod对象。让待处理的方法对象与拦截器称为一个整体,即执行链。
* 4、ModelAndView,顾名思义。Model即MVC的M，View即MVC的V。其对象存放的正是数据与视图信息。
* 5、HandlerAdapter:作用为具体处理HandlerMethod,即通过它调用某个方法。
