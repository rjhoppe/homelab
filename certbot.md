To add a new subdomain run these cmds
```
sudo certbot certonly --nginx -d sub.domain.dev
sudo nano /etc/nginx/sites-available/sub.domain.dev
sudo certbot install --cert-name sub.domain.dev
```

NOTE: Must add a new DNS A record to your DNS provider of the subdomain first or else certbot will fail the validation challenge
