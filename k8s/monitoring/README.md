📊 Monitoring Microservices with Prometheus & Grafana
=====================================================

This folder contains Kubernetes manifests to enable monitoring of the Node.js microservices (`auth`, `discounts`, and `items`) deployed on an EKS cluster. Monitoring is implemented using **Prometheus** and **Grafana**.

📂 Contents
-----------

*   Three `ServiceMonitor` manifests (one for each backend service)
*   All components are deployed under a dedicated Kubernetes namespace: `monitoring`

☸️ Step 1: Create Monitoring Namespace
--------------------------------------

    kubectl create namespace monitoring

📦 Step 2: Install Prometheus and Grafana using Helm
----------------------------------------------------

1.  Add the Helm repo:
    
        helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
        helm repo update
    
2.  Install the **kube-prometheus-stack** in the `monitoring` namespace:
    
        helm install monitoring prometheus-community/kube-prometheus-stack \
          --namespace monitoring
    
3.  To access the dashboards externally, update the service types of Prometheus and Grafana:
    
        kubectl edit svc prometheus-stack-grafana -n monitoring
        kubectl edit svc prometheus-stack-kube-prom-prometheus -n monitoring
    
    Change the `type` from `ClusterIP` to `LoadBalancer`.

        kubectl edit svc monitoring-grafana -n monitoring

    Find the line:
    
        type: ClusterIP
    
    Change it to:
    
        type: LoadBalancer
    
5.  On AWS, ensure your worker node's security group allows inbound HTTP (port 80/3000) traffic. Then wait for the external IP to be assigned:
    
        kubectl get svc -n monitoring
    

🧪 Step 3: Apply ServiceMonitors
--------------------------------

Each backend service (`auth`, `discounts`, `items`) exposes a Prometheus-compatible metrics endpoint (e.g., `/metrics`).

    kubectl apply -f auth-backend-monitor.yaml -n monitoring
    kubectl apply -f discount-backend-monitor.yaml -n monitoring
    kubectl apply -f items-service-monitor.yaml -n monitoring

📈 Step 4: Configure Grafana
----------------------------

1.  Access Grafana using the external IP
2.  Login using the default credentials:
    *   Username: `admin`
    *   Password: Run `kubectl get secret --namespace monitoring monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 -d`
3.  Go to **Data Sources** and confirm that Prometheus is set up.
4.  Import dashboards or create new ones for visualization.

✅ Summary
---------

*   Prometheus scrapes metrics from your backend services via `ServiceMonitor` resources.
*   Grafana visualizes those metrics in custom dashboards.
*   All resources are isolated in the `monitoring` namespace.
*   External access is made possible by changing service types to `LoadBalancer`.
