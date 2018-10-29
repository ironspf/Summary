# 1. 定义
它是一种包含用户界面的组件，主要用于和用户进行交互，一个应用程序可以包含多个Activity。

# 2. 创建Activity
## 2.1 创建一个Activity
- 首先在需要添加Activity的包名上右击，New→Activity→Empty Activity，在弹出的对话框中，将Activity命名为FirstActivity，去除勾选Generate Layout File和Launcher Activity两个选项。
- 勾选Generate Layout File会自动为FirstActivity生成布局文件，勾选Launcher Activity会将当前Activity设置为应用启动的第一个Activity，勾选Backwards Compatibility(AppCompat)，表示向下兼容。
- 点击Finish完成创建。

![](https://github.com/ironspf/Summary/blob/master/images/New%20Android%20Activity.png)

- 所有的Activity都需要重写Activity的onCreat()方法，代码如下所示：

```java
class FirstActivity : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
  }
}
```
## 2.2 创建和加载布局

- 布局文件来控制我们需要显示的内容。
- 右击app/src/main/res/layout目录→New→Layout resource file，在弹出的对话框中填写文件名为activity_first，根元素可以选择LinearLayout。
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

- 对这个布局文件进行修改，添加一个Button,设定显示的内容，如下所示：


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:orientation="vertical">
  <Button
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text = "Hello, Beike!"/>
</LinearLayout>
```
- 将布局文件加载到Activity中
在FirstActivity中添加如下代码：
```java
class FirstActivity : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_first)
  }
}
```

## 2.3 在AndroidManifest文件中注册
- Android中的四大组件都需要在AndroidManifest文件中注册，不然会抛出"android.content.ActivityNotFoundException"异常。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.ironspf.basic">
  <application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
    <activity android:name=".activity.FirstActivity"></activity>
  </application>
</manifest>
```

- 注册Activity之后还是不能运行的，因为没有为应用配置主程序，也就是说应用启动时不知道首先打开哪个Activity。此时需要告诉配置程序的入口，在&lt;activity&gt;标签中添加&lt;intent-filter&gt;标签，并在标签中添加 “&lt;action android:name="android.intent.action.MAIN" /&gt;”和&lt;”category android:name="android.intent.category.LAUNCHER" /&gt;”。
- 修改后的AndroidManifest文件如下：
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.ironspf.basic">
  <application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
    <activity android:name=".activity.FirstActivity">
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>
  </application>
</manifest>
```
## 2.4启动Activity
以上准备工具已经完成，只需要点击Androidstudio中的Run按钮，就会启动我们刚刚写创建的Activity，运行后的界面如下所示。
![](https://github.com/ironspf/Summary/blob/master/images/Hello%20Beike.png)

# 3. 设置点击事件
设置点击事件可以分为两种方式：添加匿名内部类和实现View.OnClickListener接口并重写其中的onClick(v: View?)函数。
<p>给上面的Button添加一个点击事件，当被点击时提示内容“我被点击了！”</p>
- 匿名内部类方式

```java
class FirstActivity : AppCompatActivity(){

  private lateinit var mBtnFirst: Button

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_first)
    initView()
  }

  private fun initView(){
    mBtnFirst = findViewById(R.id.btn_first)

    mBtnFirst.setOnClickListener(object: View.OnClickListener{
      override fun onClick(v: View?) {
        Toast.makeText(this@FirstActivity, "我被点击了！", Toast.LENGTH_SHORT).show()
      }
    })
  }

}
```

- 实现接口方式

```java
class FirstActivity : AppCompatActivity(), View.OnClickListener{

  private lateinit var mBtnFirst: Button

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_first)
    initView()
  }

  private fun initView(){
    mBtnFirst = findViewById(R.id.btn_first)
    mBtnFirst.setOnClickListener(this)
  }

  override fun onClick(v: View?) {
    when(v?.id){
      R.id.btn_first -> {
        Toast.makeText(this@FirstActivity, "我被点击了！", Toast.LENGTH_SHORT).show()
      }
    }
  }

}
```

# 4. 销毁Activity
销毁一个Activity就不像创建一个Activity那么复杂了，只需要在销毁的时候调用Activity类的finish()函数即可，便会关闭当前页面。

# 5. Activity之间传递数据
一个应用不可能只包含一个页面（Activity），那么当存在多个Activity时，它们之间是如何切换的呢？其实，前一个Activity可以通过某种方式来启动后一个Activity，前一个Activity可以向后一个Activity传递数据，同样后一个Activity也可以向前一个Activity回传数据。

## 5.1 启动Activity

- 显示启动

- 隐示启动

## 5.2 向下一个Activity传递数据


## 5.3 向上一个Activity回传数据

# 6. 生命周期
# 7. 启动模式
