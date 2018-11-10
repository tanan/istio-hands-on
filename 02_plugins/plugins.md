# Plugin

## 説明

ここでは、IstioでインストールされたPluginについてみていきます。
Istioをインストールするだけで、これらのプラグインが使えるようになり、さらには各プラグインの連携（例えばprometheusをデータソースにgrafanaで可視化等）も
自動的に実施してくれるのはかなり便利です。イケてます。

## Grafana, Prometheus, ServiceGraph, Tracing(jaeger)

kubectl port-forwardでローカルPCからコンテナポートにforwardすることができます。
下記コマンドで、port-forwardを設定する

```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090 &
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=servicegraph -o jsonpath='{.items[0].metadata.name}') 8088:8088 &
kubectl -n istio-system port-forward $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
```

- http://localhost:3000
- http://localhost:9090
- http://localhost:8088
- http://localhost:16686

## port-forwardせずに参照したい

毎回port-forwardしないと見れないのは面倒ですよね。これらのPluginを外部から参照できるようにする方法については[04_virtualservie]で扱います。
