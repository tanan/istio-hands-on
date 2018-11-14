# virtualservice

## 説明

ここでは、Istioのgatewayとvirtualserviceというリソースを使って、先ほどのプラグインを外からみれるようにしてみます。

実際のProduction環境でこれを実施してしまうと、全世界からgrafanaやprometheusのリソースが見れてしまうので、あくまでvirtual hostの設定方法を理解するために利用してください。

## 環境構築

```
cd yaml
$ kubectl apply -f namespace.yaml
$ kubectl apply -f gateway.yaml
$ kubectl apply -f grafana.yaml
$ kubectl apply -f prometheus.yaml
$ kubectl apply -f servicegraph.yaml
$ kubectl apply -f tracing.yaml
$ kubectl get gateways.networking.istio.io -n example
$ kubectl get virtualservices.networking.istio.io -n example
```

## アクセスして見る

- LoadBalancerのIPを取得

```shell:
$ kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

- /etc/hostsに下記を追加

```shell:/etc/hosts
<balancer_ip> grafana.example.com
<balancer_ip> prometheus.example.com
<balancer_ip> servicegraph.example.com
<balancer_ip> tracing.example.com
```

- 下記のURLにアクセス
  - http://grafana.example.com/
  - http://prometheus.example.com/
  - http://tracing.example.com/
  - http://servicegraph.example.com/dotviz

これで、ロードバランサー経由で各サービスに外からアクセスができるようになりました。