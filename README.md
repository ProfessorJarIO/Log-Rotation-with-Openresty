# How to use Logrotate with Openresty

1. You'll need to create a new file in the `/etc/logrotate.d/` directory. This filename can be whatever you want.

```
sudo touch /etc/logrotate.d/openresty
```

2. You will need to edit this file and use these configurations (Note that these things will differ for people. So please do your own research on Logrotate)

```
sudo vim /etc/logrotate.d/openresty
```
Inside `/etc/logrotate.d/openresty`, import these configurations:

```
/usr/local/openresty/nginx/logs/access.log {
    daily
    rotate 10
    compress
    delaycompress
    copytruncate
    notifempty
    create 0644 root root
    su root root
}

/usr/local/openresty/nginx/logs/error.log {
    daily
    rotate 10
    compress
    delaycompress
    copytruncate
    notifempty
    create 0644 root root
    su root root
}
```

3. You will now need to edit the `/lib/systemd/system/logrotate.service` file, otherwise you'll get an error

```
sudo vim /lib/systemd/system/logrotate.service
```

Inside `/lib/systemd/system/logrotate.service`, add the following line at the very end of the file:

```
ReadWritePaths=/usr/local/openresty/nginx/logs
```

There you go! I believe that's all you need to do in order to use Logrotate with Openresty. Once again, tweak these settings to how you need it. You may want different permissions, users/groups, directories, filenames, etc...