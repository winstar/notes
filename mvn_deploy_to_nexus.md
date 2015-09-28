## mvn deploy部署到nexus
==============
### settings.xml配置账号密码

```
<servers>
    <server>
        <id>deploymentRepo</id>
        <username>admin</username>
        <password>admin123</password>
    </server>
    <server>
        <id>snapshots</id>
        <username>admin</username>
        <password>admin123</password>
    </server>
</servers>
```

### 直接部署jar包
```
mvn deploy:deploy-file -DgroupId=com.renhenet -DartifactId=renhesoa-api -Dversion=1.0.0 -Dpackaging=jar -Dfile=target/renhesoa-api-1.0.0.jar -Durl=http://localhost:8083/content/repositories/thirdparty/ -DrepositoryId=deploymentRepo
```
DrepositoryId即setting.xml中得server.id

### 使用deploy插件
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-deploy-plugin</artifactId>
    <version>2.7</version>
</plugin>
```
添加deploy插件后，执行mvn deploy会报错，如下:
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy (default-deploy) on project renhesoa-api: Deployment failed: repository element was not specified in the POM inside distributionManagement element or in -DaltDeploymentRepository=id::layout::url parameter -> [Help 1]

提示还需配置distributionManagement，指定DaltDeploymentRepository，如下:

```
<distributionManagement>
   <repository>
      <id>deploymentRepo</id>
      <name>Nexus Renhe</name>
      <url>http://localhost:8083/content/repositories/thirdparty/</url>
   </repository>
</distributionManagement>
```

### 添加源码插件，同步发布源码包
```
<plugin>
    <artifactId>maven-source-plugin</artifactId>
    <version>2.4</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>jar-no-fork</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```   