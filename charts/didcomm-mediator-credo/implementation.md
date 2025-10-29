# DIDComm Mediator Credo - Implementation Guide

## Prerequisites
- Kubernetes 1.19+, Helm 3.2.0+
- Bitnami repo: `helm repo add bitnami https://charts.bitnami.com/bitnami`
- For ingress: cert-manager, external-dns, ingress controller

## Installation

1. **Build dependencies**: `cd charts/didcomm-mediator-credo && helm dependency build`
2. **Create namespace**: `kubectl create namespace <namespace>`
3. **Deploy**: `helm install <release-name> . -f <values-file>.yaml -n <namespace>`

## Validation

4. **Check pod**: `kubectl get pods -n <namespace>` (Expected: Running, 1/1)
5. **Test health**: `kubectl exec -n <namespace> <pod-name> -- wget -qO- localhost:3000/health` (Expected: "Accepted")
6. **View invitation**: `kubectl logs -n <namespace> deployment/<release-name> | grep "invitation url"`
7. **Check certificate** (if ingress enabled): `kubectl get certificate -n <namespace>`
8. **Test HTTPS**: `curl https://<your-domain>/health` (Expected: 202)

## Configuration
- **Storage:** In-memory SQLite (no PostgreSQL)
- **Service:** ClusterIP on port 3000
- **Ingress:** Optional - configure hosts/TLS for external access
- **Certificate:** Use production issuer (letsencrypt-prod) for trusted certs
