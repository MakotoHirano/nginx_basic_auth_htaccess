### nginx_basic_auth_htaccess_module
`ngx_basic_auth_htaccess` enable basic auth for each directories dinamically.
It behaves like `.htaccess` of apache.

You can decide file name to load. implicitly it is `.htaccess`. ofcouse you can set the filename in configuration file.
The grammer of the setting file are same as [nginx_basic_auth](http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html) basic module of nginx. (its built by default.)

The module try to load `basic_auth_file` from same directory of request uri.
If the module couldn't find `basic_auth_file`, the module will try to find `basic_auth_file` from parent directory.
The module will try to find the `basic_auth_file` until it will reach to document root directory (it is set by `root` configuration directive.)


### Configuration
The configuration directive is like nginx_basic_auth module of official module.

|Key|Description|Default|
|---|---|---|
|auth_basic_htaccess|realm name. any string to be shown to user.|NULL|
|auth_basic_user_file_htaccess|file name which will be searched by module.|.htaccess|


#### Example
For example, if you want to set basic auth. user `hoge` and password `fuga` for the directory `/var/www/html/dir1`

###### make htpasswd file.

```
$ cd /var/ww/html/dir1
$ sudo htpasswd -c .htpasswd hoge
[sudo] password for user:
New password:
Re-type new password:
```

###### setup configuration file like below.

```
location / {
    auth_basic_htaccess           "closed site";
    auth_basic_user_file_htaccess .htpasswd;
}
```

###### protect to be seen by users.

```
location ~ /\. {
    deny  all;
    access_log off;
    log_not_found off;
}
```

#### Install
Just as ordinal. add --add-module options to configure.

```
./configure --add-module=/path/to/nginx_basic_auth_htaccess_module
make
make install
```