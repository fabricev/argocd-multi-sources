Bootstrap argocd

helm upgrade --install my-argo-cd argo/argo-cd --version 5.27.3 -f argocd.values.yaml --create-namespace --namespace=argocd

Create the repo

```shell
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  labels:
    argocd.argoproj.io/secret-type: repository
  name: github-multi-source-git-repo
  namespace: argocd
data:
  password: Z2l0aHViX3BhdF8xMUFCUE9DNlkwdFltZjhHdkgxajBNX1Q1aWppemRWbXpOcEIydjN1YzVmNWN2bW1nazNOdHliTURvbVB2eUlYaURJUDVQRUlLVFIxTXBDUExN
  project: ZGVmYXVsdA==
  type: Z2l0
  url: aHR0cHM6Ly9naXRodWIuY29tL2ZhYnJpY2V2L2FyZ29jZC1tdWx0aS1zb3VyY2VzLmdpdA==
  username: ZmFicmljZXY=
type: Opaque
EOF
```

Create the app
```shell
cat <<EOF | kubectl apply -f -
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
EOF
```

Create the repo
