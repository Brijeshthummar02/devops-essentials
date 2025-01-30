
# Argo CD Deployment and Commands Guide

## Deploying Argo CD

### Step 1: Create a Namespace

Create a namespace for Argo CD.

```bash
kubectl create namespace argocd
```

### Step 2: Install Argo CD

Apply the Argo CD manifests to the created namespace.

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Step 3: Check Pod Status

Verify that the Argo CD pods are running correctly.

```bash
kubectl get pods -n argocd
```

### Step 4: Retrieve Admin Password

Obtain the initial admin password from the secret.

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

### Step 5: Login to Argo CD

Use the Argo CD CLI to log in with the admin credentials.

```bash
argocd login localhost:8080 --insecure
```

### Step 6: Port Forwarding (Optional)

Forward the port to access Argo CD from your local machine.

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### Step 7: Deploy Applications

Use the `argocd app create` command to deploy applications. For example, to deploy a Helm guestbook application:

```bash
argocd app create helm-guestbook   --repo https://github.com/argoproj/argocd-example-apps.git   --path guestbook   --dest-server https://kubernetes.default.svc   --dest-namespace default
```

### Step 8: Sync Application

Ensure the application is in sync with the desired state.

```bash
argocd app sync helm-guestbook
```

---

## Useful Argo CD Commands

### Application Commands

- **Create an Application**:  
  Create an application in Argo CD.

  ```bash
  argocd app create <app-name> --repo <repo-url> --path <app-path> --dest-server <cluster-server> --dest-namespace <namespace>
  ```

- **Sync an Application**:  
  Synchronize the application to the desired state.

  ```bash
  argocd app sync <app-name>
  ```

- **Delete an Application**:  
  Delete an application from Argo CD.

  ```bash
  argocd app delete <app-name>
  ```

- **List Applications**:  
  View all applications managed by Argo CD.

  ```bash
  argocd app list
  ```

### Cluster Commands

- **Add a Cluster**:  
  Add a new Kubernetes cluster to Argo CD.

  ```bash
  argocd cluster add <context-name>
  ```

- **List Clusters**:  
  View all clusters managed by Argo CD.

  ```bash
  argocd cluster list
  ```

- **Remove a Cluster**:  
  Remove a cluster from Argo CD.

  ```bash
  argocd cluster rm <context-name>
  ```

### User Management Commands

- **Change Password**:  
  Change the password for a user.

  ```bash
  argocd account update-password
  ```

- **List User Accounts**:  
  List all user accounts.

  ```bash
  argocd account list
  ```

### Configuration Commands

- **View Configuration**:  
  Display the current Argo CD configuration.

  ```bash
  argocd settings get
  ```

- **Update Configuration**:  
  Update the Argo CD configuration.

  ```bash
  argocd settings set <key>=<value>
  ```

---

## Notes

- Replace placeholders (e.g., `<app-name>`, `<repo-url>`) with actual values as required.
- Ensure `kubectl` is installed and configured to interact with your Kubernetes cluster.
- The `--insecure` flag bypasses SSL verification during login. Use it only for testing purposes.