# Complete Nginx Setup Guide for CV Website

## 1. Install Nginx
```bash
sudo apt update
sudo apt install nginx
```

## 2. Create Website Directory Structure
```bash
# Option A: Use existing directory
# Make sure path exists and has correct permissions
sudo chmod 755 /home/mohsen/Documents/kit/docs/mohsen_website

# Option B: Create new directory in standard location
sudo mkdir -p /var/www/mycv
sudo cp -R /home/mohsen/Documents/kit/docs/mohsen_website/* /var/www/mycv/
```

## 3. Set Correct File Permissions
```bash
# If using Option A (existing directory)
sudo chown -R mohsen:www-data /home/mohsen/Documents/kit/docs/mohsen_website
sudo chmod -R 755 /home/mohsen/Documents/kit/docs/mohsen_website
sudo chmod -R g+w /home/mohsen/Documents/kit/docs/mohsen_website

# If using Option B (standard location)
sudo chown -R mohsen:www-data /var/www/mycv
sudo chmod -R 755 /var/www/mycv
sudo chmod -R g+w /var/www/mycv

# Add your user to www-data group
sudo usermod -a -G www-data mohsen
# Note: Log out and log back in for this change to take effect
```

## 4. Create Nginx Configuration
```bash
sudo nano /etc/nginx/sites-available/mycv
```

Paste the following configuration (adjust the path based on your choice in step 2):

```nginx
server {
    listen 80;
    server_name localhost;

    # Disable caching for development
    add_header Cache-Control "no-cache, no-store, must-revalidate";
    expires 0;

    # Log level for debugging
    error_log /var/log/nginx/mycv-error.log;
    access_log /var/log/nginx/mycv-access.log;

    location / {
        # Use one of these paths based on your choice in step 2
        # Option A:
        root /home/mohsen/Documents/kit/docs/mohsen_website;
        # Option B:
        # root /var/www/mycv;
        
        index index.html;
        try_files $uri $uri/ =404;
    }
}
```

## 5. Enable the Site
```bash
# Create symlink
sudo ln -s /etc/nginx/sites-available/mycv /etc/nginx/sites-enabled/

# Remove default site (optional)
sudo rm /etc/nginx/sites-enabled/default

# Test configuration
sudo nginx -t
```

## 6. Restart Nginx
```bash
sudo systemctl restart nginx
```

## 7. Verify Access Permissions
```bash
# Test if Nginx can access the files
sudo -u www-data ls -la /path/to/your/website
```

## 8. Troubleshooting

### If you get 404 errors:
1. Check file paths: `ls -la /path/to/your/website`
2. Check Nginx error logs: `sudo tail -f /var/log/nginx/mycv-error.log`
3. Verify permissions: `sudo -u www-data ls -la /path/to/your/website`

### If you can't edit files:
1. Ensure you're in the www-data group: `groups`
2. Set proper group permissions: `sudo chmod -R g+w /path/to/your/website`
3. Or change ownership back to your user: `sudo chown -R mohsen:mohsen /path/to/your/website`

### If changes don't appear after editing:
1. Clear browser cache: Press Ctrl+F5 or use incognito mode
2. Restart Nginx: `sudo systemctl restart nginx`

## 9. Maintenance

### When adding new files:
```bash
# Ensure new files have correct permissions
sudo chown mohsen:www-data /path/to/your/website/new_file
sudo chmod 664 /path/to/your/website/new_file
```

### When updating the site:
```bash
# After making changes, you may need to restart Nginx
sudo systemctl restart nginx
```
