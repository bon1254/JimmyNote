# 安裝AR人臉辨識Package的Bug

Unity版本：2020.1.3f1

目標平台：Andriod

原本想練習做AR人臉辨識的時候，卻因為安裝一些Package導致出現這個BUG：

Library\PackageCache\com.unity.xr.management@3.2.17\Editor\XRGeneralBuildProcessor.cs(39,52): error CS0117: ‘BuildPipeline’ does not contain a definition for ‘GetBuildTargetName’

1.  我把先前裝的AR Foundation、ARCore XR Plugin、ARKit Face Tracking移除，然後安裝XR Plugin Manager(3.2.16)，如果是安裝3.2.17版本話還是有Bug跑出來。
2.  再依序安裝AR Foundation、ARCore XR Plugin、ARKit Face Tracking皆為4.1.1版本，這樣就沒有跑出Bug了。

參考網址來源：

[Resolved — error CS0117: ‘BuildPipeline’ does not contain a definition for ‘GetBuildTargetName’ — Unity Forum](https://www.blogger.com/u/1/blog/post/edit/1169600120833842387/7664553900053752466#)