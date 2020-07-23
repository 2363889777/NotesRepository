# jenkins

## 构建项目

1.点击构建项目

![image-20200722170231471](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722170238.png)

### 构建git项目

#### 成功环境

1.pom在根目录下

![image-20200722173659104](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722173659.png)

#### 步骤

##### 1.填写gitURL

![image-20200722170553200](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722170553.png)

![image-20200722170518240](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722170518.png)

##### 2.填写登录信息

![image-20200722170719903](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722170719.png)

###### 2.1

![image-20200722170919945](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722170920.png)

![image-20200722170829322](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722170829.png)

##### 3.配置触发器

通过什么来触发jenkins的构建

![image-20200722170951116](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722170951.png)

###### 补充

要在git中配置Webhooks

![image-20200722172525921](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722172526.png)



![image-20200722172641079](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722172641.png)

**获取url**

1. jenkins获取

![image-20200722173222909](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722173222.png)

![image-20200722173143247](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722173143.png)

2. 自己拼接

   只有ip和端口需要根据自身情况修改

   ip：主机ip 端口：tomcat端口

##### 4.构建环境

![image-20200722171051821](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722171051.png)

##### 5.构建配置

###### 构建配置-maven

![image-20200722171312431](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722171312.png)

![image-20200722171621602](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722171621.png)

###### 构建配置-shell脚本

![image-20200722171807258](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722171807.png)

~~~shell
echo "github was update"
#删除Tomcat目录下的war包和文件
rm -rf /usr/local/tomcat/apache-tomcat-8.5.51/webapps/jenkinsStrudy /usr/local/tomcat/apache-tomcat-8.5.51/webapps/jenkinsStrudy.war
#休眠5秒，等待一下
sleep 5
#将maven生成的war包移动到Tomcat下
mv /root/.jenkins/workspace/jenkinsTest/target/jenkinsStrudy.war /usr/local/tomcat/apache-tomcat-8.5.51/webapps
~~~

一般jenkins拉取的项目在`/root/.jenkins/workspace`下

##### 6.结束

点击保存即可

![image-20200722172144847](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722172145.png)

![image-20200722173527596](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200722173527.png)