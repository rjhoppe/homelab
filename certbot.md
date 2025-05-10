Check active ssl certs
```
sudo ls -l /etc/letsencrypt/live/ 
```

To add a new subdomain run these cmds
```
sudo certbot certonly --nginx -d sub.domain.dev
sudo nano /etc/nginx/sites-available/sub.domain.dev
sudo ln -s /etc/nginx/sites-available/sub.domain.dev /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
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
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Cronjob and script to auto-renew certs:

certbot-renew.sh
```
#!/bin/bash

# Run certbot and capture output
RENEW_OUTPUT=$(certbot renew 2>&1)

# Path to your Nginx configuration file (optional check)
NGINX_CONF="/etc/nginx/nginx.conf"

# Check if any certificate was renewed
if echo "$RENEW_OUTPUT" | grep -q "Congratulations! Your certificate"; then
    echo "✅ Certificate was renewed."

    # Reload or start Nginx
    if systemctl is-active --quiet nginx; then
        echo "🔄 Nginx is running, reloading..."
        systemctl reload nginx
    else
        echo "▶️ Nginx is not running, starting..."
        systemctl start nginx
    fi

    # Send ntfy notification
    curl -H "Title: SSL Certificate Renewed" \
         -H "Tags: lock,green" \
         -d "One or more SSL certificates were renewed and Nginx has been reloaded." \
         http://127.0.0.1:8000/[YOUR-TOPIC]
else
    echo "❌ No certificates were renewed."
fi
```

Cronjob
```crontab -e``` 
```30 3 * * * /home/rhoppe/scripts/certbot-renew.sh >> /var/log/certbot-renew.log 2>&1```

Don't forget to run:
`chmod +x /home/rhoppe/scripts/certbot-renew.sh`
