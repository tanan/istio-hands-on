# Blue/Green Deployment

ここでは、IstioでBlue-Green Deploymentを実現するための設定方法についてやっていきます。

## Kubernetes リソース作成

まずは、Kubernetesのリソースを作成します。

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

## リクエスト確認

```
$ curl -v http://hands-on.example.com/news
$ curl -v -H 'x-version: green' http://hands-on.example.com/news
$ kubectl logs -f [pod_name] hands-on-news-green -n hands-on
```