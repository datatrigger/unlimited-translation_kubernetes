# unlimited-translation_kubernetes

Deploying our multi-container app unlimited-translation to a GKE cluster

Secret syntax:

kubectl create secret generic mysql-db-secret \
  --from-literal=MYSQL_ROOT_PASSWORD='<root_password>' \
  --from-literal=MYSQL_USER='<password>' \
  --from-literal=MYSQL_PASSWORD=<user>
