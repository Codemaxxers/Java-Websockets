## Recent Changes
[Upgrading old dependencies to newest versions.](https://github.com/nighthawkcoders/spring_portfolio/issues/12)

# [Spring Portfolio Starter](https://nighthawkcodingsociety.com/projectsearch/details/Spring%20Portfolio%20Starter)

- Runtime link: https://spring.nighthawkcodingsociety.com/
- JWT Login: https://nighthawkcoders.github.io/APCSA/data/login
- Jokes endpoint: https://spring.nighthawkcodingsociety.com/api/jokes/



## Visual thoughts

- Starter code should be fun and practical
- Organize with Bootstrap menu
- Add some color and fun through VANTA Visuals (birds, halo, solar, net)
- Show some practical and fun links (hrefs) like Twitter, Git, Youtube
- Show student project specific links (hrefs) per page
- Show student About me pages

## Getting started

- Clone project and open in VSCode
- Verify Project Structure to use a good Java JDK (adoptopenjdk:17)
- Play or entry point is Main.java, look for Run option in code.  This eanbles Spring to load
- Java source (src/main/java/...) has Java files.  Find "controllers" path, these files enable HTTP route and HTML file relationship.
- HTML source (src/main/resources/...) had templates and supporting files.  Find index.html as this file is launched by defaul in Spring.  Other HTML files are loaded by building an "@Controller"

## IDE management

- A ".gitignore" can teach a Developer a lot about Java runtime.  A target directory is created when you press play button, byte code is generated and files are moved into this location.
- "pom.xml" file can teach you a lot about Java dependencies.  This is similar to "requirements.txt" file in Python.  It manages packages and dependencies.


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