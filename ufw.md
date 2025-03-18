Example of adding an allow rule (NOTE: the inclusion of the protocol at the end for your port)
```
sudo ufw allow from 127.0.0.1 to any port 80 proto tcp
```

Delete a specific rule
```
sudo ufw delete deny 80/tcp
```

Show all ufw rules and assign a numeric value to each rule
```
sudo ufw status numbered
```

Delete a rule by number (from the previous command output)
```
sudo ufw delete 4
```

Reload ufw after adding/deleting any rules
```
sudo ufw reload
```

Allow from any to a specific port (for testing only)
```
sudo ufw allow from any to [port]/[protocol]
```

EX:
```
sudo ufw allow from any to 64738/udp
```
