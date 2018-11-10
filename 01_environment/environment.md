# 環境構築

## 説明

ここでは、Istioを稼働させるためのGKEクラスターを作成していきます。すでに手元に環境が揃っている方は飛ばして頂いて構いません。

## gcloud コマンドインストール

- [こちら](https://cloud.google.com/sdk/install)を参考にインストール

## GCPアカウント設定

```
$ gcloud config list
$ gcloud config set project [PROJECT_ID]
$ gcloud config set compute/zone asia-northeast1-a
$ gcloud config set compute/region asia-northeast1
$ gcloud components update
```

## クラスター作成

- [参考資料](https://istio.io/docs/setup/kubernetes/platform-setup/gke/)

```
$ gcloud container clusters create istio-hands-on --cluster-version=1.11.2-gke.15 --num-nodes=3
$ gcloud container clusters get-credentials istio-hands-on
$ kubectl create clusterrolebinding cluster-admin-binding \
    --clusterrole=cluster-admin \
    --user=$(gcloud config get-value core/account)
```

## Istioインストール

- [参考資料](https://istio.io/docs/setup/kubernetes/quick-start/)

```
$ curl -L https://git.io/getLatestIstio | sh -
$ cd istio-1.0.3
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
kubectl apply -f install/kubernetes/istio-demo.yaml
```