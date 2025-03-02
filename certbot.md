To add a new subdomain run these cmds
```
sudo certbot certonly --nginx -d sub.domain.dev
sudo nano /etc/nginx/sites-available/sub.domain.dev
sudo ln -s /etc/nginx/sites-available/sub.domain.dev /etc/nginx/sites-enabled/
sudo certbot install --cert-name sub.domain.dev
```

NOTE: Must add a new DNS A record to your DNS provider of the subdomain first or else certbot will fail the validation challenge

Bare bones NGINX config (before certbot install)

```
server {
    listen 80;
    listen [::]:80;

    server_name <subdomain.domain.com>;
    allow <your IP address>;
    deny all;

    location / {
        proxy_pass http://localhost:0000; # Replace with your application's address
        proxy_set_header Host $host;
        proxy_set_header X-REAL-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $schema;
    }
}
```
