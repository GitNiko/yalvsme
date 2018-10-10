私有npm的对比
=======================

企业需求私有的npm管理，这时候有两个选择，一个付费使用`npm.org`提供的私有服务，另一个则是使用开源的npm管理工具，在自己的内网中部署。  
本文主要针对常用的三个开源npm管理工具作对比。  

## Package Registry specification 
> HTTP GET is the only method required for consuming the data in a package registry. Registries MAY use other methods for entering data into the registry, authenticating user accounts, and so on, but that is outside the scope of this specification.    
Clients SHOULD send an Accept header of application/json. Behavior of the registry in the presence of other Accept headers is undefined. For instance, a Compliant registry MAY send an HTML document description of a package if given the Accept value of text/html, or it MAY send JSON in all cases.    
 Registries MUST send responses with an application/json Content-Type if the client sends an Accept header value of application/json.

总结下`Registry`主要包含以下规定:  
- 全部是 `GET` 请求
- response的`Content-Type`是 `application/json`  
- 存在一个`registry root url`，可以通过该url，传入包名，版本可以唯一确定一个包   
- 存在一个`{registry root url}/{package name}`可以获得包的信息，其中必须包含`name`和`version`  
- 存在一个`{registry root url}/{package name}/{package version}`可以获得指定版本的包的信息，其中必须包含`name`和`version`和`dist`以及`tarball`  

npm上的`{registry root url}/{package name}`的`response body`（https://registry.npmjs.org/rct-form）：
```js
{
    "_id": "rct-form", 
    "_rev": "12-9035357c5d2e717e14360e6761b6e6ea", 
    "name": "rct-form", 
    "description": "simply create form with redux-form", 
    "dist-tags": {
        "latest": "0.5.0"
    }, 
    "versions": { 
        "0.5.0": {
            "name": "rct-form", 
            "version": "0.5.0", 
            "description": "simply create form with redux-form", 
            "main": "dist/index.js", 
            "scripts": {
                "test": "jest && codecov", 
                "local.test": "jest --coverage", 
                "retest": "jest --updateSnapshot", 
                "start": "webpack-dev-server", 
                "build": "NODE_ENV=production webpack --profile --colors --display-modules --config webpack.prod.js", 
                "precommit": "lint-staged"
            }, 
            "lint-staged": {
                "*.js": [
                    "prettier --write", 
                    "git add"
                ]
            }, 
            "repository": {
                "type": "git", 
                "url": "git+https://github.com/linehat/rct-form.git"
            }, 
            "keywords": [
                "redux-form", 
                "react"
            ], 
            "author": {
                "name": "GitNiko"
            }, 
            "license": "MIT", 
            "bugs": {
                "url": "https://github.com/linehat/rct-form/issues"
            }, 
            "homepage": "https://github.com/linehat/rct-form#readme", 
            "devDependencies": {
                "babel-core": "^6.26.0", 
                "babel-eslint": "^7.2.3", 
                "babel-jest": "^21.2.0", 
                "babel-loader": "^7.1.2", 
                "babel-plugin-transform-class-properties": "^6.24.1", 
                "babel-plugin-transform-decorators-legacy": "^1.3.4", 
                "babel-plugin-transform-object-rest-spread": "^6.26.0", 
                "babel-preset-env": "^1.6.0", 
                "babel-preset-react": "^6.24.1", 
                "babel-runtime": "^6.26.0", 
                "codecov": "^2.2.0", 
                "css-loader": "^0.28.4", 
                "html-webpack-plugin": "^2.30.1", 
                "husky": "^0.13.4", 
                "jest": "^21.2.1", 
                "lint-staged": "^4.0.0", 
                "prettier": "^1.4.4", 
                "prop-types": "^15.6.0", 
                "react": "^16.0.0", 
                "react-dom": "^16.0.0", 
                "react-hot-loader": "next", 
                "react-redux": "^5.0.6", 
                "react-test-renderer": "^16.2.0", 
                "redux": "^3.7.2", 
                "redux-form": "^7.0.4", 
                "webpack": "^3.6.0", 
                "webpack-dev-server": "^2.9.1"
            }, 
            "peerDependencies": {
                "prop-types": "^15.6.0", 
                "react": "^16.0.0 || ^15.6.0", 
                "react-redux": "^5.0.6", 
                "redux": "^3.7.2", 
                "redux-form": "^7.0.4"
            }, 
            "gitHead": "10fd5ba5184e8189ecc06659672d7235039d55dd", 
            "_id": "rct-form@0.5.0", 
            "_shasum": "6fba2b88d539731c0b58cbd31c7c6cf31bfab70c", 
            "_from": ".", 
            "_npmVersion": "3.10.10", 
            "_nodeVersion": "6.12.0", 
            "_npmUser": {
                "name": "gitniko", 
                "email": "galaxis.ling@gmail.com"
            }, 
            "dist": {
                "shasum": "6fba2b88d539731c0b58cbd31c7c6cf31bfab70c", 
                "tarball": "https://registry.npmjs.org/rct-form/-/rct-form-0.5.0.tgz"
            }, 
            "maintainers": [
                {
                    "name": "gitniko", 
                    "email": "galxis.ling@gmail.com"
                }
            ], 
            "_npmOperationalInternal": {
                "host": "s3://npm-registry-packages", 
                "tmp": "tmp/rct-form-0.5.0.tgz_1512368579976_0.1860107914544642"
            }, 
            "directories": { }
        }
    }, 
    "readme": "[![CircleCI](https://img.shields.io/circleci/project/linehat/rct-form/release.svg)](https://circleci.com/gh/linehat/rct-form/tree/release)
    [![npm](https://img.shields.io/npm/v/rct-form.svg)](https://www.npmjs.com/package/rct-form)
    [![npm](https://img.shields.io/npm/dm/rct-form.svg)](https://www.npmjs.com/package/rct-form)
    # rct-form
    simply create form with redux-form",
    "maintainers": [
        {
            "name": "gitniko", 
            "email": "galxis.ling@gmail.com"
        }
    ], 
    "time": {
        "modified": "2017-12-30T06:23:34.361Z", 
        "created": "2017-10-09T03:04:25.175Z", 
        "0.1.0": "2017-10-09T03:04:25.175Z", 
        "0.1.1": "2017-10-09T03:17:43.905Z", 
        "0.1.2": "2017-10-09T05:50:23.883Z", 
        "0.2.0": "2017-10-11T03:18:03.921Z", 
        "0.3.0": "2017-10-11T05:40:19.388Z", 
        "0.3.1": "2017-10-17T06:30:38.921Z", 
        "0.5.0": "2017-12-04T06:23:00.180Z"
    }, 
    "homepage": "https://github.com/linehat/rct-form#readme", 
    "keywords": [
        "redux-form", 
        "react"
    ], 
    "repository": {
        "type": "git", 
        "url": "git+https://github.com/linehat/rct-form.git"
    }, 
    "author": {
        "name": "GitNiko"
    }, 
    "bugs": {
        "url": "https://github.com/linehat/rct-form/issues"
    }, 
    "license": "MIT", 
    "readmeFilename": "README.md", 
    "users": {
        "gitniko": true
    }, 
    "_attachments": { }
}
```

npm上`{registry root url}/{package name}/{package version}`的`response body`（https://registry.npmjs.org/rct-form/0.1.0）:
```js
{
	"name": "rct-form",
	"version": "0.1.0",
	"description": "simply create form with redux-form",
	"main": "dist/index.js",
	"scripts": {
		"test": "jest && codecov",
		"retest": "jest --updateSnapshot",
		"start": "webpack-dev-server",
		"build": "NODE_ENV=production webpack --profile --colors --display-modules --config webpack.prod.js",
		"precommit": "lint-staged"
	},
	"lint-staged": {
		"*.js": [
			"prettier --write",
			"git add"
		]
	},
	"repository": {
		"type": "git",
		"url": "git+https://github.com/linehat/rct-form.git"
	},
	"keywords": [
		"redux-form",
		"react"
	],
	"author": {
		"name": "GitNiko"
	},
	"license": "MIT",
	"bugs": {
		"url": "https://github.com/linehat/rct-form/issues"
	},
	"homepage": "https://github.com/linehat/rct-form#readme",
	"devDependencies": {
		"babel-core": "^6.26.0",
		"babel-eslint": "^7.2.3",
		"babel-jest": "^20.0.3",
		"babel-loader": "^7.1.2",
		"babel-plugin-transform-class-properties": "^6.24.1",
		"babel-plugin-transform-decorators-legacy": "^1.3.4",
		"babel-plugin-transform-object-rest-spread": "^6.26.0",
		"babel-preset-env": "^1.6.0",
		"babel-preset-react": "^6.24.1",
		"babel-runtime": "^6.26.0",
		"codecov": "^2.2.0",
		"css-loader": "^0.28.4",
		"html-webpack-plugin": "^2.30.1",
		"husky": "^0.13.4",
		"jest": "^20.0.4",
		"lint-staged": "^4.0.0",
		"prettier": "^1.4.4",
		"prop-types": "^15.6.0",
		"react": "^15.6.1",
		"react-dom": "^15.6.1",
		"react-hot-loader": "next",
		"react-redux": "^5.0.6",
		"redux": "^3.7.2",
		"redux-form": "^7.0.4",
		"webpack": "^3.6.0",
		"webpack-dev-server": "^2.9.1"
	},
	"peerDependencies": {
		"prop-types": "^15.6.0",
		"react": "^15.6.1",
		"react-redux": "^5.0.6",
		"redux": "^3.7.2",
		"redux-form": "^7.0.4"
	},
	"gitHead": "c4f374629c216a5043db8124fb34e8906eb283ee",
	"_id": "rct-form@0.1.0",
	"_shasum": "3fafe9b358785248ddf3fdc95018b521bced8d7a",
	"_from": ".",
	"_npmVersion": "3.10.10",
	"_nodeVersion": "6.10.2",
	"_npmUser": {
		"name": "gitniko",
		"email": "galxis.ling@gmail.com"
	},
	"dist": {
		"shasum": "3fafe9b358785248ddf3fdc95018b521bced8d7a",
		"tarball": "https://registry.npmjs.org/rct-form/-/rct-form-0.1.0.tgz"
	},
	"maintainers": [{
		"name": "gitniko",
		"email": "galxis.ling@gmail.com"
	}],
	"_npmOperationalInternal": {
		"host": "s3://npm-registry-packages",
		"tmp": "tmp/rct-form-0.1.0.tgz_1507518264097_0.7703938684426248"
	},
	"directories": {}
}
```
所以`npm install`核心流程实质上是去`Registry`上寻找到指定包的`dist.tarball`的资源地址，然后下载到本地，然后解压到n`node_modules`中，至于细节上每种客户端都会做一些优化。

## npm-scope
`@somescope/somepackagename` URL-safe characters, no leading dots or underscores  
可以单独给某个scope设定单独的registry

## npmrc
npm的配置文件。会有以下四个可能存在的地方:  
- 每个项目都可以有一个配置文件(/path/to/my/project/.npmrc)  
- 每个用户下都可以有一个配置文件(~/.npmrc)  
- 全局配置($PREFIX/etc/npmrc)  
- 内置配置(/path/to/npm/npmrc)  

其中内容如下：

```js
//http://101.132.155.81:4873/:_password=123456
//http://101.132.155.81:4873/:username=niko
//http://101.132.155.81:4873/:email=galaxix@gmail.com
//http://101.132.155.81:4873/:always-auth=false
```

`nom config`使用到的配置都可以在这里设置。  有哪些配置可以查看[官方说明](https://docs.npmjs.com/misc/config)。



## 私有库发布/使用的方式

由于私库的发布/使用与是否使用私有库/共有库无关，只是一个标准的客户端(npm-cli)的行为，所以这里放在前面说明。

项目中使用配置私库一般有以下两种方式：

- `alias mynpm='npm --registry=http://registry.npm.example.com` 
- 单独给项目配置`npmrc`文件，然后在文件里设置`registry=http://registry.npm.example.com` 

这里建议用第二种，因为发布的时候还需要用到`npmrc`。

### 使用

- 进入到项目根目录下
- `echo "registry=http://registry.npm.example.com" >> .npmrc`

这时候就可以正常时用npm了，在安装依赖的时候都会去链接私库`http://registry.npm.example.com`

### 发布

- 在项目根目录下`npm login`成功，然后在`~/.npmrc`文件中拿到token，类似如下`//http://registry.npm.example.com/:_authToken="/8Ep1MXmpkSwuzcTVSZy2Q=="`
- 拿到的token配置加到项目目录下的`.npmrc`中
- 最后发布`npm publish`即可



## 都具备的特点  

- cache npmjs.org registry  
- Link multiple registries ?  干什么的  
- Override public packages ？ 是否需要    

## verdaccio

- zero-config-required local private npm registry
- own tiny database
- supports various community-made plugins to hook into services such as Amazon's s3 and Google Cloud Storage

### storage
`/storage`用户存放包信息，包缓存。例如：如果我上传了`@test/hello`包，会在该目录下生成对应路径的`/storage/@test/hello`，
而该目录下存放了压缩的`tgz`的文件，以及对应的`package.json`。其中`package.json`比项目中的`package.json`多了对应版本的信息，
实际上该文件内容就是访问`{registry root url}/{package name}`的内容。

并且如果通过`{registry root url}/{package name}`访问不存在的包会去搜索上游的`npm`服务器获取`package.json`并且存到`/storage`目录中。  
例如：访问了hello包（不是`@test/hello`)，如果上游存在改包，则会在`storage/`中生成对应的目录，并且存放对应的`package.json`。

以下是对应的目录
```shell
|-- hello
|   `-- package.json
|-- .sinopia-db.json
`-- @test
    `-- hello
        |-- hello-1.0.0.tgz
        |-- hello-1.0.2.tgz
        `-- package.json
```

### 工程架构
- express
```js

```

## 讨论

### Override public packages
例如发现了一个bug，提给了`maintainer`，但是他可能由于种种原因（不维护，不同意等等），这个时候需要覆盖一些公共的包。  
这么作不好的地方主要还是容易脱离社区，需要谨慎对待。

## lock文件索引问题  
如果在内网安装，lock的链接全是私有npm的地址。如果在外网的时候再install就会下载不了包。  


### 私有包发布

### 方式1
> Set "private": true in your package.json to prevent it from being published at all, or "publishConfig":{"registry":"http://my-internal-registry.local"} to force it to be published only to your internal registry.

### 数据迁移

## 证书问题
>Default: The npm CA certificate  
The Certificate Authority signing certificate that is trusted for SSL connections to the registry. Values should be in PEM format (Windows calls it "Base-64 encoded X.509 (.CER)") 

## 拓展
- 依赖树的dedupe算法  
- npm install原理  
- yarn做了哪些优化  

## refs
[left-pad的故事](https://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/)  
[Packages/Registry specification](http://wiki.commonjs.org/wiki/Packages/Registry)  
[npm install的原理的简单介绍](http://www.ruanyifeng.com/blog/2016/01/npm-install.html)  
[npm publish](https://docs.npmjs.com/cli/publish)  
[npm ca](https://docs.npmjs.com/misc/config)  

[npmrc](https://docs.npmjs.com/files/npmrc)  


## log
```shell
root@ceduvpn:/etc/ssl# vim 
certs/       openssl.cnf  private/     
root@ceduvpn:/etc/ssl# vim 
certs/       openssl.cnf  private/     
root@ceduvpn:/etc/ssl# vim openssl.cnf 
root@ceduvpn:/etc/ssl# cd /home/docker/
dlt-scraping/ mongo/        mysql/        .ssh/         verdaccio/    vpn/          
root@ceduvpn:/etc/ssl# cd /home/docker/
root@ceduvpn:/home/docker# ls
dlt-scraping  mongo  mysql  verdaccio  vpn
root@ceduvpn:/home/docker# su docker
docker@ceduvpn:~$ mkdir ca
docker@ceduvpn:~$ ls
ca  dlt-scraping  mongo  mysql  verdaccio  vpn
docker@ceduvpn:~$ cd ca/
docker@ceduvpn:~/ca$ ls
docker@ceduvpn:~/ca$ openssl genrsa -aes256 -out private/ca.pem 1024
private/ca.pem: No such file or directory
140088518948504:error:02001002:system library:fopen:No such file or directory:bss_file.c:398:fopen('private/ca.pem','w')
140088518948504:error:20074002:BIO routines:FILE_CTRL:system lib:bss_file.c:400:
docker@ceduvpn:~/ca$ mkdir private
docker@ceduvpn:~/ca$ ls
private
docker@ceduvpn:~/ca$ openssl genrsa -aes256 -out private/ca.pem 1024
Generating RSA private key, 1024 bit long modulus
.++++++
............++++++
e is 65537 (0x10001)
Enter pass phrase for private/ca.pem:
139794937226904:error:28069065:lib(40):UI_set_result:result too small:ui_lib.c:823:You must type in 4 to 1023 characters
Enter pass phrase for private/ca.pem:
139794937226904:error:28069065:lib(40):UI_set_result:result too small:ui_lib.c:823:You must type in 4 to 1023 characters
Enter pass phrase for private/ca.pem:
Verifying - Enter pass phrase for private/ca.pem:
docker@ceduvpn:~/ca$ openssl rsa -in private/ca.pem -out private/ca.key
Enter pass phrase for private/ca.pem:
writing RSA key
docker@ceduvpn:~/ca$ openssl req -new -key private/ca.pem -out private/ca.csr
Enter pass phrase for private/ca.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:JS
Locality Name (eg, city) []:nanjing
Organization Name (eg, company) [Internet Widgits Pty Ltd]:shein
Organizational Unit Name (eg, section) []:web
Common Name (e.g. server FQDN or YOUR name) []:npmca
Email Address []:galxis.ling@gmail.com 

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:123456
An optional company name []:shein 
docker@ceduvpn:~/ca$ ls
```