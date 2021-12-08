#### nginx
```
/etc/nginx/conf.d/myblog.conf

nginx -s reload

sudo apt install certbot python3-certbot-nginx
certbot --nginx
certbot renew --dry-run
```