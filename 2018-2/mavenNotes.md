maven

- mvn -v 查看maven版本

 - compile 编译

 - test 测试

 - clean 删除target 

 - install 安装jar包到本地仓库

- 创建目录的两种方式
1. archetype:generate 按照提示进行选择
2. archetype:generate -DgroupId = 组织名
									-DartifactId=项目名-模块名
									-Dversion=版本号
									-Dpackage=代码所在包名
									
- 坐标

 - maven通过坐标引用jar包，groupId和artifactId构成了一个jar包的坐标

- 传递依赖与排除依赖
 - 传递依赖：如果我们的项目引用了一个Jar包，而该Jar包又引用了其他Jar包，那么在默认情况下项目编译时，Maven会把直接引用和简洁引用的Jar包都下载到本地。
 - 排除依赖：如果我们只想下载直接引用的Jar包，那么需要在pom.xml中做如下配置：(将需要排除的Jar包的坐标写在中)
 


 