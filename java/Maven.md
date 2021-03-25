## Maven

### 1.MAVEN_OPTS环境变量

maven也是用java写出来的项目，也是要启动jvm来运行maven代码，进而执行各种操作的。因此maven自身的jvm内存大小也是要关注的，如果在构建特别大的项目时，可能会出现maven自身jvm内存不够，导致构建失败，比如OOM的异常。

设置MAVEN_OPTS环境变量，就是设置maven的jvm参数，可以设置为-Xms128m -Xmx512m。

### 2.maven的配置文件位置

maven有一个重要的配置文件，就是settings.xml，这个文件默认是在%M2_HOME%/conf目录下面的，但是如果你升级maven的版本，那么可能导致新的安装包的settings文件又是一片空白。

所以一般maven的配置文件都会放在当前用户的目录下的~/.m2/settings.xml中，这样就是对当前用户有效。

将maven安装目录里的conf目录下的settings.xml配置文件拷贝到.m2目录里去，作为以后maven全局唯一的配置文件。

### 3.pom.xml

<project>：pom.xml中的顶层元素 。

<modelVersion>：POM本身的版本号，一般很少变化 。

<groupId>：创建这个项目的公司或者组织，一般用公司网站后缀，比如com.company，或者cn.company，或者org.zhonghuashishan 。

<artifactId>：这个项目的唯一标识，一般生成的jar包名称，会是<artifactId>-<version>.<extension>这个格式，比如说myapp-1.0.jar 。

<packaging>：要用的打包类型，比如jar，war，等等。 

<version>：这个项目的版本号 。

<name>：这个项目用于展示的名称，一般在生成文档的时候使用 。

<url>：这是这个项目的文档能下载的站点url，一般用于生成文档 。

<description>：用于项目的描述。

### 4.maven依赖管理的基石性机制：坐标机制

#### maven坐标的介绍

每个maven项目都有一个坐标。

groupId + artifactId + version + packaging + classifier，五个维度的坐标，唯一定位一个依赖包。

任何一个项目，都是用这五个维度唯一定位一个发布包。

后面两个维度较为少用，99%的场景下，唯一定位一个依赖的就是三个维度，groupId + artifactId + version。

几乎所有你需要使用的依赖包的各个版本，都在maven的中央仓库里。 

简单来说，就是在pom.xml里配置你需要的依赖的坐标，然后maven自动从中央仓库下载后缓存到你本地仓库里。

#### 企业级的坐标设置

groupId：不是你的公司或者组织，但是以你的公司或者组织的官网的域名倒序来开头，然后加上项目名称。

artifactId：项目中的某个模块，或者某个服务。

version：这个工程的版本号。

packaging：这个工程的发布包打包方式，一般常用的就jar和war两种，java -cp执行一个jar包，war可以放到一个tomcat容器里去跑的web工程。

classifier：很少用，定义某个工程的附属项目，比如hello-world工程的，hello-world-source工程，就是源码，可能是类似于hello-world-1.0-SNAPSHOT-source.jar这样的东西。

#### 坐标的作用

一个groupId+artifactId+version，就定位了这个项目某个时间点的一个特定版本的代码，也就是一个特定的版本的代码的jar包。

然后，我们在自己的maven项目里，就可以通过<dependency>加上那个依赖的坐标，groupId+artifactId+version，去声明我要用那个依赖的哪个版本的代码的jar包。maven通过这个坐标唯一定位到那个项目的那个版本的代码的jar包。从中央仓库下载下来，给你放到本地仓库里去。

### 5.设置规范的坐标

version：1.0.0-SNAPSHOT版本。

第三位是最小版本，一般如果是修复一些bug，或者作出轻微的改动，会累加第三位小版本；

第二位是小版本，一般如果加入一些新的功能或者模块，或者做了一些重构，会累加第二位小版本；

第一位是大版本，一般就是如果整体架构有特别的升级或者变化，才会累加第一位大版本。

SNAPSHOT，就是当前这个版本下的快照版本，代表代码正在开发或者测试中，可以试用，但是没有经过完善测试，不会承诺非常稳定。 

如果没有SNAPSHOT，那么就是说已经经过完善测试，是可以发布的稳定版本。

### 6.依赖范围

<scope></scope>

maven有三套classpath，classpath，就是项目中用到的各种依赖的类，jvm在运行的时候需要去classpath下面加载对应的类。

maven在编译源代码的时候有一套classpath；在编译测试代码以及执行测试代码的时候，有一套classpath；运行项目的时候，有一套classpath；依赖范围就是用来控制依赖包与这三种classpath的关系的。

简单来说，不同的依赖范围，会导致那个依赖包可能在编译、测试或者打包运行的时候，有时候可以使用，有时候不能够使用。

compile：默认，对编译、测试和运行的classpath都有效。一般都是用这种scope。 

test：仅仅对于运行测试代码的classpath有效，编译或者运行主代码的时候无效，仅仅测试代码需要用的依赖一般都会设置为这个范围，比如junit。一些测试框架，或者只有在测试代码中才会使用的一些依赖，会设置为test，这个的好处在于说，打包的时候这种test scope的依是不会放到最终的发布包里去的。减少发布包的体积。

provided：编译和测试的时候有效，但是在运行的时候无效，因为可能环境已经提供了，比如servlet-api，一般就是这个范围，在运行的时候，servlet容器会提供依赖。servlet-api是用来开发java web项目的，可能你在开发代码和执行单元测试的时候，需要在pom.xml里面声明这个servlet-api的依赖，因为要写代码和测试代码。但是最终打完包之后，放到tomcat容器里面去跑的时候，是不需要将这个servlet-api的依赖包打入发布包中的，因为tomcat容器本身就会给你提供servlet-api的包。

runtime：测试和运行classpath有效，但是编译代码时无效，比如jdbc的驱动实现类，比如mysql驱动。因为写代码的时候是基于javax.sql包下的标准接口去写代码的。然后在测试的时候需要用这个包，在实际运行的时候才需要用这个包的，但是编译的时候只要javax.sql接口就可以了，不需要mysql驱动类。一般我们声明mysql驱动的时候，不会设置为runtime，因为也许你开发代码的时候会用到mysql驱动特定的api接口，不仅仅只是用javax.sql。

### 7.传递性依赖

maven的传递性依赖，就是说会自动递归解析所有的依赖，然后负责将依赖下载下来，接着所有层级的依赖，都会成为我们的项目的依赖，不需要我们手工干预。所有需要的依赖全部下载下来，不管有多少层级。这个就是maven的传递性依赖机制，自动给我递归依赖链条下载所有依赖的这么一个特性。

### 8.依赖调解

比如A->B->C->X(1.0)，A->D->X(2.0)，A有两个传递性依赖X，不同的版本。 

就产生了依赖冲突的问题，maven如何解决呢？依赖调解的机制。 

此时就会依赖调解，就近原则，离A最近的选用，就是X的2.0版本。 

如果A->B->X(1.0)和A->D->X(2.0)，路径等长呢？那么会选择第一声明原则，哪个依赖在pom.xml里先声明，就用哪个。

### 9.可选依赖 

<optional>true</optional> 

此时依赖传递失效，不会向上传递。 

如果A依赖于B，B依赖于C，B对C的依赖是optional，那么A就不会依赖于C。反之，如果没有optional，根据传递性依赖机制，A会依赖于C。

### 10.依赖冲突

比如你依赖了A和B，此时A依赖了C-1.0，B依赖了D，D依赖了C-2.0。

此时就会导致的事情是，由于A -> C1.0是最短路径，所以会用C1.0，B依赖的是D，D依赖的C结果用了C-1.0版本，D本来用的是C-2.0的一个方法，但是现在给D的时C-1.0的一个类。

#### 如何解决

一般来说，用最新的版本，因为新版本一般可以兼容旧版本，但是旧版本一定不会提供新版本的功能。

#### mvn depedency:tree这个命令

mvn dependency:tree，用dependency插件的tree goal，执行，进行依赖链条分析。

比如你发现C这个项目下报出了类似的问题，然后就用mvn dependency:tree这个命令看一下整个项目的依赖路径树，看看所有的依赖路径里，有哪几个版本的C项目，看一下，自己看看要用哪个版本的C，然后将其他的C版本的依赖全部手动排除掉。

定位原因，因为A-C1.0最短，所以用了C-1.0，也就是旧的版本，用C-2.0的话，A依赖的C-1.0的代码是可以运行的。

```
<dependency>
<groupId>A</groupId>
<artifactId>A</artifactId>
<version>1.0</version>
<exclusions>
		<exclusion>
			<groupId>C</groupId>
			<artifactId>C</artifactId>
		</exclusion>
</exclusions>
</dependency>
```

通过这个以后，你如果再次运行mvn dependency:tre。

### 11.仓库

maven会用仓库来存储所有的依赖，不会说是每个项目都放一份依赖。

#### 仓库的布局结构

自己windows本地的仓库，是在~/.m2/repository目录，就是maven的本地仓库。

比如我们依赖了一个项目，它的坐标如下： 

```
groupId：log4j
artifactId：log4j
version：1.2.15
```

那么这个依赖的jar包在maven仓库里的布局，就是存放在log4j/log4j/1.2.15/log4j-1.2.15.jar，log4j/log4j/1.2.15就是目录，log4j-1.2.15.jar就是依赖对应的jar包。

所以你声明依赖之后，maven实际上就是根据依赖的坐标，先到本地仓库里面去根据坐标去一层一层的定位依赖所在的目录里面有没有这个jar包，如果没有的话，就是到远程仓库里面去根据同样的布局去找依赖的jar包。

#### maven的多层仓库架构

maven仓库的大类分成本地仓库和远程仓库两种，如果你声明了一个依赖，那么在构建打包的时候，先会去本地仓库里找，这个本地仓库的地址默认就在用户主目录下面的~/.m2/repository目录下面，里面各个依赖的存放路径就是跟上面说的那样；如果本地仓库找不到，那么就会去远程仓库找，默认是去maven自己的中央仓库里找，maven的中央仓库里几乎涵盖所有的依赖；然后会将中央仓库里的依赖下载下来放到本地仓库，缓存起来，供下次使用。

中央仓库是属于一种远程仓库，但是不只是有中央仓库一种远程仓库而已。 

远程仓库还有私服和其他远程仓库，私服指的是在公司内部的局域网内，架设一个服务器，上面放一个公司内私有的仓库，就是私服；其他远程仓库，就是其他公司开放出来的仓库，比如java.net maven仓库，jboss maven仓库，等等。

##### （1）本地仓库

本地仓库，windows默认路径是~\.m2\repository，linux默认路径是/home/root/.m2/repository。

可以修改本地仓库的路径，在~/.m2/settings.xml配置文件里，可以设置：

<localRepository>某路径</localRepository>

##### （2）远程仓库

如果本地仓库没有某个依赖，那么maven就会从远程仓库去下载，默认就是从中央仓库去下载。

##### （3）中央仓库 

maven有一个自带的超级pom.xml文件，里面配置了一个默认的中央仓库：

```
<repositories>
	<repository>
		<id>central</id>
		<name>Maven Repository Switchboard</name>
		<url>http://repo1.maven.org/maven2</url>
		<layout>default</layout>
		<snapshots>
			<enabled>false</enabled>
		</snapshots>
	</repository>
</repositories> 
```

所以如果你不做任何配置，那么当你声明了某个依赖之后，就是如果本地仓库没有，就会去maven自带配置的这个中央仓库去拉取，就是http://repo1.maven.org/maven2这个地址。

##### （4）私服

一般正经公司都会自己假设一个maven私服，也就是在公司局域网内部，假设一个服务器，上面放一个maven私有仓库。

因为很多公司的服务器是不允许访问外网的，只能访问局域网，为了更好的安全性起见。而且依赖下载的速度也很快。

此时的话，核心的思路，就是在本地配置远程仓库为私服，如果本地仓库没有，先去私服找，如果私服没有，再去中央仓库找。在中央仓库找到后，先缓存在私服中，然后再缓存在本地仓库中。

此外，假设自己的私服，另外一个意义，就在于说，自己公司内部的一些发布包，可以放到私服上，供公司内的项目组之间使用。

##### （5）其他远程仓库

有些依赖可能在中央仓库没有，或者中央仓库的速度太慢，此时可能会用其他的一些远程仓库，比如jboss的仓库。java.net，google，codehaus，jboss，还有一些其他公司自己搞的Maven仓库，有少数的依赖包可能在中央仓库里找不到，只在其他仓库里。

那么私服除了连接中央仓库，还可以连接其他远程仓库。

##### （6）镜像仓库

比如说，像中央仓库在国外，很慢的，直接从中央仓库下载的话，是很慢的。

所以一般国内的一些大型的互联网公司，阿里云，会搞一个镜像仓库，完全跟中央仓库一模一样的，代理了中央仓库所有的请求。

你可以直接从阿里云镜像仓库去请求，如果有就直接返回了，国内网络的速度很快的，上百倍；阿里云如果自己没有，就会去从国外的中央仓库去下载 。

你就知道了什么啊，pom.xml里面声明了各种依赖之后，是怎么去找的，仓库，仓库的布局，多层仓库（本地、私服、镜像、中央、其他）。

### 12.私服

搭建私服的话，一般maven这块都是用nexus去搭建的。

#### nexus中的仓库

nexus安装好之后本身就内置了一些仓库，包括四种仓库类型。

hosted：宿主仓库，这个仓库，是用来让你把你公司内部的发布包部署到这个仓库里来，然后公司内的其他人就可以从这个宿主仓库里下载依赖去使用。

proxy：代理仓库，这个仓库不是用来给你公司内部的发布包部署的，是代理了公司外部的各种仓库的，比如说java.net，codehaus，jboss仓库，最最重要的，就是代理了公司外部的中央仓库，但是这里其实可以修改为nexus连接的应该是国内的阿里云镜像仓库，阿里云去连接中央仓库。

其他的一些常用的仓库，java.net，jboss，googlecode，codehaus，这种仓库，现在都很少用了。

group：仓库组，其实就是将，各种宿主仓库、代理仓库全部组成一个虚拟的仓库组，然后我们的项目只要配置依赖于一个仓库组，相当于就是可以自动连接仓库组对应的各种仓库。

最后还有仓库的状态和路径：

maven-central：这是maven中央仓库的代理仓库。

maven-releases：该仓库是个宿主仓库，用于部署公司内部的release版本的发布包（类似于1.0.0,，release的意思就是你的工程已经经过了完善的测试，单元测试，集成测试，QA测试，上生产环境使用了）到这个仓库里面，供其他同事在生产环境依赖和使用。

maven-snapshots：该仓库是个宿主仓库，用于部署公司内部的snapshot版本的发布包到这个仓库里（如果你的某个工程还在开发过程中，测试还没结束，但是，此时公司里其他同事也在开发一些工程，需要依赖你的包进行开发和测试，联调，此时你的工程的版本就是类似1.0.0-SNAPSHOT这样的版本），供其他同事在开发和测试的时候使用。

3rd party：该仓库是个宿主仓库，主要用来部署没法从公共仓库获取的第三方依赖包，比如说，你的公司依赖于第三方支付厂商的一个依赖包，那个依赖包不是开源的，是商业的包。那么你是没有办法从maven中央仓库获取的。此时，我们可能会自己手动从支付厂商那里获取到一个jar包，下载之后上传到私服里来，就放这个仓库里，3rd-party仓库。

maven-public：仓库组，上面所有release仓库都在这个仓库组内。

### 13.nexus权限管理机制

nexus的权限是典型的RBAC模型，role-based access control。每个用户可以分配多个角色，每个角色分配多个权限，每个权限就是一个具体的功能，比如浏览依赖，部署发布包，等等。

nexus默认有三个用户：

admin，管理员账号，密码是admin123。

deployment，可以搜索和部署构建，就是普通的开发账号，密码是deployment123（在nexus 3.x最新版本里已经被消除掉了）。

anonymous：如果没有给认证信息，就是这个匿名账号，可以下载依赖，查看依赖。

可以在Users页面里添加用户，管理用户，还可以对用户的Role Tree进行角色的分配，nexus有一些预定义的角色，直接可以用。

也可以对单个仓库创建角色，并给角色分配对仓库的权限，然后就可以将单个仓库的授权角色分配给不同项目的用户。

### 14.mvn命令的说明

mvn clean package：清理、编译、测试、打包。

mvn clean install：清理、编译、测试、打包、安装到本地仓库，比如你自己负责了3个工程的开发，互相之间有依赖，那么如果你开发好其中一个工程，需要在另外一个工程中引用它，此时就需要将开发好的工程jar包安装到本地仓库，然后才可以在另外一个工程声明对它的依赖，此时会直接取用本地仓库中的jar包。

mvn clean deploy：清理、编译、测试、打包、安装到本地仓库、部署到远程私服仓库，这个其实就是你负责的工程写好了部分代码，别人需要依赖你的jar包中提供的接口来写代码和测试。此时你需要将snapshot jar包发布到私服的maven-snapshots仓库中。供别人在本地声明对你的依赖和使用。

### 15.maven的生命周期

maven的生命周期，就是对传统软件项目构建工作的抽象。

清理、初始化、编译、测试、打包、集成测试、验证、部署、站点生成。

maven有三套完全独立的生命周期，clean，default和site。每套生命周期都可以独立运行，每个生命周期的运行都会包含多个phase，每个phase又是由各种插件的goal来完成的，一个插件的goal可以认为是一个功能。

这就是maven的生命周期 -> phase（可以理解为阶段） -> 插件的关系，也是maven构建执行的核心原理。

你每次执行一个生命周期，都会依次执行这个生命周期内部的多个phase，每个phase执行时都会执行某个插件的goal完成具体的功能。

#### maven的生命周期以及phase

clean生命周期包含的phase如下：

```
pre-clean
clean
post-clean
```

default生命周期包含的phase如下：

```
validate：校验这个项目的一些配置信息是否正确
initialize：初始化构建状态，比如设置一些属性，或者创建一些目录
generate-sources：自动生成一些源代码，然后包含在项目代码中一起编译
process-sources：处理源代码，比如做一些占位符的替换
generate-resources：生成资源文件，才是干的时我说的那些事情，主要是去处理各种xml、properties那种配置文件，去做一些配置文件里面占位符的替换
process-resources：将资源文件拷贝到目标目录中，方便后面打包
compile：编译项目的源代码
process-classes：处理编译后的代码文件，比如对java class进行字节码增强
generate-test-sources：自动化生成测试代码
process-test-sources：处理测试代码，比如过滤一些占位符
generate-test-resources：生成测试用的资源文件
process-test-resources：拷贝测试用的资源文件到目标目录中
test-compile：编译测试代码
process-test-classes：对编译后的测试代码进行处理，比如进行字节码增强
test：使用单元测试框架运行测试
prepare-package：在打包之前进行准备工作，比如处理package的版本号
package：将代码进行打包，比如jar包
pre-integration-test：在集成测试之前进行准备工作，比如建立好需要的环境
integration-test：将package部署到一个环境中以运行集成测试
post-integration-test：在集成测试之后执行一些操作，比如清理测试环境
verify：对package进行一些检查来确保质量过关
install：将package安装到本地仓库中，这样开发人员自己在本地就可以使用了
deploy：将package上传到远程仓库中，这样公司内其他开发人员也可以使用了
```

site生命周期的phase：

```
pre-site
site
post-site
site-deploy
```

#### 默认的phase和plugin绑定

默认maven就绑定了一些plugin goal到phase上去，比如：

类似于resources:resources这种格式，说的就是resources这个plugin的resources goal（resources功能，负责处理资源文件）。

```
process-resources                resources:resources
compile                            compiler:compile
process-test-resources           resources:testResources
test-compile                      compiler:testCompile
test                               surefire:test
package                           jar:jar或者war:war
install                             install:install
deploy                            deploy:deploy
```

site生命周期的默认绑定是：

```
site                               site:site
site-deploy                       site:deploy
```

clean生命周期的默认：

```
clean                              clean:clean
```

#### maven的命令行和生命周期

比如mvn clean package。

clean是指的clean生命周期中的clean phase。

package是指的default生命周期中的package phase。

此时就会执行clean生命周期中，在clean phase之前的所有phase和clean phase，pre clean，clean。

同时会执行default生命周期中，在package phase之前的所有phase和package phase。

### 16.插件和goal

#### plugin

每个插件都有多个goal，每个goal都是一个具体的功能。

插件的goal，写法就是plugin:goal，比如dependency:tree。

用mvn dependency:tree，就可以手动执行某个插件的goal，执行某种功能。

#### 将插件的goal绑定到phase上

```
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-source-plugin</artifactId>
			<version>2.1.1</version>
			<executions>
				<execution>
					<id>attach-sources</id>
					<phase>verify</phase>
					<goals>

						<goal>jar-no-fork</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
```

运行mvn verify，就可以看到生成了一个包含源码的jar包。

#### 配置插件

如果在命令行执行插件，可以用-Dkey=value来进行插件的设置。

比如mvn install -Dmaven.test.skip=true，就是surefire插件在测试的时候提供的参数，设置为true就会跳过测试。

此外也可以在pom.xml中用来配置。

```
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>2.1</version>
			<configuration>
				<source>1.5</source>
				<target>1.5</target>
			</configuration>
		</plugin>
	</plugins>
</build>
```

### 17.聚合功能

可以将各个模块，聚合成一个大的模块，给它一个父工程，父模块，那个父模块里面配置聚合了些子模块。只要对父模块运行一次构建命令，此时maven会自动对这个父模块下面的所有子模块都运行相应的构建命令，这样就可以保证一键自动化构建所有的模块，不要一个一个依次去构建。

如果要一次性构建多个模块的工程，那么就需要创建一个父工程，我们可以创建一个os-parent工程，在其pom.xml中加入以下内容： 

```
<groupId>com.zhss.oa</groupId>
<artifactId>oa-parent</artifactId>
<version>1.0.0-SNAPSHOT</version>
<packaging>pom</packaging>
<name>oa parent project</name>
<modules>
	<module>oa-organ</module>
	<module>oa-auth</module>
	<module>oa-flow</module>
</modules>
```

一般来说会将模块统一放在父工程的目录下，这样就可以用上述形式，否则如果多个工程平级，则要用类似于../oa-organ的相对路径。

接着对oa-parent运行mvn clean install，此时就会对oa-parent中所有的工程都进行统一的构建。

### 18.继承

maven里面提供了一个功能，叫做继承，也就是说，我们可以将一个项目中所有模块通用的一些配置，比如依赖和插件，全部放在一个父工程中，oa-parent，然后所有的子工程，声明一下从父工程中去继承。

如果直接将所有的依赖放入父工程，然后子工程用<parent>元素声明继承，然后此时会强制性继承父工程中所有的依赖和插件。

但是实际的场景下，不同的工程需要的依赖是不一样的，可能有的工程需要从父工程继承20个依赖和3个插件；有的工程需要从父工程继承10个依赖和2个插件。如果按照上面那种最low最普通的做法，会导致每个子工程都强制性从父工程继承所有的依赖。

推荐的一个做法，是在父工程中，使用<depdendencyManagement>和<pluginManagement>两个元素，来声明要被子工程继承的依赖和插件。

有一个好处，就是，如果子工程用<parent>元素声明继承父工程，此时不会强制性继承所有的依赖和插件，子工程需要同时声明，自己要继承哪些依赖和插件，但是只要声明groupId和artifactId即可，不需要声明version版本号，因为version全部放在父工程中统一声明，强制约束所有子工程的相同依赖的版本要一样。

将oa-organ、oa-auth、oa-flow三个工程的pom.xml中加入以下配置。

```
<parent>
	<groupId>com.zhss.oa</groupId>
	<artifactId>oa-parent</artifactId>
	<version>1.0.0-SNAPSHOT</version>
</parent>
```

然后将所有工程相同的依赖和插件的配置，全部放入父工程中，此时子工程就会继承父工程的依赖和插件，不需要每个工程都重复定义，而且可以在父工程中统一约束所有依赖和插件的版本。

但是这里有个问题，如果直接将所有依赖和插件直接放入父工程，会导致子工程强制性继承，也许子工程并不需要某些依赖呢？

此时最好的做法，就是在父工程中，使用<dependencyManagement>元素和<pluginManagement>元素，来声明所有的依赖和插件。

此时在子工程中，就可以对自己需要的依赖进行声明，但是不用定义版本号，只要groupId和artifactId即可。这样声明之后才会继承依赖，而且版本由父工程约束。

如果在父工程中，直接用dependencies和plugins来声明依赖和插件，子工程会强制全部继承；如果用dependencyManagemnent和pluginManagement来声明依赖和插件，默认情况下，子工程什么都不继承，只有当子工程声明了某个依赖或者插件的groupId+artifactId，但是不指定版本时，才会从父工程继承那个依赖。在规范的工程管理中，肯定都是用dependencyManagement和pluginManagemnent的。

### 19.集中约束版本号

比如你的公司要统一升级一下spring版本号，这个时候，可能就需要在一个parent的pom.xml里面，一个一个的修改，很麻烦。

所以针对这种情况，一般都会在properties元素里面，统一定义一个版本号，然后在这个类似spring的依赖里面，全部用${}占位符来引用一个版本号，那么每次修改，升级spring版本，就直接修改properties元素中的一个地方就可以了，不会出错。

### 20.基于import pom强制约束依赖方的版本号

假设说依赖了你的组件，然后呢你的组件依赖了activiti 1.3版本，但是有个问题，结果依赖方自己没头没脑的又声明了一个队activiti 1.2的依赖，此时根据依赖调解的原则，肯定是用他自己的旧版本。

比较容易出现依赖的冲突。

首先自己开发一个工程，那个工程是对外提供服务的；接着再开发一个pom包，这个pom包专门用dependencyManagement来约束依赖方的各个版本号，不要跟自己出现冲突。

然后依赖方，不是直接依赖那个工程自己的包，而是依赖于那个工程的pom类型的包，然后再在dependency里面去声明自己要的依赖，此时版本就被约束了。

```
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>com.zhss.commons</groupId>
			<artifactId>commons-flow-bom</artifactId>
			<version>1.2.9</version> 
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
```

（1）如果你是公司基础组件的开发人，你的组件依赖了很多重要的第三方的开源框架，然后你为了保证依赖你的组件的人，不会自行导入一些开源框架的过旧的版本，导致跟你的组件出现依赖冲突。

（2）你需要为你的组件开发一个类型为pom的工程，后缀名为bom，这个工程，要声明一套dependencyManagement，里面声明好对你的组件的依赖的版本号，还有你的组件使用的重要的第三方开源框架的版本号。

（3）然后依赖方在引用你的组件的时候，需要在自己的dependencyManagement中，加一个scope范围为import，类型为pom的这么依赖导入，此时就可以将你的bom包声明的那一套依赖的版本号导入他那里，作为版本的约束。

（4）然后依赖方接着在dependencies里面可以声明对你的组件的依赖，此时版本号都不用写，因为已经被你约束了。

（5）同时，假设依赖方要自己依赖一个过旧的开源框架的版本，会有提示报警，不让他自行定义过旧版本的框架。

### 21.surefire插件

#### surefire插件介绍

持续集成，说的就是每个工程师当天写完代码之后，都必须将代码合并到一个公共的持续集成的代码分支上面去，一提交，要触发jenkins持续集成服务器，要立即自动化执行一次构建，自动化既有surefire这种插件，去运行所有的单元测试，确保你的代码没有把系统给改坏。

maven的自动化运行单元测试的插件，surefire。

maven中默认内置了surefire插件来运行单元测试，与最新流行的junit单元测试框架整合非常好。一般是在default生命周期的test阶段，会运行surefire插件的test goal，然后执行src/test/java下面的所有单元测试的。

而surefire插件会根据一定的规则在sre/test/java下面找单元测试类，具体规则如下：

```
**/Test*.java
**/*Test.java
**/*TestCase.java
```

通常比较流行的是用*Test.java格式类命名单元测试的类。

#### 跳过单元测试

如果某次你的确不想要执行单元测试就打包，那么可以跳过单元测试，但是在实际开发过程中，这是绝对不被允许的。

用mvn package -DskipTests即可。

#### 指定想要运行的测试类

mvn test -Dtest=**Test，直接指定你要运行的类即可。

通常见于开发人员自己刚写了某个单元测试类，然后就想自己运行一下看看效果。可以用逗号隔开，也可以用*匹配多个类。

#### 自定义包含与排除测试类

```
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-surefire-plugin</artifactId>
	<version>2.5</version>
	<configuration>
		<includes>
			<include>**/*Tests.java</include>
		</includes>
		<excludes>
			<exclude>**/*TempTest.java</exclude>
		</excludes>
	</configuration>
</plugin>
```

#### 测试报告

单元测试覆盖率是什么意思。

比如说，你总共写了100行代码。

然后你写了3个单元测试方法，结果这3个单元测试的方法会执行你的100行代码中的80行，另外有20行代码是无论运行哪个单元测试都不会执行的。 

此时单元测试覆盖率就是80 / 100，80%。

一般单元测试覆盖率，要求的是，两种，方法覆盖率，行覆盖率。

比如你一共写了10个方法，100行代码。

方法覆盖率要达到90%，那么要求就是至少对10个方法中的9个方法都要运行。

代码覆盖率要求70%，那么要求就是至少对70行代码都要执行到。

分支覆盖率，如果你的代码中有if else，或者是try catch，相当于是你的一段代码根据不同的情况，可能会有2个分支，分支里面还可以再嵌套if else，再分出来2个分支 。

if 

 if else

else

if else

2 * 2 = 4 

你的单元测试可以覆盖几个分支，1/4，25%；2 / 4，50%分支覆盖率。

surefire插件默认就会在target/surefire-reports目录下生成错误报告，就是如果有单元测试运行失败的话，就会有。

同时可以引入cobertura插件，可以看到测试覆盖率的报告。

```
<plugin>  
	<groupId>org.codehaus.mojo</groupId>  
    <artifactId>cobertura-maven-plugin</artifactId>  
    <version>2.5.1</version>  
</plugin>
```

### 22.基于cargo对web服务进行自动化部署

cargo插件，就是专门用来进行将本地的web工程打包成一个war包，然后呢扔到远程的服务器上的Tomcat中去，然后就是给你自动化的部署。

同样需要在settings.xml中加入pluginGroup：org.codehaus.cargo。

然后执行mvn cargo:deploy即可部署。

注意事项1：在执行cargo命令的时候，切记，不要打开tomcat的任何配置问加你，因为也许会因为文件打开占用，导致一些不必要的bug。

注意事项2：如果在实验过程中，遇到一些奇怪的问题，可以考虑在tomcat的webapps下面，删除oa-web目录和oa-web.war，接着重新先关闭tomcat，再启动tomcat，然后再次把你的工程package打包，然后cargo:deploy部署。

### 23.基于资源过滤+profile功能自动适配各个发布环境

给每一套环境，都配置好一套预先定好的地址，而不是每次部署的时候去修改一个文件，接着在每次往一个环境去部署的时候，可以手动指定一个参数，那么那个参数会指定用哪个环境下的一套配置。

#### 基于资源过滤+profile的方式适配各个环境

首先，我们需要将配置对应的值从配置文件里抽取出来，用占位符替代，然后实际的值放到profile里去。

```
database.jdbc.driverClass=${database.jdbc.driverClass}
database.jdbc.connectionURL=${database.jdbc.connectionURL}
database.jdbc.username=${database.jdbc.username}
database.jdbc.password=${database.jdbc.password}
```

然后在pom.xml里加入各个环境对应的profile配置

```
<profiles>
	<profile>
		<id>local</id>
		<properties>
			<database.jdbc.driverClass>com.mysql.jdbc.Driver</database.jdbc.driverClass>
			<database.jdbc.connectionURL>
				jdbc:mysql://192.168.31.110:3306/oa_local
			</database.jdbc.connectionURL>
			<database.jdbc.username>local</database.jdbc.username>
			<database.jdbc.password>local</database.jdbc.password>
		</properties>
	</profile>
	<profile>
		<id>dev</id>
		<properties>
			<database.jdbc.driverClass>com.mysql.jdbc.Driver</database.jdbc.driverClass>
			<database.jdbc.connectionURL>
				jdbc:mysql://192.168.31.110:3306/oa_dev
			</database.jdbc.connectionURL>
			<database.jdbc.username>dev</database.jdbc.username>
			<database.jdbc.password>dev</database.jdbc.password>
		</properties>
	</profile>
</profiles>
```

配置文件里的值都是占位符，每个环境的实际值都到了对应的profile里去了。

但是此时就是，如何让项目在发布到不同环境的时候激活profile，并且将实际值替换到占位符里去呢？

为src/main/resources目录开启资源过滤功能，让maven resources插件在处理资源的时候自动去解析里面的占位符，然后找到对应profile里的实际值来进行替换。

```
<resources>
	<resource>
		<directory>${project.basedir}/src/main/resources</directory>
		<filtering>true</filtering>
	</resource>
</resources>
<testResources>
	<testResource>
		<directory>${project.basedir}/src/test/resources</directory>
		<filtering>true</filtering>
	</testResource>
</testResources>
```

 src/main/profiles

```
<resources>
	<resource>
		<directory>src/main/profiles/local</directory>
	</resource>
</resources>
```

mvn clean package -Pdev，-P就是说激活dev profile。

查看target/classes下面的资源文件，全都替换为了dev profile中的实际值。

profile有很多种激活的方式，但是常用的其实就是-P这一种激活方式即可，因为都是部署的时候用不同的-P来激活。

#### 默认激活

我们一般会设置默认激活一个profile。

在profile配置里加一个：

```
<activation>
	<activeByDefault>true</activeByDefault>
</activation>
```

然后在各个环境部署的时候，肯定会用哪个环境的-P来激活对应的profile配置进行占位符替换。

而且还可以用mvn help:active-profiles查看当前激活的是哪个profile。

### 24.版本控制与版本管理

版本管理：通常指的是maven中的version，项目的版本管理，随着整个功能的开发，bug的修复，或者大的架构升级，通常来说，都会增加版本号。

版本控制：代码版本控制，git中的多次提交，每次往git代码仓库提交一次代码，这个代码的版本就变更了一次，而git会自动记录下来每次代码版本的变更。因为每次提交代码，相当就是代码的版本就变更了一次。

### 25.版本管理

#### 版本到底是什么

版本分为两种，一种是快照（snapshot）版本，一种是发布（release）版本。

快照版本就是版本号后面加上SNAPSHOT，比如1.0.0-SNAPSHOT。

发布版本就是版本号上不加SNAPSHOT的，比如1.0.0。

1.0.0中的第一个1指的是一个重大版本，比如已经基本成型的一个系统架构。

1.0.0中的第二个0指的是一个次要版本，比如1.1.0，那么就是加了一些新的功能或者开发了一些新的模块，或者做了一些较大的代码重构，技术升级改造。

1.0.0中的第三个0指的是日常迭代的一个增量版本，比如1.1.1，一般对应着修复了一个bug，或者对某些代码做了轻微的优化或者调整。

### 26.archetype模板

#### archetype模板的介绍

artchetype其实就是个maven项目模板。

比如我们之前在eclipse里面用m2eclipse插件自动创建的maven工程，就是用的maven-archetype-quickstart模板，就是给我们创建好最基础的工程结构和pom信息。

archetype项目模板，直接创建一个项目工程出来，基于此直接开发。公司里面各个项目，很多框架版本，框架整合，基础代码，代码规范和约定，都差不多。

然后下次你们如果要新启动一个项目，直接用archetype模板创建出来工程即可，比如就创建一个parent父工程出来，然后那个项目中的每个服务就继承自自己项目的parent工程即可，很快速。