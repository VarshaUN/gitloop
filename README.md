# gitloop

Local CI/CD → GitOps loop: build, push, sync. No cloud

gitloop is a fully local CI/CD and GitOps workflow setup. The main idea here is to see how a code change automatically triggers a build, packages the application into a container image, pushes it to a local registry, and syncs those updates directly to a local Kubernetes cluster—all running on your machine without relying on external cloud services.
