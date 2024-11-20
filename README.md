# Running Minio in OpenShift

Deploys a single-instance minio service.

Create a secret with your desired admin username and password:

    kubectl create secret generic minio-admin-credentials \
      --from-literal MINIO_ROOT_USERNAME=alice \
      --from-literal MINIO_ROOT_PASSWORD=secret

Deploy the manifests:

    kubectl apply -k base

This configuration relies on OpenShift to handle TLS termination (which means that in-cluster communication is not encrypted). To have minio handle TLS termination itself, we would need to modify the routes to use a passthrough configuration (set `route.spec.tls.termination` to `passthrough`) and provide minio with a certificate and key.
