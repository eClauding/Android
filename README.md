<h1 align="center">Interview Preparation</h1>


<p align="center">
<a href="https://twitter.com/0xAwali"><img src="https://img.shields.io/twitter/follow/0xAwali?style=social"></a>
</p>

### Why frida cannot be executed from `/sdcard`  ?

Frida cannot be executed from `/sdcard` because android mounts that storage location with the `noexec` flag.

### What the difference between activity , broadcast and service ?

- **Activity:** Provides a single screen with a user interface so users can interact with the application e.g. login screen or a settings page,
-  **Broadcast:** Short-lived operations to handle the incoming event inside its `onReceive()` method e.g. app listening for a low battery warning , airplane mode changes or device boot completion.
- **Service:** Performs long-running operations or work for remote processes without a user interface e.g. playing music in the background while the user uses another app or downloading a large file.

### What the difference between Intent and PendingIntent ?

- **Intent:** Runs with the permissions of whichever context is actively executing it.
-  **PendingIntent:** Maintains and uses the **original creator app's identity and permissions** upon execution.

### What the difference between Implicit Intent and Explicit Intent ?

-  **Explicit Intent:** Supplying the context and the destination class.

```ruby
Intent intent = new Intent(getApplicationContext(), Activity.class);
startActivity(intent);
```

- **Implicit Intent:** Not supplying the context and the destination class.

```ruby
Intent intent = new Intent(Intent.ACTION_VIEW);
startActivity(intent);
```

### What is the deep link ?

Specialized hyperlink that sends users directly to a specific page ocontent inside a mobile app rather than a generic homepage or landing page e.g. `scheme://host/path`

### What the difference between scheme:// , intent:// and application link ?

- **scheme://host** Use for specific custom schemes

```ruby
scheme://HOST/PATH
```

- **intent://** Use for launching application from google browser 

```ruby
intent://HOST/PATH#Intent;scheme=SCHEME;package=NAME;S.browser_fallback_url=https%3A%2F%2FEEROR.com;end
```

- **application link** Use for **http** and **https** schemes and auto verify via digital asset links .well-known/assetlinks.json

```ruby
https://HOST
```

### What the difference between bindable service and start service ?

- **Started Service**: started by calling `startService()` or `startForegroundService()` and it performs a single task and does not return results or communicate back to the starting component directly.
- **Bound Service**: started when an application component binds to it using `bindService()` and it provides an `IBinder` interface so clients can send requests , receive results or perform inter-process communication (IPC).

### When the bindable service became not bindable service ?

If `onBind()` return the same service class that use bindable service.


### What is AIDL ?

AIDL is android interface definition language use to declare interfaces used by bindalbe service e.g. `onBind()` return `Stub` interface and this interface expose a methods to call.


### What the difference between normal , signature and dangerous permissions ?

- **Normal Permissions:** The system **automatically grants at installation** without ever prompting the user e.g. `INTERNET` access.
- **Dangerous Permissions:** The user **allow or deny via a runtime pop-up** e.g. `CAMERA` or `ACCESS_FINE_LOCATION` to access.
- **Signature Permissions:** System-level access that is **automatically granted only if** the requesting app shares the exact same cryptographic signing certificate as the application that created the permission.


### What is content provider ?

A content provider presents data to external applications as one or more tables that are similar to the tables found in a relational database.

### What is file provider ?

A content provider presents to access local files instead of the tables found in a relational database.

### What the difference between internal , external and keychain storage ?

- **Internal Storage:** A private filesystem area allocated exclusively to your app , isolated by Android's sandbox :
    - `context.getFilesDir()` → `/data/data/PACKAGE/files/`
    - `context.getCacheDir()` → `/data/data/PACKAGE/cache/`
- **External Storage:** A globally accessible storage space intended for shared media or larger files and subject to scoped storage rules :
    - `context.getExternalFilesDir(null)` → `/storage/emulated/0/Android/data/PACKAGE/files/`
    - `Environment.getExternalStoragePublicDirectory()` → `/storage/emulated/0/`
- **Keychain:** A hardware-backed secure repository inside a trusted execution environment or strong box used to generate and store cryptographic keys.

### What is scoped storage ?

A security and privacy model to give each application a dedicated , sandboxed space , limiting broad access to the device's shared storage and third-party applications can **read** other applications' media files with the `READ_EXTERNAL_STORAGE` permission.


### What is webview ?

Component that allows a native mobile application to display web content like HTML , CSS and javascript inside the application itself without opening a separate browser and exposes a java object to javascript under the name :

```ruby
WebView.addJavascriptInterface(new Class(), "NAME");
```

```ruby
private class Class {  


    @JavascriptInterface  
    public void method(String argument) {  
        try {  

        } catch (Exception e) {  

        }  
    }  
}
```

```ruby
<script>
    var output = NAME.method("value");
    document.write("<pre>" + output + "</pre>");
</script>
```


### What is custom tab ?

Android browser feature that allows a native application to launch a **customizable web browsing powered by the user's preferred browser e.g. chrome** without using a webview so it automatically shares the browser's cookies , logins , saved passwords and autofill data :
- **Shared browser state**: If the user is logged into a site in Chrome , they stay logged in inside the custom tab.
- **Inherited security**: Uses chrome's safety features like google safe browsing making it safer than a webview for external links and auth flows.


```ruby
CustomTabsIntent intent = new CustomTabsIntent.Builder()
    .setShowTitle(true)
    .build();
intent.launchUrl(context, Uri.parse("URL"));
```


### How magisk work ?


Magisk is called systemless since does not modify the system but modifies what the system or a particular process sees :

1. **Patch the boot ramdisk** so `magiskinit` runs before Android init.
2. **Hijack early boot** to patch SELinux and start `magiskd`.
3. **Magic Mount** module files into a per-process filesystem view using bind mounts , tmpfs or OverlayFS while leaving real system partitions untouched.
4. **Broker root** through `magiskd` and a UNIX socket instead of a setuid `su`.
5. **Optionally inject code with Zygisk** and use DenyList to remove magisk’s mounts from selected app namespaces.

### How KernelSU next work ?

KernelSU and KernelSU-Next is called kernel-level root because it does not broker root through a userspace daemon or replace android’s `init`instead it puts the root manager inside the linux kernel and exposes only a controlled `su` interface to userspace :

- **Integrate into the kernel** install as a patched GKI kernel or as a loadable kernel module and there is no `magiskinit` and no ramdisk `/init` replacement.
- **Hook kernel entry points** intercept the manager’s `prctl` commands and in kernelSU-next`execve`/`execveat` calls for `su`approved `su` executions are redirected by the kernel to `/data/adb/ksud` instead of a real setuid `/system/bin/su`.
- **Decide root in kernel space** maintain the allowlist in kernel data structures and apply each app’s `root_profile` UID , GID, capabilities , SELinux context , supplementary groups before the userspace command runs.
- **Optionally hide or inject per app** per-app module visibility and unmount removes module mounts from selected app mount namespaces.

### What is the core difference between Magisk and KernelSU ?

The core difference is **where the root manager lives and makes its decisions**. Magisk puts the root manager in **userspace** , while KernelSU puts it inside the **Linux kernel** :

1. **Magisk is a userspace root broker** it patches the boot ramdisk so its own `magiskinit` runs before android’s `init` then starts the `magiskd` daemon in userspace and that daemon handles root requests over a UNIX socket , applies modules through magic mount and optionally injects code with Zygisk. 

2. **KernelSU is a kernel-level root manager** it does not patch the ramdisk to inject a userspace daemon instead of that it integrates into the kernel itself either as a patched GKI kernel or as a loadable kernel module and makes the root decision **inside kernel space** as the `su` command is virtualized `/system/bin/su` does not really exist on disk and kernel hooks intercept the execution and redirect approved calls to the userspace helper `ksud`.

### What is the core difference in flutter applications  ?

Flutter app doesn't see your system proxy because of a fundamental design choice in its networking stack **Dart's `HttpClient` completely ignores the operating system's proxy settings by default** and the reason of **libflutter.so** is the focus for traffic interception and security testing :

1. **All networking happens inside it** flutter does not use the standard android java or kotlin networking stack so all DNS resolution , socket connections and TLS encryption and decryption are handled natively inside `libflutter.so`. 
   
2. **Certificate pinning** the certificate verification logic is implemented deep within the bundled BoringSSL library inside `libflutter.so` and the app does not rely on the android system's trusted CA store so importing a proxy's certificate into the system store has no effect so you must hook the functions inside this library such as `ssl_crypto_x509_session_verify_cert_chain`.

### What is inside `libflutter.so` ?

1. **Dart virtual machine** the runtime that executes your dart code.
2.  **BoringSSL** a fork of OpenSSL that handles all TLS and SSL networking and certificate verification.
3. **Skia** the 2D graphics engine responsible for rendering the app's UI.

### What is your methodology to test android application ?

#### 1 - Environment test cases :

1. **Root Detection** test if the application running on a rooted device attempt to bypass that detection using root-hiding tools such as **Magisk** or **KernelSU**.

2. **Hook Detection** test if the application detects the presence of instrumentation and hooking frameworks such as **Frida** , **Xposed** via memory map scanning , port scanning or library signatures and attempt to bypass that detection by renaming or repackaging the frida server using frida gadget mode instead of attaching externally.

3. **Emulator Detection** test if the application detects that it is running on an emulator via build properties , sensor absence or QEMU artifacts and attempt to bypass that detection by patching `build.prop` values to match a real device or by hooking the relevant `Build` sensor checks at runtime.

4. **Debug Detection** test if the application detects that it is debuggable or being actively debugged via the `android:debuggable` manifest flag , `Debug.isDebuggerConnected()`or anti-debug `ptrace` self-tracing and attempt to bypass that detection by hooking the detection method with frida to force a negative result or by patching the `ptrace` self-trace at the native layer.


#### 2 - Integrity test cases :


1. **Standalone APK integrity checks** test if the application verifies its own APK integrity independently of platform APIs via checksum validation of `classes.dex` , `AndroidManifest.xml` or the full archive to detect repackaging or by patching the stored hash constants at the smali layer.

2. **Certificate signing checks** test if the application validates its signing certificate via `PackageManager.getPackageInfo()` signature comparison , `Signature` or `SigningInfo` hash checks to detect re-signing or  patching the embedded expected-certificate hash at the native layer.


#### 3 - Code test cases :


1. **Code encryption** test whether the application code or sensitive assets are encrypted at rest to prevent static analysis and verify that decrypted code and data isn’t trivially recoverable at runtime by dumping memory.

2. **Code obfuscation** test whether the application code has been obfuscated class , method and variable name mangling , control-flow flattening and string encryption to resist reverse engineering and assess how effective that obfuscation actually is against standard decompilation tools.

3. **Cross-references hiding** test whether the application hides or obscures cross-references between code components e.g. via reflection , dynamic class loading , native code and JNI calls or indirect method invocation to make it harder for static analysis tools to trace call graphs and data flow.

4. **Client side information disclosure** test whether the application storing sensitive data such as passwords or security keys directly in the source code of an application e.g. `strings.xml`, `BuildConfig`.

#### 4 - IPC test cases :

- Using noxen to intercept intents

```ruby
noxen --new-project NAME
```

- Using jadx to find intents

```ruby
.getIntent(
```

```ruby
Intent
```

```ruby
.getActivity(
```

```ruby
registerReceiver(
```

```ruby
.getBroadcast(
```

```ruby
.getService(
```


- Using java to start activity

```ruby
public void handleClicking(View view) {
    Intent intent = new Intent();
    intent.setClassName("PACKAGE","PACKAGE.ACTIVITY");
    startActivity(intent);
}
```

- Using java to start activity and receive a result intent

```ruby
public void handleClicking(View view) {
	    Intent intent = new Intent();
	    intent.setClassName("PACKAGE","PACKAGE.ACTIVITY");
        startActivityForResult(intent,1);
}

@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    try {

    }catch (IOException e){

    }
}
```

- Using java to hijack implicit intent

```ruby
<activity  
    android:name=".MainActivity"  
    android:exported="true">  
	 <intent-filter>
		 <action android:name="ACTION" />  
	    <category android:name="android.intent.category.DEFAULT" />  
    </intent-filter>
</activity>
```

```ruby
@Override  
protected void onCreate(Bundle savedInstanceState) {

    Intent getFlag = getIntent();   
}
```

- Using java to send pending intent

```ruby
public void handleClicking(View view) {  
    Intent intent = new Intent();  
    PendingIntent pendingIntent = PendingIntent.getActivity(this,0,intent,PendingIntent.FLAG_MUTABLE);  
  
    Intent flag = new Intent();  
    flag.putExtra("PENDING",pendingIntent);  
    startActivity(flag);  
}
```


- Using jadx to find deep link

```ruby
android:scheme="
```

```ruby
android:host="
```

```ruby
.getScheme(
```


```ruby
.getHost(
```


- Using java to send broadcast

```ruby
public void handleClicking(View view) {
    Intent intent = new Intent();
    intent.setClassName("PACKAGE","PACKAGE.BROADCAST");
    sendBroadcast(intent);
}
```

- Using java to send broadcast and receive a result intent

```ruby
BroadcastReceiver result = new BroadcastReceiver() {  
    @Override  
    public void onReceive(Context context, Intent intent) {  
        Bundle flag = getResultExtras(true);
    }  
};
  
public void handleClicking(View view) {  
    Intent flag = new Intent();  
    flag.setClassName("PACKAGE","PACKAGE.BROADCAST");
    sendOrderedBroadcast(flag,null,result,null,RESULT_CANCELED,null,null);  
}  
```


- Using java to hijack broadcast

```ruby
<receiver  
    android:name=".CLASS"  
    android:enabled="true"  
    android:exported="true">  
    <intent-filter>
	    <action android:name="ACTION" />  
    </intent-filter>
</receiver>
```

```ruby
public class CLASS extends BroadcastReceiver {  
    @Override  
    public void onReceive(Context context, Intent intent) {  

        }  
    }  
}
```

```ruby
@Override  
protected void onCreate(Bundle savedInstanceState) {  
  
    BroadcastReceiver result = new CLASS();  
    registerReceiver(result,new IntentFilter("ACTION"), Context.RECEIVER_EXPORTED);  
}
```


- Using java to start service

1. Using startService to start service

```ruby
<queries>
	<package android:name="PACKAGE" />
</queries>
```

```ruby
public void handleClicking(View view) {
    Intent intent = new Intent();
    intent.setClassName("PACKAGE","PACKAGE.SERVICE");
    startService(intent);
}
```

2. Using onStartCommand to receive service

```ruby
@Override
public int onStartCommand(Intent intent, int flags, int startId) {

}
```


- Using java to bind service
1. Using bindService to bind a service

```ruby
<queries>
	<package android:name="PACKAGE" />
</queries>
```

```ruby
private final ServiceConnection connection = new ServiceConnection() {  
    @Override  
    public void onServiceConnected(ComponentName name, IBinder service) {  
        Messenger messenger = new Messenger(service);  
        Message message = Message.obtain(null,42);  
        try {  
            messenger.send(message);  
        } catch (RemoteException e) {  
            throw new RuntimeException(e);  
        }  
    }  
    @Override  
    public void onServiceDisconnected(ComponentName name) {  
        Log.i("bindService","Service Disconnected");  
    }  
};  
public void handleClicking(View view) {  
    Intent flag = new Intent();  
    flag.setClassName("PACKAGE","PACKAGE.SERVICE");  
    bindService(flag,connection,BIND_AUTO_CREATE);  
}
```

2. Using onBind to handle bindable service

```ruby
@Override
public IBinder onBind(Intent intent) {

}
```

- Using bindService to bind a service and receive a service

1. Using bindService to bind a service and replyTo to send a message

```ruby
<queries>
	<package android:name="PACKAGE" />
</queries>
```

```ruby
public void handleClicking(View view) {  
    Intent flag = new Intent();  
    flag.setClassName("PACKAGE","PACKAGE.SERVICE");  
    bindService(flag, connection, Context.BIND_AUTO_CREATE);  
}  
private final ServiceConnection connection = new ServiceConnection() {  
    @Override  
    public void onServiceConnected(ComponentName componentName, IBinder service) {  
        messenger = new Messenger(service);  
        Message message = Message.obtain(null, 2);  
        message.replyTo = Receiver;  
  
        try {  
            messenger.send(message);  
        } catch (RemoteException e) {  

        }  
    }  
  
    @Override  
    public void onServiceDisconnected(ComponentName componentName) {  

    }  
};  
  
private final Messenger Receiver = new Messenger(new IncomingHandler());  
private class IncomingHandler extends Handler {  
    IncomingHandler() {  
        super(Looper.getMainLooper());  
    }  
  
    @Override  
    public void handleMessage(@NonNull Message msg) {  
        Bundle data = msg.getData();  
        if (data != null) {  

        } else {  

        }  
    }  
}
```


2. Using onBind to handle bindable service

```ruby
@Override
public IBinder onBind(Intent intent) {

}
```

- Using jadx to trace a AIDL 

```ruby
TRANSACTION_
```

#### 5 - WebView test cases :

1. Inspect the webview from chrome on a computer

```ruby
.setWebContentsDebuggingEnabled(true);
```


2. Gets the settings object used to configure the webview's behavior

```ruby
.getSettings(
```

3. Allows JavaScript to run in the webview

```ruby
.setJavaScriptEnabled(true);
```

4. Allows file access via `file://` 

```ruby
.setAllowFileAccess(true);
```

5. Allows file access to access files from HTML page via `file://`

```ruby
.setAllowFileAccessFromFileURLs(true);
```

6. Bypass Same Origin Policy

```ruby
.setAllowUniversalAccessFromFileURLs(true);
```

7. Exposes a java object to javascript under the name

```ruby
.addJavascriptInterface(
```

```ruby
WebView.addJavascriptInterface(new Class(), "NAME");
```

```ruby
private class Class {  


    @JavascriptInterface  
    public void method(String argument) {  
        try {  

        } catch (Exception e) {  

        }  
    }  
}
```

```ruby
<script>
    var output = NAME.method("value");
    document.write("<pre>" + output + "</pre>");
</script>
```

#### 6 - Network test cases :

1. **SSL Pinning** test whether the application implements certificate and public-key pinning to validate the server’s identity beyond standard CA trust chain validation for preventing MITM interception even with a trusted proxy CA installed on the device.

2. **Insecure HTTP Connection** test whether the application transmits any traffic over unencrypted HTTP instead of HTTPS or allows cleartext traffic to be enabled via `network_security_config.xml`’s `cleartextTrafficPermitted`.

#### 7 - Storage test cases :


- Using fsmon to monitor data directory

```ruby
fsmon -t -b /sdcard/BACKUP /data/data/PACKAGE
```

1. Cache files is temporary files stored by applications to help them load faster next time.

- Using adb to `cd`  into cache files

```ruby
adb shell
```

```ruby
su
```

```ruby
cd /data/data/PACKAGE/cache/
```


2. Internal files is a private filesystem area allocated exclusively to your app

- Using adb to `cd`  into internal files

```ruby
adb shell
```

```ruby
su
```

```ruby
cd /data/data/PACKAGE/files/
```


3. Shared preferences files

- Using adb to `cd`  into shared preferences files

```ruby
adb shell
```

```ruby
su
```

```ruby
cd /data/data/PACKAGE/shared_prefs/
```


4. Database files

- Using adb to search about `db` files

```ruby
adb shell
```

```ruby
su
```

```ruby
find /data/data/PACKAGE/ -name "*.db"
```

```ruby
cd /data/data/PACKAGE/database
```

- Using sqlite3 to open `db` files

```ruby
sqlite3 FILE.db
```

```ruby
.tables
```

```ruby
select * from TABLE;
```


5. External files is a shared filesystem area allocated exclusively to your app

- Using adb to `cd`  into external files

```ruby
adb shell
```

```ruby
su
```

```ruby
cd `/storage/emulated/0/Android/data/PACKAGE/
```


#### 8 - Content and file provider test cases :


- Using java to query content provider

```ruby
<queries>  
    <package android:name="PACKAGE"/>
</queries>
```

```ruby
public void handleClicking(View view) {  
    Uri uri = Uri.parse("content://AUTHORITY/PATH");
    dump(uri);  
}
```

```ruby
public void dump(Uri uri) {  
    Cursor cursor = getContentResolver().query(uri, null, null, null, null);  
    if (cursor.moveToFirst()) {  
        do {
   
        } while (cursor.moveToNext());  
    }  
}
```

- Using java to handle content provider

1. Using `query()` to handle query operation

```ruby
@Override
public Cursor query(Uri uri,
					String[] projection,
					String selection,
                    String[] selectionArgs,
                    String sortOrder) {

}    
```

2. Using `insert()` to handle insert operation

```ruby
@Override    
public Uri insert(Uri uri,
				  ContentValues values) {

}
```

3. Using `update()` to handle update operation

```ruby
@Override
public int update(Uri uri,
				  ContentValues values,
				  String selection,
				  String[] selectionArgs) {

}
```

4. Using `delete()` to handle delete operation

```ruby
@Override
public int delete(Uri uri,
				  String selection,
				  String[] selectionArgs) {

}
```

- Using java to hijack content provider if application granted access 

```ruby
<activity  
    android:name=".CLASS"  
    android:exported="true">  
    <intent-filter>
	    <action android:name="ACTION" />  
        <category android:name="android.intent.category.DEFAULT" />  
        <data android:scheme="content" />  
        <data android:host="AUTHORTY" />  
        <data android:path="/PATH" />  
    </intent-filter>
</activity>
```


```ruby
Uri uri = getIntent().getData();  
if (uri != null){  
    Cursor cursor = getContentResolver().query(uri, null, null, null, null);  
    if (cursor.moveToFirst()) {  
        do {
   
        } while (cursor.moveToNext());  
    }  
```


- Using java to access file provider

```ruby
<queries>  
    <package android:name="PACKAGE"/>
</queries>
```

```ruby
public void handleClicking(View view) {  
    Uri uri = Uri.parse("content://AUTHORITY/PATH");
    dump(uri);  
}
```

```ruby
public void dump(Uri uri) {  
	InputStream inputStream = getContentResolver().openInputStream(data.getData());  
	BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));  
	String line;  
	while ((line = reader.readLine()) != null){
	  
	}
}
```

- Using java to handle file provider

1. Declare `file_paths` file to allow access

```ruby
 <provider 
 android:authorities="AUTHORITY" 
 android:grantUriPermissions="true"> 
	 <meta-data android:resource="@xml/file_paths" /> 
 </provider>
```

```ruby
<paths xmlns:android="http://schemas.android.com/apk/res/android">
   <files-path name="NAME" path="/"/>
</paths>
```

2. Handle file access using `openFile` 

```ruby
@Override  
public ParcelFileDescriptor openFile(Uri uri,
									 String mode) throws FileNotFoundException {
									 
}
```

#### 9 - Runtime test cases :

1. **Lock or biometric authentication bypass** If the application uses android lock or biometric authentication check for that can be bypassed or not in runtime by hooking or code level modification.
2. **Token storage and expiry** check where tokens live and expiry enforcement and server-side revocation after logout.
3. **Application memory** check for any sensitive information and data temporarily stored on application memory.
4. **Sensitive data in ADB logcat logs** check logcat logs for sensitive information and data.
5. **Background screen caching and screenshots** check for screenshots are taken when the application is sent to background and check for any sensitive data or information leaks to screenshots or not.
6. **Clipboard** check the application does not allow users to copy and paste any secret data like passwords , credit card info etc.

#### 10 - Misconfiguration test cases :

1. **Over-privileged permissions** compare declared `AndroidManifest.xml` permissions against actual app functionality.
2. **Accessibility service abuse** check whether a malicious accessibility service can read screen content or inject input into the app.
3. **Tapjacking** check whether sensitive activities set `filterTouchesWhenObscured="true"`and test overlay-based click hijacking.
4. **Task hijacking** review `taskAffinity` and `launchMode` on exported activities for task-stack hijack potential.
5. **Weak or broken algorithms** check for ECB mode , hardcoded IVs , MD5/SHA-1 used for integrity , or home-grown crypto.
6. **Backup and export** test `adb backup` extraction of application data and confirm sensitive files are explicitly excluded from backup rules.
7. **Third-party and supply chain** check dependency versions against known CVEs or unverified dynamic feature module downloads.
8. **Firebase and Google API keys** check whether the embedded config exposes open read or write rules reachable directly from the app and confirm embedded API keys are restricted by package name and SHA-1 not usable from anywhere.

#### 11 - Native layer test cases :

1. **JNI and native analysis** decompile `.so` files for hardcoded secrets or weak checks reimplemented natively for Java-layer hooking.

#### 12 - Business logic and server-side :

1. **IDOR** parameter tampering on user and object IDs in API calls the app makes.
2. **Rate limiting** check auth and OTP endpoints discovered via app traffic.
3. **And so on** check SQLi , SSRF , LFI etc.

