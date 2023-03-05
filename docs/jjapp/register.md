# 服务注册
所有的微服务都会注册到`Apollo`进行统一管理

注册的模型文件符合`octopus`规范

## 微服务配置
在`Apollo`的配置文件中需要定义注册相关的配置
```json
{
  "app_root": "/renj.io/app",
  "app_manager": "/renj.io/manager",
  "app_cache_dir": "/renj.io/cache",
  "app_log_dir": "/renj.io/log",
  "app_tmp_dir": "/renj.io/tmp",
  "app_back_up": "/renj.io/backup",
  "app_pids": "/renj.io/pids"
}
```

其中`app_root`为必选项, 它会注册到环境变量`$APP_ROOT`在微服务上下文中被访问

新增的微服务目录只需要创建到`$APP_ROOT`下就会在自动发现时基于`octopus`模型自动发现

## octopus模型目录
默认的octopus模型放置在`$APP_ROOT/.octopus`下
```bash
$ cd .octopus
touch Demo.pig
```

## 注册服务Demo
其octopus模型为
```json
{
  "name": "Demo",
  "id": "app_demo",
  "type": "Service",
  "release_status": "unreleased",
  "eng_des": "default english description",
  "chs_des": "中文描述",
  "link": "",
  "manage_cmd": {
    "start": "start.sh",
    "stop": "stop.sh",
    "restart": "restart.sh",
    "force_kill": "kill.sh",
    "check": "check.sh"
  },
  "meta": {
    "author": "",
    "domain": "",
    "language": [],
    "create_date": "",
    "version": "1.0.0",
    "dynamic_conf": false,
    "conf_type": "",
    "conf_path": ""
  },
  "run_data": {
    "envs": [],
    "ports": [],
    "random_port": false,
    "host": "localhost",
    "run_dep": null,
    "stop_chain": null
  }
}
```

## 增加服务管理脚本
服务管理脚本由`Apollo`统一维护, 放置在服务Apollo的根目录`$ROOT/conf/manager`下

默认必须注册的管理脚本为`start.sh stop.sh check.sh`
分别为启动脚本, 停止脚本, 状态检查脚本

```bash
cd conf/manager
mkdir Demo
touch start.sh stop.sh check.sh
```

在脚本中定义内容来管理你的微服务程序

### 上下文环境变量
在微服务运行时默认提供一些环境变量可以让微服务访问
- `SERVICE_ROOT` 服务根目录
- `APP_ROOT` 微服务根目录
- `APP_LOG` 微服务日志目录
- `APP_PID` 微服务PID目录
- `APP_TMP` 临时文件夹
- `APP_STATUS_OK` 状态码 正常
- `APP_STATUS_ERR` 状态码 错误
- `APP_START_ERR` 微服务启动失败
- `APP_STOP_ERR` 微服务停止失败
- `APP_EXIT_ERR` 微服务退出失败
- `APP_RESTART_ERR` 微服务重启失败
- `APP_KILL_ERR` 微服务强制停止失败
- `APP_RUN_ERR` 微服务运行时异常