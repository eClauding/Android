- Using apktool to decompile APK

```ruby
apktool d NAME.apk
```

- Using apktool to build APK again

```ruby
apktool b --output NAME.apk
```

- Using keytool to generate key

```ruby
keytool --genkey -v -keyalg RSA -keysize 2048 -validity 1000 -alias NAME.alias -keystore NAME.keystore
```

- Using jarsigner to sign APK

```ruby
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore NAME.keystore OUTPUT.apk NAME.alias
```

- Using zipalign to ensure the APK is zipaligned

```ruby
zipalign -p -f -v 4 NAME.apk OUTPUT.apk
```