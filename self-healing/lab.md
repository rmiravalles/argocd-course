## 🎯 Lab Goal

Enable and observe Argo CD's self-healing capability to automatically revert configuration drift and enforce Git as the absolute source of truth.

## 📝 Overview & Concepts

Self-healing is the ultimate enforcement mechanism in a GitOps workflow. While automated sync reacts to changes in Git, self-heal reacts to changes made directly in the cluster, correcting any drift from the desired state.

In this lab, you will enable the `selfHeal` policy on your `guestbook` application. Then, you will manually change a resource in the cluster using `kubectl`. You will then observe as Argo CD's self-heal policy immediately detects this deviation and automatically reverts your change, ensuring the cluster's live state always matches what is declared in Git.

## 📋 Lab Tasks

1.  Add the `selfHeal: true` option to the `syncPolicy` block of your `guestbook-app.yaml` manifest.
2.  Apply the updated manifest to the cluster.
3.  Use the `kubectl scale` command to manually change the number of replicas for the `guestbook` deployment to a different value (e.g., `1`).
4.  Observe in the Argo CD UI as the application becomes `OutOfSync` and is almost instantly and automatically synced back to the state defined in Git.
5.  Verify with `kubectl` that the number of replicas has been restored to the value specified in your `Application` manifest's `valuesObject` (e.g., `3` if that is the value in `values.yaml`).

## 📚 Helpful Resources

- [Argo CD - Automated Sync Policy (includes Self-Heal)](https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/)
- [Kubectl `scale` Command Documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale)

## 💭 Reflection Questions

1. Why does automated sync alone not fix configuration drift (manual cluster changes), and how does self-healing complete the GitOps enforcement loop?
2. What are the potential risks of enabling pruning in a shared namespace where multiple teams deploy applications, and how would you mitigate these risks?
3. Under what circumstances might you want to temporarily disable self-healing in production, and what are the trade-offs of doing so?

## 🏁 Lab Solution

1.  Update the `guestbook-app.yaml` manifest to include the `selfHeal: true` option in the `syncPolicy` block:

    ```yaml
    spec:
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
    ```

2.  Apply the updated manifest to the cluster:

    ```bash
    kubectl apply -f guestbook-app.yaml
    ```

3.  Manually change the number of replicas for the `guestbook` deployment:

    ```bash
    kubectl scale deployment guestbook-helm-guestbook --replicas=6
    ```

4.  Observe in the Argo CD UI that the application becomes `OutOfSync` and is automatically synced back to the state defined in Git.
