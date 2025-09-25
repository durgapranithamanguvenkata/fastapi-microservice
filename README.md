# 🚀 DEMO Microservice with Kubernetes, HPA, and Monitoring

## ✅ Project Overview
This project demonstrates a simple **Demo microservice** deployed on **Kubernetes** with the following DevOps practices:
- Containerization using **Docker**
- Deployment on **Kubernetes**
- **Horizontal Pod Autoscaler (HPA)** to scale pods based on CPU usage
- **Prometheus & Grafana** for monitoring and visualization
- **GitHub Actions CI/CD pipeline** for automated build and deploy

The microservice exposes a single endpoint:
GET /
Response: {"message": "Hello, Vivek! Your microservice is running 🚀"}



## 🏗 Architecture Diagram

```plaintext
                 ┌───────────────┐
   User Request  │   LoadBalancer │
───────────────▶ │  (Service)    │
                 └───────┬───────┘
                         │
                         ▼
                 ┌───────────────┐
                 │   Pods        │
                 │ (Deployment)  │
                 └───────┬───────┘
                         │
               Autoscaling│ (CPU > 50%)
                         ▼
                 ┌───────────────┐
                 │  HPA          │
                 └───────────────┘

   ┌────────────────────────────────────┐
   │  Monitoring Layer                  │
   │  Prometheus ───▶ Metrics ───▶ Grafana │
   └────────────────────────────────────┘
⚙️ Steps to Build, Deploy, and Test

1. Run Locally

pip install -r requirements.txt
uvicorn app.main:app --reload
Visit: http://localhost:8000

2. Build Docker Image

docker build -t <your-dockerhub-username>/fastapi-microservice .
docker run -p 8000:8000 <your-dockerhub-username>/fastapi-microservice

3. Push Image to DockerHub

docker push <your-dockerhub-username>/fastapi-microservice

4. Deploy to Kubernetes

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/hpa.yaml

Check:

kubectl get pods
kubectl get svc
kubectl get hpa

5. Access Service in Minikube

minikube service fastapi-service
This opens the service in your browser.

📈 How Autoscaling Works
The Horizontal Pod Autoscaler (HPA) monitors CPU usage of the pods.

If average CPU > 50%, it creates more pods (up to maxReplicas).

If CPU < 50%, it scales down pods (to at least minReplicas).

Command to watch scaling:


kubectl get hpa -w
You can generate load using a tool like hey or ab (ApacheBench):

hey -z 30s -c 50 http://<minikube-ip>:<nodePort>/
This will trigger the HPA to increase replicas.

📊 How to Access Grafana Dashboards
1. Install Prometheus + Grafana via Helm
bash
Copy code
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack

2. Get Grafana Password
bash
Copy code
kubectl get secret prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
Default username = admin
Password = (value from above)

3. Access Grafana
Port-forward service:

bash
Copy code
kubectl port-forward svc/prometheus-grafana 3000:80
Open http://localhost:3000 and log in.

4. Dashboards
Grafana provides default dashboards for:

Pod CPU/Memory usage

Node metrics

HPA scaling activity

You can also import dashboards from Grafana Labs (ID 6417 is good for Kubernetes cluster monitoring).

✅ Expected Outputs
API response: {"message":"Hello, Vivek! Your microservice is running 🚀"}

kubectl get pods shows multiple pods when CPU load increases

Grafana dashboard shows CPU metrics and scaling behavior

GitHub Actions pipeline automatically builds and deploys new versions on push