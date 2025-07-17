# Project: Nginx Reverse Proxy Configuration

# Step 1: Initialize Git Repository
mkdir nginx-reverse-proxy
cd nginx-reverse-proxy
git init

# Step 2: Create Directory Structure
mkdir -p nginx/sites-available

# Step 3: Create Nginx Reverse Proxy Config
cat > nginx/sites-available/stack.inndata.internal <<EOF
server {
    listen 80;
    server_name stack.inndata.internal;

    location / {
        proxy_pass http://10.10.5.54:5100;
        proxy_http_version 1.1;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host \$host;
        proxy_cache_bypass \$http_upgrade;
    }
}
EOF

# Step 4: Create README
cat > README.md <<EOF
# Nginx Reverse Proxy Configuration

## Setup Steps:

1. Copy config to /etc/nginx/sites-available/
2. Link it:
   sudo ln -s /etc/nginx/sites-available/stack.inndata.internal /etc/nginx/sites-enabled/
3. Test and reload Nginx:
   sudo nginx -t
   sudo systemctl reload nginx

Backend: 10.10.5.54:5100
Proxy Name: stack.inndata.internal
EOF

# Step 5: .gitignore
echo "*.swp" > .gitignore

# Step 6: Git Commit
git add .
git commit -m "Add Nginx reverse proxy configuration"

# Final Structure:
# nginx-reverse-proxy/
# ├── nginx/sites-available/stack.inndata.internal
# ├── README.md
# └── .gitignore
