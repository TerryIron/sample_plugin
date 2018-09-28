# 插件平台

## 插件制作
### 软件需求
* Python 2.7+

### 如何提交插件
* 进入安装目录

```
cd <directory containing this file>
```

* 新建插件

```
./new_plugin PLUGIN_NAME
```

* 开发插件

```
./workon_plugin PLUGIN_NAME
```

* 提交插件

```
./push_plugin PLUGIN_NAME
```

## 插件发布
* 进入安装目录

```
cd <directory containing this file>
```

* 打包插件

```
./build_plugin PLUGIN_NAME [VER]
```

* 安装插件

```
./install_plugin PLUGIN_NAME INSTALL_PATH
```


## 插件平台发布
* 发布平台

```
./publish_platform plugin_pack.json
```

### 插件平台配置模板
* plugin_pack.json

#### 支持branch下载

```
{
    "remote": "origin",
    "plugin_A": "0.1.0",
    "plugin_B": "0.1.2",
    "plugin_C": "0.1.3",
    "platform": {
    	"name": "test",
        "tag": "0.1.0",
        "repo": "git_url",
        "plugin_dir": "plugin"
    }
}
```

#### 支持tag下载

```
{
    "plugin_A": "0.1.0",
    "plugin_B": "0.1.2",
    "plugin_C": "0.1.3",
    "platform": {
    	"name": "test",
        "tag": "0.1.0",
        "repo": "git_url",
        "plugin_dir": "plugin"
    }
}

```

字段名 | 必要 | 说明 
:----:|:----:|:----: 
remote | N | 插件远端仓库, 默认使用origin
platform | Y | 配置平台属性
name | Y | 发布名称
tag | N | 发布分支, 默认使用master分支
repo | Y | 仓库地址
plugin_dir | Y | 插件目录
