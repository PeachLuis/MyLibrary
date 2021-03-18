# 1. 横竖屏切换设置configChanges

**古老答案：**

1. 不设置Activity的android:configChanges时，切屏会重新回调各个生命周期，切横屏时会执行一次，切竖屏时会执行两次。
2. 设置Activity的android:configChanges=”orientation”时，切屏还是会调用各个生命周期，切换横竖屏只会执行一次
3. 设置Activity的android:configChanges=”orientation |keyboardHidden|screenSize”时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法

**上述均已过时，改进方法：**

1. 在API 12 ，即Android4.0以上，configChanges属性需要设置为”orientation |keyboardHidden|screenSize”，不过去掉keyboardHidden也能得到相同的效果（目前Android4.0几乎已经没有使用，所以不再适配，即必须加上screenSize）
2. 上面的第一条无效，似乎7.0以上的版本切换横竖屏生命周期都只执行一次