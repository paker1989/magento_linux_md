Apache 使用ssl模块配置HTTPS (比较清晰和详细了)
https://blog.csdn.net/ithomer/article/details/50433363 

Apache配置ssl证书 (这也可以辅助看看)
https://www.jianshu.com/p/19858996a2b5

## 配置证书 (https://cloud.tencent.com/developer/article/1349094)
- 如果自签名的话: 生成证书
  sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
- (optional) sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
- (optional) sudo nano /etc/apache2/conf-available/ssl-params.conf:

```
  # from https://cipherli.st/
  # and https://raymii.org/s/tutorials/Strong_SSL_Security_On_Apache2.html
  ​
  SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH
  SSLProtocol All -SSLv2 -SSLv3
  SSLHonorCipherOrder On
  # Disable preloading HSTS for now.  You can use the commented out header line that includes
  # the "preload" directive if you understand the implications.
  #Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
  Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains"
  Header always set X-Frame-Options DENY
  Header always set X-Content-Type-Options nosniff
  # Requires Apache >= 2.4
  SSLCompression off 
  SSLSessionTickets Off
  SSLUseStapling on 
  SSLStaplingCache "shmcb:logs/stapling-cache(150000)"
  ​
  SSLOpenSSLConfCmd DHParameters "/etc/ssl/certs/dhparam.pem"
```
- 先备份:
  sudo cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak

- sudo nano /etc/apache2/sites-available/default-ssl.conf
  ```
    SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem
    SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
  ```

- 强制https: （其中一种方法）
  sudo nano /etc/apache2/sites-available/000-default.conf
  ```
    Redirect "/" "https://your_domain_or_IP/"
  ```
  永久重定向:
  Redirect permanent "/" "https://your_domain_or_IP/"

- 启用ssl模块
  sudo a2enmod ssl
  sudo a2enmod headers

- (optional) sudo a2enconf ssl-params

- sudo systemctl restart apache2

## 用winscp连接到ec2实例
Connecting Securely to Amazon EC2 Server with SFTP  (https://winscp.net/eng/docs/guide_amazon_ec2)

## 建立密钥对，管理用户
> 新建密钥
pairs de clié -> create key pair -> create .ppk format key pair -> store private key; (本机保存在C:\Users\bxu1\Desktop\desktop\key_files\miloe_test2)

> 检索public key
  - linux ou mac: 
  ```
    ssh-keygen -y -f /path_to_key_pair/my-key-pair.pem
  ```
  - windows电脑:
    winscp -> tools -> puttygen -> load ppk

>  apply new user  (在 Linux 实例上管理用户账户 https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/managing-users.html)
  - 用原来默认账户连上实例:
  ```
    sudo adduser newuser --disabled-password  (ubuntu)

    sudo su - newuser  (https://blog.csdn.net/m0_37263637/article/details/81460169)

    [newuser ~]$ mkdir .ssh
    [newuser ~]$ chmod 700 .ssh

    [newuser ~]$ touch .ssh/authorized_keys
    [newuser ~]$ chmod 600 .ssh/authorized_keys

    vi .ssh/authorized_keys 

    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQClKsfkNkuSevGj3eYhCe53pcjqP3maAhDFcvBS7O6V
	hz2ItxCih+PnDSUaw+WNQn/mZphTk/a/gU8jEzoOWbkM4yxyb/wB96xbiFveSFJuOp/d6RJhJOI0iBXr
	lsLnBItntckiJ7FbtxJMXLvvwJryDUilBMTjYtwB+QhYXUMOzce5Pjz5/i8SeJtjnV3iAoG/cQk+0FzZ
	qaeJAAHco+CY/5WrUBkrHmFJr6HcXkvJdWPkYQS3xqC0+FmUZofz221CBt5IMucxXPkX4rWi+z7wB3Rb
	BQoQzd8v7yeb7OzlPnWOyN0qFU0XA246RA8QFYiCNYwI3f05p6KLxEXAMPLE  (bidon)
  ```

> (optional) delete user
	```
	  sudo userdel -r olduser
	```


## WinSCP如何使用密钥登录sftp服务器
(https://blog.csdn.net/sayoko06/article/details/81126148)