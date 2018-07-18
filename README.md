# prometheus-webhook-dingtalk

Generating [DingTalk] notification from [Prometheus] [AlertManager] WebHooks.

## Building and running

### Build

```bash
make
```

### Running

```bash
./prometheus-webhook-dingtalk <flags>
```

## Usage

```
usage: prometheus-webhook-dingtalk --ding.profile=DING.PROFILE [<flags>]

Flags:
  -h, --help              Show context-sensitive help (also try --help-long and --help-man).
      --web.listen-address=":8060"
                          The address to listen on for web interface.
      --ding.profile=DING.PROFILE ...
                          Custom DingTalk profile (can be given multiple times, <profile>=<dingtalk-url>).
      --ding.timeout=5s   Timeout for invoking DingTalk webhook.
      --template.file=""  Customized template file (see template/default.tmpl for example)
      --log.level=info    Only log messages with the given severity or above. One of: [debug, info, warn, error]
      --version           Show application version.

```
## 注意事项：
> 提供了路由供` alertmanager` 的webhook_configs使用：

- ` /dingtalk/<profile>/send`  ##此处的<profile>## 需要在` -ding.profile`中指定如：
```
prometheus-webhook-dingtalk \
    --ding.profile="webhook1=https://oapi.dingtalk.com/robot/send?access_token=xxxxxxxxxxxx" \
    --ding.profile="webhook2=https://oapi.dingtalk.com/robot/send?access_token=yyyyyyyyyyy"
```
这里定义了两个webhook，一个` webhook1`,一个` webhook2`用来发送到不同钉钉机器人

然后在alermanager的配置中，加入相应的receiver(注意url)：
```
receivers:
- name: send_to_dingding_webhook1
  webhook_configs:
  - send_resolved: false
    url: http://localhost:8060/dingtalk/webhook1/send
- name: send_to_dingding_webhook2
  webhook_configs:
  - send_resolved: false
    url: http://localhost:8060/dingtalk/webhook2/send
```


## Using Docker

You can deploy this tool using the Docker image from following registry:

* [DockerHub]\: [timonwong/prometheus-webhook-dingtalk](https://registry.hub.docker.com/u/timonwong/prometheus-webhook-dingtalk/)
* [Quay.io]\: [timonwong/prometheus-webhook-dingtalk](https://quay.io/repository/timonwong/prometheus-webhook-dingtalk)


[Prometheus]: https://prometheus.io
[AlertManager]: https://github.com/prometheus/alertmanager
[DingTalk]: https://www.dingtalk.com
[DockerHub]: https://hub.docker.com
[Quay.io]: https://quay.io
