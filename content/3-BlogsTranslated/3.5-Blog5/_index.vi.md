---
title: "Blog 5"
date: 2026-06-09
weight: 5
chapter: false
pre: " <b> 3.5. </b> "
---

# Better Together: Amazon EKS Auto Mode và Istio Ambient Mesh

## Vấn đề: vận hành Kubernetes và bảo mật service-to-service khi microservices tăng mạnh

Khi hệ thống microservices tăng từ vài services lên hàng trăm, thường phát sinh 2 bài toán tốn nhiều thời gian:

- Vận hành compute infrastructure (patching nodes, scaling cluster, capacity planning)
- Bảo mật service-to-service communication (mTLS, policies, cấu hình proxy)

Blog này tóm tắt cách **Amazon EKS Auto Mode** và **Istio Ambient Mesh** kết hợp để giảm operational overhead, đồng thời cung cấp cơ chế bảo mật service-to-service dựa trên mTLS.

---

## Hiểu các thành phần

### Amazon EKS Auto Mode (tự động hoá vòng đời compute)

EKS Auto Mode mở rộng phần AWS managed từ Kubernetes control plane xuống **compute layer**, tự động hoá node lifecycle:

- Provisioning, scaling, patching, updates
- Workloads chạy trên Amazon EC2 managed instances với OS hardened, container-optimized
- Nhiều thành phần hệ thống thường phải tự cài add-ons được AWS quản lý như system components, giảm việc “canh version”

Các thành phần chính:

- **Managed instances**: AWS kiểm soát lifecycle, patching và security configuration của nodes
- **Bottlerocket-based OS**: minimal, immutable, container-optimized Linux; immutable root filesystem + SELinux giúp tăng security boundaries
- **Built-in system components**: Amazon VPC CNI, kube-proxy, Amazon EBS CSI driver, CoreDNS, AWS Load Balancer Controller (chạy như system processes)
- **Karpenter-powered scaling**: right-size instances theo pod resource requests/scheduling constraints, có consolidation và có thể tối ưu bằng Spot khi phù hợp

### Istio Ambient Mesh (mTLS/policy mà không bắt buộc sidecar)

Ambient Mesh đẩy security/policy ra khỏi từng pod (không bắt buộc sidecar cho mọi workload), tách networking khỏi vòng đời ứng dụng.

Các thành phần chính:

- **istio-cni**: chained CNI plugin, chương trình hoá iptables cho ambient workloads (coexist với AWS VPC CNI)
- **ztunnel (L3/L4)**: per-node proxy (DaemonSet) terminate mTLS, enforce L4 policies, và emit TCP telemetry
- **Waypoint proxies (L7, optional)**: Deployment theo namespace hoặc service account để có L7 features (routing, retries, timeouts, circuit breaking, L7 authorization)
- **HBONE**: protocol để tunnel traffic giữa ztunnel và waypoint với mTLS

---

## Kiến trúc giải pháp

### Ambient mode với Layer 4 features (ztunnel)

Gắn label `istio.io/dataplane-mode=ambient` cho namespace (hoặc pod) để đưa workloads vào ambient mesh. Từ đó traffic in/out của pod sẽ bị intercept ở Layer 4 bởi **ztunnel**, cho phép automatic mTLS, L4 AuthorizationPolicy và TCP observability.

![Istio ambient mode với Layer 4 features (ztunnel)](/images/3-BlogsTranslated/blog5/c-170-1.jpg)

Secure traffic flow (L4) ở mức khái quát:

- Pod startup/detection: istio-cni phát hiện ambient pods và phối hợp với ztunnel trên node
- Traffic redirection: iptables redirect traffic vào listeners của ztunnel một cách trong suốt
- HBONE tunnel: ztunnel tạo tunnel giữa source và destination
- Certificate management: ztunnel quản lý SPIFFE-based X.509 certificates để mTLS
- Policy enforcement: ztunnel phía đích evaluate L4 AuthorizationPolicy trước khi deliver traffic

### Ambient mode với Layer 7 features (waypoint)

Khi cần các tính năng HTTP-aware (routing, retries, timeouts, L7 authorization), triển khai **waypoint proxy** và gắn label (ví dụ `istio.io/use-waypoint=waypoint`) để traffic đi qua waypoint cho L7 processing.

![Istio ambient mode với Layer 7 features (waypoint)](/images/3-BlogsTranslated/blog5/c-170-2.jpg)

Luồng L7 ở mức khái quát:

- ztunnel capture và tunnel traffic đến waypoint qua HBONE
- waypoint xử lý L7 policies và routing behaviors
- traffic được tunnel sang ztunnel phía đích rồi deliver đến destination pod

---

## Implementation guide (tóm tắt thực hành)

### Prerequisites

- AWS CLI
- Terraform
- kubectl
- Helm
- envsubst

Cần AWS account có quyền tạo EKS clusters, VPCs, IAM roles và load balancers.

### Deploy EKS Auto Mode + Istio ambient mode

Sử dụng sample repo:

```bash
git clone https://github.com/aws-samples/sample-istio-ambient-eks-automode.git
cd sample-istio-ambient-eks-automode
cd terraform-blueprint/ambient
terraform init
terraform plan
terraform apply --auto-approve
```

Cấu hình kubeconfig và kiểm tra pods:

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

### Deploy sample app và kiểm tra mTLS

Deploy retail store sample app, cài Kubernetes Gateway API CRDs để cấu hình Gateway/HTTPRoute, sau đó label namespace để enable ambient mode:

```bash
kubectl label namespace default istio.io/dataplane-mode=ambient
```

Tạo traffic và dùng Kiali Traffic Graph để xác nhận các edges giữa microservices có mTLS (lock icon).

---

## Lưu ý cho production

- Nên cấu hình TLS termination trên Gateway listener (HTTPS + ACM) cho môi trường production.
- Review Istio security advisories và pin version phù hợp.
- Các resources phát sinh chi phí; nên cleanup sau khi thực hành xong.

---

## Kết luận rút ra cho báo cáo thực tập

- EKS Auto Mode giảm “toil” ở compute layer (node lifecycle) và giảm gánh quản lý add-ons.
- Istio Ambient Mesh giúp có mTLS/policies mà không bắt buộc sidecar cho mọi workload.
- Có thể triển khai incremental: bắt đầu L4 security bằng ztunnel, chỉ thêm waypoints nơi cần L7 controls.
  1. SNS deduplication TTL chỉ 5 phút
  2. SNS FIFO yêu cầu SQS FIFO
  3. Chủ động báo cho sender biết message là bản sao

---

## Staging ER7 microservice

- Lambda “trigger” đăng ký với pub/sub hub, lọc message theo attribute  
- Step Functions Express Workflow để chuyển ER7 → JSON  
- Hai Lambda:
  1. Sửa format ER7 (newline, carriage return)
  2. Parsing logic  
- Kết quả hoặc lỗi được đẩy lại vào pub/sub hub

---

## Tính năng mới trong giải pháp

### 1. AWS CloudFormation cross-stack references
Ví dụ *outputs* trong core microservice:
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
