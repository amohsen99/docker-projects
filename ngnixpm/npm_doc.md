# Configuring Nginx Proxy Manager for Odoo

This guide explains how to configure Nginx Proxy Manager (NPM) to work with Odoo based on the official Odoo Nginx configuration.

## Initial Login

1. Access NPM admin interface at `http://localhost:81`
2. Use default credentials:
   - Email: `admin@example.com`
   - Password: `changeme`
3. After first login, you will be prompted to:
   - Change your password
   - Update your email address

## Setting Up Local Domain

### 1. Edit Hosts File

1. Open terminal and edit the hosts file:

   ```bash
   sudo nano /etc/hosts
   ```
2. Add your local domain:
Examples:
   ```
   127.0.0.1    localhost
   127.0.0.1    app.local
   127.0.0.1    ams18.app.local
   127.0.0.1    mep-vm.laplace-sys.com
   ```
3. Save the file (in nano: Ctrl+O, then Enter, then Ctrl+X)

### 2. Testing Local Domain

1. Ping your domain to verify it resolves to localhost:
   ```bash
   ping app.local
   docker exec -it ngnixpm-npm-1 curl http://mep18-web-1:8069
   docker exec -it alfakhera18-web-1 bash
   curl http://alfakhera.laplace-sys.com
   curl http://mep-vm.laplace-sys.com
   
   wscat -c ws://localhost:7072/websocket
   
   curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" http://ams18.app.local:8072
   curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" http://alfakhera.laplace-sys.com/websocket
   ```

   Should return: `64 bytes from 127.0.0.1: icmp_seq=1...`

### 3. SSL for Local Development (Optional)

1. In NPM, go to "SSL Certificates"
2. Click "Add SSL Certificate"
3. Select "Self-Signed Certificate" (NOT Let's Encrypt)
   - Let's Encrypt doesn't work with .local domains
   - Self-signed is perfect for local development
4. Fill in:
   - Domain Names: app.local, ams18.app.local
   - Click "Save"

**Important SSL Notes**:

- Do NOT use Let's Encrypt for local domains (.local, .test, .localhost)
- Self-signed certificates will show security warnings in browsers
- You'll need to add security exceptions in your browser
- For production domains (like example.com), use Let's Encrypt instead

## Configuring AMS18 Instance

### 1. Main Proxy Host (HTTP/HTTPS)

1. In NPM, go to "Proxy Hosts" → "Add Proxy Host"
2. Configure:

**Details Tab:**

- Domain Names: ams18.app.local
- Scheme: http
- Forward Hostname/IP: mep18-web-1
- Forward Port: 8069
- Enable WebSocket Support: Yes

**SSL Tab:**

- SSL Certificate: Select your self-signed certificate
- Force SSL: Yes
- HTTP/2 Support: Yes

**Advanced Tab:**


```nginx
# Enable gzip compression
gzip on;
gzip_min_length 1000;
# TODO:change mep18-web-1 based on your container name
location / {
    # Proxy Headers for Odoo
    proxy_set_header X-Forwarded-Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Real-IP $remote_addr;

    # Disable redirects from Odoo
    proxy_redirect off;
    proxy_pass http://mep18-web-1:8069;

    # Cookie settings for secure session handling
    proxy_cookie_flags session_id samesite=lax secure;

    # Optional - Add SSL/TLS-related headers if using HTTPS (to be added later if needed)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains";
}

location /websocket {
    proxy_pass http://mep18-web-1:7072;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Real-IP $remote_addr;
}
```

## Testing the Setup

1. Access your AMS18 instance:

   - Main application: https://ams18.app.local
   - The WebSocket and longpolling will be handled automatically through the same domain
2. Verify WebSocket Connection:

   - Open browser developer tools (F12)
   - Go to Network tab
   - Filter by "WS" to see WebSocket connections
   - You should see successful WebSocket connections to /websocket and /longpolling

## Troubleshooting

1. If you can't access the domain:

   - Verify hosts file entries: `cat /etc/hosts`
   - Try flushing DNS: `sudo dscacheutil -flushcache`
2. If WebSocket isn't connecting:

   - Check NPM logs: Access through NPM interface
   - Verify ports are exposed in docker-compose.yml
   - Ensure proxy_mode is enabled in odoo.conf
3. SSL Certificate Issues:

   - Make sure you're using a self-signed certificate, not Let's Encrypt
   - Accept the self-signed certificate in your browser
   - You may need to accept it separately for WebSocket connections
   - In Chrome, you might need to type "thisisunsafe" while focused on the security warning page
