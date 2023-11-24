# Helper Docker image that contains the Kubernetes template.
# Used for running Coder commands in the cluster.

FROM ghcr.io/coder/coder:v2.4.0

COPY ./kubernetes/ /app/kubernetes/

WORKDIR /app/kubernetes

ENTRYPOINT ["coder"]
