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