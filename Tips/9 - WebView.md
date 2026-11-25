
You can use `data:text/html,<script>window.NAME.method("value")</script>` to execute code

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