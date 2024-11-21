# Running Minio in OpenShift

Deploys a single-instance minio service with openidc authentication.

Create a secret with your desired admin username and password:

    kubectl create secret generic minio-admin-credentials \
      --from-literal MINIO_ROOT_USERNAME=alice \
      --from-literal MINIO_ROOT_PASSWORD=secret

Create an oidc client secret for minio:

    kubectl create secret generic minio-oidc-secret \
      --from-literal MINIO_IDENTITY_OPENID_CLIENT_SECRET=secret

Create a kustomize overlay:

    mkdir -p path/to/overlay
    cd path/to/overlay
    kustomize create
    kustomize edit add resource ../../minio-with-dex

Apply the necessary patches to change URLs for your environment:

    ...

Finally, deploy from your overlay:

    kubectl apply -k path/to/overlay

---

A quick way to put the admin password in your paste buffer:

    kubectl get secret minio-admin-credentials -o jsonpath='{.data.MINIO_ROOT_PASSWORD}' | base64 -d | xclip -selection clipboard
