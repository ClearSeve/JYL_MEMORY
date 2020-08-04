# gradle

## gradle下载
```
https://services.gradle.org/distributions/
下载gradle-xxx-all.zip

对于andorid studio放入
C:\Users\jyl\.gradle\wrapper\dists\gradle-xxx-all\xxxxxxxx目录
删除临时文件，放入gradle-xxx-all.zip
```

## 使用

```
将gradle的bin目录加入环境变量path（目录下包含gradle.bat）

gradle 命令从当前目录下寻找 build.gradle文件执行构建

gradle init   创建gradle项目
[gradle init --dsl kotlin]
```


```
build.gradle定义任务AB

task AB {
    println 'abcde'
}
hello.doFirst {
    println 'aaaa'
}
hello.doLast {
    println 'zzzz'
}

执行任务：
gradle AB
```

```
build.gradle定义任务CD

task CD(dependsOn: 'TX') {
    println 'CD'
}
task TX {
    println 'TX'
}

执行任务：
gradle CD
```


## 修改仓库地址
https://maven.aliyun.com/mvn/guide


修改build.gradle文件中两处repositories 

buildscript {
    repositories {
        maven{url "https://jitpack.io"}
        maven{url 'https://maven.aliyun.com/repository/google' }
        maven{url 'https://maven.aliyun.com/repository/public'}
        maven{url 'https://maven.aliyun.com/repository/gradle-plugin'}
    }
}

allprojects {
    repositories {
        maven{url "https://jitpack.io"}
        maven{url 'https://maven.aliyun.com/repository/google' }
        maven{url 'https://maven.aliyun.com/repository/public'}
        maven{url 'https://maven.aliyun.com/repository/gradle-plugin'}
    }
}