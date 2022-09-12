# A multi container app deployed on a Kubernetes cluster 

As a non-German speaker living in Switzerland, I often need to quickly translate large texts, but I get annoyed by character limits on Google Translate or DeepL. Learning German may have been a *way* better call, but instead I decided to deploy a translation application. It's made of 3 containerized microservices:

* A Flask frontend to get inputs and display translations
* A FastAPI API backend to translate English text, using open-source models (SpaCy, Hugging Face)
* A MySQL database to store previous translations

In this repo, we deploy the app on Kubernetes cluster. In [this repo](hhttps://github.com/datatrigger/unlimited-translation_docker_swarm), the app is deployed on a single node with Docker Compose.

## Deployment on a Kubernetes cluster

Run the following commands in your Kubernetes cluster:

1) Create the secrets for the database's credentials:

```
kubectl create secret generic mysql-db-secret \
  --from-literal=MYSQL_ROOT_PASSWORD='<root_password>' \
  --from-literal=MYSQL_USER='<user>' \
  --from-literal=MYSQL_PASSWORD=<password>
```

2) Deploy the app

```
kubectl create -f unlimited-translation-k8s.yaml
```