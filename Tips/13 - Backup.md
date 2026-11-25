- Using abe.jar to extract the backup

```ruby
adb backup -f INPUT.ab -apk PACKAGE
```

```ruby
java -jar abe.jar unpack INPUT.ab OUTPUT.tar
```