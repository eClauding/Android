- Using jdax to find a native library

```ruby
System.loadLibrary("NAME")
```

```java
public class NativeExample {

    public native void sayHello();

    static {
        System.loadLibrary("NAME"); 
    }
    public static void main(String[] args) {
        new NativeExample().sayHello();
    }
}
```

```ruby
System.load("/PATH/libNAME.so")
```

```java
public class NativeExample {

    public native void sayHello();

    static {
        try {
            System.load("/home/user/libs/libNAME.so");
        } catch (UnsatisfiedLinkError e) {
        
        }
    }
    public static void main(String[] args) {
        new NativeExample().sayHello();
    }
}
```


- Using ghidra to fingerprint a native method

1. In case `public native void INSTANCE()` , You will retype variable

```ruby
Java_com_PACKAGE_name_CLASS_INSTANCE(JNIEnv *env, jobject Obj)
```

2. In case `public static native void STATIC()` , You will retype variable

```ruby
Java_com_PACKAGE_name_CLASS_STATIC(JNIEnv *env, jclass class)
```
