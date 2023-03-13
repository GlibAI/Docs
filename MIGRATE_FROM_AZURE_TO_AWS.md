# List of the Thing which we have migrated.
1. Managed Database Postgres (Azure) --> RDS postgres (AWS)
2. Storage Account Container (Azure) --> S3 (AWS)
3. Redis
4. Storage Account Container For Static File CDN (Azure) --> S3 CDN (AWS)
5. Code
6. Domain

## Managed Database Postgres (Azure) --> RDS postgres (AWS)

--> Take a dump using pg_dump command mention below

```sh
pg_dump -U {username} -h {host} -p {port} -d {dbname}> {dbname}.sql
```

--> To upload dump to new database use this command

```sh
psql -U {username} -h {host} -p {port} -d {dbname} < {dbname}.sql
```



## Storage Account Container (Azure) --> S3 (AWS)

--> Take a media dump using from azure container, command mention below

```sh
# install azure-cli
az storage copy -s https://{account_name}.blob.core.windows.net/{container_name} -d ./{directory}/ --recursive
```

--> To upload media dump to S3 container, command mention below

```sh
# install awscli
aws s3 cp {folder_name} s3://{container_name}/{folder_name} --recursive
```
