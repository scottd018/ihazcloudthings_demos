# Usage

1. Ensure the audit policy exists on the control plane nodes:

```bash
kubectl apply -f 00-audit-policy.yaml
```

This will create a config map with the contents of the audit policy, create a directory on the control plane node for 
the audit configuration, and mount the config map as a file, and copy its contents into the audit configuration 
directory.

2. Ensure the audit policy webhook exists on the control plane nodes:

```bash
kubectl apply -f 01-audit-policy-webhook.yaml
```

Performs the exact steps as above with the audit policy webhook configuration.

3. Modify the API server:

```bash
kubectl apply -f 02-audit-policy-overlay.yaml
```

Using the previously created files, this step will backup the kube-apiserver configuration and use and overlay with 
YTT in order to write a new kube-apiserver configuration to add in the additional audit flags.  Once the kubelet sees 
the new configuration, a new pod is launched with our updated kube-apiserver config.

