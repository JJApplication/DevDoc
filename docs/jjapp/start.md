# 快速开始
基于下面的步骤你将快速创建一个微服务, 并对它进行集中管理

## JJAPP的结构
```bash
|-OS
  |-Helios, NoEngine
    |-Sandwich
    |-Apollo
      |-Service1
      |-Service2
      |-Module1
      |-Module2
    |-Container1
    |-Container2    
```

**顶级入口**即外部访问的统一入口， 提供域名绑定和前置处理能力。在JJAPP中你可以使用基于Nginx的`NoEngine`或者`Helios`来作为顶级入口

**网关**Sandwich即负责所有请求转发的中间层服务，提供基于域名的微服务转发，高级路由，数据流量统计等功能

**微服务管理者**Apollo即拥有最高权限的微服务管理进程，除了顶级服务外拥有全部微服务的管理权限，负责微服务的自动发现，注册，卸载，启停

**服务和模块**在JJAPP中`Service`即微服务一般为提供对外接口的程序， `Module`即模块一般为运行于服务器上进行监控等操作的非对外接口程序

**下层容器**即提供基础数据库，缓存的公共容器， JJAPP不是基于容器化部署的集群但是实现了类似容器的能力

## 服务发现
在`octopus模型`章节可以详细了解JJAPP的微服务基础单元`octopusMeta`

每一个微服务都会被抽象成一个模型文件用于告知`Apollo`我要注册一个怎样的微服务

流程如下:
- 在$APP_ROOT目录下部署自己的微服务目录: `MyService`
- 新增微服务模型文件`MyService.yml`
- 自动发现会扫描模型并进行加载，如果匹配到`$APP_ROOT/MyService`微服务就会加载到Apollo中
- Apollo默认调用启动脚本启动微服务`MyService`

## 创建一个微服务

### 建立服务根目录
确保下载了github上最新的release中的`Apollo`和`Sandwich`

假设你的服务根目录为`/opt/group`使用项目`J-sh`初始化目录
```bash
git clone https://github.com/JJApplication/J-sh.git
bash ./J-sh/j.sh new
```

默认会创建必须的目录
- `app` 微服务目录
- `cache` 缓存目录
- `log` 日志目录
- `backup` 备份目录

Apollo的默认路径为`app/Apollo`
Sandwich的默认路径为`app/Sandwich`

#### 启动Apollo
```bash
cd app/Apollo
./start.sh
```
Apollo会默认启动到http://127.0.0.1:9090

### 部署你的微服务到服务目录下
将你的服务`MyService`部署到`/opt/group/app/MyService`

将服务的管理脚本`start.sh`, `stop.sh`放置到Apollo目录`app/Apollo/conf/manager/MyService`下

?>注意：为了方便的管理脚本而不是让脚本分散在各个微服务里， Apollo将统一管理所有服务的脚本

### 创建模型单元
模型支持`yaml`, `json`

`MyService.json`

```json
{
  "name": "MyService",
  "id": "app_myservice",
  "type": "Service",
  "release_status": "published",
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
  },
  "runtime": {
    "pid": "",
    "ports": null,
    "stop_operation": false
  }
}
```
将模型文件放置到octopus模型目录下， 默认为`app/.octopus`

访问Apollo的默认地址http://127.0.0.1:9090

等待Apollo自动发现微服务后， 你可以在Apollo的页面上看到你部署的微服务

### 访问你的微服务
如果你的微服务对外暴露了端口，而你想要通过域名进行访问，此时需要注册服务到`Sandwich`

?>假设你监听的端口是`6666`， 域名是`myname.com`

只需要修改模型文件中的`domain`和`port`

```json
  "meta": {
    "domain": "myname.com",
  },
  "run_data": {
    "ports": [6666],
    "random_port": false,
    "host": "localhost",
  }
```

`Sandwich`会自动从Apollo中同步模型数据，读取内部定义的域名和运行端口

当接受到外部的域名访问后会根据**域名:端口**对应表转发到对应的微服务上

### 使用ApolloCLI管理微服务
Apollo依赖一些微服务来实现完整的微服务管理能力， 如果你只安装了Apollo有部分功能可能无法使用

此时你可以通过Apollo的终端工具来与Apollo直接进行通信

```bash
Apollo交互式终端

Commands:
=========
  address    查看连接的unix地址
  app        显示注册的微服务列表
  backup     全局同步备份
  check      检查Apollo服务状态
  clear      clear the screen
  exit       exit the shell
  help       use 'help [command]' for command help
  reconnect  重连指定的unix地址
  reload     重载微服务模型文件
  restart    重启指定微服务
  start      启动指定微服务
  status     查看指定微服务
  stop       停止指定微服务
  sync       同步指定微服务
  version    查看ApolloCLI版本

ApolloCLI »  
```

## 基于iKun的类容器
JJAPP当前的服务管理模式还是传统的单机部署， 但是你可以使用**iKun**实现类似容器的隔离能力

只需要让**iKun**接手启动命令

```bash
# 旧
./app_service args1 args2
# 新
iKun run ./app_service args1 args2
```

使用**iKun**启动的微服务会经过iKun模拟出类似容器的隔离化环境

拥有独立的环境变量，文件系统， 网络，命名空间，资源声明

当多个服务之间需要通信时，可以通过类似容器`bridge`的方式进行服务间通信

通过**iKun**你可以以一种更高效，更独立的方式管理微服务资源