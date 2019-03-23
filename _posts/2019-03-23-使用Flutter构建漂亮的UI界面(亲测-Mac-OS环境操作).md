学习Flutter过程中,整理了一下Flutter官方教程 Building Beautiful UIs with Flutter 

原文地址 https://codelabs.flutter-io.cn/codelabs/flutter/index.html#0

1.综述

Flutter 是一个开源 SDK用于创建高性能原生iOS与Android应用

Flutter框架使你很方便地创建快速响应的UI界面,减少同步更新APP界面的工作量

Flutter通过使用[Material Design](https://material.io/)和 Cupertino (iOS) 组件使得创建一个漂亮的APP变得更容易

用户会倾心于你的APP的观感和体验,因为Flutter使用了针对Android和iOS平台的滚动条,导航栏,字体等.

通过使用设备和模拟器的热部署,开发人员会直观地感受到Flutter框架的强大和对开发效率的显著提升 

Flutter应用使用了Dart语言

如果你已经学习过Java, JavaScript, C#或Swift, 那么Dart的语法会让你倍感亲切

Dart在编译时使用了标准的Android和iOS工具,针对不同的移动平台进行处理

Dart语言提供了丰富的功能包括你可能已经非常熟悉的简洁的语法,[first-class functions](https://www.dartlang.org/guides/language/language-tour#functions) (将函数作为类似int float的基础类型,可以赋值和作为参数传递),[async/await](https://www.dartlang.org/articles/language/await-async),以及丰富的标准库可供调用

**通过本篇教程可以学习到** 

如何使用Flutter编写一个原生的iOS和Android应用

如何调试Flutter应用

如何在模拟器和真机上运行Flutter应用

2.搭建你的Flutter环境

Flutter环境涉及2个部分,Flutter SDK和一个编辑器(IDE),教程中会使用Android Studio作为例子,但你也可以使用其他你习惯使用的编辑器

实际代码运行可以选择真机(Android或iOS)连接到电脑,并开启开发者模式

使用iOS模拟器,需要安装XCode工具

使用Android模拟器,需要安装Android Studio

3.创建一个新的Flutter项目

首先根据模板创建一个简单的Flutter应用,具体可以参考

原文地址 [Getting Started with your first Flutter app](https://flutter.io/get-started/test-drive/#androidstudio).

中文地址 [编写您的第一个Flutter应用](https://flutterchina.club/get-started/codelab/)

将项目命名为 **friendlychat**, 你将会使用这个应用起步来创建一个完整的app

**提示: 如果你在你的Android Studio(或其他IDE)中,没有看到“新建Flutter项目”的选项,请确保您已正确安装了Flutter和Dart插件** (具体可以参考[plugins installed for Flutter and Dart](https://flutter.io/get-started/editor/#androidstudio).)

**本篇教程中,主要涉及修改 lib/main.dart**  这个Dart源文件

4.创建主用户界面

通过本教程你将开始实践把一个默认的样例app修改为一个聊天应用

目标是使用Flutter创建一个应用Friendlychat,一个简单的可扩展的聊天app,包含以下功能:

实时展示聊天信息

用户可以通过软键盘或者发送图标来发送一条信息

UI界面同时适用于iOS与Android设备

**iOS** **与 Android 图示如下** 

![image](https://upload-images.jianshu.io/upload_images/6402511-344d43614bc507d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](https://upload-images.jianshu.io/upload_images/6402511-fed6fb642e70b783.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

着手搭建应用的主要框架

第一个需要添加的元素是一个展示静态标题的简单标题栏

随着教程不断深入,你会逐步添加更多具有状态的和响应的UI组件

main.dart文件位于 Flutter 项目lib目录下, 包含[main()](https://www.dartlang.org/docs/dart-up-and-running/ch02.html#the-main-function)方法作为程序的启动入口

[main()](https://www.dartlang.org/docs/dart-up-and-running/ch02.html#the-main-function)和[runApp()](http://docs.flutter.io/flutter/widgets/runApp.html)方法定义直接参考默认的项目代码. [runApp()](http://docs.flutter.io/flutter/widgets/runApp.html)方法中的输入参数[Widget组件](https://docs.flutter.io/flutter/widgets/Widget-class.html) 

是Flutter框架用于展示APP运行时的界面.  代码中使用了Material Design元素作为UI界面, 创建一个新的[MaterialApp](http://docs.flutter.io/flutter/material/MaterialApp/MaterialApp.html)对象,并将它传入[runApp()](http://docs.flutter.io/flutter/widgets/runApp.html)方法,这个组件也就成为了UI界面的根节点

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_1_main_user_interface/lib/main.dart)

```

// Replace the code in main.dart with the following.

import 'package:flutter/material.dart';

void main() {

  runApp(

    new MaterialApp(

      title: "Friendlychat",

      home: new Scaffold(

        appBar: new AppBar(

          title: new Text("Friendlychat"),

        ),

      ),

    ),

  );

}

```

为了确认用户打开应用时的默认界面, 在[MaterialApp](http://docs.flutter.io/flutter/material/MaterialApp/MaterialApp.html)定义中设置home参数.

home参数引用了一个组件,该组件定义了应用的主界面 . 这里新建了一个[Scaffold](http://docs.flutter.io/flutter/material/Scaffold-class.html)组件, 其中包含了一个简单的[AppBar](http://docs.flutter.io/flutter/material/AppBar/AppBar.html)作为子控件

如果你启动这个app,可以看到如下界面

**iOS** **与 Android 图示如下**

![image](https://upload-images.jianshu.io/upload_images/6402511-46c335da520d1e61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](https://upload-images.jianshu.io/upload_images/6402511-ddba3ae0e255ff61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建聊天界面

开始着手编写一个交互式的组件, 你需要把这个简单的应用分成两个不同的组件子类:

一个不会变化的根节点FriendlychatApp组件

一个子节点ChatScreen组件,用于处理消息发送和内部状态的改变

目前两个组件都可继承 [StatelessWidget (无状态组件)](https://docs.flutter.io/flutter/widgets/StatelessWidget-class.html) 

后续我们会修改ChatScreen,继承[StatefulWidget](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)(有状态组件) 并管理状态.

[**main.dart  github代码**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_1_main_user_interface/lib/main.dart)

```

// Replace the code in main.dart with the following.

import 'package:flutter/material.dart';

void main() {

  runApp(new FriendlychatApp());

}

class FriendlychatApp extends StatelessWidget {

  @override

  Widget build(BuildContext context) {

    return new MaterialApp(

      title: "Friendlychat",

      home: new ChatScreen(),

    );

  }

}

class ChatScreen extends StatelessWidget {

  @override

  Widget build(BuildContext context) {

    return new Scaffold(

      appBar: new AppBar(title: new Text("Friendlychat")),

    );

  }

}

```

这一步引入了Flutter框架的几个重要的概念

你通过组件中的build()方法来描述用户界面

Flutter框架调用FriendlychatApp或ChatScreen中的build()方法来添加组件,并更新组件的显示层级

[@override](https://api.dartlang.org/stable/1.24.2/dart-core/override-constant.html)是Dart的一个标注,用来标识一个方法重写了父类的方法

部分组件,例如[Scaffold](https://docs.flutter.io/flutter/material/Scaffold-class.html)和[AppBar](https://docs.flutter.io/flutter/material/AppBar-class.html)是[Material Design](https://material.io/)应用所特有的组件

其他组件如[Text](https://docs.flutter.io/flutter/widgets/Text-class.html)是适用于所有app的通用组件.

Flutter框架中的不同库的组件可在同一个app中互相兼容并使用

点击 hot reload 热部署按钮可以迅速看到刚刚做的修改已经生效

把UI组件分类2个类并修改根节点组件,你应该看到界面没有发生变化

**iOS** **与 Android 图示如下**

![image](https://upload-images.jianshu.io/upload_images/6402511-069059882398c18b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](https://upload-images.jianshu.io/upload_images/6402511-0095a3553160304f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.创建一个UI界面来发送消息

接下来将会学习如何创建一个UI界面让用户输入并发送消息

![image](https://upload-images.jianshu.io/upload_images/6402511-00809c23a9c8b559.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在手机上点击文本框会弹出一个软键盘,用户可以输入非空的文字并按软件盘上的Return来发送消息

或者,用户可以通过点击输入框胖的发送按钮来发送消息

目前输入文字的界面在聊天界面的顶部,后续我们会把它移动到底部

添加一个交互式文本输入框

Flutter 框架提供了一个 Material Design 文本框组件[TextField](http://docs.flutter.io/flutter/material/TextField-class.html). 它是一个有状态的组件,拥有可以定制化的输入框属性

状态指的是指在组件从创建到生命周期结束时,可能发生变化的信息

为Friendlychat创建第一个有状态的组件需要对其做一些修改

你需要将数据封装到一个[State](http://docs.flutter.io/flutter/widgets/State-class.html)对象,然后将它和一个[StatefulWidget](http://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)类关联.

如下main.dart代码中展示了如何添加一个交互式输入框.

首先修改ChatScreen继承[StatefulWidget](http://docs.flutter.io/flutter/widgets/StatefulWidget-class.html),而非[StatelessWidget](https://docs.flutter.io/flutter/widgets/StatelessWidget-class.html).

[TextField](http://docs.flutter.io/flutter/material/TextField-class.html)用于处理可变的文本内容,  ChatScreen则作为文本框控制器, 你需要定义一个新的ChatScreenState类来继承[State](http://docs.flutter.io/flutter/widgets/State-class.html)类.

重写[createState()](https://docs.flutter.io/flutter/widgets/StatefulWidget/createState.html)方法来添加ChatScreenState类. 你需要使用这个新的类来创建带有状态的 [TextField](http://docs.flutter.io/flutter/material/TextField-class.html)组件

在[build](https://docs.flutter.io/flutter/widgets/State/build.html)[()](http://docs.flutter.io/flutter/material/State/build.html)方法上方添加一行来定义 ChatScreenState类

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart)

```

// Modify the ChatScreen class definition to extend StatefulWidget.

class ChatScreen extends StatefulWidget {                    //modified

  @override                                                        //new

  State createState() => new ChatScreenState();                    //new

}

// Add the ChatScreenState class definition in main.dart.

class ChatScreenState extends State<ChatScreen> {                  //new

  @override                                                        //new

  Widget build(BuildContext context) {

    return new Scaffold(

      appBar: new AppBar(title: new Text("Friendlychat")),

    );

  }

}

```

现在ChatScreenState的[build](https://docs.flutter.io/flutter/widgets/State/build.html)[()](http://docs.flutter.io/flutter/material/State/build.html)方法包含了之前ChatScreen的创建的所有组件

当Flutter框架调用[build()](https://docs.flutter.io/flutter/widgets/State/build.html)方法刷新 UI时,将会重建ChatScreenState和它的所有子控件

**提示:经常查看**Flutter框架的API和源码对于了解背后的实现机制很有帮助

可以使用IntelliJ编辑器, 选择一个类或者方法, **然后右键选择查找定义来查看Flutter源码**

[TextEditingController](https://docs.flutter.io/flutter/widgets/TextEditingController-class.html)对象可以用来读取输入框中的内容并在信息发送后将其清空,接下来我们来创建一下这个对象

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart)

```

// Add the following code in the ChatScreenState class definition.**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart) 

class ChatScreenState extends State<ChatScreen> {

  final TextEditingController _textController = new TextEditingController(); //new

```

接下来我们定义一个私有方法_buildTextComposer()

返回[Container](https://docs.flutter.io/flutter/widgets/Container-class.html)组件, 其中包含了[TextField](http://docs.flutter.io/flutter/material/TextField-class.html)组件

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart)
```

// Add the following code in the ChatScreenState class definition.

Widget _buildTextComposer() {

  return new Container(

    margin: const EdgeInsets.symmetric(horizontal: 8.0),

    child: new TextField(

      controller: _textController,

      onSubmitted: _handleSubmitted,

      decoration: new InputDecoration.collapsed(

        hintText: "Send a message"),

    ),

  );

}

```

[Container](https://docs.flutter.io/flutter/widgets/Container-class.html)组件首先在屏幕和输入框的四周添加了一个水平边距,这里使用的单位是逻辑像素,实际会根据不同设备转换为特定数量的物理像素

这和iOS中的points点的概念和Android中的 dip (density-independent pixels) 设备无关像素是类似的

设置[TextField](http://docs.flutter.io/flutter/material/TextField-class.html)组件来管理用户交互行为:

TextField构造器包含一个TextEditingController控制器,该控制器用来读取和清空输入框中的内容

为了在用户提交时获取通知, 使用[onSubmitted](https://docs.flutter.io/flutter/material/TextField/onSubmitted.html)参数来提供一个私有的callback回调方法 _handleSubmitted() 目前我们先实现清空输入框的功能,后续会添加实际的发送功能,代码如下

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart)

```

//Add the following code in the ChatScreenState class definition.

void _handleSubmitted(String text) {

  _textController.clear();

}

```

**提示:** 在变量前面的下划线 _ 表明该变量是在类的内部私有的.  Dart编译器会强制校验变量的私有性. 具体可参考Dart官方信息 [libraries and visibility](https://www.dartlang.org/guides/language/language-tour#libraries-and-visibility)

放置文本输入框组件

接下来需要告诉app如何展示这个文本输入框组件. ChatScreenState类的[build()](http://docs.flutter.io/flutter/material/State/build.html)方法中,在[body](https://docs.flutter.io/flutter/material/Scaffold/body.html)属性上添加一个私有方法 _buildTextComposer

_buildTextComposerm返回一个组件,组件中包含了文本输入框

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart)

```

// Modify the code in the ChatScreenState class definition as follows.

  @override

  Widget build(BuildContext context) {

    return new Scaffold(

      appBar: new AppBar(title: new Text("Friendlychat")),

      body: _buildTextComposer(), //new

    );

  }

```

将app热部署后,可以看到如下界面

**iOS** **与 Android**

![image](https://upload-images.jianshu.io/upload_images/6402511-af9b06a840322aa2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](https://upload-images.jianshu.io/upload_images/6402511-f9ee689acee37a65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加带有响应的Send发送按钮

接下来我们在文本框的右侧添加 Send发送按钮.我们使用 [Row](https://docs.flutter.io/flutter/widgets/Row-class.html)行组件作为父节点,使得文本框和发送按钮处于同一行.

然后将[TextField](http://docs.flutter.io/flutter/material/TextField-class.html)组件包裹在[Flexible](https://docs.flutter.io/flutter/widgets/Flexible-class.html)组件中,使得[Row](https://docs.flutter.io/flutter/widgets/Row-class.html)行组件中的文本框能够宽度自适应,自动填满Send按钮之外的剩余空间

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart)

```

//Modify the _buildTextComposer method with the code below to arrange the

// text input field and send button.

Widget _buildTextComposer() {

  return new Container(

    margin: const EdgeInsets.symmetric(horizontal: 8.0),

    child: new Row(                                            //new

      children: <Widget>[                                      //new

        new Flexible(                                          //new

          child: new TextField(

            controller: _textController,

            onSubmitted: _handleSubmitted,

            decoration: new InputDecoration.collapsed(

              hintText: "Send a message"),

          ),

        ),                                                      //new

      ],                                                        //new

    ),                                                          //new

  );

}

```

现在可以着手创建[IconButton](http://docs.flutter.io/flutter/material/IconButton-class.html)带Send图标的按钮组件

在icon属性中,使用常量[Icons.send](https://docs.flutter.io/flutter/material/Icons/send-constant.html)来创建一个新的[Icon](https://docs.flutter.io/flutter/material/Icons-class.html)图标实例. 该常量表示使用了Material Design图标素材库中的send发送图标

**提示:**标准Material Design图标素材库列表可以参考[Material Icons](https://material.io/icons/)官网和[Icons](http://docs.flutter.io/flutter/material/Icons-class.html)类中的常量

把[IconButton](http://docs.flutter.io/flutter/material/IconButton-class.html)组件嵌入[Container](https://docs.flutter.io/flutter/widgets/Container-class.html)父节点组件中,使得你可以自定义按钮的边距, 使之与旁边的输入框视觉上更为匹配. 对于[onPressed](https://docs.flutter.io/flutter/material/IconButton/onPressed.html)点击属性, 使用一个匿名方法来调用_handleSubmitted()方法并使用 _textController控制器来传入消息内容 

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart)

```

// Modify the _buildTextComposer method with the code below to define the

// send button.

Widget _buildTextComposer() {

  return new Container(

    margin: const EdgeInsets.symmetric(horizontal: 8.0),

    child: new Row(

      children: <Widget>[

        new Flexible(

          child: new TextField(

            controller: _textController,

            onSubmitted: _handleSubmitted,

            decoration: new InputDecoration.collapsed(

              hintText: "Send a message"),

          ),

        ),

        new Container(                                                //new

          margin: new EdgeInsets.symmetric(horizontal: 4.0),          //new

          child: new IconButton(                                      //new

            icon: new Icon(Icons.send),                                //new

            onPressed: () => _handleSubmitted(_textController.text)),  //new

        ),                                                            //new

      ],

    ),

  );

}

```

**提示:**Dart语法中, 箭头=> 表示的含义为 { return expression; }.

Dart 函数相关信息, 包括匿名和嵌套函数, 可以参考[Dart Language Tour](https://www.dartlang.org/guides/language/language-tour).

Material Design主题中按钮默认为黑色

可以通过传入颜色参数在IconButton带有图标的按钮中设置颜色,或者你也可以直接选择不同的主题

图标从[IconTheme](https://docs.flutter.io/flutter/material/IconTheme-class.html)主题组件中继承获得颜色,透明度和尺寸,使用[IconThemeData](https://docs.flutter.io/flutter/material/IconThemeData-class.html)对象来定义这些特征. 把所有组件包裹在[IconTheme](https://docs.flutter.io/flutter/material/IconTheme-class.html)组件的_buildTextComposer()方法中, 使用[data](https://docs.flutter.io/flutter/material/IconTheme/data.html)属性来确定[ThemeData](https://docs.flutter.io/flutter/material/ThemeData-class.html)当前主题所使用的具体值. 详见以下代码

[**main.dart**](https://github.com/flutter/friendlychat-steps/blob/master/offline_steps/step_2_composing_messages/lib/main.dart)

```

// Modify the _buildTextComposer method with the code below to give the

// send button the current theme's accent color.

Widget _buildTextComposer() {

  return new IconTheme(                                            //new

    data: new IconThemeData(color: Theme.of(context).accentColor), //new

    child: new Container(                                    //modified

      margin: const EdgeInsets.symmetric(horizontal: 8.0),

      child: new Row(

        children: <Widget>[

          new Flexible(

            child: new TextField(

              controller: _textController,

              onSubmitted: _handleSubmitted,

              decoration: new InputDecoration.collapsed(

                hintText: "Send a message"),

            ),

          ),

          new Container(

            margin: new EdgeInsets.symmetric(horizontal: 4.0),

            child: new IconButton(

              icon: new Icon(Icons.send),

              onPressed: () => _handleSubmitted(_textController.text)),

          ),

        ],

      ),

    ),                                                            //new

  );

}

```

[BuildContext](https://docs.flutter.io/flutter/widgets/BuildContext-class.html)上下文对象是一个当前应用组件树的句柄. 每个组件都有其自己的[BuildContext](https://docs.flutter.io/flutter/widgets/BuildContext-class.html), 它是[StatelessWidget.build](https://docs.flutter.io/flutter/widgets/StatelessWidget/build.html)或[State.build](https://docs.flutter.io/flutter/widgets/State/build.html)方法返回的组件的父节点. 这意味着_buildTextComposer()方法可以访问[State](https://docs.flutter.io/flutter/widgets/State-class.html) 对象内部的BuildContext对象,而不需要在方法中显示的传入上下文对象

热部署应用,可以看到如下界面

iOS与Android

![image](https://upload-images.jianshu.io/upload_images/6402511-525e5f5a10dc880d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](https://upload-images.jianshu.io/upload_images/6402511-7fc9bdc63745d812.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用IntelliJ来调试你的程序

IntelliJ 集成开发环境IDE可以让你很方便地调试 Flutter应用,无论是运行在真机还是模拟器上. 你可以:

选择一个设备或模拟器来调试应用

查看终端的输出信息

在代码中设置断点

在程序运行时查看变量的值

**提示:除了** IntelliJ以外, 还有许多其他工具,命令和技巧可以用来调试Flutter应用 ,更多信息可以参考 [Debugging Flutter Applications](https://flutter.io/debugging/).

IntelliJ 编辑器在程序运行时可以展示运行日志,并提供调试界面来设置断点和控制执行流程

![image](https://upload-images.jianshu.io/upload_images/6402511-d4f74dc090e23179.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用断点

打开你想要设置断点的源文件

点击你所想设置断点的行,然后在菜单中选择**Run运行 > Toggle Line Breakpoint**设置行断点. 或者你也可以点击行号右侧来设置断点

如果你还没有启动调试模式,先将程序终止,然后重新用调试模式启动

然后IntelliJ 编辑器会启动调试界面,并在到达断点时暂停程序.然后你就可以查看你所需要排查的问题了

你可以试着使用调试模式,在Friendlychat应用中的build()方法中设置断点,来调试程序

你可以通过查看调用栈来观察你的app所调用过的方法

6.创建一个UI界面来展示信息

有了基础的app框架后,你就可以着手开发展示信息的界面了

![image](https://upload-images.jianshu.io/upload_images/6402511-d614c3d4bfe45d15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

实现消息列表

接下来,你将创建一个组件来展示用户的聊天信息

你将使用多个小组件来组装一个更为复杂的组件

首先创建一个组件来展示单条消息, 然后将其嵌入一个滚动列表, 接着把滚动列表加入基础的应用框架

先来定义展示单条消息的组件

定义一个无状态组件[StatelessWidget](http://docs.flutter.io/flutter/widgets/StatelessWidget-class.html)名为ChatMessage, 它的[build()](http://docs.flutter.io/flutter/widgets/StatelessWidget/build.html)方法返回一个[Row](https://docs.flutter.io/flutter/widgets/Row-class.html)行组件,它展示了一个简单的发送信息的用户头像, 一个[Column](https://docs.flutter.io/flutter/widgets/Column-class.html)列组件包含了发送消息的人的名字以及消息内容

[**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_3_displaying_messages/lib/main.dart)

```

// Add the following class definition to main.dart.

class ChatMessage extends StatelessWidget {

  ChatMessage({this.text});

  final String text;

  @override

  Widget build(BuildContext context) {

    return new Container(

      margin: const EdgeInsets.symmetric(vertical: 10.0),

      child: new Row(

        crossAxisAlignment: CrossAxisAlignment.start,

        children: <Widget>[

          new Container(

            margin: const EdgeInsets.only(right: 16.0),

            child: new CircleAvatar(child: new Text(_name[0])),

          ),

          new Column(

            crossAxisAlignment: CrossAxisAlignment.start,

            children: <Widget>[

              new Text(_name, style: Theme.of(context).textTheme.subhead),

              new Container(

                margin: const EdgeInsets.only(top: 5.0),

                child: new Text(text),

              ),

            ],

          ),

        ],

      ),

    );

  }

}

```

参考如上代码来定义_name变量, 我们用这个变量来表示发送者的名字. 为了简化代码,这里可以先给这个变量赋一个固定值. 更通用的做法是通过用户登录验证来获取用户名字,具体可参考[Firebase for Flutter](https://codelabs.flutter-io.cn/codelabs/flutter-firebase/index.html)

[**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_3_displaying_messages/lib/main.dart)

```

 //Add the following code to main.dart.

const String _name = "Your Name";

```

为了定制[CircleAvatar](https://docs.flutter.io/flutter/material/CircleAvatar-class.html)组件, 可以把用户名称_name的首字母赋给[Text](https://docs.flutter.io/flutter/widgets/Text-class.html)组件. 我们使用[CrossAxisAlignment.start](https://docs.flutter.io/flutter/rendering/CrossAxisAlignment-class.html)对齐方式传入Row行组件中来定位头像和消息相对于父节点组件的位置

头像的父节点是一个[Row](https://docs.flutter.io/flutter/widgets/Row-class.html)行组件,主坐标为水平方向.CrossAxisAlignment.start坐标对齐方式表示从头开始(从最上端开始). 消息的父节点是一个[Column](https://docs.flutter.io/flutter/widgets/Column-class.html)列组件,主坐标为垂直方向,CrossAxisAlignment.start坐标对齐方式表示从头开始(从最左侧开始)

头像的旁边是2个纵向排列的[Text](https://docs.flutter.io/flutter/widgets/Text-class.html)文本组件,上面展示发送者的名字,下面展示消息

为了给发送者名字加上样式,使名字显示的比消息文本大一些,你需要使用Theme.of(context)来获取一个[ThemeData](https://docs.flutter.io/flutter/material/ThemeData-class.html)主题数据对象. [textTheme](https://docs.flutter.io/flutter/material/ThemeData/textTheme.html)文本主题属性代表了 Material Design的一个文本样式比如[subhead](https://docs.flutter.io/flutter/material/TextTheme/subhead.html)副标题, 使你可以避免在代码中写死字体字号等属性

目前我们还没有为app指定一个主题,Theme.of(context)会得到一个默认的Flutter主题

后续你可以重写这个默认主题,使得你的app在android和iOS中展示不同的样式

实现消息列表

接下来进一步细化UI,获取聊天消息并在界面中展示

这个列表应是可滚动的,使得用户可以查看聊天历史记录

排序应采用时间顺序,优先展示最新的消息

在ChatScreenState组件中, 添加一个[List](https://api.dartlang.org/stable/1.22.1/dart-core/List-class.html)列表成员变量_messages来表示消息列表. 列表中每一项都是一个ChatMessage聊天消息实例. 你需要把这个列表初始化为一个空列表

```

// Add the following code to the ChatScreenState class definition.

class ChatScreenState extends State<ChatScreen> {

  final List<ChatMessage> _messages = <ChatMessage>[];            // new

  final TextEditingController _textController = new TextEditingController();

```

[**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_3_displaying_messages/lib/main.dart)

当用户发送消息时,需要把新消息加入到消息列表中

修改_handleSubmitted()处理提交方法, 如下

```

// Modify the code in the _handleSubmitted method definition.

void _handleSubmitted(String text) {

  _textController.clear();

    ChatMessage message = new ChatMessage(                        //new

      text: text,                                                  //new

    );                                                            //new

    setState(() {                                                  //new

      _messages.insert(0, message);                                //new

    });                                                            //new

}

```

[**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_3_displaying_messages/lib/main.dart)

调用[setState()](https://docs.flutter.io/flutter/widgets/State/setState.html)来修改_messages列表,并且通知Flutter框架,组件树中的部分内容已经发生了修改,需要重建UI. 在[setState()](https://docs.flutter.io/flutter/widgets/State/setState.html)设置状态方法中只应该有同步操作,否则组件可能在操作完成前就已被重建.


一般来说,你是有可能调用 [`setState()`](https://docs.flutter.io/flutter/widgets/State/setState.html) 方法时使用一个空的闭包,在私有数据发生变化时,在方法外部来更新变量的值. 然而 ,在[`setState()`](https://docs.flutter.io/flutter/widgets/State/setState.html)方法的内部来更新私有数据是更为推荐的做法,这样你就不会忘记后续还需要调用这个方法.

## 放置消息列表

我们会从 `_messages`列表中获取 `ChatMessage` 组件, 然后放入一个 [`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html) 列表视图组件(含滚动条)

在`ChatScreenState`类的 [`build()`](http://docs.flutter.io/flutter/material/State/build.html) 方法中,添加一个 [`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html) 列表视图组件. 我们选择使用 [`ListView.builder`](https://docs.flutter.io/flutter/widgets/ListView/ListView.builder.html) 构造器是因为默认的构造器并不会自动检测到 `children` 子节点的更新.

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_3_displaying_messages/lib/main.dart)

```
// Modify the code in the ChatScreenState class definition as follows.

Widget build(BuildContext context) {
  return new Scaffold(
    appBar: new AppBar(title: new Text("Friendlychat")),
    body: new Column(                                        //modified
      children: <Widget>[                                         //new
        new Flexible(                                             //new
          child: new ListView.builder(                            //new 
            padding: new EdgeInsets.all(8.0),                     //new
            reverse: true,                                        //new
            itemBuilder: (_, int index) => _messages[index],      //new
            itemCount: _messages.length,                          //new
          ),                                                      //new
        ),                                                        //new
        new Divider(height: 1.0),                                 //new
        new Container(                                            //new
          decoration: new BoxDecoration(
            color: Theme.of(context).cardColor),                  //new
          child: _buildTextComposer(),                       //modified
        ),                                                        //new
      ],                                                          //new
    ),                                                            //new
  );
}
```

 [`Scaffold`](http://docs.flutter.io/flutter/material/Scaffold-class.html) 框架组件的 `body`属性现在包含了一个消息列表和一个输入框以及按钮. 我们使用了如下的布局组件

*   [`Column`](https://docs.flutter.io/flutter/widgets/Column-class.html)列,表示垂直排列组件 [`Column`](https://docs.flutter.io/flutter/widgets/Column-class.html) 列可以接收多个子组件, 形成一个滚动条, 每一行展示一个输入框
*   [`Flexible`](https://docs.flutter.io/flutter/widgets/Flexible-class.html)灵活自适应, 作为 [`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html)列表视图的父节点. 使得消息列表能够自动扩展填满[`Column`](https://docs.flutter.io/flutter/widgets/Column-class.html) 列的高度, 而[`TextField`](https://docs.flutter.io/flutter/material/TextField-class.html) 文本框则保持固定尺寸.
*   [`Divider`](https://docs.flutter.io/flutter/material/Divider-class.html)分割线, 指的是一条水平线,插入在上方的消息列表和下方的文本输入框之间
*   [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html)容器, 作为文本输入框的父节点, 可以很方便的定义背景图片, 以及空白,边距和其他排版参数. 使用 [`decoration`](https://docs.flutter.io/flutter/widgets/Container/decoration.html) 装饰来创建一个新的[`BoxDecoration`](https://docs.flutter.io/flutter/painting/BoxDecoration-class.html) 盒状装饰来定义背景颜色.这里我们使用了 [`cardColor`](https://docs.flutter.io/flutter/material/ThemeData/cardColor.html) 卡片色,  该颜色在[`ThemeData`](https://docs.flutter.io/flutter/material/ThemeData-class.html) 主题数据中有定义. 使得输入框的界面背景颜色和消息列表有所区分

[`ListView.builder`](https://docs.flutter.io/flutter/widgets/ListView-class.html) 中的参数说明如下:

*   [`padding`](https://docs.flutter.io/flutter/widgets/BoxScrollView/padding.html) 文本消息框旁的空白
*   [`reverse`](https://docs.flutter.io/flutter/widgets/ScrollView/reverse.html) 使得列表 [`ListView`](https://docs.flutter.io/flutter/widgets/ListView-class.html) 从屏幕的底部开始
*   `itemCount` 表示消息列表的条数
*   `itemBuilder` 对于每个 `[index]`组件中的内容进行渲染的方法. 由于我们不需要当前上下文, 可以忽略 [`IndexedWidgetBuilder`](https://docs.flutter.io/flutter/widgets/IndexedWidgetBuilder.html)第一个参数. 使用 `_` (下划线)作为一种简写来表示该参数不使用

热部署app后,你可以看到如下内容:

| 

**iOS**

 | 

**Android**

 |
| 

![image](http://upload-images.jianshu.io/upload_images/6402511-650c6d5d0908d632.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 | 

![image](http://upload-images.jianshu.io/upload_images/6402511-0254ba0c7fc6d784.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 |
现在你可以使用刚刚创建的UI来试着发送一些消息

| 

**iOS**

 | 

**Android**

 |
| 

![image](http://upload-images.jianshu.io/upload_images/6402511-02c99e72c5a1dc16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 | 

![image](http://upload-images.jianshu.io/upload_images/6402511-c18ea5ff8a6ea984.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 |
 ## 7. 为 App增加动画效果
你可以通过给组件增加动画效果给用户更直观顺畅的感觉.
本节内容会探讨如何给聊天消息列表增加动画效果

当用户发送新消息时,不再简单的展现给用户消息列表,而是让列表从底部淡出

Flutter 中的动画被封装在 [`Animation`](http://docs.flutter.io/flutter/animation/Animation-class.html) 对象中,包含一个类型值和状态值 (比如 *forward*向前, *backward*向后, *completed*完成, 和 *dismissed*消失). 你可以把动画效果添加到组件上或者监听动画对象的变化. 基于动画对象属性的变化, Flutter框架可以修改组件展现和重建的方式

## 指定 Animation Controller 动画控制器

使用 [`AnimationController`](http://docs.flutter.io/flutter/animation/AnimationController-class.html) 动画控制器类来指定动画的运行方式. [`AnimationController`](http://docs.flutter.io/flutter/animation/AnimationController-class.html) 类允许你定义动画中的重要属性, 比如持续时间和播放方式 (正放或倒放).

创建 [`AnimationController`](http://docs.flutter.io/flutter/animation/AnimationController-class.html)时, 需要传入 `vsync` 参数. `vsync` 使得动画在屏幕上不展现时释放它所占用的不必要的资源. 要把 `ChatScreenState` 设为 `vsync`时, 在`ChatScreenState` 类中引用一下[`TickerProviderStateMixin`](https://docs.flutter.io/flutter/widgets/TickerProviderStateMixin-class.html)  .

**提示:** 在Dart中, mixin混入表示允许一个类的body部分进行多个继承(一个类本身只能有一个父类,但body部分允许多继承). 详情可参考 [Classes](https://www.dartlang.org/articles/language/mixins), 以及 [Dart Language Tour](https://www.dartlang.org/guides/language/language-tour).

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_4_animate/lib/main.dart)

```
// Modify the code in the ChatScreenState class definition as follows.

class ChatScreenState extends State<ChatScreen> with TickerProviderStateMixin { // modified
  final List<ChatMessage> _messages = <ChatMessage>[];
  final TextEditingController _textController = new TextEditingController();
```

在`ChatMessage`类中, 添加成员变量来存储动画控制器 

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_4_animate/lib/main.dart)

```
// Modify the ChatMessage class definition as follows.

class ChatMessage extends StatelessWidget {
  ChatMessage({this.text, this.animationController});         //modified
  final String text;
  final AnimationController animationController;                   //new
```

按如下方式修改 `ChatScreenState` 类中的 `_handleSubmitted()` 方法. 初始化一个 [`AnimationController`](http://docs.flutter.io/flutter/animation/AnimationController-class.html) 对象并指定持续时间为700毫秒. (这里我们选择了一个较长的动画持续时间,使你能更清楚的看到动画过程;实际使用时, 一般会设置一个较短的持续时间并且禁用慢速模式[disable slow mode](https://flutter.io/faq/#my-app-has-a-slow-mode-bannerribbon-in-the-upper-right-why-am-i-seeing-that) )

把动画控制器添加到一个新的 `ChatMessage` 实例上,并指定在收到新消息时,正向播放动画 

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_4_animate/lib/main.dart)

```
// Modify the _handleSubmittted method definition as follows.

void _handleSubmitted(String text) {
  _textController.clear();
  ChatMessage message = new ChatMessage(
    text: text,
    animationController: new AnimationController(                  //new
      duration: new Duration(milliseconds: 700),                   //new
      vsync: this,                                                 //new
    ),                                                             //new
  );                                                               //new
  setState(() {
    _messages.insert(0, message);
  });
  message.animationController.forward();                           //new
}
```

## 添加 SizeTransition 形变组件

修改`ChatMessage`对象的 `build()` 方法来返回一个 [`SizeTransition`](https://docs.flutter.io/flutter/widgets/SizeTransition-class.html) 形变组件包住先前定义的 [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html)子组件 .  [`SizeTransition`](https://docs.flutter.io/flutter/widgets/SizeTransition-class.html) 类提供了一个动画效果, 使得子组件的长度和宽度乘以一个给定的比例.

[`CurvedAnimation`](https://docs.flutter.io/flutter/animation/CurvedAnimation-class.html) 曲线动画, 和 [`SizeTransition`](http://docs.flutter.io/flutter/widgets/SizeTransition-class.html) 形变共同作用下, 可以产生一个淡出效果. 淡出效果使得消息能在一开始快速滑入,并在结束时慢慢停下.

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_4_animate/lib/main.dart)

```
// Modify the build() method for the ChatMessage class as follows.

class ChatMessage extends StatelessWidget {
  ChatMessage({this.text, this.animationController});
  final String text;
  final AnimationController animationController;
  @override
  Widget build(BuildContext context) {
    return new SizeTransition(                                    //new
    sizeFactor: new CurvedAnimation(                              //new
        parent: animationController, curve: Curves.easeOut),      //new
    axisAlignment: 0.0,                                           //new
    child: new Container(                                    //modified
      margin: const EdgeInsets.symmetric(vertical: 10.0),
      child: new Row(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            new Container(
              margin: const EdgeInsets.only(right: 16.0),
              child: new CircleAvatar(child: new Text(_name[0])),
            ),
            new Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: <Widget>[
                new Text(_name, style: Theme.of(context).textTheme.subhead),
                new Container(
                  margin: const EdgeInsets.only(top: 5.0),
                  child: new Text(text),
                ),
              ],
            ),
          ],
        ),
      )                                                           //new
    );
  }
}
```

## 释放动画

释放动画控制器来释放你不再需要的资源是一个良好的编码习惯. 如下代码展示了通过重载`ChatScreenState`类的 [`dispose()`](https://docs.flutter.io/flutter/widgets/State/dispose.html) 方法来释放动画. 在此app中, Flutter框架不会调用 [`dispose()`](https://docs.flutter.io/flutter/widgets/State/dispose.html)方法, 因为这是一个单屏应用. 在有多个屏幕界面的复杂应用中,当 `ChatScreenState` 对象不再使用时,dispose()方法会被调用

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_4_animate/lib/main.dart)

```
// Add the following code to the ChatScreenState class definition.

@override
void dispose() {                                                   //new
  for (ChatMessage message in _messages)                           //new
    message.animationController.dispose();                         //new
  super.dispose();                                                 //new
}                                                                  //new
```

要看到动画效果, 需要重启应用并输入一写文字. 使用重启而不是热部署可以清除先前已经存在的不含有动画控制器的消息

如果你想尝试更多的动画效果, 如下可供参考:

*   加快或减慢动画速度,可以通过修改_handleSubmitted()方法中的  [`duration`](https://docs.flutter.io/flutter/animation/AnimationController/duration.html) 参数值
*   使用不同的曲线效果可以使用 [`Curves`](http://docs.flutter.io/flutter/animation/Curves-class.html) 类中定义的不同的常数值.
*   创建一个隐现效果可以在 [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html) 容器外包一个 [`FadeTransition`](https://docs.flutter.io/flutter/widgets/FadeTransition-class.html)隐现变换组件而不是使用 `SizeTransition`形变

## 8. 处理手势点击

这一部分我们会加入更多细节,包括只有当文本框中有内容时启用发送按钮,使较长的文本自动换行, 以及为 iOS 与 Android添加原生的样式.

## Make the Send button context-aware

Currently, the Send button appears enabled even when there is no text in the input field. You might want the button's appearance to change depending on whether the field contains text to send.

Define `_isComposing`, a private member variable that is true whenever the user is typing in the input field.

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_5_finishing_touches/lib/main.dart)

```
// Add the following code in the ChatScreenState class definition.

class ChatScreenState extends State<ChatScreen> with TickerProviderStateMixin {
  final List<ChatMessage> _messages = <ChatMessage>[];
  final TextEditingController _textController = new TextEditingController();
  bool _isComposing = false;                                      //new
```

To be notified about changes to the text as the user interacts with the field, pass an [`onChanged`](https://docs.flutter.io/flutter/material/TextField/onChanged.html) callback to the TextField constructor. [`TextField`](http://docs.flutter.io/flutter/material/TextField-class.html) calls this method whenever its value changes with the current value of the field. In your `onChanged` callback, call [`setState()`](https://docs.flutter.io/flutter/widgets/State/setState.html) to change the value of `_isComposing` to true when the field contains some text.

Then modify the `onPressed` argument to be `null` when `_isComposing` is false.

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_5_finishing_touches/lib/main.dart)

```
// Modify the _buildTextComposer method with the code below
// to add the onChanged() and onPressed() callbacks.

Widget _buildTextComposer() {
  return new IconTheme(
    data: new IconThemeData(color: Theme.of(context).accentColor),
    child: new Container(
      margin: const EdgeInsets.symmetric(horizontal: 8.0),
      child: new Row(
        children: <Widget>[
          new Flexible(
            child: new TextField(
              controller: _textController,
              onChanged: (String text) {          //new
                setState(() {                     //new
                  _isComposing = text.length > 0; //new
                });                               //new
              },                                  //new
              onSubmitted: _handleSubmitted,
              decoration:
                  new InputDecoration.collapsed(hintText: "Send a message"),
            ),
          ),
          new Container(
            margin: new EdgeInsets.symmetric(horizontal: 4.0),
            child: new IconButton(
              icon: new Icon(Icons.send),
              onPressed: _isComposing
                  ? () => _handleSubmitted(_textController.text)    //modified
                  : null,                                           //modified
            ),
          ),
        ],
      ),
    ),
  );
}
```

Modify `_handleSubmitted` to update `_isComposing` to false when the text field is cleared.

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_4_animate/lib/main.dart)

```
// Modify the _handleSubmittted method definition as follows.

void _handleSubmitted(String text) {
  _textController.clear();
  setState(() {                                                    //new
    _isComposing = false;                                          //new
  });                                                              //new
  ChatMessage message = new ChatMessage(
    text: text,
    animationController: new AnimationController(
      duration: new Duration(milliseconds: 700),
      vsync: this,
    ),
  );
  setState(() {
    _messages.insert(0, message);
  });
  message.animationController.forward();
}
```

The `_isComposing` variable now controls the behavior and the visual appearance of the **Send** button.

*   If the user types a string in the text field, `_isComposing` is `true` and the button's color is set to `Theme.of(context).accentColor`. When the user presses the button, the system invokes `_handleSubmitted()`.
*   If the user types nothing in the text field, `_isComposing` is `false` and the widget's [`onPressed`](http://docs.flutter.io/flutter/material/IconButton/onPressed.html) property is set to `null`, disabling the send button. The framework will automatically change the button's color to `Theme.of(context).disabledColor`.

## Wrap longer lines

When a user sends a text that exceeds the width of the UI for displaying messages, the lines should wrap so the entire message displays. Right now, lines that overflow are truncated and a error message is displayed. A simple way of making sure the text wraps correctly is to add an [`Expanded`](https://docs.flutter.io/flutter/widgets/Expanded-class.html) widget.

In this step, you'll wrap the Column widget where messages are displayed in an Expanded widget. Expanded allows a widget like Column to impose layout constraints (in this case the Column's width), on a child widget. Here it constrains the width of the [`Text`](https://docs.flutter.io/flutter/widgets/Text-class.html) widget, which is normally determined by its contents.

This part of the widget hierarchy is defined by the `build()` method of the `ChatMessage` class. We'll try out a couple of handy IntelliJ shortcuts to add a parent widget:

1.  Place the cursor in the `new Column` expression.
2.  Click the lightbulb icon in the left margin and select **Wrap with new widget** from the popup menu. IntelliJ adds a generic `new widget` expression, correctly formatted, for you to customize. This quick way of adding expressions and nesting widgets is even faster when you use the keyboard shortcut, option+return (macOS) or alt+enter (Linux, Windows).
3.  With the cursor over the highlighted `widget` keyword placeholder, press the key combination for smart code completion. If you need to look it up, see the [IntelliJ IDEA reference](https://resources.jetbrains.com/storage/products/intellij-idea/docs/IntelliJIDEA_ReferenceCard.pdf).
4.  Select **Expanded** from the list of possible objects that can be a parent of Column. It should be the first item on the list.

![image](http://upload-images.jianshu.io/upload_images/6402511-bfa363c6f2aa2dc9.png?imageMogr2/auto-orient/strip)

The following code snippet shows how the `ChatMessage` class looks after making this change:

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_5_finishing_touches/lib/main.dart)

```
//Modify the ChatMessage class definition in main.dart.

...

new Expanded(                                               //new
  child: new Column(                                   //modified
    crossAxisAlignment: CrossAxisAlignment.start,
    children: <Widget>[
      new Text(_name, style: Theme.of(context).textTheme.subhead),
      new Container(
        margin: const EdgeInsets.only(top: 5.0),
        child: new Text(text),
      ),
    ],
  ),
),                                                          //new

...
```

## Customize for iOS and Android

To give your app's UI a natural look and feel, you can add a theme and some simple logic to the [`build()`](https://docs.flutter.io/flutter/widgets/StatelessWidget/build.html) method for the `FriendlychatApp` class. In this step, you define a platform theme that applies a different set of primary and accent colors. You also customize the Send button to use a [`CupertinoButton`](https://docs.flutter.io/flutter/cupertino/CupertinoButton-class.html) on iOS and a Material Design [`IconButton`](https://docs.flutter.io/flutter/material/IconButton-class.html) on Android.

| 

**iOS**

 | 

**Android**

 |
| 

![image](http://upload-images.jianshu.io/upload_images/6402511-40adf53b8b396ccb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 | 

![image](http://upload-images.jianshu.io/upload_images/6402511-fae3cc3165082469.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 |

First, define a new [`ThemeData`](https://docs.flutter.io/flutter/material/ThemeData-class.html) object named `kIOSTheme` with colors for iOS (light grey with orange accents) and another [`ThemeData`](https://docs.flutter.io/flutter/material/ThemeData-class.html) object `kDefaultTheme` with colors for Android (purple with orange accents).

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_5_finishing_touches/lib/main.dart)

```
// Add the following code to main.dart.

final ThemeData kIOSTheme = new ThemeData(
  primarySwatch: Colors.orange,
  primaryColor: Colors.grey[100],
  primaryColorBrightness: Brightness.light,
);

final ThemeData kDefaultTheme = new ThemeData(
  primarySwatch: Colors.purple,
  accentColor: Colors.orangeAccent[400],
);
```

Modify the `FriendlychatApp` class to vary the theme using the [`theme`](https://docs.flutter.io/flutter/material/MaterialApp/theme.html) property of your app's [`MaterialApp`](https://docs.flutter.io/flutter/material/MaterialApp-class.html) widget. Use the top-level [`defaultTargetPlatform`](https://docs.flutter.io/flutter/foundation/defaultTargetPlatform.html) property and [conditional operators](https://www.dartlang.org/guides/language/language-tour#operators) to build an expression for selecting a theme.

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_5_finishing_touches/lib/main.dart)

```
// Add the following code to main.dart.

import 'package:flutter/foundation.dart';                        //new

// Modify the FriendlychatApp class definition in main.dart.

class FriendlychatApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: "Friendlychat",
      theme: defaultTargetPlatform == TargetPlatform.iOS         //new
        ? kIOSTheme                                              //new
        : kDefaultTheme,                                         //new
      home: new ChatScreen(),
    );
  }
}
```

We can apply the selected theme to the [`AppBar`](https://docs.flutter.io/flutter/material/AppBar-class.html) widget (the banner at the top of your app's UI). The `elevation` property defines the z-coordinates of the [`AppBar`](https://docs.flutter.io/flutter/material/AppBar-class.html). A z-coordinate value of `0.0` has no shadow (iOS) and a value of `4.0` has a defined shadow (Android).

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_5_finishing_touches/lib/main.dart)

```
// Modify the build() method of the ChatScreenState class.

Widget build(BuildContext context) {
  return new Scaffold(
    appBar: new AppBar(
      title: new Text("Friendlychat"),                                 //modified
      elevation:
         Theme.of(context).platform == TargetPlatform.iOS ? 0.0 : 4.0, //new
   ),
```

Customize the Send icon by modifying its [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html) parent widget in the `_buildTextComposer` method. Use the [`child`](https://docs.flutter.io/flutter/widgets/Container/child.html) property and [conditional operators](https://www.dartlang.org/guides/language/language-tour#operators) to build an expression for selecting a button.

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_5_finishing_touches/lib/main.dart)

```
// Add the following code to main.dart.

import 'package:flutter/cupertino.dart';                      //new

// Modify the _buildTextComposer method.

new Container(
   margin: new EdgeInsets.symmetric(horizontal: 4.0),
   child: Theme.of(context).platform == TargetPlatform.iOS ?  //modified
   new CupertinoButton(                                       //new
     child: new Text("Send"),                                 //new
     onPressed: _isComposing                                  //new
         ? () =>  _handleSubmitted(_textController.text)      //new
         : null,) :                                           //new
   new IconButton(                                            //modified
       icon: new Icon(Icons.send),
       onPressed: _isComposing ?
           () =>  _handleSubmitted(_textController.text) : null,
       )
   ),
```

Wrap the top-level `Column` in a [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html) widget to give it a light grey border on its upper edge. This border will help visually distinguish the app bar from the body of the app on iOS. To hide the border on Android, apply the same logic used for the app bar in the previous snippet.

### [**main.dart**](https://github.com/flutter/friendlychat-flutter/blob/master/offline_steps/step_5_finishing_touches/lib/main.dart)

```
// Modify the following lines in main.dart.

Widget build(BuildContext context) {
  return new Scaffold(
    appBar: new AppBar(
        title: new Text("Friendlychat"),
        elevation:
            Theme.of(context).platform == TargetPlatform.iOS ? 0.0 : 4.0),
    body: new Container(                                             //modified
        child: new Column(                                           //modified
          children: <Widget>[
            new Flexible(
              child: new ListView.builder(
                padding: new EdgeInsets.all(8.0),
                reverse: true,
                itemBuilder: (_, int index) => _messages[index],
                itemCount: _messages.length,
              ),
            ),
            new Divider(height: 1.0),
            new Container(
              decoration: new BoxDecoration(color: Theme.of(context).cardColor),
              child: _buildTextComposer(),
            ),
          ],
        ),
        decoration: Theme.of(context).platform == TargetPlatform.iOS //new
            ? new BoxDecoration(                                     //new
                border: new Border(                                  //new
                  top: new BorderSide(color: Colors.grey[200]),      //new
                ),                                                   //new
              )                                                      //new
            : null),                                                 //modified
  );
}
```

Hot reload the app. You should see different colors, shadows, and icon buttons for iOS and Android.</google-codelab-step>


