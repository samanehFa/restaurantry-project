📦 Filebeat + ELK Stack (Elasticsearch, Logstash, Kibana)
=========================================================

This configuration enables centralized logging for the Node.js microservices using the ELK stack and Filebeat. All logs are routed from containers into Elasticsearch and visualized in Kibana.

📊 Log Flow
-----------

    Filebeat → Logstash → Elasticsearch → Kibana

📁 Project Contents
-------------------

*   Custom config files for Filebeat, Elasticsearch, Logstash, and Kibana
*   All services deployed in the `observability` namespace
*   Files sourced from ArtifactHub and modified for this use case
*   Kibana exposed via LoadBalancer for external access

 <p>⚙️ All configuration files are adapted from <a href="https://artifacthub.io" target="_blank">ArtifactHub</a> to match the specific services in this project.</p>

🛠️ Setup Instructions
----------------------

### 1️⃣ Create Namespace

    kubectl create namespace observability

### 2️⃣ Install Filebeat

    
    cd filebeat
    helm install filebeat . -n observability
      

### 3️⃣ Install Logstash

    
    cd elk/logstash
    helm install logstash . -n observability
      

### 4️⃣ Install Elasticsearch

    
    cd elk/elasticsearch
    helm install elasticsearch . -n observability
      

### 5️⃣ Install Kibana

    
    cd elk/kibana
    helm install kibana . -n observability
      

#### 🔁 Expose Kibana via LoadBalancer

To access Kibana externally, the configuration file included the LoadBalancer settings.


    kubectl get svc -n observability kibana-kibana
      

Wait for an **EXTERNAL-IP** to be assigned. Then open:

    https://<EXTERNAL-IP>

#### ⚠️ AWS Security Group Warning

**If you are deploying on AWS, ensure that your EC2 instance’s Security Group allows inbound traffic for HTTPS.**

Without updating the security rules, you will not be able to access the exposed LoadBalancer endpoints.

📈 Kibana Setup
---------------

To obtain the username and password manually, you can retrieve it from the secret using:

    kubectl get secret elasticsearch-master-credentials -n observability -o yaml

Then decode the password field from Base64:

    echo "xxxxxxxxxx==" | base64 --decode


✅ Summary
---------

*   Logs from all backend services are collected by Filebeat
*   Logs are processed by Logstash and stored in Elasticsearch
*   Kibana provides a web UI for log search and analysis
*   Helm makes the setup modular and easy to manage
