# .htpasswd
```
# Disallow all dot files
location ~ /\. {
    deny all;
    access_log off;
    log_not_found off;
}

# Restricted
location /secret {
    auth_basic "Restricted";
    auth_basic_user_file /path/to/.htpasswd;
}
```
