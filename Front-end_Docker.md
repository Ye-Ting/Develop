#前端开发者的 Docker 之旅

##前端发展阶段及Docker是什么
>目标 ： 描述近代前端的发展历程，Docker简明讲解 并阐述 Docker 化对前端的优点


##Hello Docker
> 目标：使用 Hello World Demo 来完成一个简单的 Docker Demo

本项目代码维护在 [front-end-docker-sample](https://github.com/Ye-Ting/front-end-docker-sample) 


##Node Express Docker
> 目标： 使用 Docker 构建一个 Node Express 应用

本项目代码[node-express-docker-sample](https://github.com/Ye-Ting/node-express-docker-sample)

借助  Yeomen 生成一个 Node Express 应用 [Yeomen Express generator ](https://github.com/petecoop/generator-express)

* 默认暴露 3000 端口 通过环境变量 PORT 修改
* 启动命令 node bin/www
* 调试命令 gulp 


##Angular Docker Docker
> 目标：使用 Docker 构建一个 Angular 前端应用

本项目代码维护在 [angular-docker-sample](https://github.com/Ye-Ting/angular-docker-sample)

借助 Yeoman [generator-gulp-angular](https://github.com/Swiip/generator-gulp-angular) 生成一个 Angular 应用 

```
npm install -g yo gulp bower
npm install -g generator-gulp-angular
yo gulp-angular
```

* 依赖 node bower gulp 
* 调试命令 gulp serve
* 构建命令 gulp build 
* 构建目录 /dist

* 默认暴露 80 端口 

选择 node:0.12.7-wheezy 不会缺少各种各样的东西


##Angular Docker Advanced
> 目标：Angular 前端应用的 Docker 实践技巧   