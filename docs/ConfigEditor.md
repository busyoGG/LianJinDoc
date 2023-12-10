# 配置类生成器

## 使用说明

### 工具说明

![](https://img.busyo.buzz/imgUpload/20231210-031953-377.png)

按照图片红框所示的步骤即可一件生成配置类文件。
本工具监听配置文件夹，只要有变化自动就会改变面板显示的内容。

### 配置类说明

配置类会保存到配置类输出文件夹的Bean目录下。

在输出文件夹根目录中存在一个 `ConfigsPathConfig` 文件，该文件包含所有配置的名字和配置文件夹的名字。

在 `ConfigManager` 的调用中，我们可以直接传入 `ConfigsFolderConfig` 和 `ConfigsNameConfig` 的属性作为配置读取的路径。

例：

```C#
//Null表示根目录
Dictionary<int, DialogConfigData> configs = ConfigManager.Ins().GetConfig<DialogConfigData>(ConfigsFolderConfig.Null, ConfigsNameConfig.DialogConfig);
```

### 配置文件说明

配置表属性名后面添加 `|` 可以分割属性名和类型。

* 如果我们需要一个本配置类的类型，则我们在对应的配置项就置空。

    例：

    ![](https://img.busyo.buzz/imgUpload/20231210-033316-678.png)

* 如果我们需要一个特定的枚举类型或者类的类型，则可以用 `|` 来表示我们要生成的类型，即 `PropName|EnumType`，这样就能生成 `EnumType prop`。

    例：

    ![](https://img.busyo.buzz/imgUpload/20231210-033520-680.png)

* 如果该类型是int类型的列表，但是第一项中没有内容，我们只要把属性名写成 `PropName|int` 即可生成 `List<int>`。

    例：

    ![](https://img.busyo.buzz/imgUpload/20231210-032812-826.png)

* 如果该类型是本数据类，则我们不用写类型，但是要在配置项的位置写一个空列表 `[]` ，这样生成的时候就会生成 `List<ClassName>`。

    例：

    ![](https://img.busyo.buzz/imgUpload/20231210-032954-794.png)

* 如果要生成一个字典，则我们可以再加一个 `|` 来表示键值对类型，即写成 `PropName|type|type`，并且在配置项写一个空对象 `{}`，这样就能生成 `Dictionary<type,type>`。

    例：

    ![](https://img.busyo.buzz/imgUpload/20231210-033156-694.png)

* 如果要生成一个对象数组，只需要在数组中写入对象键值对，即可生成对象辅助类。本类型不支持自定义数组和字典类型，只能按照输入的内容创建基本数据类型的数组和字典。

    例：
    
    ![](https://img.busyo.buzz/imgUpload/20231210-195150-134.png)
    
    ![](https://img.busyo.buzz/imgUpload/20231210-195531-712.png)

## 配置路径修改

修改文件 `ConfigEditor` 中的 `_jsonUrl` 属性即可修改读取json文件的文件夹路径。

修改文件 `ConfigEditor` 中的 `_outputUrl` 属性即可修改写入配置类的路径。

![](https://img.busyo.buzz/imgUpload/20231210-061234-22.png)

## 注意

* 配置文件夹下不能创建和配置文件夹同名的文件夹。
* 配置文件夹只能存在最多二级文件夹，即最大路径为 `/Configs/Folder/Config.json`。