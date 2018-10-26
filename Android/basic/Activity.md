# 1. 定义
它是一种包含用户界面的组件，主要用于和用户进行交互，一个应用程序可以包含多个Activity。

# 2. 创建Activity
## 2.1 创建一个Activity
- 首先在需要添加Activity的包名上右击，New→Activity→Empty Activity，在弹出的对话框中，将Activity命名为KeActivity，去除勾选Generate Layout File和Launcher Activity两个选项。
- 勾选Generate Layout File会自动为KeActivity生成布局文件，勾选Launcher Activity会将当前Activity设置为应用启动的第一个Activity，勾选Backwards Compatibility(AppCompat)，表示向下兼容。
- 点击Finish完成创建。

![](https://github.com/ironspf/Summary/blob/master/images/New%20Android%20Activity.png)

- 所有的Activity都需要重写Activity的onCreat()方法，代码如下所示：

```java
class KeActivity : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
  }
}
```
## 2.2 创建和加载布局

- 布局文件来控制我们需要显示的内容。
- 右击app/src/main/res/layout目录→New→Layout resource file，在弹出的对话框中填写文件名为activity_ke，根元素可以选择LinearLayout。
- 点击OK完成创建。

![](https://github.com/ironspf/Summary/blob/master/images/New%20Layout%20Resource%20File.png)

- 点击Text选项卡，可以看到默认为我们生成了一个Linearlayout布局，内容如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:orientation="vertical">
</LinearLayout>
```

- 对这个布局文件进行修改，添加一个TextView,设定显示的内容，如下所示：


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:orientation="vertical">
  <TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text = "Hello, Beike!"/>
</LinearLayout>
```
- 将布局文件加载到Activity中
在KeActivity中添加如下代码：
```java
class KeActivity : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_ke)
  }
}
```

## 2.3 在AndroidManiFest文件中注册

# 3. 设置点击事件


# 4. 销毁Activity


# 5. Activity之间传递数据

## 5.1 启动Activity

- 显示启动
- 隐示启动

## 5.2 向下一个Activity传递数据


## 5.3 向上一个Activity回传数据

# 6. 生命周期
# 7. 启动模式
