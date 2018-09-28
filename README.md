# 插件市场 README

## Requirements(软件需求)
* Python 2.7+

## Coding Started(如何提交插件)
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

## Publish Started(如何发布插件)
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
