##create namespace in cluset
kubectl create ns cert-manager

##install cert manager with CRDs
helm install \
  cert-manager jetstack/cert-manager \
  --version v1.0.1 \
  --set installCRDs=true



##mongo replicas hostnames
echochart-mongodb-0.echochart-mongodb-headless.default.svc.cluster.local
echochart-mongodb-1.echochart-mongodb-headless.default.svc.cluster.local

##export mogno pass and rootpass
export MONGODB_ROOT_PASSWORD=$(kubectl get secret --namespace default echochart-mongodb -o jsonpath="{.data.mongodb-root-password}" | base64 --decode)
export MONGODB_PASSWORD=$(kubectl get secret --namespace default echochart-mongodb -o jsonpath="{.data.mongodb-password}" | base64 --decode)

##exec to mongodb
kubectl run --namespace default my-database-mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=$MONGODB_ROOT_PASSWORD" --image docker.io/bitnami/mongodb:4.4.7-debian-10-r9 --command -- bash
mongo admin --host "echochart-mongodb-0.echochart-mongodb-headless.default.svc.cluster.local,echochart-mongodb-1.echochart-mongodb-headless.default.svc.cluster.local:27017" --authenticationDatabase admin -u root -p $MONGODB_ROOT_PASSWORD


