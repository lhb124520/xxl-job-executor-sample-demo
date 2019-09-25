# 概述

XXL-JOB是一个轻量级分布式任务调度平台，其核心设计目标是开发迅速、学习简单、轻量级、易扩展。现已开放源代码并接入多家公司线上产品线，开箱即用。

## XXL-JOB官网：

[http://www.xuxueli.com/xxl-job/#/](http://www.xuxueli.com/xxl-job/#/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171621989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzI4NzUwOA==,size_16,color_FFFFFF,t_70)

## 快速入门
[https://www.cnblogs.com/xuxueli/p/5021979.html](https://www.cnblogs.com/xuxueli/p/5021979.html)

这些概念和入门可以很好的在网上找到资料，就不在这里一一阐述了。接下来重点讲下开发分布式任务调度执行器实例。前提是任务调度中心已经搭建好。

## 分布式任务执行器实例
项目结构如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919172258897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzI4NzUwOA==,size_16,color_FFFFFF,t_70)
**1、引入依赖**

           <dependency>
                <groupId>com.xuxueli</groupId>
                <artifactId>xxl-job-core</artifactId>
                <version>2.1.0</version>
            </dependency>
2、修改application.properties配置

    # web port
    server.port=8082
    
    # log config
    logging.config=classpath:logback.xml
    
    
    ### xxl-job admin address list, such as "http://address" or "http://address01,http://address02"
    xxl.job.admin.addresses=http://127.0.0.1:8080/xxl-job-admin
    
    ### xxl-job executor address
    xxl.job.executor.appname=xxl-job-executor-demo
    xxl.job.executor.ip=
    xxl.job.executor.port=8888
    
    ### xxl-job, access token
    xxl.job.accessToken=
    
    ### xxl-job log path
    xxl.job.executor.logpath=/data/applogs/xxl-job/jobhandler
    ### xxl-job log retention days
    xxl.job.executor.logretentiondays=-1

3、开发DemoJobHandler类

    @JobHandler(value="demoJobHandler")
    @Component
    public class DemoJobHandler extends IJobHandler {
    
    	@Override
    	public ReturnT<String> execute(String param) throws Exception {
    		XxlJobLogger.log("XXL-JOB, Hello World.Demo");
    
    		for (int i = 0; i < 5; i++) {
    			XxlJobLogger.log("beat at:" + i);
    			TimeUnit.SECONDS.sleep(2);
    		}
    		return SUCCESS;
    	}
    
    }

4、运行项目
执行器会自动注册到任务调度中心，并维持心跳。如果任务调度中心部署在本地，访问：
[http://localhost:8080/xxl-job-admin](http://localhost:8080/xxl-job-admin)

> 注意：任务调度中心必须有该项目的执行器名称，执行器项目才能自动注册到任务调度中心。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919173251987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzI4NzUwOA==,size_16,color_FFFFFF,t_70)
## 源码
GitHub：[https://github.com/lhb124520/xxl-job-executor-sample-demo](https://github.com/lhb124520/xxl-job-executor-sample-demo)

码云：[https://gitee.com/lhblearn/xxl-job-executor-sample-demo](https://gitee.com/lhblearn/xxl-job-executor-sample-demo)


