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
    android:text = "I am the first Activity"/>
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
## 2.4 启动Activity
以上准备工具已经完成，只需要点击Androidstudio中的Run按钮，就会启动我们刚刚写创建的Activity，运行后的界面如下所示。
![](https://github.com/ironspf/Summary/blob/master/images/I%20am%20the%20first%20activity.png)

# 3. 设置点击事件
设置点击事件可以分为两种方式：添加匿名内部类和实现View.OnClickListener接口并重写其中的onClick(v: View?)函数。

给上面的Button添加一个点击事件，当被点击时提示内容“我被点击了！”
* 匿名内部类方式

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

* 实现接口方式

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
- 一个应用不可能只包含一个页面（Activity），那么当存在多个Activity时，它们之间是如何切换的呢？其实，前一个Activity可以通过某种方式来启动后一个Activity，前一个Activity可以向后一个Activity传递数据，同样后一个Activity也可以向前一个Activity回传数据。

- 我们在创建第二个Activity，同样按照上述方式创建SecondActivity，此次需要勾选Generate Layout File选项，同时命名布局文件为activity_second，但是不要勾选Launcher Activity选项。

- 仍然需要在AndroidManifest文件中注册SecondActivity，此次不需要添加&lt;intent-filter&gt;标签。

- 这里我们先介绍一下Intent的概念，它不仅可以指明当前组件想要执行的动作，还可以在不同组件之间传递数据。Intent一般可被用于启动Activity、Service和发送广播等。

## 5.1 启动另外一个Activity
Intent大致可以分为两种：显式Intent和隐式Intent。

- 显示启动

Intent有多种构造函数，其中一个是Intent(packageContext: Context, cls: Class<?>)。第一个参数Context要求提供一个启动Activity的上下文，第二个参数需要指定想要启动的活动目标。

这个Intent怎么来使用呢？Activity类为我们提供了一个startActivity()的方法，这个方法的作用就是启动一个新的Activity，其接收一个Intent参数，将创建好的Intent传入便可以启动我们设定的Activity。

修改FirstActivity中Button的点击事件，代码如下所示：
```java
mBtnFirst.setOnClickListener(object: View.OnClickListener{
  override fun onClick(v: View?) {
    val intent = Intent(this@FirstActivity, SecondActivity::class.java)
    startActivity(intent)
  }
})
```
重新运行程序，在FirstActivity中点击一下按钮，跳转到SecondActivity中，如图所示。
![](https://github.com/ironspf/Summary/blob/master/images/I%20am%20the%20second%20activity.png)

- 隐示启动

需要我们指定一系列的更为抽象的action和category等信息，然后交给系统来分析这个Intent，然后启动合适的活动。

在AndroidManifest文件中注册SecondActivity时，在&lt;intent-filter&gt;标签中指定当前Activity可以响应的action和category，添加如下代码：
```xml
<activity android:name=".activity.SecondActivity">
  <intent-filter>
    <action android:name = "com.ironspf.basic.ACTION_START"/>
    <category android:name = "android.intent.category.DEFAULT"/>
  </intent-filter>
</activity>
```

在&lt;action&gt;标签中指明了当前Activity可以响应com.ironspf.basic.ACTION_START这个action，而&lt;category&gt;标签则包含了一些附加信息。只有当&lt;action&gt;和&lt;category&gt;中的内容同时匹配Intent中指定的action和category时，这个Activity才能够响应这个Intent。

修改FirstActivity中的点击事件，代码如下：
```java
mBtnFirst.setOnClickListener(object: View.OnClickListener{
  override fun onClick(v: View?) {
    val intent = Intent("com.ironspf.basic.ACTION_START")
    startActivity(intent)
  }
})
```

从代码中可以看出，我们只指定了能够响应的action，那前面说只有action和category同时满足时才能够响应，此处没有指定category，那SecondActivity能够被启动吗？当然可以，这是因为android.intent.category.DEFAULT是一种默认的category，在调用startActivity()方法时会自动将这个category添加到Intent中。

重新运行程序，在FirstActivity中点击一下按钮，同样能够启动SecondActivity，不同的是这次使用了隐式的Intent来启动的。

每个Intent只能指定一个action，却能指定多个category，目前Intent中只有一个默认的category，那再增加一个，修改代码如下：
```java
mBtnFirst.setOnClickListener(object: View.OnClickListener{
  override fun onClick(v: View?) {
    val intent = Intent("com.ironspf.basic.ACTION_START")
    intent.addCategory("com.ironspf.basic.CUSTOM_CATEGORY")
    startActivity(intent)
  }
})
```

再次重新启动程序，在FirstActivity中点击一下按钮，啊哦，程序崩溃了，报错日志为：

```java
android.content.ActivityNotFoundException: No Activity found to handle Intent { act=com.ironspf.basic.ACTION_START cat=[com.ironspf.basic.CUSTOM_CATEGORY] }
```

从错误日志可以发现，没有找到可以响应Intent的Activity。为什么呢？SecondActivity不是可以相应默认的category吗？那么我们如果在注册SecondActivity时添加上这个category，再次启动程序，在FirstActivity中点击一下按钮，SecondActivity被启动了。也就是说只有全部category匹配时才可以被启动，同样的那么如果在注册时添加了多个action或者category，在Intent中只指定了一个action或者category，那么Activity是否能够被匹配成功呢？可以自己实践一下哦！


## 5.2 向下一个Activity传递数据
Intent中提供了一系列的putExtra()方法的重载，可以把需要传递的数据存放到Intent中，启动另外一个Activity后，再把数据从Intent中取出来。例如将FirstActivity改写为：

```java
mBtnFirst.setOnClickListener(object: View.OnClickListener{
  override fun onClick(v: View?) {
    val intent = Intent(this@FirstActivity, SecondActivity::class.java)
    intent.putExtra("extra_data", "Hello, SecondActivity! I am the first activity")
    startActivity(intent)
  }
})
```

在putExtra()方法中需要接收两个参数，第一参数是key，第二个参数为需要传递的数据。然后在SecondActivity中取出数据，并Toast显示，代码如下：

```java
class SecondActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_second)

    val data = intent.getStringExtra("extra_data")
    Toast.makeText(this@SecondActivity,data, Toast.LENGTH_SHORT).show()
  }
}
```

首先获取SecondActivity的intent，然后调用getStringExtra()来获取数据。由于我们传递的是String类型的数据，故使用getStringExtra()来获取，如果传递的是Integer或者Boolean，则对应的使用getIntExtra()和getBooleanExtra()方法，以此类推。

重新运行程序，在FirstActivity中点击按钮跳转到SecondActivity中，并Toast显示了传递的内容，说明数据传递成功了。

## 5.3 向上一个Activity回传数据
既然上一个Activity可以传递数据给下一个Activity，那么后边的Activity能够可以传递数据给上一个Activity，答案当然是肯定可以的。在Activity中存一个startActivityForResult()的方法，其接收两个参数，第一个参数为Intent，第二个参数为请求码，用于在回调中判断数据的来源。

修改FirstActivity中点击事件，代码如下：
```java
mBtnFirst.setOnClickListener(object: View.OnClickListener{
  override fun onClick(v: View?) {
    val intent = Intent(this@FirstActivity, SecondActivity::class.java)
    startActivityForResult(intent, 1)
  }
})
```

在SecondActivity中为Button添加点击事件，并返回数据，代码如下：

```java
class SecondActivity : AppCompatActivity() {

  private lateinit var mBtnSecond: Button

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_second)

    mBtnSecond = findViewById(R.id.btn_second)
    mBtnSecond.setOnClickListener(object: View.OnClickListener{
      override fun onClick(v: View?) {
        val intent = Intent()
        intent.putExtra("data_return", "Hello FirstActivity")
        setResult(RESULT_OK, intent)
        finish()
      }
    })
  }
}
```

在SecondActivity中创建了一个Intent，在intent中设置需要返回的数据，然后调用Activity的setResult()方法，其接收两个参数，第一个参数用于向上一个Activity返回处理结果，一般使用RESULT_OK或者RESULT_CANCELED，第二个参数为带有数据的Intent，然后调用finish()函数关闭当前的Activity。

由于使用startActivityForResult()方法来启动SecondActivity，在SecondActivity被销毁之后会回调上一个Activity的onActivityResult()方法，因此我们需要在FirstActivity中重写这个方法来获取数据，如下所示：





# 6. 生命周期
# 7. 启动模式
