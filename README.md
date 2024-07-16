```markdown
# MyApp Helm Chart

This Helm chart deploys a simple Nginx application on Kubernetes using a Service, Deployment, and Ingress.

## Prerequisites

- Kubernetes cluster
- Helm installed and configured

## Installation

1. **Clone the repository:**

   ```sh
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Modify `values.yaml`:**

   Ensure the `values.yaml` file has the correct values. For example:

   ```yaml
   appName: myapp
   ingressName: myapp-ingress
   dns: myapp.example.com
   ```

3. **Deploy the Helm chart:**

   Replace `<release-name>` with the desired name for your Helm release.

   ```sh
   helm install <release-name> .
   ```

## Files

### `values.yaml`

This file contains the values used in the templates. For example:

```yaml
appName: myapp
ingressName: myapp-ingress
dns: myapp.example.com
```

### `templates/service.yaml`

Defines the Kubernetes Service that exposes the Nginx application.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.appName }}
spec:
  ports:
    - port: 80
  selector:
    app: {{ .Values.appName }}
```

### `templates/deployment.yaml`

Defines the Kubernetes Deployment that runs the Nginx application.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.appName }}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: {{ .Values.appName }}
  template:
    metadata:
      labels:
        app: {{ .Values.appName }}
    spec:
      containers:
        - name: nginx
          image: nginx:1.17.3
          ports:
            - containerPort: 80
```

### `templates/ingress.yaml`

Defines the Kubernetes Ingress that exposes the Nginx application externally.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Values.ingressName }}
  namespace: development
spec:
  ingressClassName: nginx
  rules:
    - host: {{ .Values.dns }}
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: {{ .Values.appName }}
                port:
                  number: 80
```

## Usage

Once the chart is deployed, you can manage the resources using `kubectl`. 

### Check the Deployment, Service, and Ingress

```sh
kubectl get deployments -n development
kubectl get services -n development
kubectl get ingress -n development
```

### Accessing the Application

The Ingress resource exposes the application externally. Ensure your DNS is correctly set up to route to your Kubernetes cluster. Access the application using the specified DNS:

```sh
http://myapp.example.com
```

### Uninstall the Chart

To uninstall/delete the `<release-name>` deployment:

```sh
helm uninstall <release-name>
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

Replace `<repository-url>` and `<repository-directory>` with the actual URL and directory name of your repository. This `README.md` now includes the Ingress configuration and instructions on how to access the application externally.
