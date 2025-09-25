This is a simple FastAPI microservice that returns a "Hello, Vivek!" message.
The service is containerized with Docker, deployed on Kubernetes, and configured with Horizontal Pod Autoscaler (HPA).
Monitoring is enabled using Prometheus & Grafana.

⚙️ Quick Start
1. Run Locally
pip install -r requirements.txt
uvicorn app.main:app --reload


Visit → http://localhost:8000

2. Build & Push Docker Image
docker build -t <your-dockerhub-username>/fastapi-microservice .
docker push <your-dockerhub-username>/fastapi-microservice

3. Deploy to Kubernetes
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/hpa.yaml


Check resources:

kubectl get pods
kubectl get svc
kubectl get hpa

✅ Expected Output

API response:

{"message": "Hello, Vivek! Your microservice is running 🚀"}


Pods scale up/down automatically when CPU usage crosses threshold.

Metrics visible in Grafana dashboards.