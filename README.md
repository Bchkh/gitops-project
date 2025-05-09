# gitops-project
 
**GitOps-driven deployment with Kubernetes, ArgoCD, and Security Scanning**  

![GitOps](https://img.shields.io/badge/GitOps-Enabled-brightgreen) 
![Trivy](https://img.shields.io/badge/Security-Trivy_Scanned-blue) 
![Kubernetes](https://img.shields.io/badge/Platform-Kubernetes-326CE5)  

## 📌 Overview  
Automated infrastructure deployment using:  
- **GitOps** (ArgoCD syncs Kubernetes manifests from Git)  
- **Security Scanning** (Trivy in CI/CD to block vulnerable images)  
- **Monitoring** (Prometheus + Grafana)  

---

## 🛠️ Prerequisites  
1. **Minikube** (or any Kubernetes cluster)  
   ```sh
   minikube start --driver=virtualbox
   ```
2. **kubectl & Helm**  
3. **GitHub/GitLab Account** (for GitOps repo)  

---

## 🔧 Installation  
### 1. Clone the Repo  
```sh
git clone https://github.com/Bchkh/gitops-project.git
cd gitops-project
```

### 2. Deploy ArgoCD  
```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 3. Add Applications via ArgoCD  
```sh
kubectl apply -f argocd/apps/ -n argocd
```

---

## 🔒 Security Scanning with Trivy  
### **CI/CD Pipeline Integration**  
Trivy scans container images on every `git push` and blocks deployments if critical vulnerabilities are found.  

#### **Scan Results**  
- **Pass**: Deployment proceeds if no critical issues.  
- **Fail**: Workflow blocks and logs vulnerabilities (e.g., `CVE-2024-45491`).  

---

## 🚀 Usage  
### **GitOps Flow**  
1. Edit manifests in `apps/` (e.g., change `replicas` in `deployment.yaml`).  
2. Push to Git → ArgoCD auto-syncs changes to Kubernetes.  

### **Manual Overrides**  
```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443  # Access ArgoCD UI
```

---

## 📊 Monitoring  
### **Access Dashboards**  
- **Prometheus**:  
  ```sh
  kubectl port-forward svc/prometheus-server -n monitoring 9090:80
  ```
- **Grafana**:  
  ```sh
  kubectl port-forward svc/grafana -n monitoring 3000:80
  ```
  - Default credentials: `admin` / `admin`  

---

## 🛡️ Security Best Practices  
- **Pinned Image Tags**: `nginx:1.25.4-alpine` (not `latest`)  
- **Network Policies**: Default-deny all pod traffic.    

---

## ❓ Troubleshooting  
| Issue | Solution |  
|-------|----------|  
| Trivy fails with CVEs | Upgrade base images or suppress false positives with `--ignore-unfixed` |  
| ArgoCD out of sync | Check `argocd app get <app-name>` for errors |  
| No metrics in Prometheus | Verify ServiceMonitor labels match Prometheus config |  

---

## 📜 License  
MIT © Bouchra EL KHARRAZ  
```

---

### **Key Features Highlighted**  
1. **Trivy Scanning**: Clear explanation of CI/CD blocking logic.  
2. **GitOps Workflow**: Simple push-to-deploy model.  
3. **Security Focus**: Pinned images, network policies, RBAC.  
4. **Troubleshooting Table**: Quick fixes for common issues.  

  

