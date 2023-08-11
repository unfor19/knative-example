# knative-example

## Requirements

- [Docker](https://docs.docker.com/get-docker/)
- [https://knative.dev/docs/getting-started/](https://knative.dev/docs/getting-started/)
- [minikube version v1.31.1+](https://minikube.sigs.k8s.io/docs/start/) to support Kubernetes version [1.27](https://kubernetes.io/blog/2023/04/11/kubernetes-v1-27-release/)
- Knative CLI
  ```bash
  brew install knative/client/kn
  ```
- Install Knative Quickstart
  ```bash
  brew install knative-sandbox/kn-plugins/quickstart
  ```
- Install Knative Functions plugin - [https://github.com/knative/func/releases](https://github.com/knative/func/releases)

## Getting Started

1. Start Kubernetes cluster locally with `minikube`

   ```bash
   kn quickstart minikube --kubernetes-version 1.27
   ```

1. Start a separated terminal for a tunnel
   ```bash
   minikube tunnel --profile knative
   ```
1. Create function
   ```bash
   kn-func create -l node knative-example
   ```
1. Install dependencies locally
   ```bash
   npm install
   ```
1. Run the function
   ```bash
   cd knative-example && kn-func run --registry docker.io/unfor19
   ```
1. Test the function
   ```bash
   cd knative-example && npm test
   ```
1. Rebuild the function Docker image
   ```bash
   cd knative-example && kn-func build --registry docker.io/unfor19
   ```
1. Push the function to the Kubernetes cluster
   ```bash
   cd knative-example && kn-func deploy --build --registry docker.io/unfor19
   ```
1. Create the function as a service for the first time
   ```bash
   kn service create knative-example \
    --image docker.io/unfor19/knative-example:latest \
    --port 8080 \
    --env TARGET=initial-value
   ```
1. Test the deployed service [http://knative-example.default.127.0.0.1.sslip.io?hello=world](http://knative-example.default.127.0.0.1.sslip.io)
   ```
   cd knative-example && npm test
   ```
   ```
   curl -L http://knative-example.default.127.0.0.1.sslip.io?hello=world
   ```
1. Update the service
   ```bash
   kn service update knative-example \
    --image docker.io/unfor19/knative-example:latest \
    --port 8080 \
    --env TARGET=updated
   ```
