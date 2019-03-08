 # Android Studio 简单管理依赖包版本
 在根目录当中创建config.gradle
 ```
 //这边很熟悉吧，不用再解释
ext {
    android = [
            minSdkVersion    : 15,
            targetSdkVersion : 22,
            compileSdkVersion: 25,
            buildToolsVersion: "25.0.0",
            applicationId    : "com.xxx.app.xxx",//包名
            versionCode      : 3,//版本号
            versionName      : "1.0.2"
    ]
//google的支持包版本
    support=[
            support_version:"25.0.0"
    ]
//其他第三方
    other=[
            retrofit:"2.1.0",
            rxjava:"1.1.7",
            rxandroid:"1.2.1",
            okhttp:"3.3.1",
            butterknife:"7.0.1",
            material_dialogs:"0.9.1.0",
            baseitemlayoutlibrary:"2.1.1",
            gson:"2.2.4",
            recovery:"0.0.8",
            multidex:"1.0.1",
            fragmentation:"0.10.1",
            WheelPicker:"1.2.4"
    ]

}
 ```
 
 接着在项目中build.gradle引入该文件
 ```
 apply from: "config.gradle" // 引入该文件，加载root build.gradle当中，所有的module就都可以使用
 ```
在其他 module 中使用
```
def support = rootProject.ext.support
def other = rootProject.ext.other
def dev = rootProject.ext.android 拆分一下rootProject//固定的方法,ext 也是固定的方法 ，android 这个就是config.gradle定义的android。
android {
    compileSdkVersion dev.compileSdkVersion  //一一对应使用，以下都是类似
    buildToolsVersion dev.buildToolsVersion

    //recommend
    dexOptions {
        jumboMode = true
    }
    defaultConfig {
        applicationId dev.applicationId
        minSdkVersion dev.minSdkVersion
        targetSdkVersion dev.targetSdkVersion
        versionCode dev.versionCode
        versionName dev.versionName
    }
}

dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile 'com.android.support:support-annotations:' + support.support_version
    compile 'com.android.support:appcompat-v7:' + support.support_version
    compile 'com.android.support:design:' + support.support_version
    compile 'com.android.support:cardview-v7:' + support.support_version
    compile 'com.android.support:recyclerview-v7:' + support.support_version
    compile 'com.squareup.retrofit2:retrofit:' + other.retrofit
    compile 'com.squareup.retrofit2:adapter-rxjava:' + other.retrofit
    compile 'com.squareup.retrofit2:converter-gson:' + other.retrofit
    compile 'io.reactivex:rxjava:' + other.rxjava
    compile 'io.reactivex:rxandroid:' + other.rxandroid
    compile 'com.squareup.okhttp3:okhttp:' + other.okhttp
    compile 'com.squareup.okhttp3:logging-interceptor:' + other.okhttp
    compile 'com.jakewharton:butterknife:' + other.butterknife
    compile 'com.afollestad.material-dialogs:commons:' + other.material_dialogs
    compile 'com.maiml:baseitemlayoutlibrary:' + other.baseitemlayoutlibrary
    compile 'com.android.support:multidex:' + other.multidex
    compile 'me.yokeyword:fragmentation:' + other.fragmentation
}
```
