# Troubleshooting

## If the installation complains:
`unable to recognize "networking/bookinfo-gateway.yaml": no matches for kind "Gateway" in version "networking.istio.io/v1alpha3"
unable to recognize "networking/bookinfo-gateway.yaml": no matches for kind "VirtualService" in version "networking.istio.io/v1alpha3"
`

Use the following to create a default profile:

`istioctl install --set profile=demo -y`
