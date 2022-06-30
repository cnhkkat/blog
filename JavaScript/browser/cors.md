# 跨域

因为浏览器的**同源策略**导致跨域。只要**协议、域名、端口号**有一个不同就是不同源。


先运行一份前端代码，localhost:9528，
再运行一份后端代码，比如 koa，localhost：9879
## CORS

简单请求：GET、POST、HEAD。非简单请求：会发出一次预检测请求，返回码是`204`，预检测通过才会真正发出请求，这才返回200。这里通过前端发请求的时候增加一个额外的`headers`来触发非简单请求。

**后端**
- 简单请求
  - 设置 `Access-Control-Allow-Origin` 字段的值
  ```js
      // *时cookie不会在http请求中带上
      ctx.set('Access-Control-Allow-Origin', '*')
      ctx.cookies.set('tokenId', '2')
      ctx.body = successBody({msg: query.msg}, 'success')
  ```

- 非简单请求
  - 如果需要http请求中带上`cookie`，需要前后端都设置`credentials`，且后端设置指定的origin
```js
    ctx.set('Access-Control-Allow-Origin', 'http://localhost:9528')
    ctx.set('Access-Control-Allow-Credentials', true)
    ctx.set('Access-Control-Request-Method', 'PUT,POST,GET,DELETE,OPTIONS')
    ctx.set('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept, t')
```

使用 `koa2-cors` 封装

```js
const koa = require('koa')
const app = new koa()

const cors = require('koa2-cors')

const koaStatic = require('koa-static')

// 导入路由
const router = require('./router')

//解析post请求体
const koaBody = require('koa-body')

// 处理静态资源 这里是前端build好之后的目录
app.use(koaStatic(
  path.resolve(__dirname, '../dist')
))

// 接收post参数解析
app.use(
  koaBody({
    multipart: true,
  })
)

// 处理 cors
app.use(
  cors({
    origin: ['http://localhost:9528'],
    credentials: true,
    allowMethods: ['GET', 'POST', 'DELETE'],
    allowHeaders: ['t', 'Content-Type']
  })
)

// 洋葱中间件  123 321 
app.use(async (ctx, next) => {
  console.log('全局中间件')
  ctx.state.env = env
  await next()
})

// 路由
app.use(router.routes()).use(router.allowedMethods())

//监听端口
app.listen(9879)
```
**前端**
```js
// api/playlist.js
import request from "@/utils/request";
const baseURL = "http://localhost:9879";

export function fetchList(params) {
  return request({
    params,
    url: `${baseURL}/playlist/list`,
    method: "get",
  });
}

// list.vue
import { fetchList } from "@/api/playlist";
fetchList({
  start: this.playlist.length,
  count: this.count,
})
  .then((res) => {
    console.log(res);
    this.playlist = this.playlist.concat(res.data);
    if (res.data.length < this.count) {
      scroll.end();
    }
    this.loading = false;
  })
  .catch((err) => {
    this.loading = false;
  });
```

## JSONP

因为 script 、img 获取资源的标签是没有跨域限制的,所以利用script发起请求。
封装前端请求, 用 promise 写法，发送请求的参数，和用来获取数据的方法 cb=jsonpCb
```js
/**
 *
 * @param url 请求的地址
 * @param data 请求的参数
 * @returns
 */
const request = ({ url, data }) => {
  return new Promise((resolve, reject) => {
    // 处理成 key=value&key1=value1
    const handleData = (data) => {
      const keys = Object.keys(data)
      const keysLen = keys.length
      return keys.reduce((pre, cur, index) => {
        const value = data[cur]
        // 用index判断是不是最后一个key
        const split = index !== keysLen - 1 ? '&' : ''
        // pre 连着 cur
        return `${pre}${cur}=${value}${split}`
      }, '')
    }

    // 动态创建script标签
    const script = document.createElement('script')
    // 获取接口返回的数据
    window.jsonpCb = (res) => {
      document.body.removeChild(script)
      delete window.jsonpCb
      resolve(res)
    }
    script.src = `${url}?${handleData(data)}&cb=jsonpCb`
    document.body.appendChild(script)


    // 相当于
    <script src='http://localhost:9879/api/jsonp?msg=helloJsonp&cb=jsonpCb' type='text/javascript'></script>
  })
}

// 使用方式
request({
  url: 'http://localhost:9879/api/jsonp',
  data: {
    // 传参
    msg: 'helloJsonp',
  },
}).then((res) => {
  console.log(res)
})
```

后端处理跨域，接收前端传过来的参数query，把返回的data数据放在**直接执行**的方法 query.cb 的参数里发给前端
```js
// 处理成功失败返回格式的工具
const { successBody } = require('../utli')
class CrossDomain {
  static async jsonp(ctx) {
    // 前端传过来的参数 get请求  msg=helloJsonp&cb=jsonpCb
    const query = ctx.request.query
    // let postData = ctx.request.body (post)
    ctx.cookies.set('tokenId', '1')
    // 后端返回一个方法cb，前端会立马执行，后端返回的数据放在方法的参数里
    ctx.body = `${query.cb}(${JSON.stringify(
      successBody({ msg: query.msg }, 'success')
      // ctx.body = successBody({postData: postData}, 'success')  (post)
    )})`
  }
}
module.exports = CrossDomain
```


## nginx代理

用 nginx 把前端域名的请求转发到后端域名，就在同一个域了

后端

```js
server{
    //监听前端
    listen 9528;
    server_name localhost;
    location ^~ /api {
        // 转发到后端
        proxy_pass http://localhost:9879;
    }    
}
```
