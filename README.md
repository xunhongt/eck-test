### Notes 

#### 0. Deployment - ECK Operator

```
kubectl apply -f crds.yaml

kubectl apply -f operator.yaml

```

#### 1. Deployment - Elasticsearch

```
kubectl apply -f quickstart-escluster.yaml

kubectl get elasticsearch -n es-dev
- based on elasticsearch's cluster health API 

kubectl get pods --selector='elasticsearch.k8s.elastic.co/cluster-name=quickstart' -n es-dev

kubectl logs -f quickstart-es-default-0 -n es-dev
```

#### 2. Request for Access
  
```
kubectl get service quickstart-es-http -n es-dev

PASSWORD=$(kubectl get secret quickstart-es-elastic-user -n es-dev -o go-template='{{.data.elastic | base64decode}}')

echo $PASSWORD
```
- Default username = elastic, stored as a k8s secret

#### 3. API calls 

```
- Within k8s cluster 

curl -u "elastic:$PASSWORD" -k "https://quickstart-es-http:9200"

- Local workstation

kubectl port-forward service/quickstart-es-http 9200

curl -u "elastic:$PASSWORD" -k "https://localhost:9200"
```

#### 4. Deployment - Kibana

```
kubectl apply -f quickstart-kibana.yaml

kubectl get kibana 
- based on elasticsearch's cluster health API 

kubectl get pod --selector='kibana.k8s.elastic.co/name=quickstart' -n es-dev

kubectl logs -f quickstart-kb-649b75b674-v7h9b
```

#### 5. Accessing Kibana
  
```
- Local workstation

kubectl port-forward service/quickstart-kb-http 5601

```
- Login using Elastic user 

#### Creating APM server instance 
  
```
kubectl apply -f quickstart-apm-server.yaml

kubectl get pods --selector='apm.k8s.elastic.co/name=apm-server-quickstart' -n es-dev

```
- Login using Elastic user 