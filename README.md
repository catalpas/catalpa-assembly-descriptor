# catalpa-assembly-descriptor

自定义的打包描述，分为两个描述文件`update`和`standalone`

> **update**

`update`只会将项目打成`jar`包然后将项目和依赖包统一的复制到`lib`文件夹下，将配置文件既`Resoures`中的文件复制到`config`文件夹中

> **standalone**

`standalone`在`update`的基础上增加两个文件夹`bin`和`logs`，分别用来保存可运行文件和日志，其中`bin`文件夹将拷贝项目中`src/main/bin`目录中的内容。`logs`文件夹自动生成