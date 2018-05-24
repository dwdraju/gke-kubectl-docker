# kubectl docker integrated with GKE
* Light weight Alpine Linux image based
* Easy integration with Google Cloud service account
* Can be used on CI-CD pipeline

## GCloud Authentication
Pull image from Docker Hub:
```
docker pull dwdraju/gke-kubectl-docker:latest
``` 
#### Browser Authentication
To authenticate the gcloud sdk docker image using browser authentication, run
```
docker run -ti --name gsdocker dwdraju/gke-kubectl-docker gcloud auth login
```

#### Service Account Authentication
* Create service account from *IAM & admin*
* Download the key file in json format and paste it on service-account-key.json
* Give permission to the service account as per requirement
* Get the *GCLOUD_PROJECT_ID* and set it as environment variable: `export GCLOUD_PROJECT_ID=myprojectID`

Run the command:
```
docker run -ti -v $(pwd)/service-account-key.json:/service-account-key.json --name gke-kubectl dwdraju/gke-kubectl-docker gcloud auth activate-service-account --key-file=/service-account-key.json  --project "$GCLOUD_PROJECT_ID" --quiet
```

Additional flags like `ACCOUNT_NAME`, `--prompt-for-password` can be used during authentication. Full list is on [Cloud SDK doc](https://cloud.google.com/sdk/gcloud/reference/auth/activate-service-account)

## Set Cluster Credentials
* Set cluster name: `export CLUSTER=mycluster`
* Set zone of cluster: `export CLUSTER_ZONE=zone`

```
docker run --rm -ti --volumes-from gke-kubectl dwdraju/alpine-gcloud gcloud container clusters get-credentials CLUSTER --zone CLUSTER_ZONE
```

## Run kubectl Command
```
docker run --rm -ti --volumes-from gke-kubectl dwdraju/alpine-gcloud kubectl get pods
```
