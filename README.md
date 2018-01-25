middlewarelibrary是一个公共模块，插件和宿主都可以进行依赖，但这里 Atlas 提供了一种特殊的依赖方式—providedCompile，插件 bundle 中使用这种依赖公共模块将不会把依赖库打入宿主 apk 和插件 so 中，只是用来帮助你在编译的时候使用依赖代码。

firstbundle和secondbundle都是插件 bundle，他们在打包时候，会以 SO 的形式在 apk 的 libs 目录下，这里的secondbundlelibrary是 secondbundle 单独依赖的公共包，这里它会使用 compile project 而不是 providedCompile。

remotebundle是远程 bundle，它不会打入到 apk 中，不占用 apk 体积，比如某些计算器小工具，当用户点击的时候，才开始进行下载插件，下载完毕后才能使用功能，这是 atlas 提供的远程插件的功能，需要在宿主 gradle 中进行特殊配置，指定插件为 remotebundle。这里的思路也可以作为上面的动态部署 so 替换思路。


单bundle调试：
在相应的bundle工程目录下执行： ../gradlew clean assemblePatchDebug



aar是一个类似于jar的文件格式。但是他们之间是有区别的。
jar：仅仅包含class和清单文件，没有资源文件。
aar：包含了class文件和资源文件。说白了就是Android的专属“jar”

awb： android wireless bundle的缩写，实际上同AAR类似，是最终构建整包前的中间产物。每个awb最终会打成一个bundle。awb与aar的唯一不同之处是awb与之对应有个packageId的定义。


host: 宿主的概念，所有的bundle可以直接调用host内的代码和资源


官方demo中文件
     module名 意义
     activitygroupcompat demo中的工具类
     app 客户端代码
     databindbundle 使用Google bind框架demo
     firstbundle 第一个业务bundle代码
     lottie splashScreen依赖的代码
     middlewarelibrary 中间键library，会打包到主dex中
     publicbundle 共bundle代码，不会打入主dex中
     remotebundle 远程bundle，在发布时不会编译进apk，而在客户端使用时，先下载后加载
     secondbundle 第二个业务bundle代码
     secondbundlelibrary 第二个业务单独依赖的代码
     splashScreen 闪屏代码


退出adb shell :   exit
aidl ：android内部进程通信接口的描述语言，通过它我们可以定义进程间的通信接口

Android打包基础
1.处理资源文件 
2.处理aidl文件  
3.编译java文件 
4.class2dex 
5.apk打包 
6.签名 
7.zipalign对齐 

发布基线版本：
在app目录下执行发布命令：gradlew publish
我们可以在目录中看到～/.m2/repository/com/taobao/android/atlasdemo/AP-debug/1.0.0/AP-debug-1.0.0.ap 文件
设置版本：app 的build.gradle中可以看到group、version、artifactId这三个字段，标示了我们发布的路径、名称、版本

常用命令(都需要到对应包下)
构建debug命令： ../gradlew assembleDebug
发布基线版本命令：../gradlew publish
单模块部署命令：../gradlew assemblePatchDebug



动态部署根据 .ap 文件生成差量内容 

DexPatch是以动态部署技术方案为基础，以快速解决线上故障为唯一目的的动态化方案。 

简单来说，动态部署是针对apk级别的动态升级，DexPatch是针对Bundle级别的动态修复(主dex可以认为是一个Bundle)



学习链接：
      

     系列介绍：http://blog.csdn.net/wdd1324/article/details/76855408

      gradle完全指南：https://www.jianshu.com/p/9df3c3b6067a
      
      atlas视频讲解：https://edu.aliyun.com/course/68/lesson/list?utm_campaign=aliyunedu&utm_medium=images&utm_source=qq&utm_content=m_24086



