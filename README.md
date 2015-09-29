title: 博客主题Denmo
date: 2015-09-27 11:08:33
tags:
---
![图片](http://ww2.sinaimg.cn/mw690/6ff3843bjw1ek0d43jkv1j21hc0u0dh3.jpg)

[这是一个连接Demo](http://andforce.com)

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
---
1. 有序列表
2. 有序列表
3. 有序列表

---
+ 无序列表
+ 无序列表
+ 无序列表

---

> 引用引用引用引用引用引用引用引用引用引用引用引用引用引用引用引用引用引用

## `javascript` 代码块
``` javascript
public class KeyboardFrameLayout extends FrameLayout {


    private KeyboardHelper mHelper;

    public KeyboardFrameLayout(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        mHelper = new KeyboardHelper(this);
    }

    public KeyboardFrameLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        mHelper = new KeyboardHelper(this);
    }

    public KeyboardFrameLayout(Context context) {
        super(context);
        mHelper = new KeyboardHelper(this);
    }

    public void setOnKeyboardStateListener(OnKeyboardStateChangeListener listener) {
        mHelper.setOnKeyboardStateListener(listener);
    }


    public KeyboardHelper getKeyBoardHelper() {
        return mHelper;
    }


    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        super.onLayout(changed, l, t, r, b);
        mHelper.init();
    }

}
```

## `shell`代码块
``` script
WDYdeMacBook-Pro:vendor andforce$ ls -al
total 24
drwxr-xr-x  5 andforce  staff   170  9 27 10:56 .
drwxr-xr-x  8 andforce  staff   272  9 27 08:43 ..
-rw-r--r--  1 andforce  staff  2321  9 27 08:43 _echo.styl
-rw-r--r--  1 andforce  staff  1053  9 27 08:43 _highlight.styl
-rw-r--r--  1 andforce  staff  2226  9 27 08:43 _normalize.styl
```
