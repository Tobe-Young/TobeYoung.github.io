
## 问题
1. 什么样的资源可以被定义为“无用”. Resources.UnloadUnusedAssets()可让它卸载吗？
2. 调用Resources.UnloadAsset()卸载一个正在使用的Asset, 它能起作用吗？如果不起作用，那怎么才能使它起作用
3. 如果一个prefab在Resources文件夹中，这个prefab引用图片资源都不在Resources文件夹中，此时加载或者卸载，过程中会发生什么？ inside-inside 情况下呢？
4. AnimationClip引用的图片，是在初始化时就立即加载 还是在播放的时候才进行加载
5. 能卸载Sprite吗？或者说必须先卸载Texture，它们有什么区别吗？
6. 等等...



## Unity UI 优化

### Unity UI 基础
1. Canvas
    Geometry
    Sub-canvas
2. Graphic 
    It is the base class for all Unity UI C# classes that provide drawable geometry to the Canvas system
3. Layout
    control the size and positioning of RectTransforms
Both **Graphic** and **Layout** components rely on the **CanvasUpdateRegistry** class
4. 
### 

## 引用
1. https://learn.unity.com/tutorial/optimizing-unity-ui#