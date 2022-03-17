# Traffic Routing

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

Reset services:

```
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml
```

## Demonstration

Add the test delay

```
kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-delay.yaml
```

A quick check

```
kubectl get virtualservice ratings -o yaml
```

## HTTP abort fault

```
kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-abort.yaml
```

A quick check

```
kubectl get virtualservice ratings -o yaml
```


## Cleanup

```
kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml
```
