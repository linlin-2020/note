# Activity、PhoneWindow、DecorView、ViewRootImpl 的关系

1. PhoneWindow是Window 的唯一子类，每个Activity都会创建一个PhoneWindow对象，你可以理解它为一个窗口，但不是真正的可视窗口，而是一个管理类，是Activity和整个View系统交互的接口，是Activity和View交互系统的中间层。

1. DecorView是PhoneWindow的一个内部类，是整个View层级的最顶层，一般包括标题栏和内容栏两部分，会根据不同的主题特性调整不同的布局。它是在setContentView方法中被创建，具体点来说是在PhoneWindow的installDecor方法中被创建。

1. ViewRootImpl是DecorView的parent，用来控制View的各种事件，在handleResumeActivity方法中被创建

## window的概念

假定没有window，只有view作为UI显示。如一个空白界面中，只有一个button，然后添加一个text view，覆盖button的一部分，显示效果是什么样？button在上面还是text view在上面。显示层级就是window存在的作用之一。又比如，某个期间弹出dialog提示用户，后续又弹出popupwindow显示在dialog下面，又弹出toast在dialog之上。这些都需要某个东西来管理这些显示的层级。

**window机制就是为了解决屏幕上的view的显示逻辑问题。**

什么是window，在Android机制中，每个view树就代表一个window。
为什么不是每个view，因为每个view在view树中的显示次序是固定的，被认为不可分割的。那啥是属于可分割的，比如在一个activity中显示一个dialog，dialog的view是在dialog中单独添加的，不属于activity中view树。

> 什么是view树？例如你在布局中给Activity设置了一个布局xml，那么最顶层的布局如LinearLayout就是view树的根，他包含的所有view就都是该view树的节点，所以这个view树就对应一个window。
> 
> 举几个具体的例子：
> 
> 我们在添加dialog的时候，需要给他设置view，那么这个view他是不属于antivity的布局内的，是通过WindowManager添加到屏幕上的，不属于activity的view树内，所以这个dialog是一个独立的view树，所以他是一个window。
> 
> popupWindow他也对应一个window，因为它也是通过windowManager添加上去的，不属于Activity的view树。
> 
> 当我们使用使用windowManager在屏幕上添加的任何view都不属于Activity的布局view树，即使是只添加一个button。

**view树是window机制的操作单位，每一个view树对应一个window。window是view的载体(管理者)，view是window的表现形式。** 通常看的dialog、popupwindow、activity都是window的表现形式。

window本身并不存在，他只是一个概念。

举个栗子：如班集体，就是一个概念，他的存在形式是这整个班的学生，当学生不存在那么这个班集体也就不存在。但是他的好处是得到了一个新的概念，我们可以以班为单位来安排活动。

总结：
- window机制是为了解决view显示混乱的问题，让所有view按照次序显示存在的。

- window是window机制中的操作单位，每个window对应一个view。

- window本身并不存在。是一个抽象的概念，view是window的表现形式，每个view的当前状态保存在WindowManagerService中的windowStatus。

***

## window的属性

### window的type

前面我们讲到window机制解决的一个问题就是view的显示次序问题，这个属性就决定了window的显示次序。

window是有分类的，不同类别的显示高度范围不同，例如我把1-1000m高度称为低空，1001-2000m高度称为中空，2000以上称为高空。window也是一样按照高度范围进行分类，他也有一个变量Z-Order，决定了window的高度。window一共可分为三类：

- 应用程序窗口：应用程序窗口一般位于最底层，Z-Order在1-99

- 子窗口：子窗口一般是显示在应用窗口之上，Z-Order在1000-1999

- 系统级窗口：系统级窗口一般位于最顶层，不会被其他的window遮住，如Toast，Z-Order在2000-2999。如果要弹出自定义系统级窗口需要动态申请权限。

Z-Order越大，window越靠近用户，也就显示越高，高度高的window会覆盖高度低的window。

window的type属性就是Z-Order的值，我们可以给window的type属性赋值来决定window的高度。系统为我们三类window都预设了静态常量，如下（以下常用参数介绍转自参考文献第一篇文章）：

应用级window
```
// 应用程序 Window 的开始值（最小值）
public static final int FIRST_APPLICATION_WINDOW = 1;

// 应用程序 Window 的基础值（？）
public static final int TYPE_BASE_APPLICATION = 1;

// 普通的应用程序
public static final int TYPE_APPLICATION = 2;

// 特殊的应用程序窗口，当程序可以显示 Window 之前使用这个 Window 来显示一些东西（？）
public static final int TYPE_APPLICATION_STARTING = 3;

// TYPE_APPLICATION 的变体，在应用程序显示之前，WindowManager 会等待这个 Window 绘制完毕
public static final int TYPE_DRAWN_APPLICATION = 4;

// 应用程序 Window 的结束值
public static final int LAST_APPLICATION_WINDOW = 99;
```

子window
```
// 子 Window 类型的开始值
public static final int FIRST_SUB_WINDOW = 1000;

// 应用程序 Window 顶部的面板。这些 Window 出现在其附加 Window 的顶部。
public static final int TYPE_APPLICATION_PANEL = FIRST_SUB_WINDOW;

// 用于显示媒体(如视频)的 Window。这些 Window 出现在其附加 Window 的后面。
public static final int TYPE_APPLICATION_MEDIA = FIRST_SUB_WINDOW + 1;

// 应用程序 Window 顶部的子面板。这些 Window 出现在其附加 Window 和任何Window的顶部
public static final int TYPE_APPLICATION_SUB_PANEL = FIRST_SUB_WINDOW + 2;

// 当前Window的布局和顶级Window布局相同时，不能作为子代的容器
public static final int TYPE_APPLICATION_ATTACHED_DIALOG = FIRST_SUB_WINDOW + 3;

// 用显示媒体 Window 覆盖顶部的 Window， 这是系统隐藏的 API
public static final int TYPE_APPLICATION_MEDIA_OVERLAY  = FIRST_SUB_WINDOW + 4;

// 子面板在应用程序Window的顶部，这些Window显示在其附加Window的顶部， 这是系统隐藏的 API
public static final int TYPE_APPLICATION_ABOVE_SUB_PANEL = FIRST_SUB_WINDOW + 5;

// 子 Window 类型的结束值
public static final int LAST_SUB_WINDOW = 1999;
```
系统级window
```
// 系统Window类型的开始值
public static final int FIRST_SYSTEM_WINDOW = 2000;

// 系统状态栏，只能有一个状态栏，它被放置在屏幕的顶部，所有其他窗口都向下移动
public static final int TYPE_STATUS_BAR = FIRST_SYSTEM_WINDOW;

// 系统搜索窗口，只能有一个搜索栏，它被放置在屏幕的顶部
public static final int TYPE_SEARCH_BAR = FIRST_SYSTEM_WINDOW+1;

// 已经从系统中被移除，可以使用 TYPE_KEYGUARD_DIALOG 代替
public static final int TYPE_KEYGUARD = FIRST_SYSTEM_WINDOW+4;

// 系统对话框窗口
public static final int TYPE_SYSTEM_DIALOG = FIRST_SYSTEM_WINDOW+8;

// 锁屏时显示的对话框
public static final int TYPE_KEYGUARD_DIALOG = FIRST_SYSTEM_WINDOW+9;

// 输入法窗口，位于普通 UI 之上，应用程序可重新布局以免被此窗口覆盖
public static final int TYPE_INPUT_METHOD = FIRST_SYSTEM_WINDOW+11;

// 输入法对话框，显示于当前输入法窗口之上
public static final int TYPE_INPUT_METHOD_DIALOG= FIRST_SYSTEM_WINDOW+12;

// 墙纸
public static final int TYPE_WALLPAPER = FIRST_SYSTEM_WINDOW+13;

// 状态栏的滑动面板
public static final int TYPE_STATUS_BAR_PANEL = FIRST_SYSTEM_WINDOW+14;

// 应用程序叠加窗口显示在所有窗口之上
public static final int TYPE_APPLICATION_OVERLAY = FIRST_SYSTEM_WINDOW + 38;

// 系统Window类型的结束值
public static final int LAST_SYSTEM_WINDOW = 2999;
```

### Window的flags参数

flag标志控制window的东西比较多，很多资料的描述是“控制window的显示”，但我觉得不够准确。

flag控制的范围包括了：各种情景下的显示逻辑（锁屏，游戏等）还有触控事件的处理逻辑。控制显示确实是他的很大部分功能，但是并不是全部。下面看一下一些常用的flag，就知道flag的功能了（以下常用参数介绍转自参考文献第一篇文章）：
```
// 当 Window 可见时允许锁屏
public static final int FLAG_ALLOW_LOCK_WHILE_SCREEN_ON = 0x00000001;

// Window 后面的内容都变暗
public static final int FLAG_DIM_BEHIND = 0x00000002;

// Window 不能获得输入焦点，即不接受任何按键或按钮事件，例如该 Window 上 有 EditView，点击 EditView 是 不会弹出软键盘的
// Window 范围外的事件依旧为原窗口处理；例如点击该窗口外的view，依然会有响应。另外只要设置了此Flag，都将会启用FLAG_NOT_TOUCH_MODAL
public static final int FLAG_NOT_FOCUSABLE = 0x00000008;

// 设置了该 Flag,将 Window 之外的按键事件发送给后面的 Window 处理, 而自己只会处理 Window 区域内的触摸事件
// Window 之外的 view 也是可以响应 touch 事件。
public static final int FLAG_NOT_TOUCH_MODAL  = 0x00000020;

// 设置了该Flag，表示该 Window 将不会接受任何 touch 事件，例如点击该 Window 不会有响应，只会传给下面有聚焦的窗口。
public static final int FLAG_NOT_TOUCHABLE      = 0x00000010;

// 只要 Window 可见时屏幕就会一直亮着
public static final int FLAG_KEEP_SCREEN_ON     = 0x00000080;

// 允许 Window 占满整个屏幕
public static final int FLAG_LAYOUT_IN_SCREEN   = 0x00000100;

// 允许 Window 超过屏幕之外
public static final int FLAG_LAYOUT_NO_LIMITS   = 0x00000200;

// 全屏显示，隐藏所有的 Window 装饰，比如在游戏、播放器中的全屏显示
public static final int FLAG_FULLSCREEN      = 0x00000400;

// 表示比FLAG_FULLSCREEN低一级，会显示状态栏
public static final int FLAG_FORCE_NOT_FULLSCREEN   = 0x00000800;

// 当用户的脸贴近屏幕时（比如打电话），不会去响应此事件
public static final int FLAG_IGNORE_CHEEK_PRESSES    = 0x00008000;

// 则当按键动作发生在 Window 之外时，将接收到一个MotionEvent.ACTION_OUTSIDE事件。
public static final int FLAG_WATCH_OUTSIDE_TOUCH = 0x00040000;

@Deprecated
// 窗口可以在锁屏的 Window 之上显示, 使用 Activity#setShowWhenLocked(boolean) 方法代替
public static final int FLAG_SHOW_WHEN_LOCKED = 0x00080000;

// 表示负责绘制系统栏背景。如果设置，系统栏将以透明背景绘制，
// 此 Window 中的相应区域将填充 Window＃getStatusBarColor（）和 Window＃getNavigationBarColor（）中指定的颜色。
public static final int FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS = 0x80000000;

// 表示要求系统壁纸显示在该 Window 后面，Window 表面必须是半透明的，才能真正看到它背后的壁纸
public static final int FLAG_SHOW_WALLPAPER = 0x00100000;
```

### window的其他属性

上面的三个属性是window比较重要也是比较复杂 的三个，除此之外还有几个日常经常使用的属性：

- x与y属性：指定window的位置

- alpha：window的透明度

- gravity：window在屏幕中的位置，使用的是Gravity类的常量

- format：window的像素点格式，值定义在PixelFormat中

### 如何给window属性赋值

window属性的常量值大部分存储在WindowManager.LayoutParams类中，我们可以通过这个类来获得这些常量。当然还有Gravity类和PixelFormat类等。

一般情况下我们会通过以下方式来往屏幕中添加一个window：

```
// 在Activity中调用
WindowManager.LayoutParams windowParams = new WindowManager.LayoutParams();
windParams.flags = WindowManager.LayoutParams.FLAG_FULLSCREEN;
TextView view = new TextView(this);
getWindowManager.addview(view,windowParams);
```

我们可以直接给WindowManager.LayoutParams对象设置属性。

第二种赋值方法是直接给window赋值，如
```
getWindow().flags = WindowManager.LayoutParams.FLAG_FULLSCREEN;
```

除此之外，window的solfInputMode属性比较特殊，他可以直接在AndroidManifest中指定，如下：
```
<activity android:windowSoftInputMode="adjustNothing" />
```

最后总结一下：

- window的重要属性有type、flags、solfInputMode、gravity等

- 我们可以通过不同的方式给window属性赋值

- 没必要去全部记下来，等遇到需求再去寻找对应的常量即可

## window机制的关键类