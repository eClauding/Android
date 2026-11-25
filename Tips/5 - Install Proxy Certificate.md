
- Using adb to push a certificate

```ruby
adb push cacert.der /sdcard/Download/
```

- Installing user certificates to system certificates on android 13 and below

```ruby
adb root
```

```ruby
adb shell
```

```ruby
cp /system/etc/security/cacerts/* /data/misc/user/0/cacerts-added/
mount -t tmpfs tmpfs /system/etc/security/cacerts
cp /data/misc/user/0/cacerts-added/* /system/etc/security/cacerts/
chown root:root /system/etc/security/cacerts/*
chmod 644 /system/etc/security/cacerts/*
chcon u:object_r:system_file:s0 /system/etc/security/cacerts/*
```

- Installing user certificates to system certificates on android 14 and above

```ruby
adb shell
```

```ruby
su
```

```ruby
mkdir -p -m 700 /data/local/tmp/tmp-ca-copy
cp /apex/com.android.conscrypt/cacerts/* /data/local/tmp/tmp-ca-copy/
mount -t tmpfs tmpfs /system/etc/security/cacerts
mv /data/local/tmp/tmp-ca-copy/* /system/etc/security/cacerts/
cp /data/misc/user/0/cacerts-added/* /system/etc/security/cacerts/
chown root:root /system/etc/security/cacerts/*
chmod 644 /system/etc/security/cacerts/*
chcon u:object_r:system_file:s0 /system/etc/security/cacerts/*
ZYGOTE_PID=$(pidof zygote || true)
ZYGOTE64_PID=$(pidof zygote64 || true)
for Z_PID in "$ZYGOTE_PID" "$ZYGOTE64_PID"; do
    if [ -n "$Z_PID" ]; then
        nsenter --mount=/proc/$Z_PID/ns/mnt -- \
            /bin/mount --bind /system/etc/security/cacerts /apex/com.android.conscrypt/cacerts
    fi
done
APP_PIDS=$(
    echo "$ZYGOTE_PID $ZYGOTE64_PID" | \
    xargs -n1 ps -o 'PID' -P | \
    grep -v PID
)
for PID in $APP_PIDS; do
    nsenter --mount=/proc/$PID/ns/mnt -- \
        /bin/mount --bind /system/etc/security/cacerts /apex/com.android.conscrypt/cacerts &
done
wait
```

- Configure emulator proxy to use IP:PORT

```ruby
adb shell settings put global http_proxy IP:PORT
```