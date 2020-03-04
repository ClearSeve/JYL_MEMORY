# npm

## 镜像

使用淘宝镜像安装库
npm install -g  包名 --registry=https://registry.npm.taobao.org

替换npm的源：
npm config set registry https://registry.npm.taobao.org
//npm config get registry  验证当前npm源

或者使用cnpm代替npm  
npm install -g cnpm --registry=https://registry.npm.taobao.org  

## 安装项目依赖

npm install  
在项目目录运行，安装项目依赖到当前目录的node_modules

## 运行

```
启动：
npm run dev  
npm start  

打包：
npm run build  


对应package.json  
"scripts": {
    "dev":"***"
    "start": "npm run dev"  
    "build": "***"
}
```

## 安装模块

npm install **  
安装到当前目录的node_modules  

npm install -g  **  
模块将被下载安装到全局目录中缓存中（C:\Users\jiyan\AppData\Roaming\npm\node_modules）  
全局没有办法用require调用包


## 创建项目

npm init
在交互命令填写项目基本信息，创建package.json文件
npm init -y  直接创建package.json文件
