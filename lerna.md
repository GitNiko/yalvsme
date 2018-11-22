使用lerna改造Web前端项目
===========================
在用到lerna前，大概解释下关于代码管理的两种模式，`monorepo`和`multirepo`。  

一般我们的前端应用会有由多个库组成,在`package.json`依赖组件库，工具库等等。除了应用代码使用单独一个仓库管理，组件库，工具库也都各自使用仓库管理代码。这种管理多个项目代码的方式就是`multirepo`。

当然还有另外一种管理方式，就是通过一个仓库管理，组件库，工具库的代码全都放在一个仓库中统一管理。

## 两种不同的代码管理模式(source control pattern)特点
### multirepo
- 不同的workflow，可以用自己擅长的工具
- 编程语言不同也不受影响
- 提交记录分开记录，互不影响
- 仓库可以比较小

### monorepo
- 由于在一个仓库中，需要统一的workflow，开发需要使用同样的工具
- 处理多语言会比较麻烦
- 提交记录在一起，回滚的时候会很麻烦
- 仓库会非常大

## 举个场景例子
- 多个不同域名下的前端应用对应不同的业务
- 使用相同的技术栈
- 共用组件库

### multirepo
- 项目对应各自仓库，修改组件库的时候需要升级版本号并且重新发布，依赖的项目也需要同样升级依赖的版本号  
- 每个项目的`.babel` `webpack` 格式化配置这些东西都需要拷贝一边
- 各自的CI也需要单独配置

### monorepo
- 组件库和应用放在同一个仓库，直接依赖，组件库改了可以直接看到效果
- 只需要一套项目配置
- CI不需要单独配置

但是我们希望能够保持组件库还能够单独发布出来，供有其他独立项目通过包依赖的方式使用，这个时候就需要lerna这个工具来帮助我们简化这件事.

## lerna
`A tool for managing JavaScript projects with multiple packages.`   


### 初始化lerna项目
`lerna bootstrap`
todo

### 项目结构
```js
- packages
  - core // 应用入口
    - node_modules
    - src
    - package.json
    - yarn.lock
  - components // 组件库
    - node_modules
    - src
    - package.json
    - yarn.lock
  - common // 工具库
    - node_modules
    - src
    - package.json
    - yarn.lock
  - moduleA // 业务模块
    - node_modules
    - src
    - package.json
    - yarn.lock
- .eslintrc.js
- .prettierrc
- lerna.json
- package.json
- yarn.lock
```

入口中一般放一些登录的逻辑，业务逻辑都放在其他依赖中(moduleA,moduleB...)。
lerna会为每个package生成package.json以及独立下载node_modules。
如果是依赖现有的模块（例如core依赖components),那么在core的node_modules中会有一个link到components的路径中。

每个package可以独立发布出去，不过一般来说只会把components和common发布。

现在每个package中开发都会使用同一套workflow, lint, babel, prettierrc, 这些都只需要配置一次。对于小规模的团队来说可以减少很多重复的工程模版，同时也能满足后期可以把共用模块独立发布出去。



## 技巧

### 统一修改版本

`learn version`在`fixed` 模式下可以统一修改包的版本：

```shell
➜  react-helper git:(master) lerna version
lerna notice cli v3.4.3
lerna info current version 0.0.0
lerna info Looking for changed packages since initial commit.
? Select a new version (currently 0.0.0) Custom Version
? Enter a custom version 6.0.0

Changes:
 - @test/react-redux-component-loader: 5.0.0-alpha1 => 6.0.0
 - @test/rrc-loader-helper: 2.1.3 => 6.0.0

? Are you sure you want to create these versions? Yes
lerna info lifecycle @test/rrc-loader-helper@2.1.3~preversion: @test/rrc-loader-helper@2.1.3

> @test/rrc-loader-helper@2.1.3 preversion /Users/niko/workspace/shein/react-helper/packages/rrc-loader-helper
> npm test && npm run build


> @test/rrc-loader-helper@2.1.3 test /Users/niko/workspace/shein/react-helper/packages/rrc-loader-helper
> ava


  6 passed

> @test/rrc-loader-helper@2.1.3 build /Users/niko/workspace/shein/react-helper/packages/rrc-loader-helper
> babel src --out-dir lib

Successfully compiled 20 files with Babel.
lerna info git Pushing tags...
lerna success version finished
```

从上面可以看到，把`react-redux-component-loader`和`rrc-loader-helpe`从原来的版本更新到了`6.0.0`，lerna会自动帮你把对应的`package.json`中的`version`改成相应的版本号。并且会触发各自包内定义的`preversion`脚本（如果定义的话）

### trouble
目前发现 如果packageA 和 packageB同时依赖相同的库的时候，同样的库代码会被同时编译两次。可能是webpack的问题，有待解决。



#### peerDepenceies

peerDepenceies 不会自动update

peerDep 应该尽可能宽松，不应该被自动update，

https://github.com/lerna/lerna/issues/1018

https://github.com/babel/babel/pull/6644



#### 版本升级

```js
monorepo
  -packages
	- a
	- b
```

a依赖b

fixed 模式下，lerna version/publish 的时候，如果：

- a更新了，则只是a版本升级
- b更新了，则a和b都会升级
- a,b都更新了，则a和b都会



#### CI/CD

版本号的升级，必须人工介入，无法自动判断到底是Patch还是Minor还是Major。

## refs

[preversion](https://docs.npmjs.com/cli/version)

