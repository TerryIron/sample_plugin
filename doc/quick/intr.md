# 插件平台

## 插件模板
* normal(基础)
* server(服务端)

## 插件基础
### 插件组成
* app.json(插件配置文件)
* app.py(插件基础模板)
* server.py(插件服务端模板)
* requirement.txt(插件依赖)
* __init__.py(插件包初始化）

### 插件模板介绍
* app.json

```
{
    "version": "0.1.0",
    "type": "test",
    "name": "name",
    "language": "python2",
    "imports": "requirements.txt",
    "actions": {
        "init": "app.init",
        "start": "app.start",
        "stop": "app.stop"
    },
    "public_actions": [
        "start",
        "stop"
    ],
    "init": "init",
    "call": "start"
}
```
字段名 | 必要 | 说明 
:----:|:----:|:----: 
version | Y | 插件版本号，支持插件升级
type | Y | 插件类型
name | Y | 插件名称
language | Y | 插件使用语言， 未来支持不同语言
imports | Y | 插件依赖文件
actions | Y | 插件操作定义
public_actions | N | 插件支持调用定义, 用于支持插件之间调用
init | Y | 插件初始化操作，请先在actions中定义
call | Y | 插件默认操作，请现在actions中定义

### 插件配置介绍
* app.py

```
import logging


class Application(object):

    def __init__(self):
        self.logger = logging.getLogger(__name__)

    def set_logger(self, logger):
        self.logger = logger


APP = None
CONFIG = {}


def init():
    global APP
    APP = Application()
    CONFIG['logger.path'] = '/tmp/$plugin_name.log'


def start(loader, **kwargs):
    return {'result': {}, 'data': {}}


def stop(loader, **kwargs):
    return {'result': {}, 'data': {}}
```
> 定义的函数中loader表示加载器类， kwargs表示数据通道

```
class Loader(object):
	"""
    插件加载类
    :prop config: 插件所在数据通道配置表
    :prop current_channel: 插件所在数据通道名称
    :prop config_channel: 插件配置通道名称
    :prop channel_scope: 插件数据通道范围
    :prop logger: 插件日志
    :prop logger_name: 插件日志打印名称
    """
```
### 插件管道
#### 插件管道启动
```flow
start=>start: 配置文件
app=>operation: 程序初始化
plugin=>operation: 插件初始化
plugin_pipes=>operation: 插件管道启动
app_start=>operation: 程序启动

start->app->plugin->plugin_pipes->app_start
```

#### 插件管道流向
```sequence
管道配置->管道基础数据: 插件管道开始
Note right of 管道基础数据: 插件A
管道基础数据-->管道结果数据: 执行插件A操作
Note right of 管道结果数据: 插件B
管道结果数据-->管道结果数据: 执行插件B操作
Note right of 管道结果数据: 插件C
管道结果数据-->管道基础数据: 执行插件C操作
Note right of 管道基础数据: 插件D
管道基础数据-->管道结果数据: 执行插件D操作
Note right of 管道结果数据: 插件E
```

### 插件配置模板
```
[plugin:main]
# 主入口
start = app

[pipeline:app]
# 管道组件
align = a.start c:result b:boom c:result p:app_test

[boardcast:boom]
# 广播组件
align = a b

[pipeline:app_test]
# 管道组件
align = c

[app:a]
# 插件实例
load = sample_a.zip

[app:b]
# 插件实例
load = sample_b.zip

[app:c]
# 插件实例
load = sample_c.zip
```

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
