# 1. View系统

## 1.1 View&ViewGroup

### Q1：简单介绍下android中View和ViewGroup两个类？

View类是UI部件的基础类，占据一块矩形的区域，主要功能包含【绘制】和【事件处理】。

子类主要分为【widget】和【ViewGroup】，前者为与用户交互的UI部件，后者作为layout的父类作为其他View的容器。

View和ViewGroup均位于android.view包。

### Q2：View中定义的属性有哪些？

| 属性名称                     | java方法                            | 属性含义                           | 取值范围                                                     |
| ---------------------------- | ----------------------------------- | ---------------------------------- | ------------------------------------------------------------ |
| android:alpha                | setAlpha(float alpha)               | 设置透明度                         | 0~1之间，0完全投屏，1完全不透明                              |
| android:clickable            | setClickable(boolean)               | 是否可点击                         | true：可点击，可配置点击事件；false：不可点击                |
| android:onClick              |                                     | 设置点击后执行的方法               | 点击后执行的方法名称                                         |
| android:background           | setBackgroundResource(int)          | 设置背景                           | 可以是颜色值、drawable资源等                                 |
| android:id                   | setId(int)                          | 设置id/唯一标识                    | 格式：android:id="@+id/id_test"                              |
| android:padding              | setPaddingRelative(int,int,int,int) | 设置内边距（内容到控件边缘的距离） | 举例：16dp @dimen/dimen16                                    |
| android:paddingLeft          |                                     | 左内边距                           | 略                                                           |
| android:paddingRight         |                                     | 右内边距                           | 略                                                           |
| android:paddingTop           |                                     | 上内边距                           | 略                                                           |
| android:paddingBottom        |                                     | 下内边距                           | 略                                                           |
| android:paddingStart         |                                     | 起始内边距                         | 兼容文字方向                                                 |
| android:paddingEnd           |                                     | 结尾内边距                         | 兼容文字方向                                                 |
| android:focusable            | setFocusable()                      | 是否可以获得焦点                   | 移动光标时，能否聚焦在该组件上<br>TextView设置跑马灯效果时需要本属性设置为true |
| android:focusableInTouchMode |                                     | 是否可以通过触摸获得焦点           |                                                              |
|                              |                                     |                                    |                                                              |
|                              |                                     |                                    |                                                              |
|                              |                                     |                                    |                                                              |
|                              |                                     |                                    |                                                              |
|                              |                                     |                                    |                                                              |
|                              |                                     |                                    |                                                              |
|                              |                                     |                                    |                                                              |

### Q3：ViewGroup有哪些内部类，包含哪些属性？

ViewGroup包含两个重要的内部类，【ViewGroup.LayoutParams】和【ViewGroup.MarginLayoutParams】类

ViewGroup.LayoutParams用于设置布局高度和宽度，其属性有：

| 属性名称              | java方法              | 属性含义     | 取值范围                             |
| --------------------- | --------------------- | ------------ | ------------------------------------ |
| android:layout_height | setAlpha(float alpha) | 设置控件高度 | 具体大小，wrap_content，match_parent |
| android:layout_width  |                       | 设置控件宽度 | 具体大小，wrap_content，match_parent |

ViewGroup.MarginLayoutParams用于设置外边距，其属性有：

| 属性名称                    | java方法                                                     | 属性含义       | 取值范围         |
| --------------------------- | ------------------------------------------------------------ | -------------- | ---------------- |
| android:layout_margin       | setAlpha(float alpha)                                        | 设置外边距     |                  |
| android:layout_marginLeft   | [setMargins](https://developer.android.google.cn/reference/android/view/ViewGroup.MarginLayoutParams#setMargins(int, int, int, int))(int left, int top, int right, int bottom) | 设置左外边距   |                  |
| android:layout_marginRight  |                                                              | 设置右外边距   |                  |
| android:layout_marginTop    |                                                              | 设置顶外边距   |                  |
| android:layout_marginBottom |                                                              | 设置底外边距   |                  |
| android:layout_start        |                                                              | 设置起始外边距 | 可以兼容文字方向 |
| android:layout_end          |                                                              | 设置结束外边距 | 可以兼容文字方向 |
|                             |                                                              |                |                  |

### Q4：margin值可以设置为负数吗

可以设置为负数，当两个组件需要重叠时，例如下面的删除图片和广告图片，是有重叠的

![image-20210516111720677](.\images\image-20210516111720677.png)

## 1.1 布局

### Q1：android的布局（layout）类由哪些？

LinearLayout：线性布局

RelativeLayout：相对布局

TableLayout：表格布局

FrameLayout：帧布局

AbsoluteLayout：绝对布局

GridLayout：网格布局

ConstraintLayout：约束布局

### Q2：LinearLayout 线性布局

LinearLayout继承自android.view.ViewGroup，位于android.widget包，提供线性布局容器能力，可以使子视图在单个方向（水平或垂直）保持对齐。

LinearLayout定义了线性布局的属性，其子类LinearLayout#LayoutParams中定义了线性布局子视图的属性。

LinearLayout中定义的属性有：

| 属性名称            | java方法            | 属性含义         | 取值范围                                                     |
| ------------------- | ------------------- | ---------------- | ------------------------------------------------------------ |
| android:orientation | setOrientation(int) | 设置方向         | vertical horizontal                                          |
| android:gravity     | setGravity(int)     | 设置内部组件位置 | bottom center center_horizontal<br />center_vertical left right start end<br />上述属性可以通过 \| 组合 |
|                     |                     |                  |                                                              |

LinearLayout#LayoutParams中定义的属性有：

| 属性名称               | java方法 | 属性含义                     | 取值范围                                                     |
| ---------------------- | -------- | ---------------------------- | ------------------------------------------------------------ |
| android:layout_gravity |          | 设置当前组件如何被放入父容器 | bottom center center_horizontal<br />center_vertical left right start end<br />上述属性可以通过 \|组合 |
| android:weight         |          | 设置子组件的权重             | int值，默认是0                                               |

### Q3：LinearLayout中weight属性的使用技巧

1. orientation为horizontal，设置layout_width，orientation为vertical，设置layout_height
2. 设置weight时，不要设置match_parent，要么统一设置为wrap_content，要么设置为0
3. 可以通过给某几个子组件配置weight，其余几个不设置weight，不设置的相当于wrap_content，设置了weight的会根据其weight的权重均分剩余空间
4. 可以通过给每一个子组件配置相同的weight，实现均分效果

### Q4：LinearLayout如何实现分割线？

有两种方法可以实现在LinearLayout中的分割线效果：

1. 通过一个高度为1dp的View实现，好处，可以自定义divider的位置，在需要的地方添加

```xml
    <View
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:background="#000000" />
```

1. 通过divider和showDivider属性，结合分割线的drawable文件实现

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:divider="@drawable/divider"
    android:showDividers="middle"
    android:gravity="center"
    android:orientation="vertical">

    <android.widget.Button
        android:layout_margin="8dp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="有按下效果的button"
        android:background="@drawable/button_selector"/>

    <com.colin.demo02.menu.textview.view.MyButton
        android:layout_margin="8dp"
        android:id="@+id/myBtn"
        android:layout_width="match_parent"
        android:layout_height="64dp"
        android:src="@mipmap/ic_tur_icon"
        android:background="@color/bottom_bg"
        android:scaleType="center"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="123" />
</LinearLayout>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" >
    <size android:height="1dp"/>
    <solid android:color="#000000"/>
</shape>
```

### Q5：LinearLayout的缺点

LinearLayout中的weight属性对于屏幕适配很有帮助。但当界面比较复杂的时候，需要嵌套多层LinearLayout，这样就会降低UI Render的效率（渲染速度），而且如果是ListView或者GridView上的item，效率会更低。另外太多层LinearLayout嵌套会占用更多的系统资源，还有可能引发stackoverflow。

开发中要多用LinearLayout+RelativeLayout

### Q6：RelativeLayout 相对布局

android.widget.RelativeLayout继承自android.view.ViewGroup，可实现相对布局。

没有指定子组件位置时，子组件是默认堆积在左上角的

RelativeLayout中的属性：

| 属性名称        | java方法        | 属性含义         | 取值范围                                                     |
| --------------- | --------------- | ---------------- | ------------------------------------------------------------ |
| android:gravity | setGravity(int) | 设置内部组件位置 | bottom center center_horizontal<br />center_vertical left right start end<br />上述属性可以通过 \| 组合 |

RelativeLayout#LayoutParams中的属性，作用于子组件

| 属性名称                         | java方法 | 属性含义                           | 取值范围    |
| -------------------------------- | -------- | ---------------------------------- | ----------- |
| android:layout_centerInParent    |          | 设置当前子组件是否相对于父组件居中 | true\|false |
| android:layout_centerVertical    |          | 相对parent是否垂直居中             | true\|false |
| android:layout_centerHorizontal  |          | 相对parent是否水平居中             | true\|false |
| android:layout_alignParentLeft   |          | 相对parent左对齐                   | true\|false |
| android:layout_alignParentRight  |          | 相对parent右对齐                   | true\|false |
| android:layout_alignParentTop    |          | 相对parent顶对齐                   | true\|false |
| android:layout_alignParentBottom |          | 相对parent底对齐                   | true\|false |
| android:layout_alignParentStart  |          | 相对parent起始对齐                 | true\|false |
| android:layout_alignParentEnd    |          | 相对parent结尾对齐                 | true\|false |
| android:layout_alignLeft         |          | 相对指定id组件左对齐               | id          |
| android:layout_alignRight        |          | 相对指定id组件右对齐               | id          |
| android:layout_alignTop          |          | 相对指定id组件顶对齐               | id          |
| android:layout_alignBottom       |          | 相对指定id组件底对齐               | id          |
| android:layout_alignStart        |          | 相对指定id组件起始对齐             | id          |
| android:layout_alignEnd          |          | 相对指定id组件结尾对齐             | id          |
| android:layout_above             |          | 位于指定id组件上方                 | id          |
| android:layout_below             |          | 位于指定id组件下方                 | id          |
| android:toLeftOf                 |          | 位于指定id组件左方                 | id          |
| android:toRightOf                |          | 位于指定id组件右方                 | id          |
| android:toStartOf                |          | 位于指定id组件起始位置             | id          |
| android:toEndOf                  |          | 位于指定id组件结束位置             | id          |
|                                  |          |                                    |             |

### Q7：RelativeLayout中某些属性冲突，遇到过吗？

RelativeLayout针对子组件，提供了layout_alignParentBottom等相对于父组件的位置属性，如果子组件设置相对RelativeLayout底对齐，而RelativeLayout的layout_height设置了wrap_content，这两个属性在定义上是矛盾的。如果如此设置，RelativeLayout无法确定自己的尺寸，高度会占满屏幕。

其他属性同理。

### Q8：TableLayout 表格布局

android.widget.TableLayout继承自android.view.ViewGroup，用于实现表格布局。实际使用的不多，简单了解下其属性：

| 属性名称                       | java方法 | 属性含义              | 取值范围    |
| ------------------------------ | -------- | --------------------- | ----------- |
| android:layout_collapseColumns |          | 设置被隐藏的列的index | 列的index值 |
| android:layout_shrinkColumns   |          | 设置被收缩的列index   | 列的index值 |
| android:layout_stretchColumns  |          | 设置被拉伸的列的index | 列的index值 |

### Q9：FrameLayout 帧布局

android.widget.FrameLayout继承自android.view.ViewGroup，用于实现帧布局。

帧布局会按照子组件的添加顺序逐个添加，新添加的会覆盖在旧组件上。可以设置前景图片（Foreground）配置一个永远在帧布局最上层的图片。

帧布局的属性有：

| 属性名称                   | java方法 | 属性含义           | 取值范围                     |
| -------------------------- | -------- | ------------------ | ---------------------------- |
| android:layout_foreground  |          | 设置帧布局前景图像 | 前景图像drawable             |
| android:foreground_gravity |          | 前景图像位置       | bottom等，多个可以用 \| 隔开 |
|                            |          |                    |                              |

### Q10：GridLayout 网格布局

android.widget.GridLayout继承自android.view.ViewGroup，用于实现网格布局。

与TableLayout比，GridLayout功能更为强大，可以直接配置几行几列，也可以设置某一个组件占几行几列。

GridLayout的属性有：

| 属性名称            | java方法 | 属性含义                 | 取值范围                 |
| ------------------- | -------- | ------------------------ | ------------------------ |
| android:orientation |          | 设置网格布局水平还是垂直 | 该值决定了行和列是怎么算 |
| android:columnCount |          | 设置网格布局的列数       | 数字                     |
| android:rowCount    |          | 设置网格布局的行数       | 数字                     |

GridLayout#LayoutParams的属性有，作用于其中的子组件

| 属性名称                  | java方法 | 属性含义       | 取值范围 |
| ------------------------- | -------- | -------------- | -------: |
| android:layout_row        |          | 设置组件行数   |     数字 |
| android:layout_column     |          | 设置组件列数   |     数字 |
| android:layout_rowSpan    |          | 子组件横跨几行 |     数字 |
| android:layout_columnSpan |          | 子组件纵跨几行 |     数字 |

### Q11：AbsoluteLayout 绝对布局

android.widget.AbaoluteLayout继承自android.view.ViewGroup，用于实现绝对布局。

绝对布局当前已弃用。

## 1.2 TextView系列

### Q1：TextView系列的类继承关系是怎样的？

![image-20210523202407468](.\images\image-20210523202407468.png)

### Q2：TextView的用法？

TextView可以实现一个带文本的控件，但其能力远不止于此，首先看下其属性：

1. 与文本相关的几个属性：

| 属性名称           | java方法       | 属性含义                 | 取值范围                                                     |
| ------------------ | -------------- | ------------------------ | ------------------------------------------------------------ |
| android:text       | setText()      | 设置文本内容             | 字符串                                                       |
| android:singleLine |                | 设置只显示一行           | @deprecated方法已弃置<br />可通过android:lines=1实现同样效果 |
| android:minLines   | setMinLines()  | 设置最小行               | 正整数                                                       |
| android:maxLines   | setMaxLines()  | 设置最多几行             | 正整数                                                       |
| android:lines      | setHeight(int) | 设置高度为固定行数的高度 | 正整数                                                       |
| android:textColor  | setTextColor() | 设置文本颜色             | 颜色资源id                                                   |
| android:textSize   | setTextSize()  | 设置文本大小             | dimens资源id                                                 |
| android:textStyle  | setTypeface()  | 设置文本风格             | **normal**(无效果)，**bold**(加粗)，**italic**(斜体)         |

2. 与文本阴影相关的几个属性：

| 属性名称             | java方法         | 属性含义                                 | 取值范围            |
| -------------------- | ---------------- | ---------------------------------------- | ------------------- |
| android:shadowColor  | setShadowLayer() | 设置阴影颜色，需要与shadowRadius一起使用 | 颜色值              |
| android:shadowRadius | setShadowLayer() | 设置阴影模糊程度                         | 浮点数，建议设置3.0 |
| android:shadowDx     | setShadowLayer() | 设置阴影在水平方向的偏移                 | 浮点数              |
| android:shadowDy     | setShadowLayer() | 设置阴影在垂直方向的偏移                 | 浮点数              |

代码&效果示意

![image-20210523210236903](.\images\image-20210523210236903.png)

3. 实现图片+文本的效果

| 属性名称               | java方法 | 属性含义             | 取值范围     |
| ---------------------- | -------- | -------------------- | ------------ |
| android:drawableTop    |          | 设置文本上方图片     | drawable资源 |
| android:drawableBottom |          | 设置文本下方图片     | drawable资源 |
| android:drawableStart  |          | 设置文本起始方向图片 | drawable资源 |
| android:drawableEnd    |          | 设置文本结束方向图片 | drawable资源 |
| android:drawableLeft   |          | 设置文本左方图片     | drawable资源 |
| android:drawableRight  |          | 设置文本右方图片     | drawable资源 |
| android:padding        |          | 设置图片与文本的边距 | dimens值     |

代码&效果示意

![image-20210523211245535](.\images\image-20210523211245535.png)

4. 通过autoLink属性实现超链接等跳转效果

| 属性名称         | java方法             | 属性含义     | 取值范围                                                     |
| ---------------- | -------------------- | ------------ | ------------------------------------------------------------ |
| android:autoLink | setAutoLinkMask(int) | 设置自动跳转 | **web**：url网页<br>**email**：电子邮件<br>**phone**拨号<br> |

5. 通过spannableString&&spannableStringBuilder实现更丰富的定制效果
6. 设置跑马灯效果

| 属性名称                   | java方法 | 属性含义                   | 取值范围                                                     |
| -------------------------- | -------- | -------------------------- | ------------------------------------------------------------ |
| android:ellipsize          |          | 设置后，超出部分不会被打断 | **marquee**跑马灯<br>**middle**省略号在中间<br>**start**省略号在开头<br>**end**省略号在结尾<br> |
| android:marqueeRepeatLimit |          | 设置跑马灯重复次数         | 正整数<br>**marquee_forever**表示永远重复                    |

另外，跑马灯效果只配置上述xml是不够的，还需要在代码中设置setSelected(true)才可生效

xml和效果示意如下：

![image-20210523214650049](.\images\image-20210523214650049.png)

![image-20210523214626622](.\images\image-20210523214626622.png)

7. 通过hint属性设置默认提示文本

| 属性名称              | java方法           | 属性含义             | 取值范围   |
| --------------------- | ------------------ | -------------------- | ---------- |
| android:hint          | setHint()          | 设置默认提示文本     |            |
| android:textColorHint | setHintTextColor() | 设置默认提示文本颜色 | 颜色资源id |

xml和效果图如下：

![image-20210526211900517](.\images\image-20210526211900517.png)

8. 调整字宽度

| 属性名称           | java方法        | 属性含义   | 取值范围 |
| ------------------ | --------------- | ---------- | -------- |
| android:textScaleX | setTextScaleX() | 设置字宽度 | float    |

![image-20210526214230408](.\images\image-20210526214230408.png)

### Q3：EditText的用法？

EditText可以实现一个可供编辑的文本框，继承自TextView，也拥有TextView的相关属性。通过几个例子介绍其用法及相关属性

1. 设置获取焦点后选中

| 属性名称                     | java方法              | 属性含义           | 取值范围 |
| ---------------------------- | --------------------- | ------------------ | -------- |
| android:**selectAllOnFocus** | setSelectAllOnFocus() | 设置获取焦点后选中 | boolean  |

**注意**：selectAllOnFocus这个属性是定义在TextView中的，但是TextView选中后无效果，所以一般是EditText在用

![image-20210526212356711](.\images\image-20210526212356711.png)

2. 限制输入类型

| 属性名称              | java方法          | 属性含义             | 取值范围 |
| --------------------- | ----------------- | -------------------- | -------- |
| android:**inputType** | setRawInputType() | 设置可输入的文本类型 | String   |

inputType与selectAllOnFocus属性一样，也是定义在TextView，但一般在EditText使用的属性。

inputType可以设置的类型有

| inputTYPE值 | 含义     |      |
| ----------- | -------- | ---- |
| phone       | 拨号键盘 |      |
| date        | 日期     |      |
|             |          |      |

具体效果可以在demo2工程中查看

3. 实现带删除图标的EditText

可通过自定义一个EditText实现，配置一个Text变化的回调，当Text有内容时，调用TextView#setCompoundDrawables方法，设置drawableRight为删除图标，并为该删除图标设置点击事件。

核心代码如下：

```java
package com.colin.demo02.menu.textview;

import android.annotation.SuppressLint;
import android.content.Context;
import android.graphics.Rect;
import android.graphics.drawable.Drawable;
import android.text.Editable;
import android.text.TextWatcher;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.widget.EditText;

import com.colin.demo02.R;

@SuppressLint("AppCompatCustomView")
public class EditTextWithDel extends EditText {

    private final static String TAG = "EditTextWithDel";
    private Drawable imgInable;
    private Context mContext;

    public EditTextWithDel(Context context) {
        super(context);
        mContext = context;
        init();
    }

    public EditTextWithDel(Context context, AttributeSet attrs) {
        super(context, attrs);
        mContext = context;
        init();
    }

    public EditTextWithDel(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        mContext = context;
        init();
    }

    private void init() {
        imgInable = mContext.getResources().getDrawable(R.drawable.ic_delete);
        addTextChangedListener(new TextWatcher() {
            @Override
            public void onTextChanged(CharSequence s, int start, int before, int count) {
            }

            @Override
            public void beforeTextChanged(CharSequence s, int start, int count, int after) {
            }

            @Override
            public void afterTextChanged(Editable s) {
                setDrawable();
            }
        });
        setDrawable();
    }

    // 设置删除图片
    private void setDrawable() {
        if (length() < 1)
            setCompoundDrawablesWithIntrinsicBounds(null, null, null, null);
        else
            setCompoundDrawablesWithIntrinsicBounds(null, null, imgInable, null);
    }

    // 处理删除事件
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (imgInable != null && event.getAction() == MotionEvent.ACTION_UP) {
            int eventX = (int) event.getRawX();
            int eventY = (int) event.getRawY();
            Log.e(TAG, "eventX = " + eventX + "; eventY = " + eventY);
            Rect rect = new Rect();
            getGlobalVisibleRect(rect);
            rect.left = rect.right - 100;
            if (rect.contains(eventX, eventY))
                setText("");
        }
        return super.onTouchEvent(event);
    }
    @Override
    protected void finalize() throws Throwable {
        super.finalize();
    }
}

```



### Q4：Button的用法？

Button可以用于实现一个按钮，在说Button之前，首先说两种特殊的drawable资源：**StateListDrawable**和**ShapeDrawable**

StateListDrawable是一种特殊的Drawable资源，是一个根节点是<selector>的xml文件，该文件可以通过配置不同属性的<item>，来实现不同场景下的不同配置。

ShapeDrawable可以实现圆角等效果的另一种特殊的Drawable资源

两种类型的Drawable会在Drawable章节详细介绍，这里演示下与Button结合的效果。

1. 设置按钮按下效果，获得焦点效果：

step 1: 设置Button的xml，background属性指定StateListDrawable资源

```xml
    <android.widget.Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="有按下效果的button"
        android:background="@drawable/button_selector"/>
```

step 2:定义指定的StateListDrawable资源文件及相关的ShapeDrawable资源文件

```xml
button_selector.xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true" android:drawable="@drawable/button_pressed" />
    <item android:drawable="@drawable/button_default" />
</selector>
```

```xml
button_pressed.xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="@color/checked" />
    <corners android:radius="4dp" />
</shape>
```

```xml
button_default.xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="@color/unchecked"/>
    <corners android:radius="4dp"/>
</shape>
```

------

*注意：如果<application>标签中配置的全局默认theme是Theme.MaterialComponents.DayNight.DarkActionBar，此时默认的<Button>实际上是<MaterialButton>，会默认使用上面theme中的属性，并忽略background属性。*

*如需上面的background生效，且不改变全局的theme，可以通过把标签显示指定为android.widget.Button实现*

### Q5：CompoundButton的用法？

Compound是“复合的、混合的”意思，继承自Button，子类有RadioButton，CheckedBox等，可以实现更复杂的Button组。其常用属性有：

| 属性名称           | java方法          | 属性含义                       | 取值范围                          |
| ------------------ | ----------------- | ------------------------------ | --------------------------------- |
| android:**button** | setButtonDrawable | 设置button关联的drawable       | 举例来讲，RadioButton左边的小圆圈 |
| android:buttonTint | setButtonTintList | 设置button关联drawable的颜色   |                                   |
| android:checked    |                   | 设置本button在所在组是否被选中 | true\|false                       |

CompoundButton的子类都可以通过调用如下接口，监听选中状态：

```java
setOnCheckedChangedListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                
            }
        })
```

### Q5：RadioButton的用法？

RadioButton实现单选按钮（每次只能选中一个），通过RadioGroup管理RadioButton，前者继承关系如下：

```
android.view.ViewGroup
    android.widget.LinearLayout
        android.widget.RadioGroup
```

RadioGroup的常用属性：

| 属性名称                  | java方法 | 属性含义                         | 取值范围 |
| ------------------------- | -------- | -------------------------------- | -------- |
| android:**checkedButton** |          | 设置本button组下，被选中的Button |          |

1. RadioButton的简单实现

```xml
    <RadioGroup
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="男"/>
        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="女"/>
    </RadioGroup>
```

```java
public class CompoundButtonPager extends BasePager {
    private RadioGroup mRadioGroup;

    public CompoundButtonPager(Context context) {
        super(context);
    }

    @Override
    protected View initView() {
        View view = View.inflate(mContext, R.layout.compound_button, null);
        mRadioGroup = view.findViewById(R.id.radio_group_demo);
        RadioGroup.OnCheckedChangeListener onCheckedChangeListener = new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {
                switch (checkedId) {
                    case R.id.men:
                        break;
                    case R.id.women:
                        break;
                }
            }
        };
        mRadioGroup.setOnCheckedChangeListener(onCheckedChangeListener);
        return view;
    }
}
```

![image-20210530105800467](.\images\image-20210530105800467.png)

2. 使用RadioButton实现底部导航效果

代码如下，注意的几个点：

​			a. 通过配置android:button = "@null"，删除选中的圆圈

​			b. 通过配置SelectListDrawable，配置RadioButton的选中效果

​			c. 点击不同的RadioButton后，保存RadioButton的position，然后根据位置，去保存有View的BasePager的List中，取出相应的View，加载到Fragment中。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <FrameLayout
        android:id="@+id/fl_text_view"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1" />

    <RadioGroup
        android:id="@+id/text_view_radio_group"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#22000000"
        android:gravity="center_vertical"
        android:orientation="horizontal"
        android:padding="4dp">

        <RadioButton
            style="@style/bottom_tab_style"
            android:id="@+id/text_view"
            android:drawableTop="@drawable/drawable_selector_01"
            android:text="TextView" />

        <RadioButton
            style="@style/bottom_tab_style"
            android:id="@+id/edit_text"
            android:drawableTop="@drawable/drawable_selector_02"
            android:text="EditText" />

        <RadioButton
            style="@style/bottom_tab_style"
            android:id="@+id/button"
            android:drawableTop="@drawable/drawable_selector_01"
            android:text="Button" />

        <RadioButton
            style="@style/bottom_tab_style"
            android:id="@+id/image_button"
            android:drawableTop="@drawable/drawable_selector_02"
            android:text="ImageButton" />
    </RadioGroup>
</LinearLayout>
```

```java
public class TextViewActivity extends AppCompatActivity {
    private List<BasePager> mPagers = new ArrayList<>();

    private RadioGroup mRadioGroup;

    private int mPosition;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.text_view_guide);
        initView();
    }

    private void setFragment() {
        FragmentManager fragmentManager = getSupportFragmentManager();
        FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
        fragmentTransaction.replace(R.id.fl_text_view, new BaseFragment(mPagers.get(mPosition)));
        fragmentTransaction.commit();
    }

    private void initView() {
        mPagers.add(new TextViewPager(this));
        mPagers.add(new EditTextPager(this));
        mPagers.add(new ButtonPager(this));
        mPagers.add(new CompoundButtonPager(this));
        mRadioGroup = findViewById(R.id.text_view_radio_group);
        mRadioGroup.setOnCheckedChangeListener((group, checkedId) -> {
            switch (checkedId) {
                case R.id.text_view:
                    mPosition = 0;
                    break;
                case R.id.edit_text:
                    mPosition = 1;
                    break;
                case R.id.button:
                    mPosition = 2;
                    break;
                case R.id.image_button:
                    mPosition = 3;
                    break;
                default:
                    break;
            }
            setFragment();
        });
        mRadioGroup.check(R.id.text_view);
    }
}
```

```java
public abstract class BasePager {
    public Context mContext;
    private View mRootView;
    private boolean mHasInit;

    public BasePager(Context context) {
        mContext = context;
        mRootView = initView();
    }

    protected abstract View initView();

    public void initData() {
    }

    public View getRootView() {
        return mRootView;
    }

    public boolean hasInit() {
        return mHasInit;
    }

    public void setHasInit(boolean hasInit) {
        this.mHasInit = hasInit;
    }
}

```

实现效果如下：

![image-20210530110233237](.\images\image-20210530110233237.png)

### Q6：CheckedBox的用法？

CheckedBox可实现复选框效果，简单实现代码如下：

```xml
    <CheckBox
        android:id="@+id/eat"
        android:text="吃饭"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <CheckBox
        android:id="@+id/sleep"
        android:text="睡觉"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

```
public class CompoundButtonPager extends BasePager implements CompoundButton.OnCheckedChangeListener {
    private static final String TAG = "CompoundButtonPager";

    private CheckBox mEat;

    private CheckBox mSleep;

    public CompoundButtonPager(Context context) {
        super(context);
    }

    @Override
    protected View initView() {
        View view = View.inflate(mContext, R.layout.compound_button, null);
        mEat = view.findViewById(R.id.eat);
        mSleep = view.findViewById(R.id.sleep);
        mEat.setOnCheckedChangeListener(this);
        mSleep.setOnCheckedChangeListener(this);
        return view;
    }

    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        Log.e(TAG, "buttomView id: " + buttonView.getId());
    }
}

```

![image-20210530170741021](.\images\image-20210530170741021.png)

### Q7：RadioButton,CheckBox结合ListView&RecyclerView的用法？

这部分会在ListView&RecyclerView中详细描述。

### Q8：ToggleButton的用法？

ToggleButton可实现开关按钮的效果，基本用法如下：

| 属性名称                  | java方法 | 属性含义               | 取值范围 |
| ------------------------- | -------- | ---------------------- | -------- |
| android:**disabledAlpha** |          | 设置按钮禁用时的透明度 |          |
| android:**textOff**       |          | 设置关闭时的文本       |          |
| android:**textOn**        |          | 设置打开时的文本       |          |

```xml
    <ToggleButton
        android:id="@+id/toggle_button"
        android:textOn="开启"
        android:textOff="关闭"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

```java
        mToggleButton.setOnCheckedChangeListener((toggleView, isChecked) -> {
            if (isChecked) {
            } else {
            }
        });
```

![image-20210530172616940](.\images\image-20210530172616940.png)

### Q9：Switch的用法？

Switch相对ToggleButton可以实现更炫酷的开关效果，其属性有：

| 属性名称            | java方法         | 属性含义                           | 取值范围                    |
| ------------------- | ---------------- | ---------------------------------- | --------------------------- |
| android:**thumb**   | setThumbResource | 设置滑块的图片，可通过selector实现 |                             |
| android:**track**   | setTrackResource | 设置底部的图片，可通过selector实现 |                             |
| android:**textOn**  |                  | 开关开启后的文本                   | 需设置showText=true才可显示 |
| android:**textOff** |                  | 开关关闭后的文本                   | 需设置showText=true才可显示 |
|                     |                  |                                    |                             |

```xml
    <Switch
        android:padding="8dp"
        android:textOn="Switch开启"
        android:textOff="Switch关闭"
        android:showText="true"
        android:thumb="@drawable/ic_pig"
        android:track="@drawable/track_selector"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

```xml
track_selector.xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_checked="true" android:drawable="@drawable/green" />
    <item android:state_checked="false" android:drawable="@drawable/gray" />
</selector>
```

![image-20210530175114684](.\images\image-20210530175114684.png)

## 1.3 ImageView系列

### Q1：ImageView系列的类继承关系是怎样的？

![image-20210530175655331](.\images\image-20210530175655331.png)

## 1.4 ProgressBar系列

### Q1：ProgressBar系列的类继承关系是怎样的？

![image-20210530175958105](.\images\image-20210530175958105.png)

# 2. MVP

## Q1：请介绍下android中的MVP设计模式

MVP是MVC的一种衍生，MVP中不允许View直接访问Model，这是MVP与MVC最大的不同之处。

优点：降低耦合，模块化，更易于维护，更方便进行单元测试。

MVP中通常包含四个要素：

 1. View：负责绘制UI元素，与用户进行交互（Activity或Fragment）

 2. View Interface：View需要实现的接口，View通过View Interface与Presenter进行交互，降低耦合，方便进行单元测试

 3. Model：负责业务Bean的操作

 4. Presenter：作为View与Model交互的纽带，承载大部分的复杂逻辑。

    

