# Nginx Setup for Local Development

## Basic Setup
1. Install Nginx: `sudo apt install nginx`
2. Create a site configuration in `/etc/nginx/sites-available/mycv`

## Configuration File
```nginx
server {
    listen 80;
    server_name localhost;

    # Disable caching for development
    add_header Cache-Control "no-cache, no-store, must-revalidate";
    expires 0;

    location / {
        root /path/to/your/mohsen_website;
        index index.html;
        try_files $uri $uri/ =404;
    }
}
```

## Enable the Site
1. Create symlink: `sudo ln -s /etc/nginx/sites-available/mycv /etc/nginx/sites-enabled/`
2. Test config: `sudo nginx -t`
3. Restart Nginx: `sudo systemctl restart nginx`

## Troubleshooting
If changes still don't appear:
1. Clear browser cache or use incognito mode
2. Check file permissions: `sudo chown -R www-data:www-data /path/to/your/mohsen_website`
3. Check error logs: `sudo tail -f /var/log/nginx/error.log`