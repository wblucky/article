Nginx接口服务反向代理基本配置

```nginx
server {
    listen 8443; # 监听的端口号
    server_name a.test.com; # 服务器名称
    client_max_body_size 100m;   # 定义读取客户端请求头的超时时间
    ssl on;
    ssl_certificate test.pem;
    ssl_certificate_key test.key;
    ssl_session_timeout 5m;
    ssl_protocols SSLv3 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES256-SHA384:AES256-SHA256:RC4:HIGH:!MD5:!aNULL:!eNULL:!NULL:!DH:!EDH:!AESGCM;
    ssl_prefer_server_ciphers on;
    location / {
        root /test-static-app; # 静态资源目录
        index index.html index.htm;
        try_files $uri $uri/ /index.html; # 动态解析目录，配合vue的history模式
    }
}
```
基本配置实现了页面及静态服务器的基本功能，并可以实现使用vue的history模式时的路由解析。进一步的，为了实现向接口服务器的统一转发，我们需要和后端开发人员规定接口名的前缀，比如所有接口的相对路径都以api开头，此时我们可以添加如下配置(和上一个location平级)，

```nginx
location /api {
   proxy_pass https://b.test.com; # 设置代理服务器的协议和地址
   proxy_cookie_domain b.test.com  a.test.com; # 修改cookie，针对request和response互相写入cookie
}       
```

其中主要依赖proxy_pass，实现将a.test.com下的/api/x接口转发到了b.test.com下面，这个过程大致如下


cookie的交互主要就是proxy_cookie_domain，加上下面这段

```nginx
proxy_cookie_domain b.test.com  a.test.com; 
```

这个实现了，a.test.com和b.test.com域名之间cookie的传递与回写。这里的理解有点小误区，晚点补充一篇详细解释上来。补上了，看这里：

如果用node来模拟一下的话，大致如下

```js
module.exports =  (router) => {
  router.get('/api/index/getCmsInfo', async function (ctx, next) {
    // 接口转发
    let result = await superagent.post('https://b.test.com/api/card/home').set(browserMsg)
    // 获取返回的set-cookie，并设置header
    let setCookie = result.headers['set-cookie']
    if (setCookie) {
        ctx.response.header['set-cookie'] = setCookie
    }
    // 返回
    ctx.response.body={
        success: true,
        result: result.body 
    }
  })
}
```

综上nginx反向代理的本质其实就是接口服务的转发与header的处理，仔细想想也就容易理解了。

常见误区

1、无用的ACA-Header ？ 网上很多的nginx跨域设置里面都加了跨域header设置相关的内容，比如

```nginx
add_header 'Access-Control-Allow-Origin' '*';
add_header 'Access-Control-Allow-Credentials' "true"; 
add_header Access-Control-Allow-Headers X-Requested-With;
```

想想上面的原理，各位看官觉得这个还有用么？ACA(Access-Control-Allow-)系列的header本身是为了cors中做协商跨域而配置的，在这里配这个纯属脱裤子放屁多此一举。

2、proxy_pass 域名带不带‘斜杠/’ ？ 同样的，在网上看到了有的网友在配置proxy_pass的时候，会在后面加一个斜杠，如下，然后说报错啦，找不到接口啦～咋整啊～

```nginx
...
location /api {
   #proxy_pass https://b.test.com;
   proxy_pass https://b.test.com/;
}       
...
```

看到这个我们来想一想哈，proxy_pass的作用是抓发，加了斜杠意味着所有的**/api请求都会转发到根目录下，也就是说 /api 会被 / 替代，这个时候接口路径就变了，少了一层/api**。而不加斜杠的时候呢？这代表着转发到b.test.com 的域名下，/api的路径不会丢失。
针对这种情况，如果后端接口统一有了规定前缀，比如**/api**，那你这里就不要配置斜杠了。另一种情况，后端接口shit一样，没有统一前缀，这边又要区分，那就在前端所有接口都加一个统一前缀，比如**/api**，然后通过加斜杠来替换掉好了～