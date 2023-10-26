---
title: Flutter踩坑报告
tags: 
    - flutter
    - bug
date: 2023-10-25 10:00:00
comment: true
---

# Flutter 踩坑总集

### 卡在assembleDebug 

卡在 Running Gradle task 'assembleDebug'... 这个步骤。而且会报网络的 Exception。

可以尝试改flutter和项目的gradle源地址到国内镜像，可能没啥用

也可以手动安装。首先在 android/gradle/wrapper/gradle-wrapper.properties 可以看到本项目使用的 Gradle 版本。在 Gradle 的官网（https://gradle.org/）就可以下载到完整的包。

在 https://services.gradle.org/distributions/ 下载对应版本的包，如 gradle-5.6.2-all.zip。

下载完成之后解压到 ～/.gradle/wrapper/dists/gradle-5.6.2-all/xxx 下。其中 xxx 为一串如 9st6wgf78h16so49nn74lgtbb 的字符串。

---

### Google font版本太低

```
import 'asset_manifest.dart';
^^^^^^^^^^^^^
```

Google font版本太低
https://dev.to/curtlycritchlow/how-to-fix-assetmanifest-is-imported-from-both-packageflutter-and-packagegooglefonts-error-28e8

---

### Execution failed for task ':app:lintVitalRelease'.

```
 Flutter Error : Could not resolve all artifacts for configuration ‘:image_picker_android:debugUnitTestRuntimeClasspath’
```

https://flutter-developer.medium.com/solved-flutter-error-could-not-resolve-all-artifacts-for-configuration-56deea1c5d12

解决方案似乎是忽略掉某些错误🤔

---

### Android 12 app安装错误

```
Targeting S+ (version 31 and above) requires that an explicit value for android:exported be defined when intent filters are present
```

https://stackoverflow.com/questions/70333565/targeting-s-version-31-and-above-requires-that-an-explicit-value-for-android

---

### 签名与流水线CI/CD

https://52smile.vip/2023/08/03/CI-CD/index.html

https://coldstone.fun/post/2020/02/26/flutter-github-actions/