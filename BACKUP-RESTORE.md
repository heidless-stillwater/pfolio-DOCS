
## [Cloud SQL(MySQL, PostgreSQL and Microsoft SQL Server) automatically backup and stores into the GCS bucket](https://medium.com/google-cloud/cloud-sql-mysql-postgresql-and-microsoft-sql-server-automatically-backup-and-stores-into-the-gcs-d01cf3677c67)



```heidless-pfolio-0-backend-0-user-0	
gcloud iam service-accounts list
--
520855347094-compute@developer.gserviceaccount.com	

--

##########################
# ERROR: (gcloud.sql.export.sql) HTTPError 403: The service account does not have the required permissions for the bucket.
--
FIX: Export instance via Console to add permission. Then CLI works fine

GOOGLE Console->SQL-><INSTANCE>->Export

--
##########################
```

# Ruby Portfolio
```
gcloud init
--
heidless-ror-0
heidlessemail05@gmail.com
heidless-ror-0
europe-west2-a
--
```
## alpha-blog
```
gcloud sql export sql alpha-blog-0-instance-0 gs://heidless-ror-0-alpha-blog-0-bucket-0/backups/alpha-blog-0-instance-0.gz \
--database=alpha-blog-0-db-0 \
--offload
```

# Heidless pfolio

```
gcloud init
--
pfolio-3
heidlessemail05@gmail.com
pfolio-3
europe-west2-a
--
```
## 
```
gcloud sql export sql  pfolio-3-instance-0 gs://pfolio-3-bucket/backups/pfolio-3-instance-0.gz \
--database=pfolio-3-db-0	 \
--offload

```
















#############################








## [pdadmin4](http://localhost/pgadmin4/browser/)
```
rob.lockhart@yahoo.co.uk
postgres

```

# MIGRATE 

## export CLOUD STORAGE
```
cd backend/BACKUPS/25-4-2024/pfolio-2/storage

gsutil cp -r gs://pfolio-2-bucket .
```

## export DB instance

## import/restore DB
```
gcloud sql instances describe pfolio-2-instance-0 | grep serviceAccountEmailAddress
--
serviceAccountEmailAddress:
p434737793407-tfz9r3@gcp-sa-cloud-sql.iam.gserviceaccount.com
--

--

gsutil iam ch serviceAccount:p434737793407-tfz9r3@gcp-sa-cloud-sql.iam.gserviceaccount.com:objectAdmin \
gs://pfolio-2-bucket

gcloud sql export sql pfolio-2-instance-0 gs://pfolio-2-bucket/backups/pfolio-2-sqldumpfile.gz \
--database=pfolio-2-db-0 \
--offload

# download: e.g. to 'backend/BACKUPS/25-4-2024/pfolio-2-sqldumpfile.gz'

  



SQL-> pfolio-2-instance-0->Import

gsutil iam ch serviceAccount:pfolio-2@appspot.gserviceaccount.com:objectAdmin \
gs://pfolio-2-bucket
  


gcloud sql import sql pfolio-2-instance-0 gs://pfolio-2-bucket/backups/Cloud_SQL_Export_2024-04-25 (08:54:45).sql --database=pfolip-2-db-0











### take backup
```
pfolio-0-instance-0->Export
--
SOURCE:
pfolio-0-db-0

DESTINATION:
pfolio-0-bucket/backups

DOWNLOAD:
backend/BACKUPS/25-4-24/db-export

--
```

## migrate backup

- ### initialise an instance
```
 pfolio-2-instance-0

```
- ### load backup
```

```














## initialise App Engine
```
gcloud app create
--
pfolio-2@appspot.gserviceaccount.com
--

# add ROLES to allow access to DB & 'secrets'
--
Secret Manager Secret Accessor
Cloud SQL Admin
Storage Admin
--

```

## ESSENTIAL: set CLOUD vars
```
source ./config/.env-vars
source ./.env-vars

```

## enable cloud proxy
```
cd config

```

```
# init proxy
cd config
#curl -o cloud-sql-proxy https://storage.googleapis.com/cloud-sql-connectors/cloud-sql-proxy/v2.6.1/cloud-sql-proxy.linux.amd64

chmod 777 cloud-sql-proxy

```

```
./cloud-sql-proxy --credentials-file ./$GCP_CREDENTIALS \
--port 1234 $GCP_PROJECT:$GCP_REGION:$GCP_INSTANCE

```

```

# kill & restart - IF address already in use
sudo lsof -i -P -n | grep LISTEN
kill -9 <PID>

```



gs://pfolio-2-bucket/backups/Cloud_SQL_Export_2024-04-25 (08:54:45).sql

```

## check if can access DB directly
```
gcloud sql connect $GCP_INSTANCE --database $GCP_DB_NAME --user=postgres --quiet

--
password:
postgres

```
















##############################
# TAKE BACKUP
- ### DB
    - ### [pg_dump](https://www.postgresql.org/docs/current/app-pgdump.html)
    - ### [Export and import using pg_dump, pg_dumpall, and pg_restore](https://cloud.google.com/sql/docs/postgres/import-export/import-export-dmp)

```
pgadmin4->pfoli0-0-gcp->pfolio-0-db-0->Tools->Backup
--
filename: backups/<BACKUP NAME>
format: Custom
encoding: UTF8
role name: pfolio-0-user-0
--

# writes backup file to pgadmin->Tools->Storage Manager
##  download & store 'backend/BACKUPS/<BACKUP_NAME>'

# store in LINUX instance
cp /mnt/c/Users/heidless/Downloads/pfolio-0-backup-2 /home/heidless/projects/PFOLIO/pfolio-exemplar/portfolio-website-customization/backend/BACKUPS

```

- ### FILES
    - ### [gcloud storage cp](https://cloud.google.com/sdk/gcloud/reference/storage/cp)

```
#gsutil cp -r gs://bucket/folder .

gsutil cp -r gs://pfolio-0-bucket .

```

## RESTORE BACKUP
```
# backups stored at 'backend/BACKUPS'

# copy BACKUP to Downloads
cp \
/home/heidless/projects/PFOLIO/pfolio-exemplar/portfolio-website-customization/backend/BACKUPS/pfolio-0-backup-2 \
/mnt/c/Users/heidless/Downloads/pfolio-0-backup-2 

# create new(blank) DB i.e. pfolio-restore-tst-0
Database->Create->Database
<DB_NAME>

# initiate backups
pgadmin-><DB_NAME>->Tools->Restore
--
format: Custom
filename: .../upload -> Downloads/pfolio-0-backup-2
role name: pfolio-0-user-0
--

```

## Restore to New Project

### Generate new Project
```
pfolio-1

```

### backup 'config'
```
cp -rf config config-pfolio-0

```