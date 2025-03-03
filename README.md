# Kubernetes WordPress Deployment

This project deploys a WordPress instance with a database using Kubernetes.

## Prerequisites
Before installing the project, ensure you have the following installed and configured:

1. Install `kubectl`:
   ```sh
   sudo apt install -y kubectl  # For Ubuntu/Debian
   brew install kubectl  # For macOS
   ```

2. Install `helm`:
   ```sh
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```

3. Configure AWS credentials:
   ```sh
   aws configure
   ```

4. Authenticate Docker with AWS ECR:
   ```sh
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 992382545251.dkr.ecr.us-east-1.amazonaws.com
   ```

5. Add the Ingress Nginx Helm repository:
   ```sh
   helm repo add ingresks-nginx https://kubernetes.github.io/ingress-nginx
   ```

6. Set the default Kubernetes namespace:
   ```sh
   kubectl config set-context --current --namespace=[namespace]
   ```

7. Install the Ingress Nginx controller:
   ```sh
   helm install [unique-name]ingress ingress-nginx/ingress-nginx --namespace [namespace] --set controller.ingressClassResource.name=[unique-name]ingress
   ```

8. Download the Helm chart for WordPress deployment:
   ```sh
   wget https://raw.githubusercontent.com/dolevtb/K8s/main/charts/mychart-0.1.0.tgz
   ```

9. Install the Helm chart:
   ```sh
   helm install [name] mychart-0.1.0.tgz --set namespaceOverride=[namespace] --set nameOverride=[uniquename]
   ```

## Important Notes
- Ensure `[unique-name]` is consistently used throughout all commands.
- Ensure `[namespace]` remains the same in all commands.
- Verify that your Kubernetes cluster is running and properly configured before installation.

### Troubleshooting
- If the ingress does not work, verify that the controller is correctly deployed:
  ```sh
  kubectl get pods -n [namespace]
  ```
- Check logs for any issues:
  ```sh
  kubectl logs -f [pod-name] -n [namespace]
  ```
