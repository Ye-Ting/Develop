#前端开发者的 Docker 之旅

##前端发展阶段及Docker是什么
>目标：描述近代前端的发展历程，Docker 简明讲解 并阐述 Docker 化对前端的优点

Docker 是运维工具而不是开发工具。

很多前端工程师把 Docker 想象成开发工具，来回推敲自己的开发过程，却找不到任何一个地方需要用到 Docker，所以对于 Docker 他们常常认为这个一个负担。

说实话，Docker 跟前端开发并没有什么关系。
反而 Docker 的引入对前端开发有了更多的要求，让前端开发者需要做开发流程之外的事情。
那么为什么我们还是要上 Docker ？

说说我，为什么要上Docker

前端部署的各个阶段

### 前端部署之 FTP

以前，我们的项目是用 Angular ＋ grunt 构建，我们将环境分为 develop, test ,production 三个环境，对应不同的 API Url ，为了方面我们的部署，针对不同的环境我们做了不同的构建任务 
像 grunt build:develop , grunt build:test ,grunt build:production .

每次需要上线前端项目，我都需要直连到服务器，把构建好的文件丢上去。
看上去还是蛮简单的，但是久而久之问题就来了：
1. 直连服务器，安全肯定得不到保证。
2. 不论哪个环境的项目上线都需要我在场，也许让运维也学学前端，那么天下就天平了
3. 另一个人开发的功能，想要上线进行测试，不是特别灵活。

### 前端部署之 Jenkins

后来，我司运维引进 Jenkins 能实现自动化部署，听着这个自动化就感觉高端大气上档次。不过我司运维话锋一转说配置较为复杂，我想复杂就复杂吧，能实现自动化，解决我上面说的几个问题就好了，能把我从无尽的运维中拯救出来，让我安安静静地做一个前端就好了。

我司运维经过了 git访问权限配置，定时检测更新配置，更新脚本编写（也就10来行，不过我司运维与我共同写的），发现 Jenkins 就配置的差不多的，但是一运行就报错。他找不到问题，让我来看看。
无奈发现当前主机没有前端构建需要的 node 和 grunt ，也许让运维也学学前端，那么天下就天平了，怎么帮，装呗。终于历经千万，我们的自动话流程跑起来了，每次我一上传代码，自动化的流程就帮我把代码发布到环境上。感觉真好。简直是配置过一次就不想再配置第二次。但是奈何，我们当前的前端项目有4个，继续配置之。

配置好之后的 Jenkins 简直是可爱，帮我从无尽的运维之中解出来，我可以安静的做一个前端了

但是好日子没过多久，随着项目的复杂度增加，相同项目在不同的分支上对第三方依赖的版本也略略有些许区别，导致环境上前端展示的效果和开发展示的效果不同，一狠心，针对 develop 和 release (git分支采用gitflow)分支单独进行配置。但是前端项目有4个，还是都配置吧。这样子 4 * 2 ＝ 8 (master生产环境分支是单独配置的) 简直了，自己挖的坑，哭着也要填完。

（番外：一个月后，服务器坏了，所有的配置的都需要重来一遍，也许让运维也学学前端，那么天下就天平了。）

优点：
1. 通过 Jenkins 作为中转避免了直连服务器，
2. Jenkins 实现了代码的自动化部署，只需提交代码便可上线环境
3. Jenkins 提供手执行任务的能力和权限管理的能力。

缺点：
1. Jenkins 配置繁琐，特别是多个项目多个分支都得配置的时候简直是丧心病狂。
2. Jenkins 执行的命令都是依赖于本机环境，如果出现 项目 A 依赖的 Node 版本是 0.10.10 ，项目 B 依赖的 Node 版本是 0.12.2 ，这个的配置就更加复杂了（我们后来对 项目 A 的 Node 版本进行了升级）

### 前端部署之 Docker 

机缘巧合，我遇到了 [Docker](https://www.docker.com/) ，其实 Docker 和 Jenkins 可以搭配着使用，上述 Jenkins 的一些问题，但是 Jenkins 给我的印象是在是太深刻，我直接放弃了，再加上市面上也有很多基于 Docker 的 CI/CD 工具使用起来都特别的方便，例如：Gitlab CI 和 Daocloud （这里有两篇介绍的文章）。

Docker 是容器，他的核心理念就是构建一次，可以运行在任何一地方可以运行容器的地方。当然引入 Docker 到我们的项目发布流程之中，我们在开发中的里面也会做一些调整。根据项目来编写该项目的 Dockerfile ，在本地如果能正常运行，再也不用担心在服务器上会有各种各样由环境不同引发的奇葩问题。由于 Docker Image 是标准的交付件，所以基于 Docker 自动化部署和自动化发布会变得非常方便，结合市面上 Docker CI/CD 厂商，开发就能具备基本的运维能力。

引入 Docker 也许对前端开发没有帮助，甚至对前端开发来说是需要多掌握一门技能的负担，但是放眼前端开发及部署的整体流程，他将前端项目环境搭建的工作交给了前端，而不是运维，让最熟悉的人做最熟悉的事情，效率成倍提高。
这也是我推崇用 Docker 部署前端的原因。

最后再用本文的核心来作为文章的总结：也许让运维也学学前端，那么天下就天平了。

相关链接 

* [Web研发模式的演变](https://github.com/lifesinger/lifesinger.github.com/issues/184)
* [研发模式的思考](https://github.com/aralejs/aralejs.org/issues/50)

##Hello Docker
> 目标：使用 Hello World Demo 来完成一个简单的 Docker Demo

本项目代码维护在 [front-end-docker-sample](https://github.com/Ye-Ting/front-end-docker-sample) 

Dockerfile

```
FROM nginx

COPY . /usr/share/nginx/html

```

##Node Express Docker
> 目标： 使用 Docker 构建一个 Node Express 应用

本项目代码 [node-express-docker-sample](https://github.com/Ye-Ting/node-express-docker-sample)

借助  Yeomen 生成一个 Node Express 应用 [Yeomen Express generator ](https://github.com/petecoop/generator-express)

* 默认暴露 3000 端口 通过环境变量 PORT 修改
* 启动命令 node bin/www
* 调试命令 gulp 

Dockerfile

```
FROM node:0.12.7-wheezy

WORKDIR /app

COPY ./package.json /app/

RUN npm install

COPY . /app/
 
EXPOSE 3000

CMD node bin/www 
```

###Node Express 应用运行优化

Dockerfile 

```
FROM node:0.12.7-wheezy

WORKDIR /app

RUN npm install -g forever

COPY ./package.json /app/

RUN npm install

COPY . /app/
 
EXPOSE 3000

CMD forever bin/www 
```

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