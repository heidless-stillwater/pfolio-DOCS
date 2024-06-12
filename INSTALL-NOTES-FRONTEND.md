

############################################
# ENV
```

export GCP_PROJECT=heidless-pfolio-1
echo $GCP_PROJECT

export GCP_APP_NAME=frontend-1
echo $GCP_APP_NAME

export GCP_BUCKET=$GCP_PROJECT-bucket
echo $GCP_BUCKET

```

## init production env
```
npm install

cp .env.production.pfolio-3 .env
--
BACKEND_HOST=heidless-pfolio-1.ew.r.appspot.com
BACKEND_URL=https://heidless-pfolio-1.ew.r.appspot.com/

--

```

## docker build | run
```
#docker build . -t $GCP_APP_NAME
#docker run -p 3000:3000 -e=PORT=3000 $GCP_APP_NAME
```

## initialse gCloud
```
gcloud init

```

## storage bucket
```
# initialise BUCKET
gsutil mb -l europe-west2 gs://$GCP_BUCKET

```

## push to repository
```


nvm use 22.2.0
npm install -g npm@10.8.1

# build in artefact repository
gcloud builds submit --tag gcr.io/$GCP_PROJECT/$GCP_APP_NAME .

# deploy app from repository
gcloud run deploy $GCP_APP_NAME-svc --image gcr.io/$GCP_PROJECT/$GCP_APP_NAME

```
