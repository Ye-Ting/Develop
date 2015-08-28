#前端开发者的 Docker 之旅

##前端发展阶段及Docker是什么
>目标 ： 描述近代前端的发展历程，Docker简明讲解 并阐述 Docker 化对前端的优点


##Hello Docker
> 目标：使用 Hello World Demo 来完成一个简单的 Docker Demo

本项目代码维护在 [front-end-docker-sample](https://github.com/Ye-Ting/front-end-docker-sample) 


##Node Express Docker
> 目标： 使用 Docker 构建一个 Node Express 应用

本项目代码 [node-express-docker-sample](https://github.com/Ye-Ting/node-express-docker-sample)

借助  Yeomen 生成一个 Node Express 应用 [Yeomen Express generator ](https://github.com/petecoop/generator-express)

* 默认暴露 3000 端口 通过环境变量 PORT 修改
* 启动命令 node bin/www
* 调试命令 gulp 

###Node Express 应用启动优化

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

fonts 在 build 的时候不会自动加载，需要修改 bower.json 

```
{
"dependencies": {
	.....
  },
"overrides": {
    "bootstrap-sass-official": {
      "main": [
        "assets/stylesheets/_bootstrap.scss",
        "assets/fonts/bootstrap/glyphicons-halflings-regular.eot",
        "assets/fonts/bootstrap/glyphicons-halflings-regular.svg",
        "assets/fonts/bootstrap/glyphicons-halflings-regular.ttf",
        "assets/fonts/bootstrap/glyphicons-halflings-regular.woff",
        "assets/fonts/bootstrap/glyphicons-halflings-regular.woff2"
      ]
    }
  }
}

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

RUN cp -R /app/dist/*  /usr/share/nginx/html

CMD ["nginx", "-g", "daemon off;"]

```
##Angular Docker Advanced
> 目标：Angular 前端应用的 Docker 实践技巧 

1. Angular 应用根据环境变量切换不同的后端 API
2. Angular 应用根据环境变量切换不同的CDN
3. Angular 应用 Docker 启动加速

本项目代码维护在 [angular-docker-sample](https://github.com/Ye-Ting/angular-docker-sample)

###Angular 应用根据环境变量切换不同的后端 API

使用 [gulp-ng-config](https://www.npmjs.com/package/gulp-ng-config) 

config.json

```
{
  "local": {
    "config": {
      "apiUrl": "http://api.angular-docker-sample.app/api/local"
    }
  },
  "test": {
    "config": {
      "apiUrl": "http://api.angular-docker-sample.app/api/test"
    }
  },
  "production": {
    "config": {
      "apiUrl": "http://api.angular-docker-sample.app/api/production"
    }    
  },
  "needReplace": {
    "config": {
      "apiUrl": "THIS_IS_API_URL_NEED_REPLACE"
    }
  }
}

```

/gulp/env.js

```
gulp.task('env:config', function () {
  var env = process.env.APP_ENV || 'local';   // 暴露 APP_ENV 环境变零

  return gulp.src(
    path.join(conf.paths.src, '/config.json')
  )
    .pipe($.ngConfig('app.config', {
      environment: env
    }))
    .pipe(gulp.dest(path.join(conf.paths.tmp, '/serve/app/')));

});
```

/src/index.module.js

```
(function() {
  'use strict';

  angular
    .module('angularDockerSample', [
    	// core
    	'app.config', // 这里是新添加的
    	// vendor
    	'ngAnimate', 
    	'ngCookies', 
    	'ngTouch', 
    	'ngSanitize', 
    	'ngResource', 
    	'ui.router', 
    	'ui.bootstrap'
    	]);

})();
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

CMD gulp build && cp -r dist/* /usr/share/nginx/html/ && nginx -g 'daemon off;'
```

###Angular 应用根据环境变量切换不同的 CDN

env.js

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

CMD gulp build && gulp cdn && cp -r dist/* /usr/share/nginx/html/ && nginx -g 'daemon off;'
```

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