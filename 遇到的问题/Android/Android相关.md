# 1. 横竖屏切换设置configChanges

**古老答案：**

1. 不设置Activity的android:configChanges时，切屏会重新回调各个生命周期，切横屏时会执行一次，切竖屏时会执行两次。
2. 设置Activity的android:configChanges=”orientation”时，切屏还是会调用各个生命周期，切换横竖屏只会执行一次
3. 设置Activity的android:configChanges=”orientation |keyboardHidden|screenSize”时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法

**上述均已过时，改进方法：**

1. 在API 12 ，即Android4.0以上，configChanges属性需要设置为”orientation |keyboardHidden|screenSize”，不过去掉keyboardHidden也能得到相同的效果（目前Android4.0几乎已经没有使用，所以不再适配，即必须加上screenSize）
2. 上面的第一条无效，似乎7.0以上的版本切换横竖屏生命周期都只执行一次



# 2. 网络请求（使用篇）

**遇到问题1：**

1. HttpURLConnection可以访问HTTP，但是HTTPS有些可访问，有些不可访问？
2. 使用OkHttp可兼容HTTP和HTTPS

**原因：**

由于Https协议存在CA证书，但CA证书又分为两类：

1. 权威机构颁发，浏览器信任，所以使用HttpURLConnection访问HTTP的方式即可访问
2. 非权威机构办法，浏览器不信任，所以使用访问HTTP的方式不成功，解决办法有两种
   1. 信任全部证书
   2. 信任指定证书

**解决办法：**

1. 信任全部证书：
2. 信任指定证书



**遇到问题2：**根据第一行代码版2中p334网络编程的最佳实践，HttpUrlConnection无法访问https的URL，但是可以访问http的URL。

**解决办法：**经过排除，不应该设置connection.setDoOutput(true)；

默认的doOutput的值为false，而这里我们使用GET方法来访问URL，应该让doOutput设置false。

# 3. ANR

ANR定义：程序无响应；

触发ANR：

- Activity 5s不响应
- Service 



**出现ANR的场景：**

1. 直接在服务中去处理一些耗时的逻辑（解决：在IntentService处理耗时逻辑）

# 4. URL和URI详解

# 5. Context，Application