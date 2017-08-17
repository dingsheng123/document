### Maven 学习


#### 坐标
+ **groupId:** 定义当前Maven项目隶属的实际项目。（和项目中的包名对应）
+ **artifactId:** 该元素定义实际项目中的一个Maven项目（模块），一般使用实际项目名称作为artifactId的前缀。
+ **version:** 该元素定义Maven项目当前所处的版本
+ **packaging:** 该元素定义Maven项目的打包方式。打包方式会影响到构建的生命周期，比如jar和war打包会使用不同的命令。当不定义packaging时，Maven会使用默认值jar
+ **classifier:** 该元素用来帮助定义构建输出的一些附属构建。注意，不能直接定义项目的classifier，因为附属构建不是项目直接默认生成的，而是由附加的插件帮助生成的。

**PS:** 上面5个元素中，groupId、artifactId、version是必须定义的，packaging是可选的，classifier是不能直接定义的。

**example:**
```
	<groupId>com.example.demo</groupId>
	<artifactId>spring-boot-demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>
```