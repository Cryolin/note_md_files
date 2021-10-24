# 1. AIDL

## 1.1 AIDL的简单示例

> 核心步骤：
>
> 1、客户端和服务端创建同样的aidl文件，注意aidl文件的包名和内容要完全一致
>
> 2、创建完aidl之后build下，会在build目录下看到自动生成的.java文件
>
> 3、 服务端创建一个暴露Service，onBind()方法返回Stub的实现
>
> 4、 客户端bindService，onServiceConnected接受到的IBinder对象，作为Stub.asInterface的传参，获取到远程代理

客户端：

![image-20210904152047155](D:/git/note_md_files/images/image-20210904152047155.png)

服务端：

![image-20210904145342943](D:/git/note_md_files/images/image-20210904145342943.png)

AIDL文件：

```aidl
package com.colin.aidl;

interface IRemoteInterface {
    String getName();
}
```

服务端的MyService：

```java
package com.colin.server;

import ...;

import com.colin.aidl.IRemoteInterface;

public class MyService extends Service {
    public MyService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
        return new IRemoteInterface.Stub() {
            @Override
            public String getName() throws RemoteException {
                return "你好，我是服务端";
            }
        };
    }
}
```

客户端MainActivity：

```java
package com.colin.client;

import ...;

import com.colin.aidl.IRemoteInterface;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private static final String TAG = "MainActivity";
    private IRemoteInterface mAidlInterface;

    private TextView mTextView;

    private Button mBind;

    private Button mUnbind;

    private Button mGetName;

    private boolean mIsBound;

    private ServiceConnection mServiceConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            Toast.makeText(getApplicationContext(), "bindService 成功", Toast.LENGTH_SHORT).show();
            mIsBound = true;
            mAidlInterface = IRemoteInterface.Stub.asInterface(service);
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            mIsBound = false;
            Toast.makeText(getApplicationContext(), "bindService 异常失败", Toast.LENGTH_SHORT).show();
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initView();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (mIsBound) {
            unbindService(mServiceConnection);
        }
    }

    private void initView() {
        mTextView = findViewById(R.id.textView);
        mBind = findViewById(R.id.bind);
        mBind.setOnClickListener(this);
        mUnbind = findViewById(R.id.unbind);
        mUnbind.setOnClickListener(this);
        mGetName = findViewById(R.id.getName);
        mGetName.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.bind:
                Intent intent = new Intent();
                intent.setClassName("com.colin.server", "com.colin.server.MyService");
                bindService(intent, mServiceConnection, BIND_AUTO_CREATE);
                break;
            case R.id.unbind:
                if (mIsBound) {
                    unbindService(mServiceConnection);
                    mIsBound = false;
                    mAidlInterface = null;
                }
                Toast.makeText(getApplicationContext(), "unbindService ", Toast.LENGTH_LONG).show();
                break;
            case R.id.getName:
                if (mIsBound && mAidlInterface != null) {
                    try {
                        Toast.makeText(getApplicationContext(), "调用服务端的getName()返回： " + mAidlInterface.getName(), Toast.LENGTH_LONG).show();
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                }
                break;
            default:
                break;
        }
    }
}
```

> 执行结果：

![image-20210904152256766](D:/git/note_md_files/images/image-20210904152256766.png)

![image-20210904152314440](D:/git/note_md_files/images/image-20210904152314440.png)

## 1.2 AIDL传递数据

> AIDL支持的数据类型有：
>
> 1、 基本数据类型（int、long、char、boolean、double等）；
>
> 2、 String和CharSequence
>
> 3、 List（只支持ArrayList，而且其中每个元素也必须是AIDL支持的类型）
>
> 4、 Map（只支持HashMap，而且其中每个元素也必须是AIDL支持的类型）
>
> 5、Parcelable：所有实现了Parcelable接口的对象
>
> 6、AIDL：所有AIDL接口自身也可以在AIDL中使用

这里仅演示下第5、第6条

在3.1节的基础上，创建两个新的aidl文件：

![image-20210904173306632](D:/git/note_md_files/images/image-20210904173306632.png)

> Book.java

```java
package com.colin.aidl;

import android.os.Parcel;
import android.os.Parcelable;

public class Book implements Parcelable{
    public String mBookName;
    public int mId;

    public Book(String bookName, int id) {
        mBookName = bookName;
        mId = id;
    }

    /**
     * 构造方法，将Parcel对象转化为Book对象
     *
     * @param in
     */
    protected Book(Parcel in) {
        mBookName = in.readString();
        mId = in.readInt();
    }

    // 把当前Book对象写入Parcel对象中
    // 注意读和写的顺序要一致
    @Override
    public void writeToParcel(Parcel parcel, int i) {
        parcel.writeString(mBookName);
        parcel.writeInt(mId);
    }

    /**
     * 实现Parcelable接口必须创建CREATOR
     * 会回调其中的方法进行对象创建
     */
    public static final Creator<Book> CREATOR = new Creator<Book>() {
        @Override
        public Book createFromParcel(Parcel in) {
            return new Book(in);
        }

        @Override
        public Book[] newArray(int size) {
            return new Book[size];
        }
    };

    // 这个方法不用管
    @Override
    public int describeContents() {
        return 0;
    }
}
```

> Book.aidl

```aidl
package com.colin.aidl;

parcelable Book;
```

> 服务端MyService.java

```java
package com.colin.server;

import ...;

public class MyService extends Service {
    private static final String TAG = "MyService";
    private Map<Integer, Book> mBookMap = new HashMap<>();

    public MyService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
        return new IRemoteInterface.Stub() {
            @Override
            public String getName() throws RemoteException {
                return "你好，我是服务端";
            }

            @Override
            public Book queryBook(int id) throws RemoteException {
                if (id < 0 || id > 10) {
                    Log.e(TAG, "invalid id");
                    return null;
                }
                return mBookMap.containsKey(id) ? mBookMap.get(id) : null;
            }

            @Override
            public boolean addBook(Book book) throws RemoteException {
                if (book == null || book.mId < 0 || book.mId > 10 || TextUtils.isEmpty(book.mBookName)) {
                    Log.e(TAG, "invalid book, add fail");
                    return false;
                }
                if (mBookMap.containsKey(book.mId)) {
                    Log.e(TAG, "already contained specified id, pls reedit id and try again");
                    return false;
                }
                mBookMap.put(book.mId, book);
                return true;
            }
        };
    }
}
```

> 客户端MainActivity.java

```java
package com.colin.client;

import ...;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private static final String TAG = "MainActivity";
    private IRemoteInterface mAidlInterface;

    private TextView mTextView;

    private EditText mAddBookIdEditText;

    private EditText mAddBookNameEditText;

    private EditText mQueryBookEditText;

    private Button mBind;

    private Button mUnbind;

    private Button mGetName;

    private Button mAddBook;

    private Button mQueryBook;

    private boolean mIsBound;

    private ServiceConnection mServiceConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            Toast.makeText(getApplicationContext(), "bindService 成功", Toast.LENGTH_SHORT).show();
            mIsBound = true;
            mAidlInterface = IRemoteInterface.Stub.asInterface(service);
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            mIsBound = false;
            Toast.makeText(getApplicationContext(), "bindService 异常失败", Toast.LENGTH_SHORT).show();
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initView();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (mIsBound) {
            unbindService(mServiceConnection);
        }
    }

    private void initView() {
        mTextView = findViewById(R.id.textView);
        mBind = findViewById(R.id.bind);
        mBind.setOnClickListener(this);
        mUnbind = findViewById(R.id.unbind);
        mUnbind.setOnClickListener(this);
        mGetName = findViewById(R.id.getName);
        mGetName.setOnClickListener(this);
        mAddBookIdEditText = findViewById(R.id.addBookIdEditText);
        mAddBookNameEditText = findViewById(R.id.addBookNameEditText);
        mAddBook = findViewById(R.id.addBook);
        mAddBook.setOnClickListener(this);
        mQueryBookEditText = findViewById(R.id.querybookEditText);
        mQueryBook = findViewById(R.id.querybook);
        mQueryBook.setOnClickListener(this);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        // 点击空白处EditText失去焦点
        InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
        imm.hideSoftInputFromWindow(mAddBookIdEditText.getWindowToken(), 0);
        imm.hideSoftInputFromWindow(mAddBookNameEditText.getWindowToken(), 0);
        imm.hideSoftInputFromWindow(mQueryBookEditText.getWindowToken(), 0);
        return super.onTouchEvent(event);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.bind:
                Intent intent = new Intent();
                intent.setClassName("com.colin.server", "com.colin.server.MyService");
                bindService(intent, mServiceConnection, BIND_AUTO_CREATE);
                break;
            case R.id.unbind:
                if (mIsBound) {
                    unbindService(mServiceConnection);
                    mIsBound = false;
                    mAidlInterface = null;
                }
                Toast.makeText(getApplicationContext(), "unbindService ", Toast.LENGTH_SHORT).show();
                break;
            case R.id.getName:
                getNameInner();
                break;
            case R.id.addBook:
                addBookInner();
                break;
            case R.id.querybook:
                queryBookInner();
                break;
            default:
                break;
        }
    }

    private void getNameInner() {
        if (!mIsBound || mAidlInterface == null) {
            Toast.makeText(getApplicationContext(), "请先bindService", Toast.LENGTH_SHORT).show();
            return;
        }
        try {
            Toast.makeText(getApplicationContext(), "调用服务端的getName()返回： " + mAidlInterface.getName(), Toast.LENGTH_SHORT).show();
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }

    private void addBookInner() {
        if (!mIsBound || mAidlInterface == null) {
            Toast.makeText(getApplicationContext(), "请先bindService", Toast.LENGTH_SHORT).show();
            return;
        }
        String bookIdStr = mAddBookIdEditText.getText().toString();
        int bookId;
        try {
            bookId = Integer.valueOf(bookIdStr);
        } catch (NumberFormatException e) {
            bookId = 0;
        }
        String bookName = mAddBookNameEditText.getText().toString();
        try {
            boolean result = mAidlInterface.addBook(new Book(bookName, bookId));
            if (result) {
                Toast.makeText(getApplicationContext(), "addBook成功！！！", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(getApplicationContext(), "addBook失败！！！", Toast.LENGTH_SHORT).show();
            }
        } catch (RemoteException e) {
            Toast.makeText(getApplicationContext(), "addBook失败！！！", Toast.LENGTH_SHORT).show();
            e.printStackTrace();
        }
    }

    private void queryBookInner() {
        if (!mIsBound || mAidlInterface == null) {
            Toast.makeText(getApplicationContext(), "请先bindService", Toast.LENGTH_SHORT).show();
            return;
        }
        String bookIdStr = mQueryBookEditText.getText().toString();
        int bookId;
        try {
            bookId = Integer.valueOf(bookIdStr);
        } catch (NumberFormatException e) {
            bookId = 0;
        }
        try {
            Book book = mAidlInterface.queryBook(bookId);
            if (book == null) {
                Toast.makeText(getApplicationContext(), "queryBook失败！！！", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(getApplicationContext(), "查询到id 为 " + bookId + " 的图书，书名为： " + book.mBookName, Toast.LENGTH_SHORT).show();
            }
        } catch (RemoteException e) {
            Toast.makeText(getApplicationContext(), "queryBook失败！！！", Toast.LENGTH_SHORT).show();
            e.printStackTrace();
        }
        return;
    }
}
```

> 执行结果：addbook

![image-20210904174018215](D:/git/note_md_files/images/image-20210904174018215.png)

>执行结果：querybook

![image-20210904174112245](D:/git/note_md_files/images/image-20210904174112245.png)

## 1.3 抽取AIDL到SDK

实际开发中，服务端可以把AIDL封装成sdk提供给客户端，有着更好的容错。

> 创建aidl的library工程，目录结构如下

![image-20210904222905948](D:/git/note_md_files/images/image-20210904222905948.png)

> 查看生成的aar，可以看到里面没有aidl文件，而是aidl生成的java文件相关的字节码文件

![image-20210904223132239](D:/git/note_md_files/images/image-20210904223132239.png)

> client端和server端将该aar依赖进来，无需添加aidl文件（略）

## 1.4 Link-To-Death

https://www.race604.com/communicate-with-remote-service-2/

## 1.5 AIDL的回调，使用RemoteCallbackList

通过回调实现服务端主动通知客户端

https://www.race604.com/communicate-with-remote-service-3/

1.4节和1.5节可参考demo05

## 1.6 AIDL做为业务方法的传参和返回值

这里模拟android的MediaPlayer的底层实现，写一个demo，核心aidl代码如下：

```aidl
// IMediaPlayerService.aidl
interface IMediaPlayerService {
    // aidl作为传参实现回调，aidl作为返回值
    IMediaPlayer createMediaPlayer(IMediaPlayerClient client);
}
```

IMediaPlayerClient是客户端注册的回调：

```aidl
// IMediaPlayerClient.aidl
interface IMediaPlayerClient {
    void onNotify(String notifyData);
}
```

IMediaPlayer是客户端拿到的MediaPlayer，用于控制播放暂停：

```aidl
// IMediaPlayer.aidl
interface IMediaPlayer {
    void play();

    void pause();
}
```

整体流程如下：

1. 客户端bindService获取IMediaPlayerService

2. 客户端通过IMediaPlayerService.createMediaPlayer，传入回调，同时服务端返回客户端一个IMediaPlayer
3. 客户端通过调用IMediaPlayer的play()，pause()方法实现播放和暂停
4. 服务端收到play()和pause()命令后，通过客户端的IMediaPlayerClient回调通知客户端

代码就不贴了，详细可参考demo06

> 注意，在这个例子中。客户端注册的回调是定义在客户端，也执行在客户端的。所以客户端实现了IMediaPlayerClient.Stub，相当于在回调的场景下，客户端和服务端角色做了切换

## 1.7 Binder.clearCallingIdentity

// TODO



# 2. Java层的Binder

## 2.1 AIDL生成的Java文件解析

写一个最简单的aidl：

```aidl
package com.colin.client;

interface IRemoteInterface {
    void getName();
}
```

其生成的java文件如下：

```java
/*
 * This file is auto-generated.  DO NOT MODIFY.
 */
package com.colin.client;
public interface IRemoteInterface extends android.os.IInterface
{
  /** Default implementation for IRemoteInterface. */
  public static class Default implements com.colin.client.IRemoteInterface
  {
    @Override public void getName() throws android.os.RemoteException
    {
    }
    @Override
    public android.os.IBinder asBinder() {
      return null;
    }
  }
  /** Local-side IPC implementation stub class. */
  public static abstract class Stub extends android.os.Binder implements com.colin.client.IRemoteInterface
  {
    private static final java.lang.String DESCRIPTOR = "com.colin.client.IRemoteInterface";
    /** Construct the stub at attach it to the interface. */
    public Stub()
    {
      this.attachInterface(this, DESCRIPTOR);
    }
    /**
     * Cast an IBinder object into an com.colin.client.IRemoteInterface interface,
     * generating a proxy if needed.
     */
    public static com.colin.client.IRemoteInterface asInterface(android.os.IBinder obj)
    {
      if ((obj==null)) {
        return null;
      }
      android.os.IInterface iin = obj.queryLocalInterface(DESCRIPTOR);
      if (((iin!=null)&&(iin instanceof com.colin.client.IRemoteInterface))) {
        return ((com.colin.client.IRemoteInterface)iin);
      }
      return new com.colin.client.IRemoteInterface.Stub.Proxy(obj);
    }
    @Override public android.os.IBinder asBinder()
    {
      return this;
    }
    @Override public boolean onTransact(int code, android.os.Parcel data, android.os.Parcel reply, int flags) throws android.os.RemoteException
    {
      java.lang.String descriptor = DESCRIPTOR;
      switch (code)
      {
        case INTERFACE_TRANSACTION:
        {
          reply.writeString(descriptor);
          return true;
        }
        case TRANSACTION_getName:
        {
          data.enforceInterface(descriptor);
          this.getName();
          reply.writeNoException();
          return true;
        }
        default:
        {
          return super.onTransact(code, data, reply, flags);
        }
      }
    }
    private static class Proxy implements com.colin.client.IRemoteInterface
    {
      private android.os.IBinder mRemote;
      Proxy(android.os.IBinder remote)
      {
        mRemote = remote;
      }
      @Override public android.os.IBinder asBinder()
      {
        return mRemote;
      }
      public java.lang.String getInterfaceDescriptor()
      {
        return DESCRIPTOR;
      }
      @Override public void getName() throws android.os.RemoteException
      {
        android.os.Parcel _data = android.os.Parcel.obtain();
        android.os.Parcel _reply = android.os.Parcel.obtain();
        try {
          _data.writeInterfaceToken(DESCRIPTOR);
          boolean _status = mRemote.transact(Stub.TRANSACTION_getName, _data, _reply, 0);
          if (!_status && getDefaultImpl() != null) {
            getDefaultImpl().getName();
            return;
          }
          _reply.readException();
        }
        finally {
          _reply.recycle();
          _data.recycle();
        }
      }
      public static com.colin.client.IRemoteInterface sDefaultImpl;
    }
    static final int TRANSACTION_getName = (android.os.IBinder.FIRST_CALL_TRANSACTION + 0);
    public static boolean setDefaultImpl(com.colin.client.IRemoteInterface impl) {
      if (Stub.Proxy.sDefaultImpl == null && impl != null) {
        Stub.Proxy.sDefaultImpl = impl;
        return true;
      }
      return false;
    }
    public static com.colin.client.IRemoteInterface getDefaultImpl() {
      return Stub.Proxy.sDefaultImpl;
    }
  }
  public void getName() throws android.os.RemoteException;
}
```

## 2.2 动手写aidl生成的java文件，取代aidl

可以手动写aidl生成的java文件，可以精简部分代码。

> step 1：抽取IInterface接口，客户端和服务端都要有该接口。可以把一些常量放到接口中

```java
package com.colin.aidl;

import android.os.IInterface;
import android.os.RemoteException;

public interface IRemoteInterface extends IInterface {
    String DESCRIPTOR = "com.colin.aidl.IRemoteInterface";

    int TRANSACTION_getName = android.os.IBinder.FIRST_CALL_TRANSACTION + 0;

    String getName() throws RemoteException;
}
```

> step 2：实现服务端，实现如下三个方法即可

```java
package com.colin.server;

import ...

public class MyRemoteStub extends Binder implements IRemoteInterface {
    @Override
    public String getName() throws RemoteException {
        return "This is server";
    }

    @Override
    public IBinder asBinder() {
        return this;
    }

    @Override
    protected boolean onTransact(int code, @NonNull Parcel data, @Nullable Parcel reply, int flags) throws RemoteException {
        switch (code)
        {
            case INTERFACE_TRANSACTION:
            {
                reply.writeString(DESCRIPTOR);
                return true;
            }
            case TRANSACTION_getName:
            {
                data.enforceInterface(DESCRIPTOR);
                java.lang.String _result = this.getName();
                reply.writeNoException();
                reply.writeString(_result);
                return true;
            }
            default:
            {
                return super.onTransact(code, data, reply, flags);
            }
        }
    }
}
```

> 实现客户端，同样也是实现三个方法

```java
package com.colin.client;

import ...

public class MyRemoteProxy implements IRemoteInterface {
    // 客户端需要持有IBinder（实际上是BinderProxy）
    private IBinder mRemote;

    public MyRemoteProxy(IBinder iBinder) {
        mRemote = iBinder;
    }

    // 客户端通过mRemote.transact() 调用服务端
    @Override
    public String getName() throws RemoteException {
        Parcel _data = Parcel.obtain();
        Parcel _reply = Parcel.obtain();
        String _result;
        try {
            _data.writeInterfaceToken(DESCRIPTOR);
            mRemote.transact(TRANSACTION_getName, _data, _reply, 0);
            _reply.readException();
            _result = _reply.readString();
        }
        finally {
            _reply.recycle();
            _data.recycle();
        }
        return _result;
    }

    // 客户端返回mRemote
    @Override
    public IBinder asBinder() {
        return mRemote;
    }
}
```

虽然，但是transact和onTransact写起来太麻烦了，实际使用中还是不要手动写了。

## 2.3 类图

前面铺垫了这么多，来看下这块的类图吧：

![image-20210919105100679](.\images\image-20210919105100679.png)

> 1、 最核心的两个接口，用来描述远程对象的IBinder和用来描述业务接口的IInterface，二者可以相互转换
>
> 2、 注意到IBinder中是没有onTransact()方法的，因为这是服务端明确的方法。例如对于子类BinderProxy是不需要该方法的
>
> 3、 Stub是服务端的实现类，同时具备了IBinder和IInterface的特性
>
> 4、 Proxy是客户端的代理类，需要绑定BinderProxy，然后通过后者的transact()方法进行远程调用



# 3. native层的Binder

## 3.1 类图

与java层的结构类似，以IAudioTrack接口为例，类图如下

.\edraw\【Binder】Binder类图（native层）.png

![image-20211011231834458](.\images\image-20211011231834458.png)



## 3.2 native层Binder的一般开发流程

以IAudioTrack接口为例，

> 首先，在一端（可以是服务端，也可以是客户端，这个不重要）定义IAudioTrack.h的库文件，把IAudioTrack和BnAudioTrack定义好。

![image-20211003213523516](.\images\image-20211003213523516.png)

> 然后，服务端导入IAudioTrack.h，并实现BnAudioTrack。

![image-20211003213807800](.\images\image-20211003213807800.png)

> 最后，客户端同样导入IAudioTrack.ha，并实现BpAudioTrack

![image-20211003213911362](.\images\image-20211003213911362.png)

## 3.3 native层传递数据

native层同样是通过parcelable对象进行跨进程数据传递，在接口层面与java层稍有不同。

|        | 写入parcel      | 从parcel读取     |
| ------ | --------------- | ---------------- |
| java   | writeToParcel() | 通过CREATOR      |
| native | writeToParcel() | readFromParcel() |
|        |                 |                  |



# 