# NeteaseCloudMusicApi

网易云音乐 NodeJS 版 API

## 灵感来自

[disoul/electron-cloud-music](https://github.com/disoul/electron-cloud-music)

[darknessomi/musicbox](https://github.com/darknessomi/musicbox)

[sqaiyan/netmusic-node](https://github.com/sqaiyan/netmusic-node)

## 工作原理

跨站请求伪造 (CSRF), 伪造请求头 , 调用官方 API

## 功能特性

1. 登录
2. 刷新登录
3. 发送验证码
4. 校验验证码
5. 注册(修改密码)
6. 获取用户信息 , 歌单，收藏，mv, dj 数量
7. 获取用户歌单
8. 获取用户电台
9. 获取用户关注列表
10. 获取用户粉丝列表
11. 获取用户动态
12. 获取用户播放记录
13. 获取精品歌单
14. 获取歌单详情
15. 搜索
16. 搜索建议

## 安装

```shell
$ git clone git@github.com:Binaryify/NeteaseCloudMusicApi.git

$ npm install
```

## 运行

```shell
$ node app.js
```

服务器启动默认端口为 3000, 若不想使用 3000 端口 , 可使用以下命令 : Mac/Linux

```shell
$ PORT=4000 node app.js
```

windows 下使用 git-bash 或者 cmder 等终端执行以下命令 :

```shell
$ set PORT=4000 && node app.js
```

## 可以使用代理

在 query 参数中加上 proxy=your-proxy 即可让这一次的请求使用 proxy

```javascript
// 例子
const url = `http://localhost:3000/song/url?id=33894312&proxy=http://121.196.226.246:84`;
fetch(url).then(function() {
  // do what you want
});

// 结果
// {"data":[{"id":33894312,"url":"http://m10.music.126.net/20180104125640/930a968b3fb04908b733506b3833e60b/ymusic/0fd6/4f65/43ed/a8772889f38dfcb91c04da915b301617.mp3","br":320000,"size":10691439,"md5":"a8772889f38dfcb91c04da915b301617","code":200,"expi":1200,"type":"mp3","gain":-2.0E-4,"fee":0,"uf":null,"payed":0,"flag":0,"canExtend":false}],"code": 200}
```

v3.3.0 后支持使用 PAC 代理,如 `?proxy=http://192.168.0.1/proxy.pac`

## 更新到 v3.0 说明

!>2018.10.14 更新到 3.0.0,使用了模块化机制,因为部分接口参数和 url 做了调整,如还不想升级到 3.0.0,请查看 [v2 的文档](http://binaryify.github.io/NeteaseCloudMusicApi/#/v2), [更新日志](https://github.com/Binaryify/NeteaseCloudMusicApi/blob/master/CHANGELOG.MD), [2.0+下载地址](https://github.com/Binaryify/NeteaseCloudMusicApi/releases/tag/v2.20.5), 同时 2.0+ 将不再维护

## Docker 容器运行

> 注意: 在 docker 中运行的时候, 由于使用了 request 来发请求, 所以会检查几个 proxy 相关的环境变量(如下所列), 这些环境变量 会影响到 request 的代理, 详情请参考[request 的文档](https://github.com/request/request#proxies), 如果这些环境变量 指向的代理不可用, 那么就会造成错误, 所以在使用 docker 的时候一定要注意这些环境变量. 不过, 要是你在 query 中加上了 proxy 参数, 那么环境变量会被覆盖, 就会用你通过 proxy 参数提供的代理了.

request 相关的环境变量

1. http_proxy
2. https_proxy
3. HTTP_PROXY
4. HTTPS_PROXY
5. no_proxy
6. NO_PROXY

```shell
docker pull binaryify/netease_cloud_music_api

docker run -d -p 3000:3000 --name netease_cloud_music_api    binaryify/netease_cloud_music_api


// 或者
docker run -d -p 3000:3000 binaryify/netease_cloud_music_api

// 去掉或者设置相关的环境变量

docker run -d -p 3000:3000 --name netease_cloud_music_api -e http_proxy= -e https_proxy= -e no_proxy= -e HTTP_PROXY= -e HTTPS_PROXY= -e NO_PROXY= binaryify/netease_cloud_music_api

// 或者
docker run -d -p 3000:3000 -e http_proxy= -e https_proxy= -e no_proxy= -e HTTP_PROXY= -e HTTPS_PROXY= -e NO_PROXY= binaryify/netease_cloud_music_api
```

> 以下是自行 build docker 镜像方式

```
$ git clone https://github.com/Binaryify/NeteaseCloudMusicApi && cd NeteaseCloudMusicApi

$ sudo docker build . -t netease-music-api

$ sudo docker run -d -p 3000:3000 netease-music-api
```

## 接口文档

### 调用前须知

!> 本项目不提供线上 demo，请不要轻易信任使用他人提供的公开服务，以免发生安全问题,泄露自己的账号和密码

!> 为使用方便,降低门槛, 文档示例接口直接使用了 GET 请求,本项目同时支持 GET/POST 请按实际需求使用

!> 由于接口做了缓存处理 ( 缓存 2 分钟,不缓存数据极容易引起网易服务器高频 ip 错误 , 可在 app.js 设置 , 可能会导致登陆后获取不到 cookie), **相同的 url** 会在两分钟内只向网易服务器发一次请求 , 如果遇到不需要缓
存结果的接口 , 可在请求 url 后面加一个时间戳参数使 url 不同 , 例子 :
`/simi/playlist?id=347230&timestamp=1503019930000` (之所以加入缓存机制是因为项目早期没有缓存机制，很多 issues 都是报 IP 高频，请按自己需求改造缓存中间件(app.js)，源码不复杂)

!> 如果是跨域请求 , 请在所有请求带上 `xhrFields: { withCredentials: true }` (axios 为 `withCredentials: true`)否则
可能会因为没带上 cookie 导致 301, 具体例子可看 `public/test.html`, 例子使用 jQuery 和 axios

!> 301 错误基本都是没登录就调用了需要登录的接口,如果登陆了还是提示 301, 基本都是缓存把数据缓存起来了,解决方法是加时间戳或者等待 2 分钟或者重启服务重新登录后再调用接口,可自行改造缓存方法

!> 部分接口如登录接口不能调用太频繁 , 否则可能会触发 503 错误或者 ip 高频错误 ,若需频繁调用 , 需要准备 IP 代理池 (更新:已加入缓存机制,但仍需注意).

!> 本项目仅供学习使用,请尊重版权，请勿利用此项目从事商业行为

!> 文档可能会有缓存 , 如果文档版本和 github 上的版本不一致,请清除缓存再查看

!> 由于网易限制,此项目在国外服务器上使用会受到限制,如需解决 , 可使用大陆服务器或者使用代理 , 感谢 [@hiyangguo](https://github.com/hiyangguo)提出的[解决方法](https://github.com/Binaryify/NeteaseCloudMusicApi/issues/29#issuecomment-298358438):
在 '/util/request.js' 的 'headers' 处增加 `X-Real-IP':'211.161.244.70' // 任意国内 IP`
即可解决

!> 图片加上 `?param=宽y高` 可控制图片尺寸，如 `http://p4.music.126.net/JzNK4a5PjjPIXAgVlqEc5Q==/109951164154280311.jpg?param=200y200`, `http://p4.music.126.net/JzNK4a5PjjPIXAgVlqEc5Q==/109951164154280311.jpg?param=50y50`

### 登录

说明 : 登录有两个接口

#### 1. 手机登录

**必选参数 :** `phone`: 手机号码 `password`: 密码

**接口地址 :** `/login/cellphone`

**可选参数 :** `countrycode`: 国家码，用于国外手机号登陆，例如美国传入：`1`

**调用例子 :** `/login/cellphone?phone=xxx&password=yyy`

#### 2. 邮箱登录

~~ 注意 : 此接口被网易和谐了 , 待修复 , 暂时使用手机登录 (2017.05.20)~~

> 更新 : 此接口已经可以正常使用(2018.07.03)

**必选参数 :** `email`: 163 网易邮箱  
`password`: 密码

**接口地址 :** `/login`

**调用例子 :** `/login?email=xxx@163.com&password=yyy`

返回数据如下图 :
![登录](https://raw.githubusercontent.com/Binaryify/NeteaseCloudMusicApi/master/static/%E7%99%BB%E5%BD%95.png)

完成登录后 , 会在浏览器保存一个 Cookies 用作登录凭证 , 大部分 API 都需要用到这个
Cookies

#### 注意

调用登录接口的速度比调用其他接口慢 , 因为登录过程调用了加密算法

### 刷新登录

说明 : 调用此接口 , 可刷新登录状态

**调用例子 :** `/login/refresh`

### 发送验证码

说明 : 调用此接口 ,传入手机号码, 可发送验证码

**必选参数 :** `phone`: 手机号码

**可选参数 :**
`ctcode`: 国家区号,默认 86 即中国

**接口地址 :** `/captcha/sent`

**调用例子 :** `/captcha/sent?phone=13xxx`

## License

[The MIT License (MIT)](https://github.com/Binaryify/NeteaseCloudMusicApi/blob/master/LICENSE)
