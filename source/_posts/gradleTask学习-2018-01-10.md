---
title: task学习
date: 2018-01-10 23:23:07
tags: [gradle]
categories: 笔记
top: false
---
# java代码写在脚本中
gradle构建脚本本可以写java代码，groove语言支持java代码

<!-- more -->

## java 类定义

 ```
public class VersionEntity {

	private Integer major;
	
	private Integer minor;
	
	private boolean release;

	@Override
	public String toString() {
		return "VersionEntity [major=" + major + ", minor=" + minor + ", release=" + release + "]";
	}
	public VersionEntity(Integer major, Integer minor, boolean release) {
		super();
		this.major = major;
		this.minor = minor;
		this.release = release;
	}
	public VersionEntity() {
		super();
		this.major = 2121;

	}
	
	
}
 ```
 
定义属性文件
ext.f = file("version.properties");

## java 方法定义
 ```
VersionEntity GenerateVersion() throws FileNotFoundException, IOException{
	
	Properties properties = new Properties();
	
	properties.load(new FileInputStream(f));

	return new VersionEntity(Integer.valueOf(properties.get("major").toString()),Integer.valueOf(properties.get("minor").toString()), properties.getProperty("release").toString().equals("false"));

}
 ```

## 调用project的task方法创建继承于defaultTask的task
 ```
task ver {
     /*直接调用java方法*/
     project.version = GenerateVersion()
     version.release = true
	 /*配置代码块中定义 输入 输出，定义的task便能执行增量式构建*/
     inputs.property('release',version.release)
     outputs.file f 
     doLast {
     println  project.version
      ant.propertyfile(file: f){
	       /*修改属性文件中的值*/
	       entry(key: 'release',type: 'string',operation: '=',value: 'true')
	       println 'ver执行'
      }
      println  project.version
    }              
}
 ```



# 自定义task类
自定义task 类  也可以在java代码中定义，不过需要导入相应的包，此处gradle已经提供

 ```
class myDefaultTask extends DefaultTask {

   /* 使用注解标识 输入 输出*/
   @Input String pi
   @OutputDirectory  po
   
   myDefaultTask (){
   
   group = '定义task'
   description = '定义task,封装实现，只提供输入输出'
   }
   
   /*定义动作*/
   @TaskAction 
   void start() {
   
        /*这里定义逻辑*/
        println '输入的是：'+pi
   
   }

}
 ```
 
 
# 测试增量式构建
 ```
task ceShiMyDefaultTask(type: myDefaultTask){         
        /*输入输出不变时，不在执行task*/
        pi = 'hahhh--900'
        po=  file("/lib")

       doLast {println 'ceShiMyDefaultTask'}
 }
 ```
 
# 文件复制，可以参考api中Copy类
## 这种文件复制 类似于 调用方法 
  ```
 task ccc(){
   doLast{
	   copy() {
	   /* from into  都是copy对象的方法*/
	     from "lib" 
	    into "test"
	   }
   }
}
  ```

## 这种文件复制 类似于继承   输入输出定义在父类中   此task执行一次就不会执行了
```

task hhh(type: Copy){
 inputs.property('release',11)
 outputs.dir   file("111")  
  doLast {
  println '-----------------------00000'
  
  }
  from "111" 
  into "2222"
  include "111.png"
}
 ```
 
# task规则 
简单task只需project的task即可以添加，但是task规则只能通过taskContainer容器的addRule(String,Closure)方法
 ```
 tasks.addRule('Pattern: Test<Classifier>My: 测试task规则.'){
     String taskName ->
	 if (taskName.startsWith('Test') && taskName.endsWith('My')){
	    task (taskName) {
	      doLast{
		     String classifier = taskName - 'Test' - 'My'
		     switch(classifier){
		     
		       case '1' : println '任务---1'
		                  break
		       case '2' : println '任务---2'
		                  break
		       default:  println '任务000'
		     }
		   }
	    }
	 }
 
 }
 ```
 
 
# 编写生命周期钩子，类似于监听，事件是任务蓝图创建完成后 
```
 gradle.taskGraph.whenReady {taskGraph ->  
 
    /*包含不一定执行动作，一定执行配置，因为有可能是最新的，*/
    if(taskGraph.hasTask(':addTask')) {  
       println ' 存在的逻辑'
    } else {  
        println ' 不存在的逻辑'
    }  
   
}  
```
 
# 不依赖依赖的task，控制执行顺序
1 finalizedBy就是在task执行完之后要执行的task。

2 mustRunAfter并不会添加依赖，它只是告诉Gradle执行的优先级如果两个task同时存在

# 总结
+ 重点把握对生命周期、执行顺序的认识
- 借助gradle插件库查看源码，进行学习