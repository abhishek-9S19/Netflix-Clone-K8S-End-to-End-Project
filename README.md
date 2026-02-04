## CI Baseline (Frozen)

This project has a stable CI pipeline implemented using Jenkins with the following responsibilities:

### What CI DOES
- Checks out code from GitHub
- Installs Node.js dependencies
- Builds a Docker image for the frontend application
- Pushes the image to Docker Hub
- Runs Trivy image scan for vulnerability visibility (non-blocking)

### What CI DOES NOT DO (Intentionally)
- Does NOT deploy to Kubernetes
- Does NOT run kubectl from Jenkins
- Does NOT use cluster-admin credentials in CI

### Rationale
CI and CD are intentionally separated.
Kubernetes deployment requires authenticated access and RBAC, which is handled
in a dedicated CD phase to follow real-world DevOps practices.

This avoids insecure patterns such as embedding kubeconfig or admin tokens
directly in Jenkins pipelines.

