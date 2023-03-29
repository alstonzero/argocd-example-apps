# app of apps pattern

https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/

## 声明式定义parent app：

- `parent-app.yaml`
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-of-apps
  namespace: argocd
spec:
  destination:
    namespace: argocd
    server: https://kubernetes.default.svc
  project: default
  source:
    path: app
    repoURL: https://github.com/alstonzero/argocd-example-apps.git
    targetRevision: automated-sync
  syncPolicy:
    automated:
      prune: true 
    syncOptions:
    - CreateNamespace=true
```
