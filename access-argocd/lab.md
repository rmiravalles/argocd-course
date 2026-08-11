## 🎯 Lab Goal

Access the Argo CD web UI and install the command-line interface (CLI) to gain full control over your Argo CD installation.

## 📝 Overview & Concepts

Argo CD is now running in our cluster, but by default, it's not exposed to the outside world. To interact with it, we need to connect securely. In this lab, we'll first retrieve the auto-generated initial admin password from a Kubernetes secret. Then, we'll use `kubectl port-forward` to create a secure tunnel from our local machine to the Argo CD server pod. This will allow us to log in via a web browser. Finally, we'll install the powerful `argocd` CLI for command-line operations and log in with it as well.

## 📋 Lab Tasks

1.  Retrieve the initial admin password for the `admin` user from the `argocd-initial-admin-secret` Kubernetes secret and decode it.
2.  Use `kubectl port-forward` to make the Argo CD server UI accessible on `localhost:8080`.
3.  Log in to the Argo CD web UI in your browser using the `admin` username and the retrieved password.
4.  Install the `argocd` CLI tool on your local machine.
5.  Log in to the Argo CD API server using the `argocd` CLI.

## 📚 Helpful Resources

- [Argo CD Docs - Log In To The UI / CLI](https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)
- [Argo CD CLI Installation Guide](https://argo-cd.readthedocs.io/en/stable/cli_installation/)
- [Kubectl `port-forward` Command Documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward)

## 💭 Reflection Questions

1. Why is `kubectl port-forward` an option to access Argo CD during development, and why might it not be suitable for production environments?
2. What security implications should you consider after retrieving the initial admin password, and what's the best practice for managing it?
3. In what scenarios would you prefer using the Argo CD CLI over the web UI, and vice versa?

## ✅ Lab Solution

### 1. Retrieve the initial admin password

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```

### 2. Port-forward the Argo CD server
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### 3. Log in to the Argo CD web UI
Open your browser and go to `https://localhost:8080`. Use `admin` as the username and the retrieved password.

### 4. Install the Argo CD CLI
Follow the instructions for your operating system from the [Argo CD CLI Installation Guide](https://argo-cd.readthedocs.io/en/stable/cli_installation/).

### 5. Log in to the Argo CD API server using the CLI
```bash
argocd login localhost:8080 --username admin --password <retrieved-password> --insecure
```
