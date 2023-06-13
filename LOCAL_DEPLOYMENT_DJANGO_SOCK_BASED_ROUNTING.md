# Prerequisites for local django deployment Socket based routing

## Major Step:
#### Major Step 1: Code and this dependency
#### Major Step 2: Create .ini file and Ubuntu service file
#### Major Step 3: Nginx (Socket based routing)
#### Major Step 3: Domain


### Major Step 1: Code and this dependency
1. Clone the code from git (github/bitbucket/gitlab)
2. Install the dependency
3. run the celery worker (if needed)
4. run the celery beat (if needed)
5. run any other service (if needed)

### Major Step 2: Create .ini file and Ubuntu service file
1. install uwsgi **(for example: pip install uwsgi)**
2. Create a .ini file in `/etc/uwsgi/` with name `your_domain_name.ini` **(for example: nano /etc/uwsgi/your_domain_name.ini)**
3. Add the following code in the file
   ```
   [uwsgi]
   project = your_project_name # Example: project = invogo
   base = /home/your_user_name # Example: base = /home/ajitmourya
   
   chdir = %(base)/%(project)  # Example: chdir = /home/ajitmourya/invogo
   home = %(base)/.pyenv/versions/your_python_version # Example: home = /home/ajitmourya/.pyenv/versions/3.8.6/envs/invogo
   module = %(project).wsgi:application # Example: module = invogo.wsgi:application
   
   master = true
   processes = 5 # Example: no of processes (its depend on number of cpu) = 5
   
   socket = /tmp/%(project).sock # Example: socket = /tmp/invogo.sock
   chmod-socket = 666
   vacuum = true
   ```
4. Create a service file in `/etc/systemd/system/` with name `your_domain_name.service` **(for example: nano /etc/systemd/system/your_domain_name.service)**
5. Add the following code in the file
   ```
   [Unit]
   Description=uWSGI instance to serve your_project_name
   After=network.target
   
   [Service]
   User=your_user_name # Example: User=ajitmourya
   Group=www-data
   WorkingDirectory=/home/your_user_name/your_project_name # Example: WorkingDirectory=/home/ajitmourya/invogo
   Environment="PATH=/home/your_user_name/.pyenv/versions/your_python_version/bin" # Example: Environment="PATH=/home/ajitmourya/.pyenv/versions/3.8.6/bin"
   ExecStart=/home/your_user_name/.pyenv/versions/your_python_version/bin/uwsgi --ini /etc/your_domain_name.ini # Example: ExecStart=/home/ajitmourya/.pyenv/versions/3.8.6/bin/uwsgi --ini /etc/uwsgi/invogo.ini
   
   [Install]
   WantedBy=multi-user.target
   ```

6. Enable the service **(for example: sudo systemctl enable your_domain_name)**
7. Start the service **(for example: sudo systemctl start your_domain_name)**
8. Check the status of the service **(for example: sudo systemctl status your_domain_name)**
9. Restart the service **(for example: sudo systemctl restart your_domain_name)**
10. Stop the service **(for example: sudo systemctl stop your_domain_name)**


### Major Step 3: Nginx (Socket based routing)
1. Install nginx
2. Configure nginx
   1. Create a file in `/etc/nginx/sites-available/` with name `your_domain_name` **(for example: nano /etc/nginx/sites-available/your_domain_name)**
   2. Add the following code in the file
      ```
      server {
        listen 80;
        server_name your_domain_name;
        location / {
            include uwsgi_params;
            uwsgi_pass unix:/tmp/your_project_name.sock; # Example: uwsgi_pass unix:/tmp/invogo.sock;
        }
      }
      ```
   3. Create a symbolic link in `/etc/nginx/sites-enabled/` **(for example: ln -s /etc/nginx/sites-available/your_domain_name /etc/nginx/sites-enabled)**
3. Check the nginx configuration **(for example: sudo nginx -t)**
4. Restart the nginx **(for example: sudo systemctl restart nginx)**


### Major Step 4: Domain
map the domain name with your server ip address with A record in DNS server