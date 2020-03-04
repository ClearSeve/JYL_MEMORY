# Vue

```
cnpm install webpack-dev-server -g
cnpm install -g vue-cli

vue init webpack vuetest
进入项目目录vuetest
安装依赖：cnpm install
运行项目：npm run dev

打包：
npm run build            (npm run build:prod)

放入tomcat方法：
修改config\index.js中build节点下的assetsPublicPath: '/prj',
生成文件在dist目录下
生产文件放入tomcat服务器，访问http://127.0.0.1:8080/prj
```
