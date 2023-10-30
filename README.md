# midjourney-proxy

代理 MidJourney 的discord频道，实现api形式调用AI绘图

[![GitHub release](https://img.shields.io/static/v1?label=release&message=v2.3&color=blue)](https://www.github.com/novicezk/midjourney-proxy)
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

## 使用前提
1. 注册 MidJourney，创建自己的频道，参考 https://docs.midjourney.com/docs/quick-start
2. 获取用户Token、服务器ID、频道ID：[获取方式](./docs/discord-params.md)

## 风险须知
1. 作图频繁等行为，可能会触发midjourney账号警告，请谨慎使用
2. 为减少风险，请设置`mj.discord.user-agent` 和 `mj.discord.session-id`
3. 默认使用user-wss方式，可以获取midjourney的错误信息、图片变换进度等，但可能会增加账号风险
4. 支持设置mj.discord.user-wss为false，使用bot-token连接wss，需添加自定义机器人：[流程说明](./docs/discord-bot.md)

## Zeabur 部署
基于Zeabur平台部署，不需要自己的服务器: [部署方式](./docs/zeabur-start.md)

配置好的环境可以直接用浏览器访问，大家可以参考下我的：https://mjdemo.zeabur.app/mj/

## Docker 部署
## 请注意这里的配置，原作者的说明中docker用的变量名有问题，会造成提示“无可用的账号实例”的错误。
1. /xxx/xxx/config目录下创建 application.yml(mj配置项)、banned-words.txt(可选，覆盖默认的敏感词文件)；参考src/main/resources下的文件
2. 启动容器，映射config目录
```shell
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -v /xxx/xxx/config:/home/spring/config \
 --restart=always \
 novicezk/midjourney-proxy:2.5.4
```
3. 访问 `http://ip:port/mj` 查看API文档

附: 不映射config目录方式，直接在启动命令中设置参数
```shell
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -e mj_discord_guild_id=xxx \
 -e mj_discord_channel_id=xxx \
 -e mj_discord_user_token=xxx \
 --restart=always \
 novicezk/midjourney-proxy:2.5.4
```
## 配置项
- mj_discord_guild_id：discord服务器ID
- mj_discord_channel_id：discord频道ID
- mj_discord_user_token：discord用户Token
- mj_discord_session-id：discord用户的sessionId，不设置时使用默认的，建议从interactio请求中复制替换掉
- mj_discord_user-agent：调用discord接口、连接wss时的user-agent，默认使用作者的，建议从浏览器network复制替换掉
- mj_discord_user-wss：是否使用user-token连接wss，默认true
- mj_discord_bot-token：自定义机器人Token，user-wss=false时必填
- 更多配置查看 [Wiki / 配置项](https://github.com/novicezk/midjourney-proxy/wiki/%E9%85%8D%E7%BD%AE%E9%A1%B9)

## Wiki链接
1. [Wiki / API接口说明](https://github.com/novicezk/midjourney-proxy/wiki/API%E6%8E%A5%E5%8F%A3%E8%AF%B4%E6%98%8E)
2. [Wiki / 任务变更回调](https://github.com/novicezk/midjourney-proxy/wiki/%E4%BB%BB%E5%8A%A1%E5%8F%98%E6%9B%B4%E5%9B%9E%E8%B0%83)
2. [Wiki / 更新记录](https://github.com/novicezk/midjourney-proxy/wiki/%E6%9B%B4%E6%96%B0%E8%AE%B0%E5%BD%95)

## 注意事项
常见问题及解决办法见 [Wiki / FAQ](https://github.com/novicezk/midjourney-proxy/wiki/FAQ) 
