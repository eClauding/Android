
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

- Using java to handle content provider

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