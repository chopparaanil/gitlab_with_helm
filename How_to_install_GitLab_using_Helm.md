# Tab 1

# **1\. Add GitLab Helm repo**

helm repo add gitlab https://charts.gitlab.io/  
helm repo update

---

**2\. Create namespace**  
kubectl create namespace gitlab

---

**3\. Create values file (recommended)**  
vi gitlab-values.yaml

### **Minimal working config:**

global:  
 hosts:  
   domain: example.com  
   https: false

 ingress:  
   configureCertmanager: false

nginx-ingress:  
 controller:  
   service:  
     type: NodePort  
gitlab-runner:  
 install: false

---

**4\. Install GitLab**  
helm install gitlab gitlab/gitlab \\  
 \-n gitlab \\  
 \-f gitlab-values.yaml  
---

**5\. Upgrade (after changes)**  
helm upgrade gitlab gitlab/gitlab \\  
 \-n gitlab \\  
 \-f gitlab-values.yaml  
---

**6\. Uninstall GitLab**  
helm uninstall gitlab \-n gitlab  
---

 **7\. Delete namespace (full cleanup)**  
kubectl delete namespace gitlab  
---

**8\. Check status**  
helm list \-n gitlab  
kubectl get pods \-n gitlab  
kubectl get svc \-n gitlab  
---

**9\. Get NodePort (access UI)**  
kubectl get svc \-n gitlab | grep nginx

Example output:

80:31866

 Access:

http://gitlab.example.com:31866  
---

**10\. Get root password**  
kubectl get secret gitlab-gitlab-initial-root-password \\  
\-n gitlab \\  
\-o jsonpath="{.data.password}" | base64 \--decode ; echo  
---

#  **11\. Exec into toolbox**

kubectl exec \-it \-n gitlab deploy/gitlab-toolbox \-- bash

# Tab 2

