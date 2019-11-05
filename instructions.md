
# Docker  workshop

clone the repo
```
git clone https://github.com/npatta01/simple-workshop.git
cd simple-workshop
```

set some variables
```
GCP_PROJECT_ID=np-public-training
APP_NAME=image-classifier
```

build the image (approach 1)
```
CONTAINER_URL=gcr.io/${GCP_PROJECT_ID}/${APP_NAME}-${USER}
docker build --tag $CONTAINER_URL .
```
push the image to google container registry
```
docker push $CONTAINER_URL
```


build the image (approach 2)
gcloud builds submit --tag $CONTAINER_URL .


## Kubernetes Workshop 

Get credentials of cluster 
```
gcloud container clusters get-credentials walmart-test --zone us-central1-a --project np-public-training
```

Create your own namespace
```
kubectl create namespace $USER
```

Create a deployment / replication controller 
```
kubectl --namespace $USER run $APP_NAME --image=$CONTAINER_URL --port=5000 --replicas 2
```

kubectl -n $USER get pods 


kubectl -n $USER expose deployment $APP_NAME  --port=80 --target-port=5000 --type="LoadBalancer"

```
kubectl -n $USER get svc -w 
```



## Helm section
```
wget https://get.helm.sh/helm-v2.15.2-linux-amd64.tar.gz
tar -zxvf helm-v2.15.2-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
helm init --client-only
```


install helm
```
helm install --namespace $USER --name neo4j-$USER stable/neo4j --set acceptLicenseAgreement=yes --set neo4jPassword=mySecretPassword
 ```

install neo4j 
```
helm install --namespace $USER  --name "airflow-$USER" stable/airflow
```



# Admin
````
kubectl --namespace kube-system create serviceaccount tiller

kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

helm init --service-account tiller --upgrade
```


