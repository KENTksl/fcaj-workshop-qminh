---
title: "Blog 5"
date: 2026-06-09
weight: 5
chapter: false
pre: " <b> 3.5. </b> "
---
# Better Together: Amazon EKS Auto Mode and Istio Ambient Mesh

## Problem statement: scale operations and service-to-service security

As a microservices architecture grows from a few services to hundreds, two operational pain points quickly dominate:

- Keeping compute infrastructure running smoothly (patching nodes, scaling, capacity planning)
- Securing service-to-service communication (mTLS, policies, proxy configuration)

This blog summarizes how **Amazon EKS Auto Mode** and **Istio Ambient Mesh** work together to reduce operational overhead while providing automatic mTLS-based service-to-service security.

---

## Understanding the components

### Amazon EKS Auto Mode (compute lifecycle automation)

EKS Auto Mode extends AWS management from the Kubernetes control plane to the **compute layer**, automating the node lifecycle:

- Provisioning, scaling, patching, and updates
- Workloads run on Amazon EC2 managed instances with a hardened container-optimized OS
- Many typical production add-ons are managed as system components, reducing compatibility work

Key parts:

- **Managed instances**: AWS controls node lifecycle, patching, and security configuration
- **Bottlerocket-based OS**: minimal, immutable, container-optimized Linux; immutable root and SELinux help enforce boundaries
- **Built-in system components**: Amazon VPC CNI, kube-proxy, Amazon EBS CSI driver, CoreDNS, AWS Load Balancer Controller (as system processes)
- **Karpenter-powered scaling**: right-sizes instances based on pod requests and scheduling constraints; performs consolidation (and may move to Spot when possible)

### Istio Ambient Mesh (mTLS and policies without sidecars by default)

Ambient Mesh shifts traffic security and policy enforcement out of individual pods (no per-pod sidecars by default), decoupling networking from application lifecycles.

Key parts:

- **istio-cni**: a chained CNI plugin that programs iptables for ambient workloads (coexists with AWS VPC CNI)
- **ztunnel (L3/L4)**: per-node proxy (DaemonSet) that terminates mTLS, enforces L4 policies, and emits TCP telemetry
- **Waypoint proxies (L7, optional)**: per-namespace or per-service-account Deployments for L7 features (routing, retries, timeouts, circuit breaking, L7 auth)
- **HBONE**: protocol used to tunnel traffic between ztunnel and waypoints with mTLS

---

## Solution architecture

### Ambient mode with Layer 4 features (ztunnel only)

Label the namespace (or specific pods) with `istio.io/dataplane-mode=ambient`. Traffic in/out of pods is then intercepted by **ztunnel** at Layer 4, enabling automatic mTLS, L4 authorization policies, and TCP telemetry.

![Istio ambient mode with Layer 4 features (ztunnel)](/images/3-BlogsTranslated/blog5/c-170-1.jpg)

Secure traffic flow (L4):

- Pod startup and detection: istio-cni detects ambient pods and coordinates with the node-local ztunnel
- Traffic redirection: iptables redirects inbound/outbound traffic to ztunnel transparently
- HBONE tunnel: ztunnel establishes HBONE tunnels between source and destination
- Certificate management: ztunnel manages SPIFFE-based X.509 certs for mTLS
- Policy enforcement: destination ztunnel evaluates L4 AuthorizationPolicy before delivering traffic

### Ambient mode with Layer 7 features (waypoint)

When you need HTTP-aware controls (routing, retries, timeouts, L7 auth), deploy a **waypoint proxy** and label workloads (for example `istio.io/use-waypoint=waypoint`) so traffic goes through the waypoint for L7 processing.

![Istio ambient mode with Layer 7 features (waypoint)](/images/3-BlogsTranslated/blog5/c-170-2.jpg)

High-level L7 flow:

- ztunnel captures and tunnels traffic to waypoint via HBONE
- waypoint enforces L7 policies and applies L7 routing behaviors
- traffic then tunnels to the destination ztunnel, which delivers to the destination pod

---

## Implementation guide (hands-on summary)

### Prerequisites

- AWS CLI
- Terraform
- kubectl
- Helm
- envsubst

You also need an AWS account with permissions for EKS clusters, VPCs, IAM roles, and load balancers.

### Deploy EKS Auto Mode + Istio ambient mode

Use the AWS sample:

```bash
git clone https://github.com/aws-samples/sample-istio-ambient-eks-automode.git
cd sample-istio-ambient-eks-automode
cd terraform-blueprint/ambient
terraform init
terraform plan
terraform apply --auto-approve
```

Configure access and verify:

```bash
aws eks --region us-west-2 update-kubeconfig --name ambient
kubectl get pods -A
```

### Observability (Prometheus + Kiali)

```bash
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.28/samples/addons/prometheus.yaml \
  -f https://raw.githubusercontent.com/istio/istio/release-1.28/samples/addons/kiali.yaml

kubectl wait --for=condition=Ready --timeout=240s pods --all -n istio-system
kubectl port-forward svc/kiali 20001:20001 -n istio-system
```

### Deploy a sample microservices app

Deploy a retail store sample app with Helm charts, then install the **Kubernetes Gateway API CRDs** (not included by default) so Istio can manage ingress through Gateway/HTTPRoute.

Label the namespace for ambient mode:

```bash
kubectl label namespace default istio.io/dataplane-mode=ambient
```

Generate traffic and verify in Kiali that service-to-service edges show mTLS (lock icon).

---

## Notes for production

- Configure TLS termination on the Gateway listener (HTTPS + ACM) for production usage.
- Review Istio security advisories and pin versions that meet your security requirements.
- The deployed resources incur cost; clean up when done.

---

## Takeaways for an internship report

- EKS Auto Mode reduces compute-layer toil (nodes) and limits operational overhead on Kubernetes add-ons.
- Istio Ambient Mesh provides automatic mTLS and policy enforcement without forcing every pod into sidecar proxies.
- The combination supports gradual adoption: start with L4 security via ztunnel, add waypoints only where L7 control is needed.
  2. SNS FIFO requires SQS FIFO  
  3. Ability to proactively notify the sender that the message is a duplicate  

---

## Staging ER7 Microservice

- Lambda “trigger” subscribed to the pub/sub hub, filtering messages by attribute  
- Step Functions Express Workflow to convert ER7 → JSON  
- Two Lambdas:  
  1. Fix ER7 formatting (newline, carriage return)  
  2. Parsing logic  
- Result or error is pushed back into the pub/sub hub  

---

## New Features in the Solution

### 1. AWS CloudFormation Cross-Stack References
Example *outputs* in the core microservice:
```yaml
Outputs:
  Bucket:
    Value: !Ref Bucket
    Export:
      Name: !Sub ${AWS::StackName}-Bucket
  ArtifactBucket:
    Value: !Ref ArtifactBucket
    Export:
      Name: !Sub ${AWS::StackName}-ArtifactBucket
  Topic:
    Value: !Ref Topic
    Export:
      Name: !Sub ${AWS::StackName}-Topic
  Catalog:
    Value: !Ref Catalog
    Export:
      Name: !Sub ${AWS::StackName}-Catalog
  CatalogArn:
    Value: !GetAtt Catalog.Arn
    Export:
      Name: !Sub ${AWS::StackName}-CatalogArn
