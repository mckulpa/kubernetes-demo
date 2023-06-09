# Helm - Templating

Preview before installing
```bash
helm template kubernetes-demo-chart
```
```bash
helm install kubernetes-demo kubernetes-demo-chart --dry-run --debug 
```

Notice two ways of passing values from root `values.yaml` to sub-charts:
```yaml
global:
    replicas: 1
```

is accessible in all sub-charts by expression `{{ .Values.global.replicas }}`
```yaml
api-gateway:
    replicas: 2
```

is accessible in `api-gateway` sub-chart by expression `{{ .Values.replicas }}`

Notice each sub-chart may have its own `values.yaml` with its own values - see `api-gateway/values.yaml`
and `containerPort: 8087` accessible by expression `{{ .Values.containerPort }}`

Notice values may be also overridden from command line:
```bash
helm install kubernetes-demo kubernetes-demo-chart --set global.replicas=2
```

## Azure Application Gateway Ingress

Notice that ingress has been extended with additional configuration allowing it to be run on Azure depending on `api-gateway.cloud` value.
```bash
helm install kubernetes-demo kubernetes-demo-chart --set api-gateway.cloud=true
```

Test the service:
```bash
curl -X POST https://xapi.eun.dev.csi1.tescocloud.com/v1/person
```

## Other

Notice it is still not possible to install two instances of our system due to naming clashes.
