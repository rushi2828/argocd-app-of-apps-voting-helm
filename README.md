# argocd-app-of-apps-voting-demo


# Create kind cluster
kind create cluster --name voting-app --config kind-config/kind-config.yaml

# Create argocd namespace
kubectl create namespace argocd

# Install argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Waiting for all ArgoCD pods to be in 'Running' status..."
<!-- while true; do
    not_running=$(kubectl get pods -n "${ARGOCD_NAMESPACE}" --no-headers | grep -v 'Running' | wc -l)
    total=$(kubectl get pods -n "${ARGOCD_NAMESPACE}" --no-headers | wc -l)
    if [ "$total" -gt 0 ] && [ "$not_running" -eq 0 ]; then
        echo "✅ All ArgoCD pods are running."
        break
    else
        echo "⏳ Waiting... ($((total-not_running))/$total running)"
        sleep 5
    fi
done -->


# Argocd ui password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# Port-forward for argocd ui
kubectl port-forward svc/argocd-server -n argocd 8081:443

# Install ingress-nginx
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml


# Wait for ready ingress
kubectl wait --namespace ingress-nginx --for=condition=Ready pod --selector=app.kubernetes.io/component=controller --timeout=180s

# Verify all ingress pods are running
get all -n ingress-nginx
