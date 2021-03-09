## This repo has a sample deployment and Ingress Config for Anthos on BareMetal. We cover both HTTP and HTTPS Ingress

### Deploy the app

```
kubectl apply -f deploy.yaml
kubectl apply -f service.yaml
```

```
mkdir certs && cd certs

openssl genrsa -out test-ingress.key 2048

openssl req -new -key test-ingress.key -out test-ingress.csr -subj "/CN=example.com"

openssl x509 -req -days 365 -in test-ingress.csr -signkey test-ingress.key -out test-ingress.crt

kubectl create secret tls nginx-certs --cert test-ingress.crt --key test-ingress.key -n gke-system

cd ..

kubectl apply -f ingress.yaml
```
