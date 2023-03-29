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

- 查看结果
由于在定义child app的`helm-guestbook.yaml`和`helm-hooks.yaml`manifest中添加了下面内容，即设置了[syncPolicy](https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/)为auto-sync

```yaml
spec:
  syncPolicy:
    automated: {}
```
因此helm-guestbook和helm-hooks这两个app在创建parent app的同时也被sync。

![image](https://user-images.githubusercontent.com/35916199/228449208-3596943b-569c-4ccd-aff6-0e8e5bc2cb95.png)
