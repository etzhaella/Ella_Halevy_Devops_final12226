# Helm Chart: AWS Monitor Flask

Helm chart for deploying the **Flask AWS monitoring application** (EC2, VPCs, Load Balancers, AMIs) to Kubernetes.

## What Is Implemented

- **Deployment** – Runs the Flask app with configurable replicas, env vars, resource limits, and liveness/readiness probes.
- **Service** – ClusterIP (default) exposing the app inside the cluster; port and target port are configurable.
- **Ingress** – Optional; enable in `values.yaml` to expose the app via HTTP(S) (e.g. with an Ingress controller like nginx).
- **values.yaml** – Configurable options:
  - Image repository and tag (default: `etzhaella/flask-aws-monitor:latest`)
  - Replica count and scaling (replicaCount, resources)
  - Environment variables (e.g. `AWS_DEFAULT_REGION`, `FLASK_RUN_PORT`)
  - AWS credentials from an existing Kubernetes secret (recommended for production)
  - Ingress host, annotations, TLS
  - Probes, security context, nodeSelector, affinity, tolerations

## Prerequisites

- Kubernetes cluster (e.g. minikube, EKS, AKS).
- `kubectl` and `helm` installed.
- Docker image available (e.g. from Docker Hub: `etzhaella/flask-aws-monitor:latest`).

## AWS Credentials

The app needs `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Two options:

1. **From a Kubernetes secret (recommended)**  
   Create a secret, then in `values.yaml` set:
   ```yaml
   awsCredentials:
     useExistingSecret: true
     secretName: aws-monitor-secret
   ```
   Create the secret:
   ```bash
   kubectl create secret generic aws-monitor-secret \
     --from-literal=AWS_ACCESS_KEY_ID=your_key \
     --from-literal=AWS_SECRET_ACCESS_KEY=your_secret \
     -n <namespace>
   ```

2. **Not using a secret**  
   Leave `useExistingSecret: false`. You must then provide credentials another way (e.g. IAM role for service account on EKS, or inject env elsewhere). The chart does not store raw credentials in values.

## How to Run / Test

1. **Install the chart** (example release name `aws-monitor`, namespace `default`):
   ```bash
   helm install aws-monitor ./helm -n default
   ```

2. **Access the app** (ClusterIP default):
   ```bash
   kubectl port-forward svc/aws-monitor-aws-monitor-flask 5000:80 -n default
   ```
   Open http://127.0.0.1:5000 – you should see EC2 instances, VPCs, Load Balancers, and AMIs (if AWS credentials are set).

3. **Upgrade after changing values**:
   ```bash
   helm upgrade aws-monitor ./helm -n default -f my-values.yaml
   ```

4. **Uninstall**:
   ```bash
   helm uninstall aws-monitor -n default
   ```

## Overriding Values

- **Custom image:**  
  `helm install aws-monitor ./helm --set image.repository=myrepo/flask-aws-monitor --set image.tag=v1.0`

- **Enable Ingress:**  
  `helm install aws-monitor ./helm --set ingress.enabled=true --set ingress.hosts[0].host=monitor.mydomain.com`

- **More replicas / resources:**  
  Edit `values.yaml`: `replicaCount: 2`, and adjust `resources` as needed.

## Missing / Optional

- **HorizontalPodAutoscaler (HPA)** – Not included; you can add one manually or extend the chart.
- **PodDisruptionBudget** – Placeholder in `values.yaml`; uncomment and tune if needed.
- **Service type LoadBalancer** – Change `service.type` in `values.yaml` if you want a cloud load balancer instead of Ingress.

## Chart Structure

```
helm/
├── Chart.yaml
├── values.yaml
├── README.md
└── templates/
    ├── _helpers.tpl
    ├── deployment.yaml
    ├── service.yaml
    ├── ingress.yaml
    └── NOTES.txt
```
