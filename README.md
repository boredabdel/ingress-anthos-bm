## This repo has a sample deployment and Ingress Config for Anthos on BareMetal. We cover both HTTP and HTTPS Ingress

### Deploy the app
```
kubectl apply -f deploy.yaml
kubectl apply -f service.yaml
```

### Check the app have been deployment properly, you should 3 pods
```
kubectl get pods
```
### Fetch the istio-ingress gateway VIP
```
INGRESS_VIP=$(kubectl get svc istio-ingress -n gke-system -o jsonpath="{.spec.loadBalancerIP}{'\n'}")
```

### Option1: Deploy HTTP Ingress
```
kubectl apply ingress-http.yaml
```
### Try the HTTP Ingress
```
curl http://$INGRESS_VIP
```

### Option 2: Deploy HTTPS Ingress
```
mkdir certs && cd certs

openssl genrsa -out test-ingress.key 2048

openssl req -new -key test-ingress.key -out test-ingress.csr -subj "/CN=example.com"

openssl x509 -req -days 365 -in test-ingress.csr -signkey test-ingress.key -out test-ingress.crt

kubectl create secret tls example-com-certs --cert test-ingress.crt --key test-ingress.key -n gke-system

cd ..

kubectl apply -f ingress-https.yaml
```

### Try the HTTPS Ingress
```
curl -k -HHost:example.com --resolve "example.com:443:$INGRESS_VIP" "https://example.com"
```
