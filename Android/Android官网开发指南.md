# Fragment

## 1. 创建Fragment

1. 需保证外层build.gradle中使用google()库

   ```groovy
   buildscript {
       ...
   
       repositories {
           google()
           ...
       }
   }
   
   allprojects {
       repositories {
           google()
           ...
       }
   }
   ```

2. 添加androidx依赖

   ```groovy
   dependencies {
       def fragment_version = "1.3.1"
   
       // Java language implementation
       implementation "androidx.fragment:fragment:$fragment_version"
       // Kotlin
       implementation "androidx.fragment:fragment-ktx:$fragment_version"
   }
   ```

3. 创建自己的Fragment类，使其继承自Fragment，在构造函数中传入该Fragment的layout位置（这里不同于第一行代码中的在onCreateView方法传入layout）

   ```java
   class ExampleFragment extends Fragment {
       public ExampleFragment() {
           super(R.layout.example_fragment);
       }
   }
   ```

   > Fragment库还提供了特定的Fragment类：
   >
   > 1. DialogFragment：代替在Activity中的使用Dialog helper创建DialogActivity
   > 2. PreferenceFragmentCompat
   >    Displays a hierarchy of Preference objects as a list. You can use PreferenceFragmentCompat to create a settings screen for your app.