- Using fridump3 to dump the memory

```ruby
frida-ps --usb --applications --installed
```

```ruby
fridump3 --usb --strings 'APPLICATION NAME' --out OUT 
```