## Container Documentation for Drupal Documentation

The CleanStart Drupal image provides a production-ready, security-hardened container optimized for enterprise environments. Built on a minimal base OS with comprehensive security hardening, this image delivers reliable application execution with advanced security features.

📌 **Base Foundation**: Production-ready container from cleanstart.

**Image Path**: `ghcr.io/cleanstart-containers/drupal`

**Registry**: cleanstart Registry

## Pull Latest Image
Download the container image from the registry

```bash
docker pull ghcr.io/cleanstart-containers/drupal:latest
```
```bash
docker pull ghcr.io/cleanstart-containers/drupal:latest-dev
```

CleanStart’s Drupal image runs as a non-root user (uid 65532) for better security — but the baked-in log directory is owned by root, so Apache can’t write to it and crashes on startup.
The volume mount gives Apache a log directory it actually owns.

## Production Deployment
Deploy with production security settings

```bash
mkdir -p ./logs && sudo chown 65532:65532 ./logs

docker run -d --name drupal-prod \
  --security-opt=no-new-privileges \
  --user 65532:65532 \
  --restart unless-stopped \
  -v "$(pwd)/logs:/var/log/apache2" \
  -p 80:80 \
  ghcr.io/cleanstart-containers/drupal:latest-dev
```

Volume Mount Mount local directory for persistent data

```bash
mkdir -p ./logs && sudo chown 65532:65532 ./logs

docker run -it --rm --name drupal \
  -v "$(pwd)/logs:/var/log/apache2" \
  ghcr.io/cleanstart-containers/drupal:latest-dev
```

Port Forwarding Run with custom port mappings

```bash
docker run -it --rm --name drupal \
  -p 8080:80 \
  -v "$(pwd)/logs:/var/log/apache2" \
  ghcr.io/cleanstart-containers/drupal:latest-dev
```

## Kubernetes Security Context
Recommended security context for Kubernetes deployments

```yaml
securityContext:
  allowPrivilegeEscalation: false
  capabilities:
    drop:
    - ALL
  readOnlyRootFilesystem: true
  runAsUser: 65532
  runAsGroup: 65532
```

## Documentation Resources
Essential links and resources for further information

- **Container Registry**: [https://www.cleanstart.com/](https://www.cleanstart.com/)
- **CleanStart Community Images**: [https://hub.docker.com/u/cleanstart](https://hub.docker.com/u/cleanstart)
- **How-to-Run CleanStart images & sample projects**: [https://github.com/cleanstart-containers](https://github.com/cleanstart-containers)
  - How to run sample projects using Dockerfile
  - How to deploy via Kubernetes YAML
  - How to migrate from public images to CleanStart images

---

**Vulnerability Disclaimer**

CleanStart offers Docker images that include third-party open-source libraries and packages maintained by independent contributors. While CleanStart maintains these images and applies industry-standard security practices, it cannot guarantee the security or integrity of upstream components beyond its control.

Users acknowledge and agree that open-source software may contain undiscovered vulnerabilities or introduce new risks through updates. CleanStart shall not be liable for security issues originating from third-party libraries, including but not limited to zero-day exploits, supply chain attacks, or contributor-introduced risks.

Security remains a shared responsibility: CleanStart provides updated images and guidance where possible, while users are responsible for evaluating deployments and implementing appropriate controls.
