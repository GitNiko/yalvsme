gitlab-ci 接入 runner
=========================================
## 开发流程
![workflows](https://docs.gitlab.com/ee/ci/img/pipelines-goal.svg)
## gitlab-ci中的概念
### Pipelines  
几个`stage`串联在一起就构成了一个`pipelines`。每次执行构建的时候会启动这个`pipelines`。
![pipelines](https://docs.gitlab.com/ee/ci/img/pipelines.png)
### Stages
定义在`gitlab-ci.yml`中。定义流程的阶段，是`jobs`的一个集合。每个`jobs`会在相应的`stage`中执行，并且这些`jobs`是并行执行的。`stage`可能定义`build`,`test`,`deploy`这种阶段。
### Jobs
定义在`gitlab-ci.yml`中。是一串bash脚本，可以执行我们的编译，测试，部署等一切所需要的任务，就如同我们在本地终端中执行命令一样。

## 使用场景
### 打包adnroid/IOS
### 发布npm包
### 部署web项目