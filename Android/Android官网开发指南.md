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

3. 创建自己的Fragment类，使其继承自Fragment，在构造函数中传入该Fragment的layout文件（这里不同于第一行代码中的在onCreateView方法传入layout）

   ```java
   class ExampleFragment extends Fragment {
       public ExampleFragment() {
           super(R.layout.example_fragment);
       }
   }
   ```