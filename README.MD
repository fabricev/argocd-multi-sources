Bootstrap argocd

helm upgrade --install my-argo-cd argo/argo-cd --version 5.27.3 -f argocd.values.yaml --create-namespace --namespace=argocd

Create the repo

```yaml
apiVersion: v1
kind: Secret
metadata:
  labels:
    argocd.argoproj.io/secret-type: repository
  name: github-multi-source-git-repo
  namespace: argocd
data:
  password: Z2l0aHViX3BhdF8xMUFCUE9DNlkwMEd6VEYybGNVR2RmX3VzR1M5T2REeUVzaThYQmlLbjZYaEtsT256RWV6YUQ3cGRNYlNvS3NFc0pLUE9FNFJSWHdJUzVjY0xU
  project: ZGVmYXVsdA==
  type: Z2l0
  url: aHR0cHM6Ly9naXRodWIuY29tL2ZhYnJpY2V2L2FyZ29jZC1tdWx0aS1zb3VyY2VzLmdpdA==
  username: ZmFicmljZXY=
type: Opaque
```

Create the app
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: apps
  namespace: argocd
spec:
  destination:
    namespace: argocd
    server: https://kubernetes.default.svc
  project: default
  source:
    path: ./apps
    repoURL: https://github.com/fabricev/argocd-multi-sources.git
    targetRevision: HEAD
  syncPolicy:
    automated: {}
    syncOptions:
    - CreateNamespace=true
```

Create the repo