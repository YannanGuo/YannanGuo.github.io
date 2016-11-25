title: WebView三部曲：代码交互
date: 2016-7-31 00:00:00
tags:
---

本系列文章讨论WebView的各种用法以及使用技巧, 一共包含三篇文章:

- [WebView三部曲：基本用法](https://github.com/guoxiaoxing/android-hybird-app/blob/master/webview/doc/WebView三部曲：基本用法.md) 
- [WebView三部曲：代码交互](https://github.com/guoxiaoxing/android-hybird-app/blob/master/webview/doc/WebView三部曲：代码交互.md) 
- [WebView三部曲：性能优化](https://github.com/guoxiaoxing/android-hybird-app/blob/master/webview/doc/WebView三部曲：性能优化.md)

<!-- more -->

# WebView三部曲：性能优化

作者: 郭孝星  
邮箱: guoxiaoxingse@gmail.com  
博客: https://guoxiaoxing.github.io/  
简书: http://www.jianshu.com/users/66a47e04215b/latest_articles

**关于作者**

>郭孝星，字仲明，非著名程序员，代码洁癖患者，爱编程，好音乐，喜烹饪，爱一切有趣的事物和人。

**关于文章**

>作者的文章都会同时发布在个人博客和简书博客上, 文章顶部也会附上文章的Github链接。如果文章中有什么疑问欢迎发邮件与我交流, 对于交流的问
题, 请描述清楚并附上代码与日志, 我一般都会给予回复。如果文章中有什么错误, 也欢迎斧正。如果你觉得本文章对你有所帮助, 也欢迎去star文
章, 关注文章的最新的动态。

**关于文章**

>作者的每一篇文章都会同时发布在Github、CSDN和知乎上, 文章顶部也会附上Github上的文章链接和代码链接。如果文章中有什么疑问欢迎发邮件与
我交流, 对于交流的问题, 请描述清楚并附上代码与日志, 我一般都会给予回复。如果文章中有什么错误, 也欢迎斧正。如果你觉得本文章对你
有所帮助, 也欢迎去star文章, 关注文章的最新的动态。

本系列文章讨论WebView的各种用法以及使用技巧, 一共包含三篇文章:

- [WebView三部曲：基本用法](https://github.com/guoxiaoxing/android-hybird-app/blob/master/webview/doc/WebView三部曲：基本用法.md) 
- [WebView三部曲：代码交互](https://github.com/guoxiaoxing/android-hybird-app/blob/master/webview/doc/WebView三部曲：代码交互.md) 
- [WebView三部曲：性能优化](https://github.com/guoxiaoxing/android-hybird-app/blob/master/webview/doc/WebView三部曲：性能优化.md)


# 一 优化网页加载速度

默认情况html代码下载到WebView后，webkit开始解析网页各个节点，发现有外部样式文件或者外部脚本文件时，会异步发起网络请求下载文件，但如果
在这之前也有解析到image节点，那势必也会发起网络请求下载相应的图片。在网络情况较差的情况下，过多的网络请求就会造成带宽紧张，影响到css或
js文件加载完成的时间，造成页面空白loading过久。解决的方法就是告诉WebView先不要自动加载图片，等页面finish后再发起图片加载。

设置WebView, 先禁止加载图片

```java
WebSettings webSettings = mWebView.getSettings();

//图片加载
if(Build.VERSION.SDK_INT >= 19){
    webSettings.setLoadsImagesAutomatically(true);
}else {
    webSettings.setLoadsImagesAutomatically(false);
}
```

覆写WebViewClient的onPageFinished()方法, 页面加载结束后再加载图片

```java
@Override
public void onPageFinished(WebView view, String url) {
    super.onPageFinished(view, url);
    if (!view.getSettings().getLoadsImagesAutomatically()) {
        view.getSettings().setLoadsImagesAutomatically(true);
    }
}
```

**注意**: 4.4以上系统在onPageFinished时再恢复图片加载时,如果存在多张图片引用的是相同的src时，会只有一个image标签得到加载，因而对于这样的系统我们就先直接加载。

# 二 硬件加速页面闪烁问题

4.0以上的系统我们开启硬件加速后，WebView渲染页面更加快速，拖动也更加顺滑。但有个副作用就是，当WebView视图被整体遮住一块，然后突然恢复时（比如使用SlideMenu将WebView从侧边
滑出来时），这个过渡期会出现白块同时界面闪烁。解决这个问题的方法是在过渡期前将WebView的硬件加速临时关闭，过渡期后再开启，如下所示:

过度前关闭硬件加速

```java
if(Build.VERSION.SDK_INT > Build.VERSION_CODES.HONEYCOMB){
    mWebView.setLayerType(View.LAYER_TYPE_SOFTWARE, null);
}
```

过度前开启硬件加速

```java
if(Build.VERSION.SDK_INT > Build.VERSION_CODES.HONEYCOMB){
    mWebView.setLayerType(View.LAYER_TYPE_HARDWARE, null);
}
```

以上三篇, 大致就说这么多,  在实际的项目中我们通常会自己去封装一个H5Activity用来统一显示H5页面, 下面就提供了完整的H5Activity, 封装了WebView各种特性与jockeyjs代码交互。
               
该H5Activity提供WebView常用设置、H5页面解析、标题解析、进度条显示、错误页面展示、重新加载等功能。可以拿去稍作改造, 用于自己的项目中。
               
```java
package com.guoxiaoxing.webview;

import android.content.Context;
import android.graphics.Bitmap;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.Build;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.text.TextUtils;
import android.util.Log;
import android.view.KeyEvent;
import android.view.View;
import android.view.Window;
import android.webkit.JsResult;
import android.webkit.WebChromeClient;
import android.webkit.WebResourceError;
import android.webkit.WebResourceRequest;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.ProgressBar;

import com.jockeyjs.Jockey;
import com.jockeyjs.JockeyImpl;

/**
 * Author: guoxiaoxing
 * Date: 16/7/21 下午4:03
 * Function: H5Activity 提供各种功能载入Web页面
 * <p>
 * For more information, you can visit https://github.com/guoxiaoxing or contact me by
 * guoxiaoxingv@163.com
 */
public class H5Activity extends AppCompatActivity {

    public static final String H5_URL = "H5_URL";
    private static final String JOCKEY_EVENT_NAME = "JOCKEY_EVENT_NAME";
    private static final String TAG = H5Activity.class.getSimpleName();

    private Toolbar mToolbar;
    private ProgressBar mProgressBar;

    private Jockey mJockey;
    private WebView mWebView;
    private WebViewClient mWebViewClient;
    private WebChromeClient mWebChromeClient;

    private String mUrl;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        supportRequestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.activity_h5);
        setupView();
        setupSettings();
    }

    @Override
    protected void onStart() {
        super.onStart();
        setupJockey();
        setupData();
    }

    private void setupView() {
        mToolbar = (Toolbar) findViewById(R.id.h5_toolbar);
        mProgressBar = (ProgressBar) findViewById(R.id.h5_progressbar);
        mWebView = (WebView) findViewById(R.id.h5_webview);
    }

    private void setupSettings() {

        mWebView.setScrollBarStyle(WebView.SCROLLBARS_INSIDE_OVERLAY);
        mWebView.setHorizontalScrollBarEnabled(false);
        mWebView.setOverScrollMode(WebView.OVER_SCROLL_NEVER);

        WebSettings mWebSettings = mWebView.getSettings();
        mWebSettings.setSupportZoom(true);
        mWebSettings.setLoadWithOverviewMode(true);
        mWebSettings.setUseWideViewPort(true);
        mWebSettings.setDefaultTextEncodingName("utf-8");
        mWebSettings.setLoadsImagesAutomatically(true);

        //JS
        mWebSettings.setJavaScriptEnabled(true);
        mWebSettings.setJavaScriptCanOpenWindowsAutomatically(true);

        mWebSettings.setAllowFileAccess(true);
        mWebSettings.setUseWideViewPort(true);
        mWebSettings.setDatabaseEnabled(true);
        mWebSettings.setLoadWithOverviewMode(true);
        mWebSettings.setDomStorageEnabled(true);


        //缓存
        ConnectivityManager connectivityManager = (ConnectivityManager) this.getSystemService(Context.CONNECTIVITY_SERVICE);
        NetworkInfo info = connectivityManager.getActiveNetworkInfo();
        if (info != null && info.isConnected()) {
            String wvcc = info.getTypeName();
            Log.d(TAG, "current network: " + wvcc);
            mWebSettings.setCacheMode(WebSettings.LOAD_DEFAULT);
        } else {
            Log.d(TAG, "No network is connected, use cache");
            mWebSettings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
        }

        if (Build.VERSION.SDK_INT >= 16) {
            mWebSettings.setAllowFileAccessFromFileURLs(true);
            mWebSettings.setAllowUniversalAccessFromFileURLs(true);
        }

        if (Build.VERSION.SDK_INT >= 12) {
            mWebSettings.setAllowContentAccess(true);
        }

        setupWebViewClient();
        setupWebChromeClient();
    }

    private void setupJockey() {
        mJockey = JockeyImpl.getDefault();
        mJockey.configure(mWebView);
        mJockey.setWebViewClient(mWebViewClient);
        mJockey.setOnValidateListener(new Jockey.OnValidateListener() {
            @Override
            public boolean validate(String host) {
                return "yourdomain.com".equals(host);
            }
        });

        //TODO set your event handler
        mJockey.on(JOCKEY_EVENT_NAME, new EventHandler());
    }

    private void setupData() {
        mUrl = getIntent().getStringExtra(H5_URL);
        if (TextUtils.isEmpty(mUrl)) {
            //TODO show error page
        } else {
            mWebView.loadUrl(mUrl);
        }
    }

    private void setupWebViewClient() {
        mWebViewClient = new WebViewClient() {
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {
                //TODO 处理URL, 例如对指定的URL做不同的处理等
                return false;
            }

            @Override
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
            }

            @Override
            public void onPageStarted(WebView view, String url, Bitmap favicon) {
                super.onPageStarted(view, url, favicon);
            }

            @Override
            public void onReceivedError(WebView view, WebResourceRequest request, WebResourceError error) {
                super.onReceivedError(view, request, error);
            }
        };
        mWebView.setWebViewClient(mWebViewClient);
    }

    private void setupWebChromeClient() {
        mWebChromeClient = new WebChromeClient() {
            @Override
            public void onReceivedTitle(WebView view, String title) {
                super.onReceivedTitle(view, title);
                mToolbar.setTitle(title);

            }

            @Override
            public void onProgressChanged(WebView view, int newProgress) {
                super.onProgressChanged(view, newProgress);
                mProgressBar.setProgress(newProgress);
                if (newProgress == 100) {
                    mProgressBar.setVisibility(View.GONE);
                } else {
                    mProgressBar.setVisibility(View.VISIBLE);
                }
            }

            @Override
            public boolean onJsAlert(WebView view, String url, String message, JsResult result) {
                return super.onJsAlert(view, url, message, result);
            }
        };
        mWebView.setWebChromeClient(mWebChromeClient);
    }

    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if ((keyCode == KeyEvent.KEYCODE_BACK) && mWebView.canGoBack()) {
            mWebView.goBack();
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }
}

```