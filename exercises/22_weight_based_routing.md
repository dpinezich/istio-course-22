# Traffic Routing

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

First install a virtual service layer:

```kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml```

## Weight Based

Now we shift 50% to v3

```kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml```

Do a quick check what happened:

```kubectl get virtualservices -o yaml```

You like v3? Shift 100% to it

```kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-v3.yaml```

Do a quick check what happened:

```kubectl get virtualservices -o yaml```

## Cleanup

```
kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml
kubectl delete -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml
kubectl delete -f samples/bookinfo/networking/virtual-service-reviews-v3.yaml
```
