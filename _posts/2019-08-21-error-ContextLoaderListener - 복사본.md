---
title: "Eclipse, STS 에서 톰캣 실행 시 org.apache.catalina.core.StandardContext listenerStart"
categories: error
date: 2019-08-21 19:37:00
lastmod: 2019-08-22 10:40:00
classes: wide
---

이클립스나 STS에서 톰캣 서버 실행 시 아래와 같은 예외가 발생한다면,

### <case 1>

```
2019. 8. 2 오전 11:18:51 org.apache.catalina.core.StandardContext listenerStart
심각: Error configuring application listener of class org.springframework.web.context.ContextLoaderListener
java.lang.ClassNotFoundException: org.springframework.web.context.ContextLoaderListener
at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1688)
at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1533)
at org.apache.catalina.core.DefaultInstanceManager.loadClass(DefaultInstanceManager.java:525)
at org.apache.catalina.core.DefaultInstanceManager.loadClassMaybePrivileged(DefaultInstanceManager.java:507)
at org.apache.catalina.core.DefaultInstanceManager.newInstance(DefaultInstanceManager.java:124)
at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4701)
at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5260)
at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1525)
at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1515)
at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:303)
at java.util.concurrent.FutureTask.run(FutureTask.java:138)
at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:886)
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:908)
at java.lang.Thread.run(Thread.java:662)
```

### <case 2>

```
8월 10, 2019 1:54:15 오후 org.apache.catalina.core.StandardContext listenerStart
SEVERE: Error configuring application listener of class org.springframework.web.util.Log4jConfigListener
java.lang.ClassNotFoundException: org.springframework.web.util.Log4jConfigListener
at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1713)
at org.apache.catalina.loader.WebappClassLoader.loadClass(WebappClassLoader.java:1558)
at org.apache.catalina.core.DefaultInstanceManager.loadClass(DefaultInstanceManager.java:527)
at org.apache.catalina.core.DefaultInstanceManager.loadClassMaybePrivileged(DefaultInstanceManager.java:509)
at org.apache.catalina.core.DefaultInstanceManager.newInstance(DefaultInstanceManager.java:137)
at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4823)
at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5381)
at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1559)
at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1549)
at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:334)
at java.util.concurrent.FutureTask.run(FutureTask.java:166)
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
at java.lang.Thread.run(Thread.java:722)
```

두가지 경우 모두 maven dependency를 등록해주면 된다.

가끔 maven update를 하면서 사라지는 경우가 있다고 한다.

 

해당 프로젝트의 properties >> Deployment Assembly를 선택한 후,

"Add" >> Java Build Path Entries >> Maven Dependencies 선택한 후, "Apply"를 해준다.

 

이후 톰캣 서버를 재시작 하면 정상적으로 작동한다.

