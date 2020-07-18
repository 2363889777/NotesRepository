#SSM框架IDEA的配置

> 用于IDEA搭建SSM框架的配置环境

###1. 设置IDEA的maven项目的架构设置

设置Project Structure，将所缺的Sources，Tests，Resources配置完成，以及web.xml的版本更改为3.1。

###2. 创建ssm框架的package，以及对应的包

创建domain，mapper，service，web

domain：存放实体类

mapper：存放数据库操作的类

service：存放服务层

web：存放springMVC的servlet控制器，controller文件

###3. 在resources文件夹中创建spring以及mybatis文件夹，用于存放对应的文件

		resources：
	
			spring文件夹：用于存放spring所需的配置文件
				
				applicationContext.xml：用于配置spring的配置信息

				springmvc-servlet.xml：用于配置springMVC的配置
	
			mybatis文件夹：用于存放mybatis所需的配置文件

				mapper文件夹：用于存放mybatis的sql配置

				mybatis-config.xml：用于配置mybatis的全局配置
	
			database.properties：用于配置数据库的配置信息
	
			log4j.xml：用于配置日志的配置信息

###4. webapp下的配置

在webapp下搭建缺少的一些环境以及内容

static文件夹：用于存放web工程需要的资源文件

		css文件夹：用于存放页面的css文件

		js文件夹：用于存放页面的js文件

		img文件夹：用于存放页面的img文件

		lib文件夹：用于存放页面的lib文件

WEB-INF文件夹下创建views文件夹：存放页面(文件安全)	


###5. 配置SSM框架所缺少的环境配置

####配置pom.xml文件中缺少的ssm框架依赖的jar包

	<dependencies>
	    <dependency>
	      <groupId>junit</groupId>
	      <artifactId>junit</artifactId>
	      <version>4.12</version>
	      <scope>test</scope>
	    </dependency>
	
	    <!--配置JavaEE的开发jar包-->
	    <dependency>
	      <groupId>javax</groupId>
	      <artifactId>javaee-api</artifactId>
	      <version>8.0</version>
	      <scope>provided</scope>
	    </dependency>
	
	    <!--配置mysql所需的jar包-->
	    <dependency>
	      <groupId>mysql</groupId>
	      <artifactId>mysql-connector-java</artifactId>
	      <version>5.1.48</version>
	    </dependency>
	
	    <!--配置阿里巴巴的德鲁伊数据源-->
	    <dependency>
	      <groupId>com.alibaba</groupId>
	      <artifactId>druid</artifactId>
	      <version>1.1.21</version>
	    </dependency>
	
	    <!--配置JSTL-->
	    <dependency>
	      <groupId>jstl</groupId>
	      <artifactId>jstl</artifactId>
	      <version>1.2</version>
	    </dependency>
	
	    <!--配置mybatis-->
	    <dependency>
	      <groupId>org.mybatis</groupId>
	      <artifactId>mybatis</artifactId>
	      <version>3.4.6</version>
	    </dependency>
	
	    <!--配置spring-->
	    <dependency>
	      <groupId>org.springframework</groupId>
	      <artifactId>spring-context</artifactId>
	      <version>5.2.1.RELEASE</version>
	    </dependency>
	
	    <!--配置springMVC-->
	    <dependency>
	      <groupId>org.springframework</groupId>
	      <artifactId>spring-webmvc</artifactId>
	      <version>5.2.1.RELEASE</version>
	    </dependency>
	
	    <!--配置spring对数据库支持的jar包-->
	    <dependency>
	      <groupId>org.springframework</groupId>
	      <artifactId>spring-jdbc</artifactId>
	      <version>5.2.1.RELEASE</version>
	    </dependency>
	
	    <!--配置spring整合mybatis需要的jar包-->
	    <dependency>
	      <groupId>org.mybatis</groupId>
	      <artifactId>mybatis-spring</artifactId>
	      <version>1.3.2</version>
	    </dependency>
	
	    <!--配置hibernate-->
	    <dependency>
	      <groupId>org.hibernate.validator</groupId>
	      <artifactId>hibernate-validator</artifactId>
	      <version>6.1.0.Final</version>
	    </dependency>
	
	    <!--配置aop注解-->
	    <dependency>
	      <groupId>org.aspectj</groupId>
	      <artifactId>aspectjweaver</artifactId>
	      <version>1.9.5</version>
	    </dependency>
	
	    <!--配置日志-->
	    <dependency>
	      <groupId>org.apache.logging.log4j</groupId>
	      <artifactId>log4j-core</artifactId>
	      <version>2.11.2</version>
	    </dependency>
	
	</dependencies>

####1.配置mybatis-config.xml的内容

	<?xml version='1.0' encoding='UTF-8'?>
	<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd" >
	<configuration>
	
	    <!--全局配置-->
	    <settings>
	        <!--启用缓存-->
	        <setting name="cacheEnabled" value="true"/>
	        <!--启用日志-->
	        <setting name="logImpl" value="LOG4J2"/>
	        <!--启用主键生成策略-->
	        <setting name="useGeneratedKeys" value="true"/>
	        <!--启用下划线命名自动转驼峰命名-->
	        <setting name="mapUnderscoreToCamelCase" value="true"/>
	    </settings>
	
	</configuration>

####2.配置database.properties的数据库配置文件

	jdbc.driver=com.mysql.jdbc.Driver
	jdbc.url=jdbc:msql://localhost:3306/bbs
	jdbc.username=root
	jdbc.password=

####3. 配置log4j.xml的日志配置

	<?xml version="1.0" encoding="UTF-8"?>
	<Configuration status="WARN">
		<!--日志追加器：日志通过什么方式输出-->
		<Appenders>
			<Console name="Console" target="SYSTEM_OUT">
				<PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />
			</Console>
		</Appenders>
		
		<Loggers>
			<Root level="debug">
				<AppenderRef ref="Console" />
			</Root>
			<!-- <logger name="org.springframework.web" level="error" ></logger> -->
		</Loggers>
	</Configuration>

####4.配置applicationContext.xml的spring配置信息

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	       xmlns:context="http://www.springframework.org/schema/context"
	       xmlns:aop="http://www.springframework.org/schema/aop"
	       xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:tx="http://www.alibaba.com/schema/stat"
	       xsi:schemaLocation="http://www.springframework.org/schema/beans
	       http://www.springframework.org/schema/beans/spring-beans.xsd
	       http://www.springframework.org/schema/context
	       http://www.springframework.org/schema/context/spring-context.xsd
	       http://www.springframework.org/schema/aop
	       http://www.springframework.org/schema/aop/spring-aop.xsd
	       http://www.springframework.org/schema/mvc
	       http://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.alibaba.com/schema/stat http://www.alibaba.com/schema/stat.xsd">
	
	
	    <!--配置属性文件的加载-->
	    <context:property-placeholder location="classpath:database.properties"></context:property-placeholder>
	
	    <!--配置数据源-->
	    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
	        <property name="driverClassName" value="${jdbc.driver}"/>
	        <property name="url" value="${jdbc.url}"/>
	        <property name="username" value="${jdbc.username}"/>
	        <property name="password" value="${jdbc.password}"/>
	    </bean>
	
	    <!--配置sqlsessionFactory-->
	    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	        <property name="dataSource" ref="dataSource"/>
	        <property name="configLocation" value="classpath:mybatis/mybatis-config.xml"/>
	        <property name="mapperLocations" value="classpath:mybatis/mapper/*.xml"/>
	    </bean>
	
	    <!--配置mapper扫描注解-->
	    <bean id="scannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
	        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
	        <property name="basePackage" value="com.sugarzhang.spring.mapper"/>
	    </bean>
	
	    <!--配置事务管理器-->
	    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	        <property name="dataSource" ref="dataSource"/>
	    </bean>
	
	    <!--配置注解扫描的包-->
	    <context:component-scan base-package="com.sugarzhang.spring" use-default-filters="true">
	        <!--排除Controller-->
	        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	    </context:component-scan>
	
	    <!--配置事务注解-->
	    <tx:annotation-driven/>
	
	    <!--配置aop-->
	    <aop:aspectj-autoproxy/>
	
	</beans>

####5.配置springmvc-servlet.xml的springMVC的配置信息

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	       xmlns:mvc="http://www.springframework.org/schema/mvc"
	       xmlns:context="http://www.springframework.org/schema/context"
	       xsi:schemaLocation="http://www.springframework.org/schema/beans
	       http://www.springframework.org/schema/beans/spring-beans.xsd
	       http://www.springframework.org/schema/mvc
	       https://www.springframework.org/schema/mvc/spring-mvc.xsd
	       http://www.springframework.org/schema/context
	       https://www.springframework.org/schema/context/spring-context.xsd">
	
	    <!--配置视图解析器-->
	    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	        <property name="prefix" value="/WEB-INF/views/"/>
	        <property name="suffix" value=".jsp"/>
	    </bean>
	
	    <!--配置资源过滤器-->
	    <mvc:default-servlet-handler/>
	
	    <!--配置mvc扫描的包-->
	    <context:component-scan base-package="com.sugarzhang.spring" use-default-filters="false">
	        <!--只扫描controller-->
	        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	    </context:component-scan>

		<!--配置MVC的注解扫描驱动-->
	    <mvc:annotation-driven/>
	
	</beans>

####6.配置web.xml的配置信息

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	         version="3.1">
	    
	    <!--配置springIOC-->
	    <context-param>
	        <param-name>contextConfigLocation</param-name>
	        <param-value>classpath:spring/applicationContext.xml</param-value>
	    </context-param>
	    <listener>
	        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	    </listener>
	    
	    <!--配置springMVC控制器-->
	    <servlet>
	        <servlet-name>SpringMVC</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	        <init-param>
	            <param-name>contextConfigLocation</param-name>
	            <param-value>classpath:spring/springmvc-servlet.xml</param-value>
	        </init-param>
	        <load-on-startup>1</load-on-startup>
	    </servlet>
	    <servlet-mapping>
	        <servlet-name>SpringMVC</servlet-name>
	        <url-pattern>/</url-pattern>
	    </servlet-mapping>
	
	    <!--配置springMVC编码-->
	    <filter>
	        <filter-name>encodingFilter</filter-name>
	        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	        <init-param>
	            <param-name>encoding</param-name>
	            <param-value>UTF-8</param-value>
	        </init-param>
	        <init-param>
	            <param-name>forceEncoding</param-name>
	            <param-value>true</param-value>
	        </init-param>
	    </filter>
	    <filter-mapping>
	        <filter-name>encodingFilter</filter-name>
	        <url-pattern>/*</url-pattern>
	    </filter-mapping>
	    
	</web-app>
