koa 

// session
var session = require('koa-generic-session');
var redisStore = require('koa-redis');

app.use(session({
    secret: 'XXX',
    proxy: true,
    resave: false,
    saveUninitialized: true,
    cookie: {
        path: '/',
        httpOnly: true,
        maxage: null,
        rewrite: true,
        signed: true
    },
    // TODO need a memory session store that support ttl
    store: new redisStore({
        host: localhost,
        port: '6379',
        ttl: 10*60*60*24 //设置10天超期
    })
}));
// 本地启动 redis： redis-server 
// key: cookie name defaulting to koa.sid
// Koa 应用是一个包含一系列中间件 generator 函数的对象。 这些中间件函数基于 request 请求以一个类似于栈的结构组成并依次执行。

koa 得好好捋捋...
