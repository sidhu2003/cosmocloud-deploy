# cosmocloud-deploy

## Deployment Overview

The Helm chart will deploy the following components:

- **Backend**
  - Image: `shreybatra/sample-backend`
  - Environment variable: `REDIS_URI=redis://redis-svc:6379`
  - Service Type: `ClusterIP`
  - Port: 8000
  - Service Name: `backend-svc`

- **Frontend**
  - Image: `shreybatra/sample-frontend`
  - Environment variable: `BACKEND_URL=http://backend-svc:8000`
  - Service Type: `NodePort`
  - Port: 5175
  - NodePort: 31000
  - Service Name: `frontend-svc`

- **Redis**
  - Image: `redis`
  - Service Type: `ClusterIP`
  - Port: 6379
  - Service Name: `redis-svc`

## Deployment Instructions

Follow these steps to deploy the application on your Kubernetes cluster using Helm:

1. Clone the repository:

    ```bash
    git clone https://github.com/sidhu2003/cosmocloud-deploy.git
    cd cosmocloud-deploy
    ```

2. Install Helm (if not already installed). Follow the [Helm installation guide](https://helm.sh/docs/intro/install/).

3. Install the Helm chart using the following command:

    ```bash
    helm install testapp cosmocloud-deploy --atomic --timeout 30s
    ```

4. To verify the deployment, run:

    ```bash
    kubectl get deployments
    kubectl get services
    ```

You should see 1 replica of the Backend, Frontend, and Redis services deployed successfully.

## Helm Chart Structure

The `cosmocloud-deploy` Helm chart is organized as follows:

```bash
cosmocloud-deploy/
├── charts/         # For subcharts (if any, optional for this task)
├── templates/      # Contains all YAML manifests
├── values.yaml     # Default configuration for the chart
├── Chart.yaml      # Metadata about the Helm chart
└── README.md       # Optional documentation
```

## Services

- **Backend Service (ClusterIP)**: Accessible only within the cluster, listens on port 8000.
- **Frontend Service (NodePort)**: Exposed externally on port 31000, listens on port 5175.
- **Redis Service (ClusterIP)**: Accessible only within the cluster, listens on port 6379.

## Environment Variables

- **Backend Deployment**: `REDIS_URI=redis://redis-svc:6379` (environment variable to link Backend to Redis).
- **Frontend Deployment**: `BACKEND_URL=http://backend-svc:8000` (environment variable to link Frontend to Backend).

## Notes

- The Helm chart assumes that you're using Kubernetes with the default namespace.
- The services are configured for internal communication within the Kubernetes cluster. The Frontend is exposed externally via NodePort for testing purposes.

## Troubleshooting

- If you encounter issues, check the logs of individual services:

    ```bash
    kubectl logs <pod-name>
    ```

- You can also check the events in the cluster for potential issues:

    ```bash
    kubectl get events
    ```

