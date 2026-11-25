- Using adb for logging 

```ruby
adb shell pm list packages -3
```

```ruby
adb logcat --clear
```

```ruby
frida-ps --usb --applications --installed
```

```ruby
adb logcat --pid=$(adb shell pidof -s PACKAGE) --file OUTPUT.txt
```