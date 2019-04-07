# 環境構築

## 説明

ここでは、Istioを稼働させるためのGKEクラスターを作成していきます。すでに手元に環境が揃っている方は飛ばして頂いて構いません。

## gcloud コマンドインストール

* [こちら](https://cloud.google.com/sdk/install)を参考にインストール

## GCPアカウント設定

```sh
$ gcloud config list
$ gcloud config set project [PROJECT_ID]
$ gcloud config set compute/zone asia-northeast1-a
$ gcloud config set compute/region asia-northeast1
$ gcloud components update
```

## クラスター作成

バージョンが時期によって変わっているかもしれませんので、コマンドから確認して入れるか、引数なしのデフォルト値を利用してください。

```sh
$ gcloud container get-server-config --zone asia-northeast1-a
```

- [参考資料](https://istio.io/docs/setup/kubernetes/platform-setup/gke/)

```sh
$ gcloud container clusters create istio-hands-on --cluster-version=1.12.5-gke.5 --num-nodes=3
$ gcloud container clusters get-credentials istio-hands-on
$ kubectl create clusterrolebinding cluster-admin-binding \
    --clusterrole=cluster-admin \
    --user=$(gcloud config get-value core/account)
$ kubectl get nodes
```

## Istioインストール

Istioをインストールします。また、Istioをインストールした時点で、自動的にTCPロードバランサーも作成されます。

* [参考資料](https://istio.io/docs/setup/kubernetes/quick-start/)

```sh
$ curl -L https://git.io/getLatestIstio | sh -
$ cd istio-1.1.2
$ kubectl create namespace istio-system
$ helm template install/kubernetes/helm/istio-init --name istio-init --namespace istio-system > istio-init-1.1.2.yaml
$ kubectl apply -f istio-init-1.1.2.yaml
$ kubectl get crds | grep 'istio.io\|certmanager.k8s.io' | wc -l
53
$ helm template install/kubernetes/helm/istio --name istio --namespace istio-system \
--values install/kubernetes/helm/istio/values-istio-demo.yaml > istio-install-1.1.2.yaml
$ kubectl apply -f istio-install-1.1.2.yaml
```

下記のコマンドで、Istioがインストールされたことを確認してみます。

```sh
$ kubectl get pods -n istio-system
NAME                                      READY   STATUS      RESTARTS   AGE
grafana-7b46bf6b7c-rv9k6                  1/1     Running     0          2m
istio-citadel-5589dfdf7b-l5vss            1/1     Running     0          2m
istio-cleanup-secrets-1.1.2-9wrk8         0/1     Completed   0          3m
istio-egressgateway-54c7959bd5-wkhcs      1/1     Running     0          2m
istio-galley-7759bb98fb-n6v9m             1/1     Running     0          2m
istio-grafana-post-install-1.1.2-85lhk    0/1     Completed   0          3m
istio-ingressgateway-744b97db5b-8kl4l     1/1     Running     0          2m
istio-init-crd-10-c69jh                   0/1     Completed   4          6m
istio-init-crd-11-8mxct                   0/1     Completed   4          6m
istio-pilot-596d79d669-r77p4              2/2     Running     0          2m
istio-policy-6cf7c6fc67-sngzx             2/2     Running     4          2m
istio-security-post-install-1.1.2-cfrzd   0/1     Completed   0          3m
istio-sidecar-injector-6f6f78645f-5hrsp   1/1     Running     0          2m
istio-telemetry-7869bd9c6f-lqqqk          2/2     Running     3          2m
istio-tracing-75dd89b8b4-wthbx            1/1     Running     0          2m
kiali-5d68f4c676-n4ths                    1/1     Running     0          2m
prometheus-89bc5668c-dpc29                1/1     Running     0          2m
```