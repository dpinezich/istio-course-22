# Traffic Routing

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

First install a virtual service layer:

```kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml```

Do a quick check:

```kubectl get virtualservices -o yaml```

or with

```kubectl get destinationrules -o yaml```


## Identity Based

Apply default destination rule:

```kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml```

Then install a different virtual service layer:

```kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml```

Have a look at, what needs to be done?:

```kubectl get virtualservice reviews -o yaml```

## Cleanup

```
kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml
kubectl delete -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml
```
