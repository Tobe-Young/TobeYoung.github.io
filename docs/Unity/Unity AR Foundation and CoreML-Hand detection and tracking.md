# Unity AR Foundation and CoreML: Hand detection and tracking

## 基本基础知识

Swift调用OC方法

OC调用Swift方法

## 会遇到的问题

Unity 2019.2版本后，会将所有的cpp files 打包进目标为UnityFramework.framework中。因为无法在framework中添加ModuleName-Bridging-Header.h文件，那么会报错：**error: using bridging headers with framework targets is unsupported**

解决办法：
https://github.com/jwtan/SwiftToUnityExample



## 引用
1. https://www.jiadongchen.com/2019/07/unity-ar-foundation-and-coreml-hand-detection-and-tracking/
2. https://github.com/jwtan/SwiftToUnityExample