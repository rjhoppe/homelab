Test new nginx config
```
sudo nginx -t
```

Reload new nginx config
```
sudo systemctl reload nginx
```

Create new available site
```
sudo nano /etc/nginx/sites-available/[your domain here]
```

Create symlink between new available site and sites enabled
```
sudo ln -s /etc/nginx/sites-available/portainer.rjhoppe.dev /etc/nginx/sites-enabled/
```

View logs
```
sudo cat /var/log/nginx/error.log
```

To enable websockets (specifically for ntfy), add this to the location section:
```
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "Upgrade";
```
