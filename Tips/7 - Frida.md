### Installing Frida

- Starting emulator with a writable system

```ruby
emulator -writable-system -avd NAME
```

- Using adb to be a root

```ruby
adb root
```

```ruby
adb remount
```

- Using adb to push frida

```ruby
adb push frida /sdcard/Download/
```

- Using adb to move frida to bin folder 

```ruby
adb shell
```

```ruby
cp /sdcard/Download/frida /system/bin/frida
```

```ruby
chmod 755 /system/bin/frida
```

### Tracing Method

```ruby
frida-ps --usb --applications --installed
```

```ruby
frida-trace --usb --file 'PACKAGE NAME' --include-java-method 'PACKAGE.NAME.CLASS!METHOD'
```

```ruby
frida-trace --usb --file 'PACKAGE NAME' --include-java-method --include-java-method 'PACKAGE.NAME.*!*'
```

### Bypass SSL Pinning

- Using frida to bypass SSLContext and okhttp3 certificate pinning

```ruby
Java.perform(() => {
    var PlatformClass = Java.use("com.android.org.conscrypt.Platform");
    PlatformClass.checkServerTrusted.overload('javax.net.ssl.X509TrustManager', '[Ljava.security.cert.X509Certificate;', 'java.lang.String', 'com.android.org.conscrypt.AbstractConscryptSocket').implementation = function() {
        console.log("Check server trusted");
    }
})
```

```ruby
Java.perform(() => {
    var BuilderClass = Java.use("okhttp3.OkHttpClient$Builder");
    BuilderClass.certificatePinner.implementation = function() {
        console.log("Certificate pinner called");
        return this;
    }
})
```

```ruby
frida-ps --usb --applications --installed
```

```ruby
frida --usb --load SSLContext.js --load okhttp3.js --file 'PACKAGE NAME'
```
