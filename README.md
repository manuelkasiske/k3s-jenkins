# Install Helm

```sh
curl -LO https://git.io/get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
cat << EOF > rbac-config.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
EOF
kubectl apply -f rbac-config.yaml
helm init --service-account tiller
```

# prepare the storage class: local-path
```sh
curl -LO https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
cat local-path-storage.yaml 
kubectl apply -f local-path-storage.yaml 
```

# install
```sh
helm install -f jenkins-values.yaml stable/jenkins
```

In the jenkins-values.yaml persistence volume claim has been overwritten to storageClass: local-path

see /opt/local-path-provisioner/



