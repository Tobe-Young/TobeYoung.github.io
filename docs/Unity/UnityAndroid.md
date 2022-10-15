
# Unity Android 一些注意的点

1. 关于C# 调用native函数
**C# [DllImport(libxx)]** vs 在Android平台会和在Java中调用**System.LoadLibrary(xx)**; 都会调用**JNI_OnLoad**函数。

2. 


