### Patch APK file

- Using objection to enable debugging and use frida

```ruby
objection patchapk --enable-debug --source NAME.apk
```

### Bypass SSL Pinning

- Using objection to bypass SSL pinning

```ruby
frida-ps --usb --applications --installed
```

```ruby
objection --name "APPLICATION NAME" start
```

```ruby
android sslpinning disable
```