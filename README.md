# gke_kind_flux
Just another test project for training on Flux

1. Clone repo 
2. Fix vars with yous 
3. Execute commands below

```
terraform init
terraform plan -var-file="vars.tfvars"
terraform apply -var-file="vars.tfvars"
kubectl get ns
curl -s https://fluxcd.io/install.sh | bash
flux get all
```

* create basic demo ns

Check flux logs
```
flux logs -f
k get ns
```

```
flux create source git kbot \
    --url=https://github.com/Alexskl25/kbot \
    --branch=main \
    --namespace=demo \
    --export
---
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: kbot
  namespace: demo
spec:
  interval: 1m0s
  ref:
    branch: main
  url: https://github.com/Alexskl25/kbot
```

```
flux create helmrelease kbot \
    --namespace=demo \
    --source=GitRepository/kbot \
    --chart="./helm" \
    --interval=1m \
    --export
---
apiVersion: helm.toolkit.fluxcd.io/v2
kind: HelmRelease
metadata:
  name: kbot
  namespace: demo
spec:
  chart:
    spec:
      chart: ./helm
      reconcileStrategy: ChartVersion
      sourceRef:
        kind: GitRepository
        name: kbot
  interval: 1m0s
```

* Add changes to the GIT and PUSH it

```
flux logs -f
```

* Destroy evriroment
```
terraform destroy -var-file="vars.tfvars"
```

# The end
