# A multi container app deployed on a Kubernetes cluster 

As a non-German speaker living in Switzerland, I often need to quickly translate large texts, but I get annoyed by character limits on Google Translate or DeepL. Learning German may have been a *way* better call, but instead I decided to deploy a translation application. It's made of 3 containerized microservices:

* A [Flask frontend](https://github.com/datatrigger/unlimited_translation-frontend-k8s) to get inputs and display translations
* A [FastAPI API backend](https://github.com/datatrigger/unlimited_translation-backend) to translate English text, using open-source models (SpaCy, Hugging Face)
* A [MySQL database](https://hub.docker.com/_/mysql) to store previous translations

In this repo, we deploy the app on Kubernetes cluster, see also this [blog post](https://blog.vlgdata.io/post/unlimited_translation_kubernetes/).

For the deployment on a single node with Docker Compose, see this [repo](https://github.com/datatrigger/unlimited-translation_docker_swarm) and this [blog post](https://blog.vlgdata.io/post/unlimited_translation_deploy_with_docker_compose/).

## Deployment on a Kubernetes cluster

Run the following commands in your Kubernetes cluster:

1) Create the secrets for the database's credentials:

```
kubectl create secret generic mysql-db-secret \
  --from-literal=MYSQL_ROOT_PASSWORD='<root_password>' \
  --from-literal=MYSQL_USER='<user>' \
  --from-literal=MYSQL_PASSWORD='<password>'
```

2) Deploy the app

```
kubectl apply -f unlimited-translation-k8s-dev.yaml
```

3) Connect to the app through the IP of the ```frontend-flask-service``` Service:

```
kubectl get svc -o wide
```

*Note 1*: the manifest used in the above command requests a ```LoadBalancer``` service and an automatically provisioned `PersistentVolume`, so it is expected to be applied on a cluster managed by a cloud provider (unless you implemented these features yourself... I used GKE).

*Note 2*: the ```unlimited-translation-k8s.yaml``` contains an additional Ingress to publish the app on my personal domain, [translate.vlgdata.io](https://translate.vlgdata.io)
