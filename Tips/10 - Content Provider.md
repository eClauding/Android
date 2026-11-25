
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