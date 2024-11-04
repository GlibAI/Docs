# Application Deployment Using Docker
1. **Docker Command for Build with tag (re-tag),Save,Load image**
2. **Create Docker Network**
2. **Run Docker For Database**
3. **Run Docker For Redis**
4. **Run Docker For ML Model**
5. **Run Docker For One-time Application Migration**
6. **Run Docker For Application (Web & Worker)**
7. **Run Docker For Nginx (If required)**

**NOTE: If docker command raise issue then use sudo**

### Docker Command for Build,Save,Load image
```ssh
docker build -t tage_name .
docker image tag source_tage_name target_tage_name
docker save tage_name > tage_name.tar
docker load --input tage_name.tar
```

### Create Docker Network
```ssh
docker network create application_network
```

### Run Docker For Database
```ssh
docker run -d --name postgres --network application_network -e POSTGRES_USER={{DB_USERNAME}} -e POSTGRES_PASSWORD={{DB_PASSWORD}} -e POSTGRES_DB={{DB_NAME}} -e PGDATA=/var/lib/postgresql/data/pgdata -v ./postgresql/data:/var/lib/postgresql/data/pgdata postgres:14
```

### Run Docker For Redis
```ssh
docker run -d --name redis --network application_network redis:7
```

### Run Docker For ML Model
```ssh
docker run -d --name layoutlmv2 --network application_network --env-file ./.env -v ./layoutlmv2/model:/opt/ml/model entities:latest
docker run -d --name detr --network application_network --env-file ./.env -v ./detr/model:/opt/ml/model table:latest
```

### Run Docker For One-time Application Migration
```ssh
docker run --rm --name application_migrate --network application_network --link postgres --link redis --env-file ./.env application:latest python manage.py migrate_schemas
docker run --rm --name application_public --network application_network --link postgres --link redis --env-file ./.env application:latest python manage.py make_public_tenant
```

### Run Docker For Application (Web & Worker)
```ssh
docker run -d --name application_webserver --network application_network --link postgres --link redis --env-file ./.env --entrypoint ./custom_entrypoint.sh -v ./custom_entrypoint.sh:/home/application/custom_entrypoint.sh -v ./media:/home/application/media -v ./static:/home/application/static -p 8000:8000 application:latest
```

#### Custom Entrypoint (name: custom_entrypoint.sh)
```ssh
#!/bin/bash
python manage.py runserver 0.0.0.0:8000 & celery -A application_project worker -l INFO -c 2
```

### Run Docker For Nginx (If required)
```ssh
docker run -d --name nginx_server --network application_network -v ./nginx.conf:/etc/nginx/nginx.conf -v ./certs:/etc/nginx/certs/ -v ./static:/etc/nginx/static/ -p 443:443 nginx:latest
```

#### Nginx Conf (name: nginx.conf)
```ssh
events {
    worker_connections 1024;
}

http {
    upstream django {
        server application_webserver:8000;
    }
 
    server {
        listen 443 ssl;
        server_name {{tenant_domain}};

        ssl_certificate /etc/nginx/certs/glib/certificate.crt;
        ssl_certificate_key /etc/nginx/certs/glib/private.key;
        
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers HIGH:!aNULL:!MD5;
        
        #location /static/ {
        #    alias /etc/nginx/static/;
        #}

        location / {
            proxy_pass http://application_webserver:8000;
            # proxy_set_header Host {{tenant_domain}}; (if required)
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
        client_max_body_size 250M;
    }
 
    
    server {
        listen 443 ssl;
        server_name {{admin_domain}};

        ssl_certificate /etc/nginx/certs/admin/certificate.crt;
        ssl_certificate_key /etc/nginx/certs/admin/private.key;
        
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers HIGH:!aNULL:!MD5;
        
        #location /static/ {
        #    alias /etc/nginx/static/;
        #}

        location / {
            proxy_pass http://application_webserver:8000;
            # proxy_set_header Host {{admin_domain}}; (if required)
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
        client_max_body_size 250M;
    }
}
```