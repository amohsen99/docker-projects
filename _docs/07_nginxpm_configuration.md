Below is a concise summary of the steps to configure Nginx Proxy Manager (NPM) for an Odoo Docker container (`MEP18-Web-1:8069`) and set up WebSocket support. These steps assume NPM is already installed and accessible.



# Nginx Proxy Manager Configuration

### Summary Steps
1. **Log in to Nginx Proxy Manager**
   - Access the NPM web interface (e.g., `http://<npm-ip>:81`) with admin credentials.

2. **Add a New Proxy Host**
   - Go to "Hosts" > "Proxy Hosts" > "Add Proxy Host".
   - **Domain Names**: Enter your domain (e.g., `vm.mep.com`).
   - **Scheme**: Select `http`.
   - **Forward Hostname/IP**: Enter the Odoo container name or IP (e.g., `mep18-web-1` or Docker network IP).
   - **Forward Port**: Set to `8069`.
   - Save the proxy host.

3. **Enable WebSocket Support**
   - Edit the newly created proxy host.
   - Go to the "Advanced" tab.
   - Add the following custom configuration:
     ```
     location /websocket {
      proxy_pass http://mep18-web-1:8072;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';
      proxy_set_header Host $host;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Real-IP $remote_addr;
     }
     ```
   - Save the changes.

4. **Configure SSL (Optional)**
   - In the "SSL" tab, select "Request a new certificate" (e.g., via Let’s Encrypt).
   - Enter your domain and email, then save to enable HTTPS.

5. **Test the Configuration**
   - Access `http://vm.mep.com` (or `https://` if SSL is enabled).
   - Verify Odoo loads and WebSocket features (e.g., live chat) work.

6. **Add your local domain**:
     ```bash
          sudo nano /etc/hosts
     ```
    Examples:
       ```
       127.0.0.1    localhost
       127.0.0.1    app.local
       127.0.0.1    vm.mep.com
       ```
    Save the file (in nano: Ctrl+O, then Enter, then Ctrl+X)
---

### Notes
- **Container Name**: If `MEP18-Web-1` is the Docker container name, ensure NPM and Odoo are on the same Docker network (e.g., use `docker network ls` and `docker network connect` if needed).
- **Port**: Odoo typically runs on `8069` by default in Docker; adjust if your setup uses a different port.
- **WebSocket**: The custom config enables WebSocket support, required for Odoo features like real-time chat or notifications.
