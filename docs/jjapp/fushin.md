# FuShin
FuShin Stone

`Fushin`是一个适用于JJApplication的一站式微服务框架

使用`fushin` 生成对应的微服务模板

或者使用`go module` Fushin搭建一个JJApplication

## 安装
?>fushin v1.1.7前的版本
```bash
# need at least go1.17
$ go get github.com/JJApplication/fushin
# or
$ go install github.com/JJApplication/fushin
```
?>fushin v1.1.7后的版本
```bash
$ go get github.com/JJApplication/fushin
```
`fushin > v1.1.7`去除了内置的服务模板创建, 请使用Create-JJApp库创建微服务模板

## 微服务类型
在JJApplication中支持以下服务类型
- web
- module
- noengine
- client
- empty

`web`为典型的提供web体验的微服务
`module`为服务的模块
`noengine`为典型的NoEngine服务
`client`为典型的客户端服务
`empty`为一个空白的模板

## 微服务通信协议
默认支持的通信协议在对用户侧基于http, http2, ws
在服务端基于http或tcp的rpc, 基于unix domain的uds, 和自定义`protof - Plnack`

## 服务管理机制
所有的服务会生成一个模型文件`${App}.pig`
模型文件中记录了服务的基础信息，通信协议，对外端口等
模型文件由`octopusTree`统一管理，由`Apollo`服务解析存储，并做统一的管理调度

## 服务间通信机制
在服务器上的各个微服务之间的通信全部基于`unix domain`
由服务`octopusTwig`管理调用服务间uds的通信传输

## 服务模型设计
当前模型文件为基于`json`的`pig`文件, 支持环境变量和自定义字段覆盖
后期支持`yml`的文件格式
模型文件的定义基于`octopus_meta`

## 引入Fushin
使用`Demo()`快速启动一个例子
```go
import (
    "github.com/JJApplication/fushin"
)

package main
func main() {
	// Demo会默认开启一个http server和一个uds Server
	fushin.Demo()
}
```
结果
```bash
{"level":"INFO","time":"2022-8-14 15:36:33","caller":"http/engine_log.go:30","message":"<Server> signal [interrupt] registered"}
{"level":"INFO","time":"2022-8-14 15:36:33","caller":"http/engine_log.go:30","message":"<Server> server init success"}
{"level":"INFO","time":"2022-8-14 15:36:33","caller":"http/engine_log.go:30","message":"<Server> server will listen on 0.0.0.0:10086"}
{"level":"INFO","time":"2022-8-14 15:36:33","caller":"http/engine_log.go:30","message":"<Server> server domain name is []"}
{"level":"INFO","time":"2022-8-14 15:36:33","caller":"http/engine_log.go:30","message":"<Server> server debug: false"}
{"level":"INFO","time":"2022-8-14 15:36:33","caller":"http/engine_log.go:30","message":"<Server> server enableLog: true"}
{"level":"INFO","time":"2022-8-14 15:36:33","caller":"uds/log.go:33","message":"<UDSServer> uds server listen @ [/tmp/DemoUDS]"}
```

### 使用Fushin模块
Fushin对微服务所需的大部分模块做到了封装和开箱即用
例如引入`uds server`

```go
package main

import (
	"fmt"

	"github.com/JJApplication/fushin/server/uds"
)

func main() {
	us := uds.New("unix socket address")
    // change config
	us.Option.MaxSize = 1<<20
	us.Option.AutoRecover = true
	if err := us.Listen(); err != nil {
		fmt.Println(err)
	}
}
```

## 编译/贡献
编译此项目
```bash
cd Fushin && go mod download
go build
```

`Fushin`的可执行二进制仅提供根据模板生成项目代码的能力

运行基准测试
```bash
go test -v ./xxx
```

## 使用Fushin开发的项目
- [Hermes](https://github.com/JJApplication/Hermes)
- [WDNMD](https://github.com/JJApplication/WDNMD)
- and so on.