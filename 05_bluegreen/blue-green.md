# Blue/Green Deployment

```
kubectl apply -f namespace.yaml
kubectl apply -f deployment-blue.yaml
kubectl apply -f deployment-green.yaml
kubectl apply -f service.yaml
```

## Istio リソース作成

```
kubectl apply -f virtual-service.yaml
kubectl apply -f destination-rule.yaml
```