---
layout: post
title: Gradle - The Best Automation Build Tool of 21 Century
date: 2015-06-28 18:58:17
tags:
- Gradle
categories: 
- Java
- Gradle
---


# 声明依赖
```groovy
apply plugin: 'java'

repositories {
    mavenCentral()       // 使用maven中央仓库
    jcenter()            // 使用jcenter仓库
    maven { url "http://100.50.36.60:8087/nexus/content/groups/public/" }
}

dependencies {
    compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'
    testCompile group: 'junit', name: 'junit', version: '4.+'
    compile 'org.hibernate:hibernate-core:3.6.7.Final'
    compile('org.hibernate:hibernate-core:3.6.7.Final')
}
```






# Gradle发布构件
```gradle
apply plugin: 'java'
apply plugin: 'maven'
group='com.zqf'
version='1.0.0'
repositories {
	//依赖maven远程仓库
	maven { url "http://nexus.zhaiqianfeng.com:8081/nexus/content/groups/public" }
}
dependencies {
    testCompile 'junit:junit:4.12'
}
//发布构件
uploadArchives{
	repositories{
		mavenDeployer{
			//发布到maven本地文件
			repository(url:"file://localhost/tmp/maven-rpo/")
			
			//发布到maven远程仓库
			repository(url: "http://nexus.zhaiqianfeng.com:8081/nexus/content/repositories/thirdparty/") {
			    authentication(userName: "admin", password: "admin123")
			}
		}	
		
		//发布到ivy本地文件
		ivy{
			url "/tmp/ivy-rpo/"
		}
		
		//发布到maven本地仓库
		mavenLocal()
		//更多的仓库.....
	}
}

```

