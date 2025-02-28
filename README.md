# FluxCD Cheatsheet (GitOps Continuous Deployment)

## **FluxCD Overview**
FluxCD is a GitOps tool that automates Kubernetes deployments by syncing manifests from a Git repository.

---

## **FluxCD Installation**
```sh
brew install fluxcd/tap/flux    # Install Flux CLI (MacOS)
flux install                    # Install Flux on Kubernetes cluster
```

### **Verify Installation**
```sh
flux --version                 # Check Flux version
kubectl get pods -n flux-system # Verify Flux components
```

---

## **Bootstrap Flux with Git Repository**
```sh
flux bootstrap github \
  --owner=<github-user> \
  --repository=<repo-name> \
  --branch=main \
  --path=clusters/my-cluster \
  --personal
```

---

## **Managing Workloads with Flux**

### **Create a Flux Source (Git Repository)**
```sh
flux create source git my-app \
  --url=https://github.com/<github-user>/<repo-name> \
  --branch=main \
  --interval=1m
```

### **Deploy Kubernetes Manifests from Git**
```sh
flux create kustomization my-app \
  --source=GitRepository/my-app \
  --path="./manifests" \
  --prune=true \
  --interval=5m
```

### **Sync Flux with Git Repository**
```sh
flux reconcile source git my-app
flux reconcile kustomization my-app
```

---

## **Observability & Debugging**
```sh
flux get sources git         # List all Git sources
flux get kustomizations      # List active kustomizations
flux logs                    # View Flux logs
```

### **Suspend and Resume Sync**
```sh
flux suspend kustomization my-app   # Suspend a kustomization
flux resume kustomization my-app    # Resume a kustomization
```

---

## **Flux Helm Integration**
### **Add a Helm Repository as a Source**
```sh
flux create source helm my-chart \
  --url=https://charts.bitnami.com/bitnami \
  --interval=5m
```

### **Deploy a Helm Release**
```sh
flux create helmrelease my-app \
  --source=HelmRepository/my-chart \
  --chart=nginx \
  --namespace=default \
  --interval=5m
```

---

## **Uninstall Flux**
```sh
flux uninstall --silent
kubectl delete namespace flux-system
```

---

## **Useful FluxCD Resources**
- [FluxCD Docs](https://fluxcd.io/docs/)
- [FluxCD GitOps Guide](https://fluxcd.io/flux/guides/gitops/)
- [FluxCD GitHub](https://github.com/fluxcd/flux2)

---

This cheatsheet provides a quick reference for FluxCD installation, GitOps workflows, Helm integration, and debugging. Let me know if you need any modifications!

