# Hermes
邮件模块，接收指定邮件模板发送到目标邮箱

## 依赖部件
- Twig

## 配置
```bash
export User=xxx@163.com
export Pass=xxx
export Host=smtp.163.com
export Port=465
export Nickname=Hermes
export UnixAddress=/tmp/Hermes.sock
```
在环境变量中配置smtp账户

`User` 发信邮件账户

`Pass` 账户密码

`Host` smtp服务器地址

`Port` smtp服务器端口

`Nickname` 邮件对外呈现的标题名称

`UnixAddress` 内部通信的uds地址

## 定制模板
邮件模板放置在`/tmpl`下， 基于go template开发

## 内置事件
```go
func init() {
	events = make(map[string]uds.Func, 20)
	events["ping"] = eventPing()
	events["send"] = eventSend()
	events["sendSync"] = eventSendSync()
	events["sendCron"] = eventSendCron()
	events["sendSchedule"] = eventSendSchedule()
	events["sendMgek"] = eventSendMgek()
	events["sendAlarm"] = eventSendAlarm()
	events["sendAlarmHtml"] = eventSendAlarmHtml()
	events["sendHomeSub"] = eventSendHomeSub()
	events["sendBlogSub"] = eventSendBlogSub()
	events["tasks"] = eventTasks()
	events["cancelTask"] = eventCancelTask()
}
```

`Hermes`是一个uds服务器，可以接受外部的uds通信
你可以通过`twig`服务或者直接发送uds报文来让Hermes做出响应

## 接收发件请求
作为发送方, 使用uds客户端发送如下报文
```go
		res, err := udsc.SendWithRes(uds.Req{
			Operation: send,
			Data: mailReq(mailInfo{
				Type:     "",
				Message:  "xxx",
				IsFile:   false,
				Subject:  "xxx",
				Attach:   nil,
				To:       []string{wc.To},
				Cc:       nil,
				Bcc:      nil,
				SyncTask: false,
				CronJob:  "",
			}),
			From: "Your Name",
			To:   []string{"Hermes"},
		})
```

`Hermes`定义了`mailInfo`模型

`Type` 邮件类型 纯文本|HTML

`Message` 邮件文本

`IsFile` 是否为带附件邮件

`Subject` 副标题

`Attach` 附件

`To` 邮寄地址

`Cc` 抄送

`Bcc` 密抄

`SyncTask` 是否异步发送

`CronJob` 定时发送的定时周期