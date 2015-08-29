# 前端开发者的 Docker 之旅

## 前端发展阶段及 Docker 是什么
>目标：描述近代前端的发展历程，Docker 简明讲解 并阐述 Docker 化对前端的优点

一、简单明快的早期时代
可称之为 Web 1.0 时代，非常适合创业型小项目，不分前后端，经常 3-5 人搞定所有开发。页面由 JSP、PHP 等工程师在服务端生成，浏览器负责展现。基本上是服务端给什么浏览器就展现什么，展现的控制在 Web Server 层。

二、后端为主的 MVC 时代
为了降低复杂度，以后端为出发点，有了 Web Server 层的架构升级，比如 Structs、Spring MVC 等，这是后端的 MVC 时代。

三、Ajax 带来的 SPA 时代
历史滚滚往前，2004 年 Gmail 像风一样的女子来到人间，很快 2005 年 Ajax 正式提出，加上 CDN 开始大量用于静态资源存储，于是出现了 JavaScript 王者归来的 SPA （Single Page Application 单页面应用）时代。

这种模式下，前后端的分工非常清晰，前后端的关键协作点是 Ajax 接口。看起来是如此美妙，但回过头来看看的话，这与 JSP 时代区别不大。复杂度从服务端的 JSP 里移到了浏览器的 JavaScript，浏览器端变得很复杂。类似 Spring MVC，这个时代开始出现浏览器端的分层架构：

四、前端为主的 MV* 时代
为了降低前端开发复杂度，除了 Backbone，还有大量框架涌现，比如 EmberJS、KnockoutJS、AngularJS 等等。

1、前后端职责很清晰。前端工作在浏览器端，后端工作在服务端。清晰的分工，可以让开发并行，测试数据的模拟不难，前端可以本地开发。后端则可以专注于业务逻辑的处理，输出 RESTful 等接口。

2、前端开发的复杂度可控。前端代码很重，但合理的分层，让前端代码能各司其职。这一块蛮有意思的，简单如模板特性的选择，就有很多很多讲究。并非越强大越好，限制什么，留下哪些自由，代码应该如何组织，所有这一切设计，得花一本的厚度去说明。

3、部署相对独立，产品体验可以快速改进。

五、Node 带来的全栈时代
前端为主的 MV* 模式解决了很多很多问题，但如上所述，依旧存在不少不足之处。随着 Node.js 的兴起，JavaScript 开始有能力运行在服务端。这意味着可以有一种新的研发模式：

在这种研发模式下，前后端的职责很清晰。对前端来说，两个 UI 层各司其职：

1. Front-end UI layer 处理浏览器层的展现逻辑。通过 CSS 渲染样式，通过 JavaScript 添加交互功能，HTML 的生成也可以放在这层，具体看应用场景。
2. Back-end UI layer 处理路由、模板、数据获取、cookie 等。通过路由，前端终于可以自主把控 URL Design，这样无论是单页面应用还是多页面应用，前端都可以自由调控。后端也终于可以摆脱对展现的强关注，转而可以专心于业务逻辑层的开发。

通过 Node，Web Server 层也是 JavaScript 代码，这意味着部分代码可前后复用，需要 SEO 的场景可以在服务端同步渲染，由于异步请求太多导致的性能问题也可以通过服务端来缓解。前一种模式的不足，通过这种模式几乎都能完美解决掉。

从 Ajax 带来的 SPA 时代 开始，前端作为一个单独的职业划分开始渐渐的被人们所接受。


other


一. 刀耕火种
1. 静态页面
最早期的Web界面基本都是在互联网上使用，人们浏览某些内容，填写几个表单，并且提交。当时的界面以浏览为主，基本都是HTML代码，有时候穿插一些JavaScript，作为客户端校验这样的基础功能。代码的组织比较简单，而且CSS的运用也是比较少的。
2. 带有简单逻辑的界面
这个界面带有一段JavaScript代码，用于拼接两个输入框中的字符串，并且弹出窗口显示。
3. 结合了服务端技术的混合编程
由于静态界面不能实现保存数据等功能，出现了很多服务端技术，早期的有CGI（Common Gateway Interface，多数用C语言或者Perl实现的），ASP（使用VBScript或者JScript），JSP（使用Java），PHP等等，Python和Ruby等语言也常被用于这类用途。
4.组件化的萌芽
这个时代，也逐渐出现了组件化的萌芽。比较常见的有服务端的组件化，比如把某一类服务端功能单独做成片段，然后其他需要的地方来include进来，典型的有：ASP里面数据库连接的地方，把数据源连接的部分写成conn.asp，然后其他每个需要操作数据库的asp文件包含它。
二. 铁器时代
这个时代的典型特征是Ajax的出现。
1. AJAX
AJAX其实是一系列已有技术的组合，早在这个名词出现之前，这些技术的使用就已经比较广泛了，GMail因为恰当地应用了这些技术，获得了很好的用户体验。
2. JavaScript基础库
Prototype框架主要是为JavaScript代码提供了一种组织方式，对一些原生的JavaScript类型提供了一些扩展，比如数组、字符串，又额外提供了一些实用的数据结构，如：枚举，Hash等，除此之外，还对dom操作，事件，表单和Ajax做了一些封装。
3. 模块代码加载方式
以上这些框架提供了代码的组织能力，但是未能提供代码的动态加载能力。动态加载JavaScript为什么重要呢？因为随着Ajax的普及，jQuery等辅助库的出现，Web上可以做很复杂的功能，因此，单页面应用程序（SPA，Single Page Application）也逐渐多了起来。
三. 工业革命
这个时期，随着Web端功能的日益复杂，人们开始考虑这样一些问题：

如何更好地模块化开发
业务数据如何组织
界面和业务数据之间通过何种方式进行交互
在这种背景下，出现了一些前端MVC、MVP、MVVM框架，我们把这些框架统称为MV*框架。这些框架的出现，都是为了解决上面这些问题，具体的实现思路各有不同，主流的有Backbone，AngularJS，Ember，Spine等等，本文主要选用Backbone和AngularJS来讲述以下场景。
1. 数据模型
在这些框架里，定义数据模型的方式与以往有些差异，主要在于数据的get和set更加有意义了，比如说，可以把某个实体的get和set绑定到RESTful的服务上，这样，对某个实体的读写可以更新到数据库中。另外一个特点是，它们一般都提供一个事件，用于监控数据的变化，这个机制使得数据绑定成为可能。
2. 控制器
在Backbone中，是没有独立的控制器的，它的一些控制的职责都放在了视图里，所以其实这是一种MVP（Model View Presentation）模式，而AngularJS有很清晰的控制器层。

还是以这个todo为例，在AngularJS中，会有一些约定的注入，比如$scope，它是控制器、模型和视图之间的桥梁。在控制器定义的时候，将$scope作为参数，然后，就可以在控制器里面为它添加模型的支持。
3. 视图
在这些主流的MV*框架中，一般都提供了定义视图的功能。
4. 模板
模板是这个时期一种很典型的解决方案。我们常常有这样的场景：在一个界面上重复展示类似的DOM片段，例如微博。以传统的开发方式，也可以轻松实现出来，比如：
5. 路由
通常路由是定义在后端的，但是在这类MV*框架的帮助下，路由可以由前端来解析执行。比
6. 自定义标签
用过XAML或者MXML的人一定会对其中的可扩充标签印象深刻，对于前端开发人员而言，基于标签的组件定义方式一定是优于其他任何方式的，看下面这段HTML：



当然我们这篇文章的重点前端部署

Docker 是运维工具而不是开发工具。

很多前端工程师把 Docker 想象成开发工具，来回推敲自己的开发过程，却找不到任何一个地方需要用到 Docker，所以对于 Docker 他们常常认为这个一个负担。

说实话，Docker 跟前端开发并没有什么关系。
反而 Docker 的引入对前端开发有了更多的要求，让前端开发者需要做开发流程之外的事情。
那么为什么我们还是要使用 Docker ？

说说我，为什么要使用 Docker。

前端部署的各个阶段

### 前端部署之 FTP

以前，我们的项目是用 Angular ＋ grunt 的前端应用。
我们将环境分为 develop, test ,production 三个环境，对应不同的 API Url及配置。
为了方面我们的部署，针对不同的环境我们做了不同的构建任务 
例如：grunt build:develop 、grunt build:test 和 grunt build:production 。
每次前端构建之后，将会生成一系列经过静态文件，将这些静态文件放到 Web 服务器服目录就可以被访问了。

每次需要上线前端项目，我都需要直连到服务器，把构建好的静态文件丢上去。
简单粗暴，但是久而久之问题就来了：

1. 直连服务器，安全肯定得不到保证。
2. 不论哪个环境项目上线都需要我参与。也许让运维也学学前端，那么天下就天平了
3. 上线项目的方式不够灵活，由较大的局限。

### 前端部署之 Jenkins

后来，我司运维引进 Jenkins 据说能实现自动化部署，听着这个自动化就感觉高端大气上档次。不过我司运维话锋一转说配置较为复杂。我想复杂就复杂吧，通过 Jenkins 能实现自动化，解决之前的几个问题，能把我从无尽的运维中拯救出来，让我安安静静地做一个前端就好了。

我司运维经过了 git访问权限配置，定时检测更新配置，更新脚本编写（也就10来行，不过我司运维与我共同写的），发现 Jenkins 就配置的差不多的，但是一运行就报错。他找不到问题，让我来看看。
无奈发现当前主机没有前端构建需要的 node 和 grunt 。也许让运维也学学前端，那么天下就天平了。怎么办，装呗。终于历经千万，我们的自动化流程跑起来了，每次我一上传代码，Jenkins 就能帮我把代码发布到环境上。感觉真好。简直是配置过一次就不想再配置第二次。但是奈何，我们当前的前端项目有4个，继续配置之。

配置好之后的 Jenkins 简直是可爱，帮我从无尽的运维之中解出来，我可以安静的做一个前端了

但是好日子没过多久，随着项目的复杂度增加，相同项目在不同的分支上对第三方依赖的版本也略略有些许区别，导致环境上前端展示的效果和开发展示的效果不同，一狠心，针对 develop 和 release (git分支采用gitflow)分支单独进行配置。但是前端项目有4个，还是都配置吧。这样子 4 * 2 ＝ 8 (master生产环境分支是单独配置的) 简直了，自己挖的坑，哭着也要填完。

（番外：一个月后，服务器坏了，所有的配置的都需要重来一遍，也许让运维也学学前端，那么天下就天平了。）

说说好处：

1. 通过 Jenkins 作为中转避免了直连服务器，
2. Jenkins 实现了代码的自动化部署，只需提交代码便可上线环境
3. Jenkins 提供手动执行任务的功能和权限管理的能力。上线方式相当的灵活。

说说问题：

1. Jenkins 配置繁琐，特别是多个项目多个分支都得配置的时候简直是丧心病狂。
2. Jenkins 执行的命令都是依赖于本机环境，如果出现 项目 A 依赖的 Node 版本是 0.10.10 ，项目 B 依赖的 Node 版本是 0.12.2 ，这个的配置就更加复杂了（我们后来对 项目 A 的 Node 版本进行了升级）

### 前端部署之 Docker 

机缘巧合，我遇到了 [Docker](https://www.docker.com/) ，其实 Docker 和 Jenkins 可以搭配着使用，上述 Jenkins 的一些问题，但是 Jenkins 给我的印象是在是太深刻，我直接放弃了，再加上市面上也有很多基于 Docker 的 CI/CD 工具使用起来都特别的方便，例如： [Daocloud](https://www.daocloud.io/) 和 [Gitlab CI](https://ci.gitlab.com/)（相关链接中有两篇讲解如何搭建持续集成环境文章）。

Docker 是容器管理引擎，他的核心理念就是构建一次，可以运行在任何一地方可以运行容器的地方。

当然引入 Docker 到我们的项目开发部署流程之中，我们在开发中的里面也会做一些调整。根据项目来编写该项目的 Dockerfile ，在本地如果能正常运行，再也不用担心在服务器上会有各种各样由环境不同引发的奇葩问题。由于 Docker Image 是标准的交付件，所以基于 Docker 自动化部署和自动化发布会变得非常方便，结合市面上 Docker CI/CD 厂商，我们能非常方便的实现`开发>测试>部署`自动化。

引入 Docker 也许对前端开发没有帮助，甚至对前端开发来说是需要多掌握一门技能的负担，但是放眼前端开发及部署的整体流程，他将前端项目环境搭建的工作交给了前端，而不是运维，让最熟悉的人做最熟悉的事情，效率成倍提高。
这也是我推崇用 Docker 部署前端的原因。

最后再用本文的核心来作为文章的总结：也许让运维也学学前端，那么天下就天平了。

相关链接 

* [基于Docker 与 DaoCloud 建立简单够用的持续集成环境](https://github.com/Ye-Ting/docker-ci/blob/master/daocloud.md)
* [基于docker 与 gitlab CI 做一个简单够用的持续集成环境](https://github.com/Ye-Ting/docker-ci/blob/master/gitlab.md)
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