# Activity's ４lanchMode
+ standard singleTop 
+ singleTask 
    - 当设置为singleTask的activity第一次启动时，先获取自己的taskAffinity,然后检查已存在的task中是否
    有一个的affinity的属性与taskAffinity相等，如果有，那就在这个task中生成实例，如果没有找到，那么
    就在一个新的task中生成实例
    - 如果并不是第一次启动，那么某个task中已经有了这个activity的实例，就去找到这个task,将这个实例以上的都出栈，
    把这个实例保持在栈顶，执行栈顶的activity的onNewIntent()方法
+ singleInstance
# Activity lifeCircle

# Service two way start
+ bindService()
+ startService()

# IntentService future:  
while things down,stop itself,  do tings in annother thread, 
a command start from onStartCommand() method

# Fragment lifeCircle:
| Activity | Fragment |
|----------|-------------|
|created   |onAttach();|
|          |onCreate();|
|          |onCreateView();|
|          |onActivityCreated();|
|started   |onStart(); |
|paused    |onPause();|
|resumed   |onResume();|
|stoped    |onStop();|
|destryed  |onDestryView();|
|          |onDestroy();|
|          |onDetach();|
# Fragment BackStack:
before commit,call the addToBackstack(),such as:

```java
Fragment newFragment = new ExampleFragment();
FragmentTransaction transaction = getFragmentManager().beginTransaction();
transaction.replace(R.id.fragment_container, newFragment);
transaction.addToBackStack(null);
transaction.commit();
```


# How to add a Fragment:
1. define it in the XNL　file(<fragment element>)
2. in the code ,add it to the ViewGroup

```java
FragmentManager fragmentManager = activity.getFragmentManager();
FragmentTransaction fragmentTransaction = fragmentManeger.beginTransaction();
//other operations as add,remove,replace
fragmentTransaction.add(R.id.xxx,fragment);
fragmentTransaction.commit();
```

# Add a fragment without giving a UI:
activity.add(Fragment fragment,String tag);
# find the fragment: 
findFragmentByTag(Strng str);

# Class<?>　其中的？有什么用
如果是单独的？，代表这是一个直接或者间接继承自Object的类
如果是　？　extends Class ,代表这是某個类（Class）的子类

# 抽象类和接口的区别
+ 接口不允许方法有具体实现，抽象类可以有具体的实现
+ 接口中的方法默认且只能是public修饰的
+ 一个类可以实现多个接口，但只能继承一个抽象类
+ 一个是类，一个是接口，例如类可以有main方法直接运行，类可以有构造器，
　使用抽象类使用extends,使用接口使用implements
+ 对接口添加新的方法，实现这个接口的所有类都要修改代码已实现新加入的方法，
　而对抽象类添加默认已有实现的方法的话，子类不需要任何修改
  注意：JAVA 8 中接口已经可以加入default关键字修饰的方法，这种方法会提供默认实现，因此实现次接口的类不需要更改代码

# 什么时候使用接口，什么时候使用抽象类
+ 如果方法中有具体实现的话使用抽象类，没有的话使用接口
+ 如果想实现多个接口的话使用接口，抽象类只能继承一个

#　设计模式
+ 观察者模式
+ 工厂模式
+ 单例模式

# Android的事件传递机制
+ 关键的３个方法，按调用事件顺序，```dispatchTouchEvent(),onInterceptTouchEvent(),onTouchEvent().```
+ 对于ViewGroup来说，首先调用dispatchTouchEvent(),然后会检查onInterceptTouchEvent()的返回值，如果返回true，代表
　事件被拦截了，事件就不会再传递给子View,如果然后会检查onInterceptTouchEvent()的返回值为false,代表不拦截，那么事件会
　传递给子View
+ 对于View来说，首先调用dispatchTouchEvent()，然后如果onTouchListener不为空，就会执行onTouchListener.onTouch()方法，
　如果该方法返回true,代表事件被消费了，那么不执行onTouchEvent()方法，如果返回false,代表不消费,那么会执行onTouchEvent()

```java 
public boolean dispatchTouchEvent(MotionEvent event) {  
    if (mOnTouchListener != null && (mViewFlags & ENABLED_MASK) == ENABLED &&  
            mOnTouchListener.onTouch(this, event)) {  
        return true;  
    }  
    return onTouchEvent(event);  
}  
```

#重写View
1. 自定义属性
2. 在View的构造方法中获取属性
3. 重写　onMessure()
4. 重写  onLayout()
5. 重写　onDraw()