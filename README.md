# [English documentation](README_EN.md)
# [接口文档](https://oppf.xmcsrv.com/static/md/docs/javadoc/index.html)
# 新的FunSDKDemo，目前还在更新完善中（老的FunSDKDemo可以正常使用并且会持续更新）

## 通过Gradle集成
### 1.1 创建工程在 Android Studio 中新建工程。
### 1.2 配置 build.gradlebuild.gradle 文件里添加集成准备中下载的 dependencies 依赖库。
```
repositories {
    mavenCentral()
}

defaultConfig {
    ndk {
        abiFilters "armeabi-v7a","arm64-v8a"
    }
    //目前支持armeabi-v7a和arm64-v8a，请不要在工程中添加x86，不然无法运行
 }

dependencies {
    // FunSDK 当前最新稳定版本：
    implementation 'io.github.xmcamera:libxmfunsdk:2.1'
}
```

## 初始化
### 1.前往：前往（https://oppf.xmcsrv.com/#/docs?md=readGuide） 新人指南，注册申请成为开放平台开发者，然后到【控制台】-【创建应用页面】中创建Android应用，等应用审核通过后就可以获取到AppKey、movedCard和AppSecret等信息。
### 2.在AndroidManifest.xml加上申请到的AppKey、movedCard和AppSecret等信息
```
<meta-data
    android:name="APP_UUID"
    android:value="需要替换的内容" />
<meta-data
    android:name="APP_KEY"
    android:value="需要替换的内容" />
<meta-data
    android:name="APP_SECRET"
    android:value="需要替换的内容" />
<meta-data
    android:name="APP_MOVECARD"
    android:value="需要替换的内容" />
```
### 3.在Application处理
```
 /* 如果是P2P定制服务器的话请参考以下方法
 * int customPwdType 加密类型 默认传0
 * String customPwd 加密字段 默认传 ""
 * String customServerAddr 定制服务器域名或IP
 * int customPort 定制服务器端口
 * XMFunSDKManager.getInstance(0,"",customServerAddr,customPort).initXMCloudPlatform(this);
 */
 
XMFunSDKManager.getInstance().initXMCloudPlatform(this);
如果是低功耗设备（门铃、门锁等）还需要调用以下方法：
FunSDK.SetFunIntAttr(EFUN_ATTR.SUP_RPS_VIDEO_DEFAULT, SDKCONST.Switch.Open);
初始化打印：
XMFunSDKManager.getInstance().initLog();
```

## [账号登录和临时登录](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/账号登录和临时登录)

## [账号注册](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/账号注册)

## [账号删除](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/账号删除)

## [添加设备](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/添加设备)

## [删除设备](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/删除设备)

## [登录设备](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/登录设备)

## [实时预览](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/实时预览)

## [录像回放](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/录像回放)

## [报警推送订阅](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/报警推送订阅)

## [设备配置](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/设备配置)

## [云存储集成](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/云存储集成)

## [混淆处理](https://gitlab.xmcloud.io/demo/FunSDKDemo_Android/wikis/混淆处理)

