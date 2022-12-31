
# Unity Android 一些注意的点

1. 关于C# 调用native函数
**C# [DllImport(libxx)]** vs 在Android平台会和在Java中调用**System.LoadLibrary(xx)**; 都会调用**JNI_OnLoad**函数。

2. 构建.androidlib 插件包时，**xx.jar**包中**去掉BuildConfig.class**这种自动生成的文件，Unity直接构建xxx.apk包时，会有重复符号的构建错误。

3. https://stackoverflow.com/questions/28805607/exclude-buildconfig-class-and-r-class-from-android-library-jar-in-gradle


4. Android GPU 分析工具
    **Qualcomm** Adreno	[snapdragon-profiler](https://developer.qualcomm.com/software/snapdragon-profiler)
    **Arm**	Mali	[Arm Mobile Studio](https://developer.arm.com/tools-and-software/graphics-and-gaming/arm-mobile-studio)
    **Imagination Tech** PowerVR	[PowerVR Graphics Tools](https://www.imaginationtech.com/blog/profiling-and-debugging-with-powervr-graphics-tools/)

5. 性能监控之内存和CPU
    1. https://www.jianshu.com/p/d01402dc0524

    VSS- Virtual Set Size 虚拟耗用内存（包含共享库占用的内存）
    RSS- Resident Set Size 实际使用物理内存（包含共享库占用的内存）
    PSS- Proportional Set Size 实际使用的物理内存（比例分配共享库占用的内存）
    USS- Unique Set Size 进程独自占用的物理内存（不包含共享库占用的内存）
    一般来说内存占用大小有如下规律：VSS >= RSS >= PSS >= USS
    https://blog.csdn.net/adaptiver/article/details/7084364
6. 