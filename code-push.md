## 什么是code push  
微软提供的开源的react native/cordova 在线热修复服务，使得我们能够在不更新apk／ipa的情况下，更新我们的JS代码。提供更快捷的发版本／修复bug的方式。客户端本身是会链接code push提供的服务器拉取最新的代码。  [详细介绍](https://github.com/Microsoft/react-native-code-push)

## 哪些场景无法使用
* 原生代码更新了 
* 资源文件更新了，比如字体库 
* 应用权限变更  

## 回滚
code push 每次即将更新一个本地版本的时候，会自动备份当前版本代码，如果本次更新失败，则会自动回退使用备份的那个版本的代码。

## 配置注意点
* 全部操作都需要通过[cli](https://microsoft.github.io/code-push/docs/getting-started.html)，包括账号注册，app注册，app发布。   
* 一个app只能对应一个平台，即使这个app发布的时候可以指定平台ios/android。因为code push 的deployment 不区分平台。 

> 
* CodePush deployments do not track, or have any concept of platform (Android/iOS) 
* The release-react command can only release one platform at a time 
* An Android bundle released to an iOS app, or vice versa, will be silently rejected by our SDK   

所以需要注册两个app，比如app-ios,app-android这样。[问题链接](https://github.com/Microsoft/react-native-code-push/issues/569)  

* 注意自己使用的react native版本，必须与code push的[rn插件匹配](https://github.com/Microsoft/react-native-code-push#supported-react-native-platforms)，code push的rn插件并不兼容前面的版本。 


## 更新策略
可以通过JS代码开发更新过程的交互体验，默认是后台更新。android平台可以随意定制自己的更新策略。**然而走apple store的应用则需要注意不能显示的让用户感知到更新，否则会被平台审核拒绝**。  

## 更新流程
`code-push release-react thrall ios` 首先把代码推送到deployment上,然后通过`code-push promote thrall-ios Staging Production` 把已经推送到deployment上的版本更新到production上，这样才算把代码推送到线上环境。

##  更新时机
默认是每次app进程启动的时候才检查更新，下载，等待下次进程启动的时候更新本地的JS。  
也可以是每次app由后台唤起的时候检查更新，下载，等待下次进程启动的时候更新本地的JS。 
当然也可以通过JS代码实现JS重新加载。   
[具体参考](https://github.com/Microsoft/react-native-code-push#plugin-usage)

## 更新过程记录android
app启动,注意code push的log
![粘贴图片_156_](./asset/code-push/粘贴图片(156).png)

通过cli推送更新
![粘贴图片_157_](./asset/code-push/粘贴图片(157).png)

查看更新状况
![粘贴图片_158_](./asset/code-push/粘贴图片(158).png)

客户再次启动或者从后台唤醒（根据自己设定的更新策略)
![粘贴图片_159_](./asset/code-push/粘贴图片(159).png)

应用进程关闭后再次启动应用
![粘贴图片_160_](./asset/code-push/粘贴图片(160).png)

查看更新状况，发现已经有1个终端
![粘贴图片_161_](./asset/code-push/粘贴图片(161).png)

