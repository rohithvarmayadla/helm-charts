# DIDComm Mediator Credo - Implementation Guide

## Prerequisites
- Kubernetes 1.19+, Helm 3.2.0+
- CrunchyData PostgreSQL Operator (for production deployment)
- For ingress: cert-manager, external-dns, ingress controller

## Installation

1. **Deploy PostgreSQL** (production):
   - Create PostgresCluster resource with CrunchyData operator
   - Configure HA: 2+ replicas with anti-affinity
   - Enable PgBouncer for connection pooling
   - Configure backups with pgBackRest

2. **Create namespace**: `kubectl create namespace <namespace>`

3. **Deploy mediator**:
   ```bash
   helm install <release-name> . -f <values-file>.yaml -n <namespace>
   ```

## Validation

4. **Check pod**: `kubectl get pods -n <namespace>` (Expected: Running, 1/1)
5. **Test health**: `curl https://<your-domain>/health` (Expected: "Accepted")
6. **Verify endpoints**: Check logs for both HTTPS and WSS endpoints
7. **Check certificate** (if ingress enabled): `kubectl get certificate -n <namespace>`

## Production Configuration

### Storage
- **Type**: PostgreSQL persistent storage
- **Pickup Type**: `postgres` (required for message persistence)
- **Pickup Strategy**: `QueueAndLiveModeDelivery` (resilient, production-ready)
- **Connection**: Use PostgreSQL HA endpoint for reliability

### Endpoints
- **HTTPS**: For standard HTTP requests
- **WSS**: For WebSocket connections (real-time message pickup)
- Both protocols supported on same port (3000)

### Pickup Strategies
- `DirectDelivery`: Immediate delivery only
- `QueueOnly`: Store and retrieve manually
- `QueueAndLiveModeDelivery`: Try direct, fallback to queue (recommended)

### Push Notifications
- Requires Firebase configuration (project-id, client-email, private-key)
- Set `USE_PUSH_NOTIFICATIONS: "true"` to enable
- Update secret: `<release-name>-agent` with Firebase credentials

### Important Notes
- Template fix required: Move `env:` key outside `{{- with .Values.environment }}` block in deployment.yaml
- Use PostgreSQL HA endpoint instead of PgBouncer if encountering pg_hba.conf issues
- Set `POSTGRES_ADMIN_USER` to mediator user (with CREATEDB privileges)
- Secret `pickup-connection-json` should contain: `{"useBaseConnection":true}`
