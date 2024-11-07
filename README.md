# Running Minio in OpenShift

Deploys a four-node [distributed] Minio cluster (with one PV per node).

Create a secret with your desired admin username and password:

    kubectl create secret generic minio-config \
      --from-literal MINIO_ROOT_USERNAME=alice \
      --from-literal MINIO_ROOT_PASSWORD=secret

Deploy the manifests:

    kubectl apply -k .

Note that the console won't be available until after the cluster is healthy.

[distributed]: https://min.io/docs/minio/linux/operations/install-deploy-manage/deploy-minio-multi-node-multi-drive.html

This configuration relies on OpenShift to handle TLS termination (which means that in-cluster communication is not encrypted). To have minio handle TLS terminiation itself, we would need to modify the routes to use a passthrough configuration (set `route.spec.tls.termination` to `passthrough`) and provide minio with a certificate and key.
