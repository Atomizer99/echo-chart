##create namespace in cluset
kubectl create ns cert-manager

##install cert manager with CRDs
helm install \
  cert-manager jetstack/cert-manager \
  --version v1.0.1 \
  --set installCRDs=true
