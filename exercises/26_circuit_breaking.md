# Traffic Routing

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

Do a quick reset:

```
kubectl delete virtualservice httpbin
kubectl delete destinationrule httpbin
kubectl delete deploy httpbin-v1 httpbin-v2 sleep
kubectl delete svc httpbin
```

Create a httpbin:

```
kubectl apply -f samples/httpbin/httpbin.yaml
```

## Circuit Breaker

```
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: httpbin
spec:
  host: httpbin
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 1
      http:
        http1MaxPendingRequests: 1
        maxRequestsPerConnection: 1
    outlierDetection:
      consecutive5xxErrors: 1
      interval: 1s
      baseEjectionTime: 3m
      maxEjectionPercent: 100
EOF
```

A quick check

```
kubectl get destinationrule httpbin -o yaml
```

## Adding a client (fortio)

Inject the client with the Istio sidecar proxy so network interactions are governed by Istio.

```
kubectl apply -f samples/httpbin/sample-client/fortio-deploy.yaml
```

Log in to the client pod and use the fortio tool to call httpbin. 
Pass in curl to indicate that you just want to make one call:

```
export FORTIO_POD=$(kubectl get pods -l app=fortio -o 'jsonpath={.items[0].metadata.name}')
kubectl exec "$FORTIO_POD" -c fortio -- /usr/bin/fortio curl -quiet http://httpbin:8000/get
```

## Calling the circuit breaker (tripping ;) )

Call the service with two concurrent connections (-c 2) and send 20 requests (-n 20):

```
kubectl exec "$FORTIO_POD" -c fortio -- /usr/bin/fortio load -c 2 -qps 0 -n 20 -loglevel Warning http://httpbin:8000/get
```

More concurrent connections:

```
kubectl exec "$FORTIO_POD" -c fortio -- /usr/bin/fortio load -c 3 -qps 0 -n 30 -loglevel Warning http://httpbin:8000/get
```

See the istio proxy:

```
kubectl exec "$FORTIO_POD" -c istio-proxy -- pilot-agent request GET stats | grep httpbin | grep pending
```

## Cleanup

```
kubectl delete destinationrule httpbin
kubectl delete deploy httpbin fortio-deploy
kubectl delete svc httpbin fortio
```
