# 前端开发者的 Docker 之旅

## 前端发展阶段
>目标：描述近代前端的发展历程，引出前端部署问题

## 也许让运维也学学前端，那么天下就天平了
>目标：定义 Docker 与前端的关系，介绍前端部署的几种方式，阐述 Docker 化的前端的优点所在

Close this.

## Hello Docker
> 目标：使用静态页面来完成一个简单的 Docker Demo

Close this.

## Node Express Docker
> 目标： 用 Docker 镜像的方式搭建 Node Express 应用

Close this.

## Angular Docker
> 目标：用 Docker 镜像的方式搭建 Angular 前端应用

Close this.

##Angular Docker Advanced
> 目标：Angular 前端应用的 Docker 实践技巧 

1. Angular 应用根据环境变量切换不同的后端 API
2. Angular 应用根据环境变量切换不同的 CDN
3. Angular 应用 Docker 启动加速

本项目代码维护在 [angular-docker-sample](https://github.com/Ye-Ting/angular-docker-sample)

引入 Docker 之后我们的前端构建过程也需要做相应的变化，那就是基于环境变量，
下面我基于在做 Angular Docker 之初遇到的几个较为困难的问题做了一些讲解。

### Angular 应用根据环境变量切换不同的后端 API

Close this.

###Angular 应用根据环境变量切换不同的 CDN

在 Angular 项目在实际运用中，我们需要静态文件放在 CDN 上

怎么通过单一的 Docker image 实现 CDN 地址可变呢？

我们基于 [Angular Docker]() 来做一些改进。

gulp 是一个很好用的构建工具，不紧是因为他的语法灵活，更是因为有非常多的插件我供我们使用。

在这个例子中使用 [gulp-cdnizer](https://www.npmjs.com/package/gulp-cdnizer) 

### Angular 优化

首先，安装插件 `gulp-cdnizer`

```
npm install gulp-ng-config --save-dev
```

接着，我们在 `/gulp/env.js` 文件下编写相应的 Task

```
gulp.task('cdn', function () {

  var cdn = process.env.APP_CDN || '';

  return gulp.src(
    path.join(conf.paths.dist, '*.html')
  )
    .pipe($.cdnizer({
      defaultCDNBase: cdn,
      files         : [
        'scripts/*.js',
        'styles/*.css'
      ]
    }))
    .pipe(gulp.dest(
      path.join(conf.paths.dist)
    ));
});

```

### 构建 Docker Image

对 Dockerfile 启动命令 CMD 进行细微的调整。

```
FROM node:0.12.7-wheezy

RUN apt-key adv --keyserver pgp.mit.edu --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
RUN echo "deb http://nginx.org/packages/mainline/debian/ wheezy nginx" >> /etc/apt/sources.list

ENV NGINX_VERSION 1.7.12-1~wheezy

RUN apt-get update && \
    apt-get install -y ca-certificates nginx && \
    rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80

RUN npm install -g bower gulp

WORKDIR /app

COPY ./package.json /app/
COPY ./bower.json /app/
RUN npm install && bower install --allow-root

COPY . /app/

CMD gulp build && gulp cdn && cp -r dist/* /usr/share/nginx/html/ && nginx -g 'daemon off;'
```

有了 Dockerfile 以后，我们可以运行下面的命令构建前端镜像并命名为 my-angular-cdn-app：

```bash
docker build -t my-angular-cdn-app .
```

### 部署 Docker Image

最后，让我们从镜像启动容器：

```
docker run -p 80:80 -e "APP_CDN=这里填写你自己的CDN" my-angular-cdn-app
```

这样子我们就能从 80 端口去访问我们的 Angular 应用，并且该 Angular 应用是对应着不同的 apiUrl。


###Angular 应用 Docker 启动加速

env.js

```
gulp.task('env:config', function () {
  var env = process.env.APP_ENV || 'needReplace';

  return gulp.src(
    path.join(conf.paths.src, '/config.json')
  )
    .pipe($.ngConfig('app.config', {
      environment: env
    }))
    .pipe(gulp.dest(path.join(conf.paths.tmp, '/serve/app/')));

});

gulp.task('env:replace', ['cdn'], function () {
  var envConfig   = jsonfile.readFileSync(path.join(conf.paths.src,
    '/config.json'));
  var needReplace = envConfig.needReplace;
  var env         = process.env.APP_ENV || 'local';

  var stream = gulp.src(
    path.join(conf.paths.dist, '/scripts/*.js')
  );
  for (var key in needReplace.config) {
    stream = stream.pipe(
      $.replace(
        needReplace.config[key],
        envConfig[env]['config'][key])
    );
    console.log(key, needReplace.config[key]);
    console.log(key, envConfig[env]['config'][key]);
  }

  return stream.pipe(gulp.dest(path.join(conf.paths.dist, '/scripts')));

});
```

Dockerfile

```

FROM node:0.12.7-wheezy

RUN apt-key adv --keyserver pgp.mit.edu --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
RUN echo "deb http://nginx.org/packages/mainline/debian/ wheezy nginx" >> /etc/apt/sources.list

ENV NGINX_VERSION 1.7.12-1~wheezy

RUN apt-get update && \
    apt-get install -y ca-certificates nginx && \
    rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80

RUN npm install -g bower gulp

WORKDIR /app

COPY ./package.json /app/
COPY ./bower.json /app/
RUN npm install && bower install --allow-root

COPY . /app/

RUN gulp build 

CMD gulp env:replace && cp -r dist/* /usr/share/nginx/html/ && nginx -g 'daemon off;'
```

##终极 Docker 调试 DaoCloud 