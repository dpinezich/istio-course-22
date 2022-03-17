# Observability

## Preparation

Be sure that istioctl is available, otherways:

```
cd home/data/istio-xx.xx.x
export PATH=$PWD/bin:$PATH
```

Then install the Grafana-Addon:

```kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.13/samples/addons/grafana.yaml```

Then install the Prometheus-Addon

```kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.13/samples/addons/prometheus.yaml```

## Start Prometheus

```istioctl dashboard prometheus```

## Start Grafana

```istioctl dashboard grafana```

## Queries with prometheus and grafana:

Total count of all requests to the productpage service:

```istio_requests_total{destination_service="productpage.default.svc.cluster.local"}```

Total count of all requests to v3 of the reviews service:

```istio_requests_total{destination_service="reviews.default.svc.cluster.local", destination_version="v3"}```

This query returns the current total count of all requests to the v3 of the reviews service.
Rate of requests over the past 5 minutes to all instances of the productpage service:

```rate(istio_requests_total{destination_service=~"productpage.*", response_code="200"}[5m])```