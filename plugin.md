# 创建插件  https://sourceforge.net/p/easyrec/wiki/Plugin%20Guide/

参考 easyrec\easyrec-plugins\easyrec-plugins-itemitem\src\main\resources\easyrec-plugin.xml

1. mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-generator -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

2. 添加依赖
```
 <dependencies>
       <dependency>
           <groupId>org.easyrec</groupId>
           <artifactId>easyrec-plugin-api</artifactId>
           <version>${project.version}</version>
       </dependency>
       <dependency>
           <groupId>org.easyrec</groupId>
           <artifactId>easyrec-utils</artifactId>
           <version>0.95</version>
       </dependency>
       <dependency>
           <groupId>org.easyrec</groupId>
           <artifactId>easyrec-core</artifactId>
           <version>0.95</version>
       </dependency>
       <dependency>
           <groupId>org.easyrec</groupId>
           <artifactId>easyrec-domain</artifactId>
           <version>0.95</version>
       </dependency>
   </dependencies>
```
3. 创建 easyrec-plugin.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="myGenerator" class="com.mycompany.app.MyGenerator"/>
</beans>
```

4. 实现接口 org.easyrec.plugin.support.GeneratorPluginSupport

GeneratorConfiguration 定义插件配置(org.easyrec.plugin.configuration.PluginParameter),下面三个为默认的配置
    configurationName: 配置名称,用于在不同的配置间切换
    tenantId: 租户id，谁拥有这个插件
    associationType: 关联类型 描述item关联 默认是 "IS_RELATED"
GeneratorStatistics 统计
    startDate: 自动记录doExecute开始
    endDate: 自动记录doExecute结束
    numberOfRulesCreated: 生成器生成的推荐和item关联数量
    numberOfActionsConsidered: 用于计算推荐的action数量(buy view等)

doExecute 实现推荐逻辑

RunConditionEnabled 接口用于判断 插件是否可以运行
    例如 ARM查询检查自从上次运行是否有新的action,如果没有变化,则不需要计算,相当于一个节约 计算量的逻辑判断

doInstall() 安装
doInitialize() 初始化
doCleanup() 清理
doUninstall() 卸载


5. 测试插件 参考 org.easyrec.plugin.itemitem.cli.ItemItemCLI
测试类需要继承  org.easyrec.plugin.cli.AbstractGeneratorCLI,并提供 配置类和统计类

getConfigurations() 配置所有需要的spring 配置文件列表
getGenerator() 返回生成器实例
```
public static void main(final String[] args) {
   final MyGeneratorCLI generatorCLI = new MyGeneratorCLI();
   generatorCLI.processCommandLineCall(args);
}
```

6. 上传插件 http://localhost:8081/easyrec-web/dev/plugins?tenantId=EASYREC_DEMO&operatorId=orangeyts
