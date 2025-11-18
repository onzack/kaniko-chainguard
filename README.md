# Kaniko Container Image

This repository contains a up-to-date container image of [Kaniko](https://github.com/GoogleContainerTools/kaniko), based on the forked source code that is being maintained and published by [Chainguard](https://github.com/chainguard-dev/kaniko).

## Current Version

**Latest Build:** `latest`  
**Commit SHA:** `2d06870`  
**Build Date:** `2025-11-18`  
**Source Repository:** [chainguard-dev/kaniko](https://github.com/chainguard-dev/kaniko)

> This version information is automatically updated when a new image is built.

## What is Kaniko?

Kaniko is a tool to build container images from a Dockerfile, inside a container or Kubernetes cluster. It doesn't depend on a Docker daemon and executes each command in a Dockerfile completely in userspace. This enables building container images in environments that can't easily or securely run a Docker daemon, such as a standard Kubernetes cluster.

## Usage

### Gitlab CI pipeline
```
kaniko-build-image:
  stage: build
  image:
    name: ghcr.io/onzack/chainguard-kaniko-debug:latest
    entrypoint: [""]
    pull_policy: always
  before_script:
    - cat <ADDITIONAL CA CERT> >> /kaniko/ssl/certs/additional-ca-cert-bundle.crt
    - |
        mkdir -p /kaniko/.docker
        cat <<EOF > /kaniko/.docker/config.json
        {
          "auths": {
            "<REGISTRY URL>": {
              "auth": "$(echo -n <REGISTRY USER>:<REGISTRY USER PASSWORD> | base64)"
             }
          }
        }
      EOF
      
  script:
    - /kaniko/executor
      --context ${CI_PROJECT_DIR}
      --dockerfile Dockerfile
      --cache=true
      --cache-run-layers
      --cache-copy-layers
      --no-push=false
      --destination <REGISTRY URL>/<IMAGE PATH AND NAME>:<IMAGE TAG>
```

## Contributing

This repository contains a container image based on the upstream Chainguard Kaniko project. For contributions related to Kaniko itself, please refer to:

- [Chainguard Kaniko Repository](https://github.com/chainguard-dev/kaniko)
- [Original Kaniko Repository](https://github.com/GoogleContainerTools/kaniko)

## Support

For issues specific to this container image, please open an issue in this repository. For general Kaniko questions or issues, please refer to the upstream repositories.

## License

This project is licensed under the same terms as the upstream Kaniko project. Please refer to the [Chainguard Kaniko license](https://github.com/chainguard-dev/kaniko/blob/main/LICENSE) for details.

## Related Links

- [Kaniko Documentation](https://github.com/GoogleContainerTools/kaniko)
- [Chainguard Kaniko Repository](https://github.com/chainguard-dev/kaniko)
