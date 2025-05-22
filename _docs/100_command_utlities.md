

# Common Commands Utilities

### Upgrade Odoo module from docker container
```bash
python3 /usr/bin/odoo --db_host=postgres-pg-db-1 --db_port=5432 --db_user=odoo --db_password=admin --http-port=8090 --xmlrpc-port=8090 --gevent-port=8091 --addons-path=/usr/lib/python3/dist-packages/odoo/addons,/mnt/odoo18/addons_thp/muk_web_theme,/mnt/odoo18/addons_thp/lp,/mnt/extra-addons --logfile=/var/log/odoo/odoou.log --log-handler=:DEBUG --stop-after-init -d MEP_18_2025 -u all
```

```bash
python3 /usr/bin/odoo --db_host=postgres-pg-db-1 --db_port=5432 --db_user=odoo --db_password=LP_P@ssw0rd --http-port=8090 --xmlrpc-port=8090 --gevent-port=8091 --addons-path=/usr/lib/python3/dist-packages/odoo/addons,/mnt/odoo18/addons_thp/muk_web_theme,/mnt/odoo18/addons_thp/lp,/mnt/extra-addons --logfile=/var/log/odoo/odoou.log --log-handler=:DEBUG --stop-after-init -d MEP_18_2025 -u all
```




### SSL / Local domain / Self-Signed Certificate
```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout odoo.gvitt.local.key \
-out odoo.gvitt.local.crt
    
sudo chown abadr:abadr odoo.gvitt.local.crt odoo.gvitt.local.key
sudo chown abadr:abadr odoo.gvitt.local.crt odoo.gvitt.local.crt
```
    
    

## Docker Commands Examples
### Container Management
- List running containers: `docker ps`
- List all containers: `docker ps -a`
- Run a container: `docker run -d --name mep18-web-1 -p 8069:8069 mep18-web-1`
- Stop a container: `docker stop mep18-web-1`
- Start a container: `docker start mep18-web-1`
- Remove a container: `docker rm mep18-web-1`

### Terminal Access
- Run terminal in running container: `docker exec -it mep18-web-1 bash`
- Run terminal in new container: `docker run -it mep18-web-1 bash`
- Run a command in terminal: `docker exec -it mep18-web-1 odoo --help`


```bash
sudo systemctl restart docker
sudo docker update --restart=no 2d3fd235536f
docker image ls

sudo mkdir -p /var/log/odoo/mep18
sudo chmod 777 /var/log/odoo/mep18
   
docker exec -it mep18-web-1 /bin/bash
docker exec -it mep18-web-1 cat /var/log/odoo/odoo.log
docker exec -it mep18-web-1 /bin/bash -c "ls /mnt/odoo18"
docker exec -it mep18-web-1 /bin/bash -c "ls /mnt/extra-addons"

docker exec mep18-web-1 pip show pandas

docker exec -it mep18-web-1 ls -l /usr/lib/python3/dist-packages
sudo iptables -L INPUT -n --line-numbers | grep -E '(80|443)'
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

sudo chown -R ubuntu:ubuntu /opt
sudo chown -R it:it /opt
sudo chmod -R 777 /opt
sudo chmod -R 755 /opt
- move user ubuntu to root group
sudo usermod -g root ubuntu
- Extract file
tar -xzvf odoo_17.0+e.latest.tar.gz
mv odoo-17.0+e.latest odoo17e

```
-------------------------------


### Image Management
- List images: `docker images`
- Pull an image: `docker pull mep18-web-1`
- Remove an image: `docker rmi mep18-web-1`

### Logs and Monitoring
- View Odoo logs: `docker logs mep18-web-1`
- Follow Odoo logs in real-time: `docker logs -f mep18-web-1`
- Inspect container: `docker inspect mep18-web-1`

### Network and Volume
- List networks: `docker network ls`
- Connect to network: `docker network connect my-network mep18-web-1`
- List volumes: `docker volume ls`

### Cleanup
- Remove stopped containers: `docker container prune`
- Remove unused images: `docker image prune`

