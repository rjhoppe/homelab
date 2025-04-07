Get the IP address for given lxc container
```
lxc-info -n [YOUR LXC CONTAINER ID]
```

Check if a port is open or being used
```
lsof | grep [YOUR PORT NUMBER]
```

Start portainer
```
sudo docker run -d -p 127.0.0.1:9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce
```

Get WireGuard (wg-easy) Docker location
```
/docker/wg-easy
```

Run wg-easy
```
docker-compose up -d
```
