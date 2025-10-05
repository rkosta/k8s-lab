# Kubernetes Lab

A comprehensive collection of Kubernetes manifests and hands-on exercises for learning and mastering Kubernetes concepts, from basic pods to advanced deployment strategies.

## Overview

This repository contains structured learning materials organized by chapters, covering essential Kubernetes concepts through practical YAML manifests. Each example is self-contained and designed to demonstrate specific Kubernetes features and best practices.

## Features

- **Progressive Learning Path**: Organized chapter-by-chapter from basics to advanced topics
- **Production-Ready Examples**: Real-world configurations with resource limits, health checks, and security best practices
- **Hands-on Labs**: Interactive exercises to reinforce learning
- **Troubleshooting Scenarios**: Pre-configured examples for debugging common Kubernetes issues
- **Deployment Strategies**: Blue-green deployments, rolling updates, and advanced rollout patterns
- **Security Best Practices**: Pod security contexts, RBAC, and secure configurations
- **Multi-Container Patterns**: Sidecar containers, init containers, and shared volumes

## Repository Structure

```
k8s-lab/
├── ch1/          # Kubernetes Fundamentals
├── ch4/          # Pods, Security & Troubleshooting
├── ch5/          # Deployments, Jobs & Advanced Strategies
└── ch6/          # Services & Networking
```

### Chapter 1: Kubernetes Fundamentals
- Basic deployment configurations
- Service creation and exposure
- Labels and selectors exercises
- Simple pod definitions

**Key Files:**
- [first-deployment.yaml](ch1/first-deployment.yaml) - Basic 3-replica nginx deployment
- [first-service.yaml](ch1/first-service.yaml) - Service configuration
- [exercise-pod.yaml](ch1/exercise-pod.yaml) - Simple pod example
- [exercise-labels-*.yaml](ch1/) - Label selector exercises

### Chapter 4: Pods, Security & Troubleshooting
- Pod lifecycle and management
- Multi-container pod patterns
- Init containers
- Security contexts and best practices
- Resource requests and limits
- Common troubleshooting scenarios

**Key Files:**
- [lab1-basic-pod.yaml](ch4/lab1-basic-pod.yaml) - Basic pod with resource limits
- [lab2-multi-container.yaml](ch4/lab2-multi-container.yaml) - Writer/reader sidecar pattern
- [lab3-init-container.yaml](ch4/lab3-init-container.yaml) - Init container setup
- [security-best-practices.yaml](ch4/security-best-practices.yaml) - Production security configuration
- [pod-security-standards.yaml](ch4/pod-security-standards.yaml) - PSS compliance examples
- [troubleshoot-crashloop.yaml](ch4/troubleshoot-crashloop.yaml) - CrashLoopBackOff debugging
- [troubleshoot-pending.yaml](ch4/troubleshoot-pending.yaml) - Pending pod scenarios
- [troubleshoot-networking.yaml](ch4/troubleshoot-networking.yaml) - Network issue debugging

### Chapter 5: Deployments, Jobs & Advanced Strategies
- Deployment configurations
- ReplicaSet relationships
- Rolling updates and rollbacks
- Blue-green deployments
- Batch jobs and parallel processing
- Production-grade deployments with health checks

**Key Files:**
- [production-deployment.yaml](ch5/production-deployment.yaml) - Full production configuration with probes, affinity, and resource management
- [basic-deployment.yaml](ch5/basic-deployment.yaml) - Simple deployment example
- [rolling-update-demo.yaml](ch5/rolling-update-demo.yaml) - Update strategies
- [advanced-rollout-strategies.yaml](ch5/advanced-rollout-strategies.yaml) - Blue-green deployment pattern
- [basic-job.yaml](ch5/basic-job.yaml) - Batch job with completions
- [parallel-job.yaml](ch5/parallel-job.yaml) - Parallel processing job
- [deployment-rs-relationship.yaml](ch5/deployment-rs-relationship.yaml) - ReplicaSet relationships

### Chapter 6: Services & Networking
- ClusterIP services
- Service discovery
- Backend/frontend communication patterns

**Key Files:**
- [clusterip-demo.yaml](ch6/clusterip-demo.yaml) - Backend service with client testing

## Prerequisites

- **Kubernetes Cluster**: k3s (recommended) or cloud-based cluster (GKE, EKS, AKS)
- **kubectl**: Included with k3s installation
- **Basic Knowledge**: Understanding of containers and Docker

## Installation

### Kubernetes Cluster Setup with k3s

k3s is a lightweight, production-ready Kubernetes distribution perfect for learning, development, and edge deployments. It uses 40% less memory than standard Kubernetes and comes with everything pre-configured.

**Install k3s**:
```bash
# Install k3s (single-node cluster)
curl -sfL https://get.k3s.io | sh -

# Check installation
sudo k3s kubectl get nodes

# Make kubectl accessible without sudo
sudo chmod 644 /etc/rancher/k3s/k3s.yaml
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $USER:$USER ~/.kube/config

# Verify kubectl works
kubectl get nodes
```

**k3s Features**:
- Single binary < 100MB
- Built-in local storage provider
- Minimal resource requirements (512MB RAM recommended)
- Perfect for Raspberry Pi, edge devices, and development
- Production-ready with HA support

**Uninstall k3s** (if needed):
```bash
# Remove k3s completely
sudo /usr/local/bin/k3s-uninstall.sh
```

### Alternative: Other Kubernetes Distributions

If you prefer different environments:

**Minikube** (local development):
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start
```

**kind** (Kubernetes in Docker):
```bash
go install sigs.k8s.io/kind@latest
kind create cluster --name k8s-lab
```

## Getting Started

1. Clone the repository:
```bash
git clone <repository-url>
cd k8s-lab
```

2. Verify cluster connectivity:
```bash
kubectl cluster-info
kubectl get nodes
```

## Usage

### Applying Manifests

Deploy any example to your cluster:

```bash
# Apply a single manifest
kubectl apply -f ch1/first-deployment.yaml

# Apply all manifests in a chapter
kubectl apply -f ch4/

# Apply with namespace
kubectl apply -f ch5/production-deployment.yaml -n production
```

### Viewing Resources

```bash
# List deployments
kubectl get deployments

# Get pods with labels
kubectl get pods --show-labels

# Describe a resource
kubectl describe pod <pod-name>

# View logs
kubectl logs <pod-name>
kubectl logs -f <pod-name>  # Follow logs

# For multi-container pods
kubectl logs <pod-name> -c <container-name>
```

### Hands-on Labs

**Lab 1: Basic Pod Operations** ([ch4/lab1-basic-pod.yaml](ch4/lab1-basic-pod.yaml))
```bash
# Deploy the pod
kubectl apply -f ch4/lab1-basic-pod.yaml

# Check pod status
kubectl get pods lab1-basic-pod

# Execute commands in the pod
kubectl exec -it lab1-basic-pod -- /bin/bash

# View resource usage
kubectl top pod lab1-basic-pod

# Clean up
kubectl delete -f ch4/lab1-basic-pod.yaml
```

**Lab 2: Multi-Container Pattern** ([ch4/lab2-multi-container.yaml](ch4/lab2-multi-container.yaml))
```bash
# Deploy multi-container pod
kubectl apply -f ch4/lab2-multi-container.yaml

# View logs from writer container
kubectl logs lab2-multi-container -c writer

# View logs from reader container
kubectl logs lab2-multi-container -c reader -f

# Clean up
kubectl delete -f ch4/lab2-multi-container.yaml
```

**Lab 3: Blue-Green Deployment** ([ch5/advanced-rollout-strategies.yaml](ch5/advanced-rollout-strategies.yaml))
```bash
# Deploy blue and green versions
kubectl apply -f ch5/advanced-rollout-strategies.yaml

# Check both deployments
kubectl get deployments -l app=color-demo

# Test the service (initially pointing to blue)
kubectl run test-pod --rm -it --image=busybox -- wget -qO- color-service

# Switch to green deployment
kubectl patch service color-service -p '{"spec":{"selector":{"color":"green"}}}'

# Test again to verify switch
kubectl run test-pod --rm -it --image=busybox -- wget -qO- color-service
```

### Troubleshooting Examples

**CrashLoopBackOff Debugging**:
```bash
kubectl apply -f ch4/troubleshoot-crashloop.yaml
kubectl get pods -w
kubectl logs troubleshoot-crashloop
kubectl describe pod troubleshoot-crashloop
```

**Testing Production Deployment**:
```bash
# Deploy production app
kubectl apply -f ch5/production-deployment.yaml

# Watch rollout status
kubectl rollout status deployment/production-web-app

# Check health probes
kubectl get pods -l app=production-web -o wide
kubectl describe pod <pod-name> | grep -A 5 "Liveness\|Readiness"

# View pod distribution (anti-affinity)
kubectl get pods -l app=production-web -o wide
```

## Configuration

### Resource Requirements

Most examples include resource requests and limits:

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "50m"
  limits:
    memory: "128Mi"
    cpu: "100m"
```

Adjust these based on your cluster capacity. For resource-constrained environments (like Raspberry Pi or small VMs), reduce these values.

### Security Contexts

Examples demonstrate security best practices:

- Running as non-root user
- Read-only root filesystem
- Dropped capabilities
- seccomp profiles
- Pod Security Standards compliance

See [security-best-practices.yaml](ch4/security-best-practices.yaml) for comprehensive security configuration.

## Key Concepts Demonstrated

### Pods
- Single and multi-container pods
- Init containers for setup tasks
- Shared volumes between containers
- Resource constraints
- Security contexts

### Deployments
- Replica management
- Rolling updates with maxUnavailable and maxSurge
- Revision history and rollbacks
- Pod anti-affinity for high availability
- Progressive deadline and backoff limits

### Health Checks
- **Liveness Probes**: Restart unhealthy containers
- **Readiness Probes**: Control traffic routing
- **Startup Probes**: Handle slow-starting applications

### Jobs
- One-time batch processing
- Parallel job execution
- Completion tracking
- Retry policies with backoffLimit

### Services
- ClusterIP for internal communication
- Service discovery via DNS
- Port mapping and named ports

### Best Practices
- Resource requests and limits
- Health check configuration
- Security hardening
- Graceful termination
- Configuration management
- Observability (metrics annotations)

## Development

### Testing Changes

Before applying to production:

```bash
# Dry run to validate syntax
kubectl apply -f <file> --dry-run=client

# Validate against cluster
kubectl apply -f <file> --dry-run=server

# View diff before applying
kubectl diff -f <file>
```

### Cleanup

Remove all resources from a chapter:

```bash
kubectl delete -f ch4/
```

Remove specific resources:

```bash
kubectl delete deployment <name>
kubectl delete pod <name>
kubectl delete service <name>
```

## Common Commands Reference

```bash
# Get resources
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get jobs

# Describe resources
kubectl describe pod <name>
kubectl describe deployment <name>

# Logs
kubectl logs <pod-name>
kubectl logs -f <pod-name>               # Follow
kubectl logs <pod-name> --previous       # Previous instance

# Execute commands
kubectl exec -it <pod-name> -- /bin/sh
kubectl exec <pod-name> -- env

# Port forwarding
kubectl port-forward pod/<pod-name> 8080:80
kubectl port-forward service/<service-name> 8080:80

# Scaling
kubectl scale deployment <name> --replicas=5

# Rollout management
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>

# Debug
kubectl top pods
kubectl top nodes
kubectl get events --sort-by='.lastTimestamp'
```

## Troubleshooting

### Pod Won't Start

1. Check pod status: `kubectl get pods`
2. View events: `kubectl describe pod <name>`
3. Check logs: `kubectl logs <name>`
4. Verify image availability
5. Check resource constraints

### ImagePullBackOff

- Verify image name and tag
- Check image registry accessibility
- Verify imagePullSecrets if using private registry

### CrashLoopBackOff

- Check application logs: `kubectl logs <pod> --previous`
- Verify liveness probe configuration
- Check resource limits
- Review container command/args

### Pending Pods

- Check node resources: `kubectl describe nodes`
- Verify PVC bindings
- Review node selectors and affinity rules
- Check for taints and tolerations

## Learning Resources

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [Kubernetes Best Practices](https://kubernetes.io/docs/concepts/configuration/overview/)
- [Pod Security Standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/)

## License

This project is provided as-is for educational purposes. Feel free to use and modify for learning and training.

## Contributing

This is a personal learning repository. However, if you find issues or have suggestions:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## Acknowledgments

Created as a hands-on learning resource for mastering Kubernetes concepts through practical examples and exercises.
