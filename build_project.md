# 打包和部署

平時我們開發時，是在本地做開發，每次打開的都是 http://localhost:8080/#/ 。 而真實的項目中，我們肯定要把項目部署到某個地方。

所以我們需要對項目進行打包和編譯。 

另外，在正式上線之前，也要在測試服務器上進行發佈， 這樣才能看到一些平時在 localhost上看不到的問題。

## 打包

直接使用下面命令,就可以把vue項目打包:

```
$ npm run build
```

這個命令運行過程如下:

```
siwei@siwei-linux:/workspace/test_vue_0613$ npm run build

> test_vue_0613@1.0.0 build /workspace/test_vue_0613
> node build/build.js

⠦ building for production...
Starting to optimize CSS...
Processing static/css/app.32ddfe6eea5926f8e3c760d764fef3fa.css...
Processed static/css/app.32ddfe6eea5926f8e3c760d764fef3fa.css, before: 142, after: 74, ratio: 52.11%
Hash: f89cd58bdaf8a153e13e
Version: webpack 2.6.1
Time: 18658ms
                                                  Asset       Size  Chunks             Chunk Names
                  static/js/app.d8b9f437c302a7070fe7.js     9.1 kB       0  [emitted]  app
               static/js/vendor.33c767135f1684f458a7.js     122 kB       1  [emitted]  vendor
             static/js/manifest.75e2ba037e0bc6934514.js    1.51 kB       2  [emitted]  manifest
    static/css/app.32ddfe6eea5926f8e3c760d764fef3fa.css   74 bytes       0  [emitted]  app
              static/js/app.d8b9f437c302a7070fe7.js.map    63.5 kB       0  [emitted]  app
static/css/app.32ddfe6eea5926f8e3c760d764fef3fa.css.map  623 bytes       0  [emitted]  app
           static/js/vendor.33c767135f1684f458a7.js.map     950 kB       1  [emitted]  vendor
         static/js/manifest.75e2ba037e0bc6934514.js.map    14.6 kB       2  [emitted]  manifest
                                             index.html  522 bytes          [emitted]

  Build complete.

  Tip: built files are meant to be served over an HTTP server.
  Opening index.html over file:// won't work.

```

可以看到： 

1. 整個過程耗時 18.658s
2. 使用的 webpack 版本是 2.6.1
3. 對CSS文件進行了優化.  (優化的比率是 52.11%)
4. 所有的 .vue 文件,都被打包編譯成了下面的文件:

```
$ find ./dist
.
./static
./static/css
./static/css/app.32ddfe6eea5926f8e3c760d764fef3fa.css
./static/css/app.32ddfe6eea5926f8e3c760d764fef3fa.css.map
./static/js
./static/js/vendor.33c767135f1684f458a7.js.map
./static/js/app.d8b9f437c302a7070fe7.js.map
./static/js/manifest.75e2ba037e0bc6934514.js
./static/js/manifest.75e2ba037e0bc6934514.js.map
./static/js/app.d8b9f437c302a7070fe7.js
./static/js/vendor.33c767135f1684f458a7.js
./index.html
```

其中, 包括了 js, css , map 和 index.html

我們需要把它放到 http 服務器上,例如: nginx , apache.

## 部署

### 1. 上傳代碼到遠程服務器

我們使用 scp 或者 ftp方式,可以把代碼上傳到服務器, 假設我們的服務器是 linux,

- 路徑是 :  /opt/app/test_vue
- 服務器ip:  123.255.255.33
- 服務器ssh端口: 6666  
- 服務器用戶名: root


```
$ scp -P 6666 -r dist root@123.255.255.33:/opt/app
index.html                                      100%  528     0.5KB/s   00:00
app.32ddfe6eea5926f8e3c760d764fef3fa.css        100%   74     0.1KB/s   00:00
app.32ddfe6eea5926f8e3c760d764fef3fa.css.map    100%  623     0.6KB/s   00:00
vendor.33c767135f1684f458a7.js.map              100%  927KB 927.3KB/s   00:00
app.d8b9f437c302a7070fe7.js.map                 100%   63KB  62.6KB/s   00:00
manifest.75e2ba037e0bc6934514.js                100% 1511     1.5KB/s   00:00
manifest.75e2ba037e0bc6934514.js.map            100%   14KB  14.3KB/s   00:00
app.d8b9f437c302a7070fe7.js                     100% 9323     9.1KB/s   00:00
vendor.33c767135f1684f458a7.js                  100%  119KB 118.7KB/s   00:00
```

這樣,就把本地的 `dist` 目錄,上傳到了遠程的 /opt/app目錄上.

### 2. 配置遠程服務器

2.1 登陸遠程服務器:

```
$ ssh root@123.255.255.23 -p 6666
(輸入密碼， 回車)

Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.13.0-86-generic x86_64)

root@my_server:~#

```

2.2 把剛纔上傳的文件夾重命名成: `vue_demo`:

```
# mv /opt/app/dist /opt/app/vue_demo
```

2.3 配置nginx, 使 域名: `vue_demo.siwei.me` 指向該位置:

把下面代碼,加入到nginx的配置文件中(`/etc/nginx/nginx.conf`)

```
  server {
          listen       80;
          server_name  vue_demo.siwei.me;
          client_max_body_size       500m;
          charset utf-8;
          root /opt/app/vue_demo;
  }

```

重啓nginx之前,測試一下剛纔加入的代碼是否有問題:

```
# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

可以看到,沒問題.

2.4 然後重啓nginx :

```
# nginx -s stop
# nginx
```


### 3. 修改域名配置

nginx跑起來之後, 我們就要配置域名. 否則無法訪問.

3.1 我們需要增加個二級域名:  vue_demo.siwei.me

以dnspod爲例, 需要設置這個二級域名的: A記錄. IP地址:

![增加二級域名vue_demo.siwei.me](./images/add_a_record.png)

保存.

3.2 回到命令行, 輸入 ping 命令,確認:

```
$ ping vue_demo.siwei.me
PING vue_demo.siwei.me (123.57.235.33) 56(84) bytes of data.
64 bytes from 123.57.235.33: icmp_seq=1 ttl=54 time=5.79 ms
64 bytes from 123.57.235.33: icmp_seq=2 ttl=54 time=6.38 ms
64 bytes from 123.57.235.33: icmp_seq=3 ttl=54 time=9.25 ms
```

說明 我的二級域名 `vue_demo.siwei.me` 已經可以正常指向到 我的服務器ip了.

### 4. 部署完成!

打開瀏覽器, 訪問 http://vue_demo.siwei.me 就可以看到效果了，如下圖所示:

![vuejs_vue_demo](./images/vuejs_vue_demo.siwei.me.png)
