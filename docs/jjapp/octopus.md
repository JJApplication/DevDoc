# octopus meta
octopus-meta是Apollo运行时的App 模型定义

## 模型
`App | Meta` 为octopus-meta定义的app模型

```go
// App model for app
type App struct {
	Name          string `json:"name" validate:"required" bson:"name" yaml:"name"`           // 服务名称
	ID            string `json:"id" validate:"required" bson:"id" yaml:"id"`                 // 服务唯一ID
	Type          string `json:"type" bson:"type" yaml:"type"`                               // service | middleware
	ReleaseStatus string `json:"release_status" bson:"release_status" yaml:"release_status"` // published | pending | testing
	EngDes        string `json:"eng_des" bson:"eng_des" yaml:"eng_des"`                      // 英文描述
	CHSDes        string `json:"chs_des" bson:"chs_des" yaml:"chs_des"`                      // 中文描述
	Link          string `json:"link" bson:"link" yaml:"link"`                               // 服务对外提供的URL链接
	// 管理项
	ManageCMD CMD `json:"manage_cmd" bson:"manage_cmd" yaml:"manage_cmd"` // 管理命令组
	// 元数据
	Meta Meta `json:"meta" bson:"meta" yaml:"meta"` // 服务元数据
	// 动态依赖配置
	RunData RunData `json:"run_data" bson:"run_data" yaml:"run_data"` // 服务运行依赖

	Runtime Runtime `json:"runtime" bson:"runtime" yaml:"runtime"` // 服务运行时数据

	ResourceLimit ResourceLimit `json:"resource_limit" bson:"resource_limit" yaml:"resource_limit"` // 运行时资源限制
}
```
## 对外接口
### AutoLoad
自动加载meta文件， 当环境变量`APP_ROOT`存在并且`$APP_ROOT/.octopus`或`$APP_ROOT/meta`存在时
会自动从此目录加载模型文件

### Load(p string)
传入指定的模型文件目录并加载模型文件

### SetOctopusMetaDir(p string)
配置全局生效的模型文件路径，会在Load接口中生效，不影响Autoload逻辑

### Octopus{}
Octopus结构体暴露了内部的json模型加载方法
```bash
var OctopusIterator = Octopus{Type: "default", AutoEnv: true}
```
在octopus-meta内部定义一个全局的OctopusIterator默认使用，通过`AutoEnv`可以控制meta的值是否支持从环境变量加载

## 拥有完整的测试用例
```bash
$ bash ./test.sh

=== RUN   TestJson
    json_lib_test.go:32: test json success
    json_lib_test.go:36: {Name:test Age:10 Meta:{Sp:}}
--- PASS: TestJson (0.00s)
=== RUN   TestJsonWithEnv
    json_lib_test.go:49: test envFlag true success
    json_lib_test.go:53: {Name:jsonTest Age:10 Meta:{Sp:}}
--- PASS: TestJsonWithEnv (0.00s)
=== RUN   TestJsonWithNoEnv
    json_lib_test.go:62: test envFlag false success
    json_lib_test.go:66: {Name:$test Age:0 Meta:{Sp:}}
--- PASS: TestJsonWithNoEnv (0.00s)
=== RUN   TestLoad
    meta_test.go:24: map[test:{ app_t Service published default english description 默认中文描述  {start.sh stop.sh restart.sh kill.sh check.sh} { renj.io []  1.0.0 false  } {[] [] true localhost [] []} { [] false} {0 0 0 0 0 0 0 0 0 0}}]
--- PASS: TestLoad (0.00s)
=== RUN   TestAutoLoad
    meta_test.go:34: map[test:{T app_t Service published default english description 默认中文描述  {start.sh stop.sh restart.sh kill.sh check.sh} { renj.io []  1.0.0 false  } {[] [] true localhost [] []} { [] false} {0 0 0 0 0 0 0 0 0 0}}]
--- PASS: TestAutoLoad (0.00s)
=== RUN   TestLoadApp
    meta_test.go:44: { app_t Service published default english description 默认中文描述  {start.sh stop.sh restart.sh kill.sh check.sh} { renj.io []  1.0.0 false  } {[] [] true localhost [] []} { [] false} {0 0 0 0 0 0 0 0 0 0}}
--- PASS: TestLoadApp (0.00s)
```

## 注册模型
所有的**JJApp**都是一个octopus定义的模型, 在模型目录下创建一个微服务模型文件例如`Demo.pig`
其中的内容为JSON或者YAML语义化数据

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
  },
  "runtime": {
    "pid": "",
    "ports": null,
    "stop_operation": false
  },
  "resource_limit": {
    "min_cpu": 0,
    "max_cpu": 0,
    "min_mem": 0,
    "max_mem": 0,
    "ave_cpu_peak": 0,
    "ave_mem_peak": 0,
    "max_read": 0,
    "max_write": 0,
    "max_request": 0,
    "max_client": 0
  }
}
```

基于**服务发现策略**在模型创建成功后, `Apollo`会自动发现名为`Demo`的服务并注册

?>与模型定义对应的是微服务路径, 在校验`$APP_ROOT/$NAME`对应的服务存在后才会成功注册服务