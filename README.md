# Manage-Kubernetes-in-Google-Cloud-Challenge-Lab

### Create a GKE cluster named with the dynamic cluster name with the following configuration:
 
```
gcloud config set compute/zone us-central1-a

gcloud container clusters create <dynamic cluster name> \

--release-channel regular \

--cluster-version 1.25.6-gke.200 \

--num-nodes 3 \

--min-nodes 2 \

--max-nodes 6 \

--enable-autoscaling

```

### Enable the Prometheus managed collection on the GKE cluster.
```
gcloud container clusters update <dynamic cluster name> --enable-managed-prometheus --zone us-central1-a
 
Create a namespace on the cluster named  <dynamic namespace name>.
 
kubectl create ns <dynamic namespace name>
```

### Download a sample Prometheus app:
 
```
gsutil cp gs://spls/gsp510/prometheus-app.yaml .
```
 
### Update the <todo> sections (lines 35-38) with the following configuration.
```
containers.image: nilebox/prometheus-example-app:latest
containers.name: prometheus-test
ports.name: metrics
```

### Deploy the application onto the <dynamic namespace name> namespace on your GKE cluster.
 
```
kubectl -n <dynamic namespace name> apply -f prometheus-app.yaml
``` 
 
### Download the pod-monitoring.yaml file:
 
```
gsutil cp gs://spls/gsp510/pod-monitoring.yaml .
```

### Update the <i>todo</i> sections (lines 18-24) with the following configuration:
```
metadata.name: prometheus-test
labels.app.kubernetes.io/name: prometheus-test
matchLabels.app: prometheus-test
endpoints.interval: <dynamic interval period>
```

### Apply the pod monitoring resource onto the  <dynamic namespace name> namespace on your GKE cluster.
 
```
kubectl -n <dynamic namespace name> apply -f pod-monitoring.yaml
```

### Download the demo deployment manifest files:
``` 
gsutil cp -r gs://spls/gsp510/hello-app/ .
 ```

### Create a deployment onto the <dynamic namespace name> namespace on your GKE cluster from the helloweb-deployment.yaml manifest file. It is located in the hello-app/manifests folder.
``` 
export PROJECT_ID=$(gcloud config get-value project)
export REGION="us-central1"
cd ~/hello-app
gcloud container clusters get-credentials <dynamic cluster name> --zone us-central1-a
kubectl -n <dynamic namespace name> apply -f manifests/helloweb-deployment.yaml
```
