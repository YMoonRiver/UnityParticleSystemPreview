# Unity 粒子特效预览工具
## 原因
在使用 Unity 制作完成粒子特效预制后，保存到工程统一的特效目录里，待到需要使用的时候，再去选择相应的粒子特效预制。当特效预制越来越多后，就会越来越难以分辨哪个才是真正需要使用的，而 Unity 并没有提供像模型动作动画 AnimationClip 那样可以预览资源的功能，只能一个个拖动到场景里面去预览播放，非常的费时费劲。

## 目标
实现像模型动作动画那样可以直接在检视器里进行预览粒子。

![](http://img.blog.csdn.net/20161031204154560)

## 解决
粒子特效预览最后要实现的效果跟模型动作动画预览类似，先将 FBX 模型换成粒子物体，然后再实现其在预览窗口里面可以进行播放。

![](http://img.blog.csdn.net/20161031204228633)

首先实现预览窗口里面的三维空间，使用到的编辑器类 PreviewRenderUtility，它创建了一个隐藏的摄像机，cullingMask 设置为只要渲染的特定 Layer 层，渲染成目标纹理，然后绘制纹理到预览窗口里。接着，创建所要被渲染的物体。格子平面地板，指示方向箭头，还有粒子特效对象。

![](http://img.blog.csdn.net/20161031204254884)

最后实现粒子的预览播放。粒子在编辑模式下的模拟播放有两种方式，一种直接调用 Simulate 方法，播放的效果跟实际运行播放的效果可能存在差别。另一种则是调用锁定粒子，再调用 Play 方法，效果会跟实际运行播放的效果一样，因为 Unity 内部会对锁定的粒子对象，进行真实计算。这里使用的是第二种方法，锁定粒子的方法并没有开放出来，所以得反射 ParticleSystemEditorUtils.lockedParticleSystem 方法。

![](http://img.blog.csdn.net/20161031204322640)

对于粒子特效根节点就带有 Mesh 的话，就无法显示原本的 Mesh 预览窗口，所以在工具栏加个按钮，可以进行切换。

![](http://img.blog.csdn.net/20161031204347181)

![](http://img.blog.csdn.net/20161031204357266)

按钮 PS （Show particle system preview）可以在原先的预览窗口跟粒子特效预览窗口之间进行切换。

## 结语
Unity 编辑器提供了灵活的扩展方法，但是很多都是没有文档的，需要去研究它自身是如何使用的，才能方便移植扩展。粒子特效的模拟播放方式，在不选中粒子对象的情况下，又想让粒子可以播放，那么就只有锁定粒子这种方式。

## 源码
AssetStore 地址：https://www.assetstore.unity3d.com/cn/#!/content/73346

Github 地址：https://github.com/akof1314/UnityParticleSystemPreview
