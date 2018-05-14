### 生命周期的几种普通情况
* 针对一个特定的Activity，第一次启动，回调如下，onCreate->onStart->onResume
* 用户打开新的Activity的时候原来的Activity的回调 onPause->onStop
* 再次回到原来的activity的时候onRestart->onStart->onResume
* 一个Activity中打开另外一个Activity再返回的全部生命周期 AonPause->BonCreate->BonStart->BonResume->AonStop->BonPause-AonReStart->AonStart->AonResume->BonStop->BOnDestroy
* 按back回退的时候onPause->onStop->onDestroy
* 按home切换到桌面又回到activity onPause->onStop->onRestart->onStart->onResume
* 调用finish之后onDestroy
#### 特殊情况下的生命周期
##### 横竖屏切换
首先了解两个回调方法onSaveInstanceState和onRestoreInstanceState，save是调用在onStop之前的和onPause没有先后顺序，，这个方法只在Activity方法被异常终止的情况下调用，当异常终止的Activity倍被重新建立之后，系统会调用onRestoreInstance和onCreate方法
onRestoreInstanceState回调方法调用的时候表明Bundle对象非空，不用加非空判断，onCreate择需要加非空
###### 生命周期
onPause->onSaveInstanceState->onStop->onDestroy->onCreate->onStart->onRestoreInstanceState->onResume
为了防止Activity的销毁和重建，可以在AndroidManifest中制定activity中指定如下属性
```
android : configChanges = "orientation|screenSize"
 //这样，销毁和重建的时候，会调用下面的方法，onConfigurationChanged
```
##### 资源不足导致优先级地的被杀死
(1) 前台Activity——正在和用户交互的Activity，优先级最高。<br>
(2) 可见但非前台Activity——比如Activity中弹出了一个对话框，导致Activity可见但
是位于后台无法和用户交互。<br>
(3) 后台Activity——已经被暂停的Activity，比如执行了onStop，优先级最低。<br>
当系统内存不足时，会按照上述优先级从低到高去杀死目标Activity所在的进程。我
们在平常使用手机时，能经常感受到这一现象。这种情况下数组存储和恢复过程和
上述情况一致，生命周期情况也一样


