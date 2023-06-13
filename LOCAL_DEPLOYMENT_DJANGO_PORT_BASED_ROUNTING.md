# Prerequisites for local django deployment Port based routing

## Major Step:
#### Major Step 1: Code and this dependency
#### Major Step 2: Nginx (Port based routing)
#### Major Step 3: Domain


### Major Step 1: Code and this dependency
1. Clone the code from git (github/bitbucket/gitlab)
2. Install the dependency
3. run the server with port (8000/your choice)
4. run the celery worker (if needed)
5. run the celery beat (if needed)
6. run any other service (if needed)

### Major Step 2: Nginx (Port based routing)
You can run multiple application in different port and add the port in nginx configuration file with domain name

1. Install nginx
2. Configure nginx
   1. Create a file in `/etc/nginx/sites-available/` with name `your_domain_name` **(for example: nano /etc/nginx/sites-available/your_domain_name)**
   2. Add the following code in the file
      ```
      server {
        listen 80;
        server_name your_domain_name;
        location / {
            proxy_pass http://localhost:8000;
        }
      }
      ```
   3. Create a symlink in `/etc/nginx/sites-enabled/` with name `your_domain_name`  **(for example: ln -s /etc/nginx/sites-available/your_domain_name /etc/nginx/sites-enabled/your_domain_name)**
3. Check the nginx configuration **(for example: sudo nginx -t)**
4. Restart nginx **(for example: sudo service nginx restart)**

#### Major Step 3: Domain
map the domain name with your server ip address with A record in DNS server