
- Using fsmon to monitor data directory

```ruby
fsmon -t -b /sdcard/BACKUP /data/data/PACKAGE
```

### Cache Files

Temporary files stored by applications to help them load faster next time.

- Using adb to `cd`  into shared preferences files

```ruby
adb shell
```

```ruby
su
```

```ruby
cd /data/data/PACKAGE/cache/
```


### Internal Files

A private filesystem area allocated exclusively to your app

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


### Shared Preferences Files

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


### Database Files

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


### External Files

A shared filesystem area allocated exclusively to your app

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

