# Observability

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

First install Jaeger:

```kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.13/samples/addons/jaeger.yaml```

## Test Jaeger

Test Jaeger

```istioctl dashboard jaeger```

## Do some requests

```for i in $(seq 1 100); do curl -s -o /dev/null "http://$GATEWAY_URL/productpage"; done```

Note, please replace $GATEWAY_URL with the url at the end of ./start.sh

Example:
```for i in $(seq 1 100); do curl -s -o /dev/null "http://http://192.168.49.2:32132/productpage"; done```



Alternate way for $GATEWAY_URL:
```
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
kubectl get gateway
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```