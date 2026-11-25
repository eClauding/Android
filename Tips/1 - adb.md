- Using adb to connect to your android

```ruby
adb devices
```

```ruby
adb -s 'SERIAL NUMBER' shell
```

```ruby
su
```

- Using adb to install applications

```ruby
adb install NAME.apk
```

- Using adb to list full path of packages

```ruby
adb shell pm list packages -f
```

- Using adb to push and pull files

```ruby
adb push FILE /PATH
```

```ruby
adb pull /PATH
```

