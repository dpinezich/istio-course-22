# Observability

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

First install a sleep timer:

```kubectl apply -f samples/sleep/sleep.yaml```

Set the SOURCE_POD Env:

```export SOURCE_POD=$(kubectl get pod -l app=sleep -o jsonpath={.items..metadata.name})```

Start the httpbin sample

```kubectl apply -f samples/httpbin/httpbin.yaml```

## Test the access log

Default setup check

```kubectl exec "$SOURCE_POD" -c sleep -- curl -sS -v httpbin:8000/status/418```

Check the sleep's log

```kubectl logs -l app=sleep -c istio-proxy```

Check httpbin's log:

```kubectl logs -l app=httpbin -c istio-proxy```

Create some more entries:

```kubectl exec "$SOURCE_POD" -c sleep -- curl -sS -v httpbin:8000/status/418```

And re-check the logs.