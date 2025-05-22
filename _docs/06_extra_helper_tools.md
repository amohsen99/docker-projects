# Install Extra Helper Tools

**Install Portainer-ce**
   ```bash
   cd /opt/addons_ams/docker/portainer
   docker compose up -d
   ```
    - portainer-url: http://<srv-ip>:9000
    - username: a.badr@laplacesoftware.com
    - password: LP_P@ssw0rd

**Install PgAdmin4**
   ```bash
   cd /opt/addons_ams/docker/pgadmin
   docker compose up -d
   ```
    - portainer-url: http://<srv-ip>:5050
    - username: a.badr@laplacesoftware.com
    - password: LP_P@ssw0rd

**Install Nginx Proxy Manager (NPM)**
   ```bash
   cd /opt/addons_ams/docker/ngnixpm
   docker compose up -d
   ```
    - portainer-url: http://<srv-ip>:81
    - username: a.badr@laplacesoftware.com
    - password: LP_P@ssw0rd

### Notes
    TODO change passwords in production server 
