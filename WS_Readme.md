# Websocket Readme

Read this link to learn about websockets and how they are implimented in this project, since the framework for this is based off of the codemaxxer repository: https://codemaxxers.github.io/codemaxxerblog/2024/05/20/Websocket-documentation.html

## NGINX CONFIGURATION

```

server {
    server_name ______; # Change server name to the one on R53

    # Configure CORS Headers for typical HTTP requests
    location / {
        proxy_pass http://localhost:____; # Change port to port on Docker
        proxy_http_version 1.1;  # Ensures HTTP/1.1 is used, which is necessary for WebSocket upgrades
        proxy_set_header Upgrade $http_upgrade; # Required for WebSocket
        proxy_set_header Connection $connection_upgrade; # Required for WebSocket
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Simple requests
        if ($request_method ~* "(GET|POST|PUT|DELETE)") { # Customize Request methods based on your needs
        }
        # Preflighted requests 
        if ($request_method = OPTIONS ) {
                add_header "Access-Control-Allow-Credentials" "true";
                add_header "Access-Control-Allow-Origin"  "Origin";
                add_header "Access-Control-Allow-Methods" "GET, POST, PUT, DELETE, OPTIONS, HEAD"; # Make sure the request methods above match here
                add_header "Access-Control-Allow-Headers" "Authorization, Origin, X-Requested-With, Content-Type, Accept";
                return 200;
        }
    }

    listen [::]:443 ssl; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/codemaxxerlink.stu.nighthawkcodingsociety.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/codemaxxerlink.stu.nighthawkcodingsociety.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
    if ($host = codemaxxerlink.stu.nighthawkcodingsociety.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

    listen 80;
    listen [::]:80;
    server_name codemaxxerlink.stu.nighthawkcodingsociety.com;
    return 404; # managed by Certbot
}

# Necessary for WebSocket upgrade
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}


```