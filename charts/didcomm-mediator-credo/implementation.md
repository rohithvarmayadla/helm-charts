# DIDComm Mediator Credo - Dev Implementation

## Prerequisites
- AKS cluster: `aks-sharedaks-cnc-cluster`
- Helm 3.2.0+
- Bitnami repo added: `helm repo add bitnami https://charts.bitnami.com/bitnami`

## Installation

1. **Build dependencies**
   ```bash
   cd charts/didcomm-mediator-credo && helm dependency build
   ```

2. **Create namespace**
   ```bash
   kubectl create namespace didcomm-mediator-dev
   ```

3. **Deploy**
   ```bash
   helm install didcomm-mediator-dev . \
     -f didcomm-mediator-dev-values.yaml \
     -n didcomm-mediator-dev
   ```

## Validation

4. **Check pod status**
   ```bash
   kubectl get pods -n didcomm-mediator-dev
   # Expected: STATUS=Running, READY=1/1
   ```

5. **Test health endpoint**
   ```bash
   kubectl exec -n didcomm-mediator-dev <pod-name> -- wget -qO- localhost:3000/health
   # Expected: "Accepted"
   ```

6. **View mediator invitation**
   ```bash
   kubectl logs -n didcomm-mediator-dev deployment/didcomm-mediator-dev-didcomm-mediator-credo | grep "invitation url"
   ```

## Deployed Status ✅
- **Service:** ClusterIP `10.0.141.205:3000` (internal only)
- **Storage:** SQLite in-memory (no PostgreSQL)
- **Configuration:** `didcomm-mediator-dev-values.yaml`
