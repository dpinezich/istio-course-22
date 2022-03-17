# Observability

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

First install Kiali:

```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.13/samples/addons/kiali.yaml
kubectl -n istio-system get svc kiali
```

## Test Gateway

Test Gateway

```
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
kubectl get gateway
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```

## Open the dashboard

```istioctl dashboard kiali```


## Do some requests (different tab)

```curl http://$GATEWAY_URL/productpage```

or

```watch -n 1 curl -o /dev/null -s -w %{http_code} $GATEWAY_URL/productpage```