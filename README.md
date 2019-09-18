# catalpa-assembly-descriptor

自定义的打包描述，分为两个描述文件`update`和`standalone`

> **update**

`update`只会将项目打成`jar`包然后将项目和依赖包统一的复制到`lib`文件夹下，将配置文件既`Resoures`中的文件复制到`config`文件夹中

> **standalone**

`standalone`在`update`的基础上增加两个文件夹`bin`和`logs`，分别用来保存可运行文件和日志，其中`bin`文件夹将拷贝项目中`src/main/bin`目录中的内容。`logs`文件夹自动生成

### 如何使用

在`pom.xml`中加入

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>3.1.1</version>
    <dependencies>
        <dependency>
            <groupId>com.github.catalpas</groupId>
            <artifactId>catalpa-assembly-descriptor</artifactId>
            <version>0.0.1</version>
        </dependency>
    </dependencies>
    <executions>
        <execution>
            <id>xry-assemble</id>
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <descriptorRefs>
            <descriptorRef>catalpa-assembly-update</descriptorRef>
        </descriptorRefs>
    </configuration>
</plugin>
```

然后运行`mvn clean package`

**more**

既然本插件会将`resoures`文件夹下的文件拷贝到`config`文件夹，那么在生成的`jar`文件中就不需要包含任何的配置文件了，可以添加下面的内容来避免配置文件冲突。

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <configuration>
        <archive>
            <addMavenDescriptor>false</addMavenDescriptor>
            <index>true</index>
            <manifest>
                <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
                <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
            </manifest>
        </archive>
        <excludes>
            <exclude>**/**.properties</exclude>
            <exclude>**/**.xml</exclude>
            <exclude>**/**.yml</exclude>
        </excludes>
    </configuration>
</plugin>
```
这一段的内容会将所有`properties`，`xml`和`yml`文件剔除，如果在实际应用中有用到别的配置文件请自行添加