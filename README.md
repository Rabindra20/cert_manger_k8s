# cert-manger
## Deploy 
Run cert.sh check cert is deployed or not .if present no need to run cert.sh or manually deploy cert-manager
### OR
Do manually<br/>
### Install all cert-manager components
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml
```
By default, cert-manager will be installed into the `cert-manager` namespace
```
kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges --all-namespaces
```
### Deploy ClusterIssuer
Change email and class in `cert-issuer-nginx-ingress.yaml` before deployment check ClusterIssuer is deployed or not .if present no need to deploy
```
kubectl apply -f cert-issuer-nginx-ingress.yaml
```
#### OR
keep `ClusterIssuer` in ingress annotations in ingress file then no need to deploy `ClusterIssuer` cert-issuer-nginx-ingress.yaml file
```
 annotations:
    kubernetes.io/ingress.class: <> eg:-azure/application-gateway
    cert-manager.io/cluster-issuer: <> eg:-letsencrypt-cluster-issuer-prod
```
### Deploy Certificate
change dnsname,namespace and ClusterIssuer name before deploy in certificate.yaml file
```
kubectl apply -f certificate.yaml -n namespace
```
check certificate issued or not
```
kubectl describe certificate/example-app  -n namespace
```
Add tls in ingress
```
  tls:
  - hosts:
    - <> #eg:-example.com
    secretName: <> #eg:-example
```
### Troubleshoot
- There was another ingress defined in another namespace that did define the same hostname, but failed to link to a proper secret with the TLS cert. When I deleted that one, it immediately worked.


