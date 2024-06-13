
## [Cloud SQL(MySQL, PostgreSQL and Microsoft SQL Server) automatically backup and stores into the GCS bucket](https://medium.com/google-cloud/cloud-sql-mysql-postgresql-and-microsoft-sql-server-automatically-backup-and-stores-into-the-gcs-d01cf3677c67)

#######################
####### Heidless pfolio

```
gcloud init
--
heidless-pfolio-1
heidlessemail05@gmail.com
heidless-pfolio-1-backend-0-instance-0
europe-west2-a
heidless-pfolio-1-bucket

--
```
## TAKE BACKUP
```
--
# set permissoons on INSTANCE service account
export SQL_SVC_ACC=`gcloud sql instances describe ${GCP_INSTANCE} | grep serviceAccountEmailAddress`
echo $SQL_SVC_ACC

--
serviceAccountEmailAddress: p449207231494-mxctja@gcp-sa-cloud-sql.iam.gserviceaccount.com

--
echo '#####################################################'
echo 'SET PERMISSIONS - objectAdmin & legacyBucketWriter'

gsutil iam ch serviceAccount:p449207231494-mxctja@gcp-sa-cloud-sql.iam.gserviceaccount.com:objectAdmin \
gs://heidless-pfolio-1-bucket

gsutil iam ch serviceAccount:p449207231494-mxctja@gcp-sa-cloud-sql.iam.gserviceaccount.com:legacyBucketWriter \
gs://heidless-pfolio-1-bucket

--

echo '######################'
echo 'set ENV'
source config/.env-vars

timestamp=`date +%s`
MSG=2nd-DRAFT

echo PROJECT: $GCP_PROJECT
echo INSTANCE: $GCP_INSTANCE
echo DB: $GCP_DB_NAME
echo USER: $GCP_DB_USER
echo BUCKET: $GCP_BUCKET

export BK_COMMENT='LatestSnapshot'
echo COMMENT: $BK_COMMENT

export BK_TIMESTAMP=`date +%s`
echo TIMESTAMP: $BK_TIMESTAMP

export GCP_FILE=${GCP_INSTANCE}-${BK_COMMENT}-${BK_TIMESTAMP}.gz
echo FILE: $GCP_FILE

echo " "

echo '######################'
echo 'BACKUP INSTANCE'
echo $GCP_INSTANCE
echo $GCP_BUCKET
echo $GCP_FILE
echo $GCP_DB_NAME
echo ' '
gcloud sql export sql ${GCP_INSTANCE} ${GCP_BUCKET}/backups/${GCP_FILE}    \
--database=${GCP_DB_NAME}	 \
--offload

```

# DOWNLOAD BACKUP
cd <BACKUP DIR>
echo $GCP_BUCKET
echo $GCP_FILE
gcloud storage cp ${GCP_BUCKET}/backups/${GCP_FILE} .

# RE-CREATE DB
echo 're-create DB'
echo $GCP_DB_NAME
echo $GCP_INSTANCE
echo ' '
gcloud sql databases delete $GCP_DB_NAME \
    --instance $GCP_INSTANCE

gcloud sql databases create $GCP_DB_NAME \
    --instance $GCP_INSTANCE

# UPLOAD BACKUP
echo $GCP_BUCKET
echo $GCP_FILE
gcloud storage cp ${GCP_FILE} ${GCP_BUCKET}/backups/${GCP_FILE}

# RESTORE BACKUP
gcloud sql import sql ${GCP_INSTANCE} ${GCP_BUCKET}/backups/${GCP_FILE} \
--database=${GCP_DB_NAME}

# DOWNLOAD ALL IMAGES
echo GCP_IMAGE_DIR: ${GCP_IMAGE_DIR}
echo GCP_ICON_DIR: ${GCP_ICON_DIR}
echo " "
gsutil cp -r ${GCP_IMAGE_DIR} .
gsutil cp -r ${GCP_ICON_DIR} .

# UPLOAD ALL IMAGES
cd <BACKUP DIR>
cd images
echo ${GCP_IMAGE_DIR}
gsutil cp -r * ${GCP_IMAGE_DIR}
cd ..

cd icons
echo ${GCP_ICON_DIR}
gsutil cp -r * ${GCP_ICON_DIR}
cd ..



