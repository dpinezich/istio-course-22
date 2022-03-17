# Traffic Routing

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

First install a virtual service layer:

```kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml```

## Request Timeouts

Route all to v2

```
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
    - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v2
EOF
```

Add a two second delay:

```
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: ratings
spec:
  hosts:
  - ratings
  http:
  - fault:
      delay:
        percent: 100
        fixedDelay: 2s
    route:
    - destination:
        host: ratings
        subset: v1
EOF
```

Do a quick check what happened:

```kubectl get virtualservices -o yaml```

## Cleanup

```
kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml
```
