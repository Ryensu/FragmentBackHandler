# FragmentBackHandler

[![Release](https://jitpack.io/v/ikidou/FragmentBackHandler.svg)](https://jitpack.io/#ikidou/FragmentBackHandler)

FragmentBackHandler 是一个 Fragment拦截Back键的一个库，仅需两步即可搞定，同时支持多Fragment以及Fragment嵌套。

详细内容参见Blog [两步搞定Fragment的返回键](http://www.jianshu.com/p/fff1ef649fc0)
## Download

```gradle
allprojects {
	repositories {
	    jcenter()
		maven { url "https://jitpack.io" }
	}
}
dependencies {
    compile 'com.github.ikidou:FragmentBackHandler:1.0'
}
```

## Usage

1、 在Activity中覆盖`onBackPressed()`方法
```java
public class MyActivity extends FragmentActivity {
    @Override
    public void onBackPressed() {
        if (!FragmentBackHandler.Helper.handleBackPress(this)) {
            super.onBackPressed();
        }
    }
}
```

2、实现 `FragmentBackHandler`

```java
public class MyFragment extends Fragment implements FragmentBackHandler {
    @Override
    public boolean onBackPressed() {
        if (handleBackPressed) {
            //外理返回键
            return true;
        } else {
            // 如果不包含子Fragment
            // 或子Fragment没有外理back需求
            // 可如直接 return false;
            return FragmentBackHandler.Helper.handleBackPress(this);
        }
    }
}
```

或让需要拦截Back键的Fragment及父Fragment继承`BackHandledFragment`

```java
// [推荐] 适合所有Fragment，只要需要栏截时return true即可，其它的无需关心。
// 当然你也可以让你的BaseFragment 继承 BackHandledFragment
public class MyFragment extends BackHandledFragment {
    // 如果return false 将自动调用FragmentBackHandler.Helper.handleBackPress(this);
    @Override
    public boolean interceptBackPressed() {
        if (handleBackPressed) {
            //外理返回键
            return true;
        } else {
            return false;
        }
    }
}
```

### 已知的问题：
1. 在ViewPager中如果当前右面缓存的Fragment中想拦截Back键时，即使没有显示也会收到`onBackPressed()`。