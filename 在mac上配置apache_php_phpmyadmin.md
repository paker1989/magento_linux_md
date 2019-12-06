## apache / httpd
```
 sudo apachectl stop
 sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2>/dev/null

 brew install httpd
```

## 配置apache httpd.conf
```
vi /usr/local/etc/httpd/httpd.conf
```
修改:
- AllowOverride All
- LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so

## 配置php
```
brew install php@7.4
```
修改:
- LoadModule php7_module /usr/local/opt/php@7.4/lib/httpd/modules/libphp7.so
- httpd.conf:
  <IfModule dir_module>
    DirectoryIndex index.html index.php
  </IfModule>
  <FilesMatch \.php$>
    SetHandler application/x-httpd-php
  </FilesMatch>
然后
```
  sudo apachectl start
```

## Annexe
- 默认web目录:
  /usr/local/var/www
- httpd目录:
  /usr/local/etc/httpd/httpd.conf
- 有用文章: 
  https://getgrav.org/blog/macos-catalina-apache-multiple-php-versions
