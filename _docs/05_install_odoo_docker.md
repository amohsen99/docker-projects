
# Install Odoo in Docker


### Steps to Deploy, Read Logs, and Stop Services with Docker Compose

1. **Create Docker Network**  
   ```bash
   docker network create lp-apps
   ```

2. **Prepare Log Directory**  
   Create and set permissions for the log directory:  
   ```bash
   sudo mkdir -p /var/log/odoo/mep18
   sudo chmod 777 /var/log/odoo/mep18
   ```

3. **Run Docker Compose**  
   Assuming the YAML is saved as `docker-compose.yml`, start services in detached mode:
   sure you did the clone steps before this
   ```bash
   cd /opt/addons_ams/docker/mep18
   docker compose up -d
   ```

4. **Utility Commands to Read Logs**  
   - **Read Docker Container Logs** (from the `web` service):  
     ```bash
     docker compose logs web
     ```
     - Add `-f` to follow logs: `docker compose logs -f web`.
   - **Read Odoo Logs from Mounted Directory** (specific to `/var/log/odoo/mep18`):  
     ```bash
     tail -f /var/log/odoo/mep18/odoo.log
     ```
     - Adjust filename if Odoo uses a different log name.

5. **Stop and Remove Services**  
   Shut down the services and remove containers:  
   ```bash
   docker compose down
   ```
   - Add `--volumes` to remove volumes too: `docker compose down --volumes`.

### Notes
- Assumes Docker (with `docker compose` plugin) is already installed from previous steps.
- Uses `docker compose` (modern v2) instead of `docker-compose`, compatible with the latest stable Docker as of February 23, 2025.
- The external network `lp-apps` persists after `down`; remove it manually if needed with `docker network rm lp-apps`.
- Paths in the YAML (e.g., `../../..`) must resolve correctly from your working directory.

