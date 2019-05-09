---
title: idea搭建osgi项目开发学习
tags:
  - osgi
categories:
  - osgi
abbrlink: 1364452984
date: 2019-05-09 09:39:44
---

本文介绍了用Idea搭建OSGI项目开发的过程，演示使用的JDK8，Equinor OSGI Framework。 
<!-- more -->
###OSGI简介
OSGI的全称是Open Service Gateway Initiative，直译就是开放服务网关。最新的OSGI定义是The Dynamic Module System for Java，即面向java的动态模块化系统。
在传统Web开发中，我们为了进行功能的分离，经常会进行模块划分，比如基础信息模块交由A和B做，接口信息模块交由C和D做。最终，再汇集到一起，组成一个完整的项目。在整个流程中，我们做到的只是逻辑上的解耦，最终这些模块还是运行于同一服务器上，共享同一个classpath。这时就会出现一个局限性问题，比如现在接口规范改了，只想停掉接口信息模块，而基础信息模块仍能正常运行，这显然是无法实现的。而使用OSGI可以完美解决这个问题，OSGI是基于模块（Bundle）驱动的，每个模块都有属于自己的classpath和类加载器，模块之间通过服务注册和发现进行关联，每个模块有着自己独立的生命周期，我们可以动态地对模块进行加载、卸载、更新。摘自https://www.jianshu.com/p/11dcea36b957。
OSGI可以理解成是JVM单进程内的SOA，当然也支持多进程分布式的模块之间的调用。

### Equinor下载
下载地址：https://download.eclipse.org/equinox/
本文下载的是equinox-SDK-4.11.zip，下载后进行解压，后面需要用到这个解压目录。
![](https://upload-images.jianshu.io/upload_images/2113235-43e97794f34784e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### Idea创建OSGI工程
File -> New -> Project，选择Java，点击Next，创建一个空工程。
![](https://upload-images.jianshu.io/upload_images/2113235-b94c63722b02e2e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
继续点击Next。
![](https://upload-images.jianshu.io/upload_images/2113235-bb30148d93e1a871.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
填写项目名称，这里叫osgi_demo。
![](https://upload-images.jianshu.io/upload_images/2113235-8041cf63191bdc4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
分别创建api、server、client三个OSGI模块。
创建模块时勾选OSGI作为开发环境，Use library从刚才下载的Equinox解压的目录下的plugins目录中选择org.eclipse.osgi_3.13.300.v20190218-1622.jar。
![](https://upload-images.jianshu.io/upload_images/2113235-a7389ef8bfdc5ae9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建模块完成之后，打开idea的preferences，在Languages & Frameworks找到OSGI Framework Instances选项。
![](https://upload-images.jianshu.io/upload_images/2113235-0bcd32ec1db6364e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加Equinox，Home directory选择刚才解压的Equinox目录。
![](https://upload-images.jianshu.io/upload_images/2113235-9a550becba7fd80a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 编写演示代码
结构如下图
![](https://upload-images.jianshu.io/upload_images/2113235-c17d06d2826cab33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

api模块中定义接口类IHelloService
```
package osgi.demo.api;

public interface IHelloService {
    /**
     * 和某人打招呼
     * @param somebody
     * @return
     */
    String sayHello(String somebody);
}

```
server模块接口实现类HelloServiceImpl

```
package osgi.demo.server;

import osgi.demo.api.IHelloService;

public class HelloServiceImpl implements IHelloService {
    @Override
    public String sayHello(String somebody) {
        return "hello " + somebody;
    }
}
```
server模块服务注册类HelloServerBundle
```
package osgi.demo.server;

import org.osgi.framework.BundleActivator;
import org.osgi.framework.BundleContext;
import osgi.demo.api.IHelloService;

import java.util.Dictionary;
import java.util.Hashtable;

public class HelloServerBundle implements BundleActivator {
    @Override
    public void start(BundleContext bundleContext) throws Exception {
        IHelloService service = new HelloServiceImpl();
        Dictionary<String , Object> properties = new Hashtable<>();
        //服务注册
        bundleContext.registerService(IHelloService.class, service, properties);
    }

    @Override
    public void stop(BundleContext bundleContext) throws Exception {
    }
}
```
client模块调用服务类HelloClientBundle
```
package osgi.demo.client;

import org.osgi.framework.BundleActivator;
import org.osgi.framework.BundleContext;
import org.osgi.framework.ServiceReference;
import osgi.demo.api.IHelloService;

import java.util.Objects;

public class HelloClientBundle implements BundleActivator {
    @Override
    public void start(BundleContext bundleContext) throws Exception {
        //获取到IHelloService服务引用
        ServiceReference<IHelloService> reference = bundleContext.getServiceReference(IHelloService.class);
        if (Objects.nonNull(reference)) {
            //发现服务
            IHelloService service = bundleContext.getService(reference);
            if (Objects.nonNull(service)) {
                System.out.println(service.sayHello("jecyhw"));
            }
            //注销服务
            bundleContext.ungetService(reference);
        }
    }

    @Override
    public void stop(BundleContext bundleContext) throws Exception {

    }
}
```
### 各模块OGSI配置
api模块配置，导出接口定义所在包osgi.demo.api（Additional properties是bundle的相关属性配置的地方）。
![](https://upload-images.jianshu.io/upload_images/2113235-d856e844e1d643bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

server模块配置，配置HelloServerBundle类作为该bundle的启动类。
![](https://upload-images.jianshu.io/upload_images/2113235-b169fd64ecdc2296.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

client模块配置，配置HelloClientBundle类作为该bundle的启动类。
![](https://upload-images.jianshu.io/upload_images/2113235-738a8792facef2e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### osgi启动配置并运行
选择Edit Configurations。
![](https://upload-images.jianshu.io/upload_images/2113235-291126f2deb669b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加OSGI Bundles。
![](https://upload-images.jianshu.io/upload_images/2113235-47ae7baffd39f90a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

配置如下。
![](https://upload-images.jianshu.io/upload_images/2113235-fa6d9dfc3bdcf2c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> client模块调用了server的服务，按照依赖关系，server模块需要先启动，把服务注册在osgi框架中，client模块才能调用到，Start level是用来定义bundle模块的启动优先级，值越小，启动优先级越高。


> Framework start level是整个osgi框架的启动级别，也就是整个项目的启动级别，大于这个值的bundle模块是不会被启动的。如果这个值为1，client模块的启动级别为2，client模块是不会被启动的，可以调整试试。

点击OK之后，就可以运行了。
![](https://upload-images.jianshu.io/upload_images/2113235-0048aa3657fbb7f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果截图。
![](https://upload-images.jianshu.io/upload_images/2113235-e5e6a863ef10c162.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###参考资料
[Java模块化之路 —— OSGI介绍](https://www.jianshu.com/p/11dcea36b957)
深入理解OSGi:Equinox原理、应用与最佳实践