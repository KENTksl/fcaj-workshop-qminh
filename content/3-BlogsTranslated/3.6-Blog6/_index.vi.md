---
title: "Blog 6"
date: 2026-01-27
weight: 6
chapter: false
pre: " <b> 3.6. </b> "
---

# AWS: Vì sao agentic AI là bước ngoặt cho enterprise modernization

## Tổng quan: hiện đại hoá nhanh hơn nhiều lần với chi phí thấp hơn

Bài viết xem **agentic AI** như một điểm bẻ lái quan trọng của enterprise modernization. Ý chính là: doanh nghiệp có thể hiện đại hoá hệ thống nhanh hơn rất nhiều và giảm mạnh chi phí khi dùng AI agents để xử lý technical debt, migration và modernization work vốn trước đây tiêu tốn nhiều tháng cùng nhiều nhóm kỹ sư.

Tính cấp bách về mặt kinh doanh được nêu rất rõ:

- Technical debt ở Mỹ được ước tính khoảng **2,41 nghìn tỷ USD mỗi năm**
- Cần khoảng **1,52 nghìn tỷ USD** để xử lý
- Doanh nghiệp nào tận dụng được intelligent agents cho bài toán này sẽ không chỉ giảm chi phí mà còn tạo ra lợi thế cạnh tranh về time-to-market, business intelligence và tốc độ đổi mới

AWS định vị **AWS Transform** là dịch vụ agentic AI đầu tiên được xây riêng cho enterprise transformation.

---

## Vì sao thời điểm này đặc biệt quan trọng

Lãnh đạo doanh nghiệp hiện chịu áp lực phải tăng tốc với AI, nhưng để khai thác AI ở quy mô lớn thì bước nền tảng thường vẫn là **migration và modernization lên cloud**. Legacy applications, hạ tầng cũ và các cấu hình bảo mật/network lâu năm chính là thứ làm chậm toàn bộ quá trình.

Bài viết đưa ra một ví dụ thực tế: một khách hàng có **6.000 virtual machines** và giấy phép VMware sắp hết hạn. Vấn đề không chỉ là chuyển server, mà còn phải xử lý:

- Các cấu hình mạng tích luỹ trong nhiều năm
- Security policies và permissions
- Mapping dependencies giữa các môi trường

Theo cách truyền thống, đây là kiểu công việc mất nhiều tuần, phụ thuộc bảng tính và rất dễ sai sót. AWS dùng ví dụ này để giải thích vì sao cần một cách tiếp cận mới.

---

## Mô hình 3 lớp của AWS Transform

AWS Transform được mô tả là hệ thống 3 lớp. Đây là điểm phân biệt nó với automation đơn thuần hoặc chatbot tổng quát.

### Layer 1: Specialized AI agents

Đây là các agent chuyên biệt cho từng bài toán transformation, ví dụ:

- Modernize các mainframe languages như **COBOL** sang ngôn ngữ hiện đại như **Java**
- Migrate các cấu hình mạng phức tạp từ VMware sang AWS
- Nâng cấp các ứng dụng **.NET** cũ lên framework versions mới hơn

### Layer 2: Agentic AI coordination system

Lớp thứ hai điều phối toàn bộ công việc. Nó quyết định:

- Gọi agent nào
- Task nào có thể chạy song song
- Dependencies nào cần được sắp xếp thứ tự

Ở ví dụ VMware networking, AWS cho biết hệ thống đã điều phối đồng thời các agents cho **network discovery**, **security mapping** và **validation**, giúp rút ngắn công việc từ **hai tuần** xuống còn **tám giờ** với độ chính xác cao hơn làm tay.

### Layer 3: Natural language collaboration interface

Lớp thứ ba là giao diện ngôn ngữ tự nhiên, giúp các nhóm kỹ thuật và business làm việc cùng nhau dễ hơn. Nó giúp giảm khoảng cách giữa:

- Business stakeholders
- Architects
- Developers

Nhờ vậy, các quyết định kỹ thuật phức tạp có thể được giải thích và thảo luận theo ngôn ngữ kinh doanh rõ ràng hơn.

---

## Kinh tế mới của modernization

Bài viết cho rằng agentic AI làm thay đổi bài toán kinh tế của transformation:

- Những dự án vốn mất **12 đến 18 tháng** có thể hoàn thành trong **vài tuần**
- AWS ghi nhận tốc độ transformation tổng thể khoảng **nhanh hơn 5 lần**
- Với VMware migration, mức tăng tốc có thể lên tới **80 lần** khi hệ thống chạy song song nhiều tác vụ

Điểm quan trọng là AWS không xem đây chỉ là tiết kiệm thời gian, mà là một mô hình vận hành mới cho modernization.

---

## Các ví dụ khách hàng và tác động đo được

### Experian

Experian cần modernize 7 internal applications đang chạy trên các phiên bản .NET cũ. Dùng AWS Transform, họ tự động hoá việc chuyển sang **.NET 8** trên cloud infrastructure và đạt được:

- Giảm khoảng **40% developer effort**
- Tiết kiệm khoảng **300 engineering days** cho 7 projects

Giá trị ở đây là doanh nghiệp không cần kéo developers khỏi các dự án có tác động cao khác để xử lý modernization.

### CSL

CSL, một công ty công nghệ sinh học toàn cầu, dùng AWS Transform để tăng tốc planning cho **5.000 servers tại 29 data centers**. Bài viết nêu:

- **Nhanh hơn 10 lần** ở khâu planning
- Application discovery giảm từ **hàng giờ** xuống còn **5 phút**

Tác động kinh doanh được nối trực tiếp với khả năng đưa thuốc cứu người đến bệnh nhân nhanh hơn.

### BMW Group

BMW Group dùng AWS Transform cho mainframe-related work và đạt:

- Giảm **75% thời gian testing**
- Tăng **60% độ phủ test**

Trường hợp này cho thấy agentic AI có thể vừa giảm rủi ro vừa tăng tốc modernization.

### Air Canada

Air Canada có technical debt ở hàng nghìn Lambda functions đang dùng runtime đã hết vòng đời hỗ trợ. Platform team dùng AWS Transform để nâng cấp **Node.js 16 lên 20** chỉ trong vài ngày, đạt:

- **90% efficacy**
- Giảm **80% thời gian và chi phí dự kiến**

---

## Modernization như một năng lực liên tục

Một ý rất quan trọng của bài là modernization không nên còn bị xem là dự án “một lần rồi thôi”. Frameworks sẽ tiếp tục thay đổi, security requirements luôn tiến hoá, và business priorities cũng liên tục cập nhật. AWS mô tả một tương lai nơi adaptation trở thành hoạt động liên tục thay vì disruptive.

Bài viết cũng đề cập tới khả năng **composability** mới, cho phép partners đưa vào:

- Industry-specific data
- Purpose-built agents
- Internal knowledge

Điều này mở ra khả năng mở rộng hệ thống ra ngoài các use case tổng quát để phù hợp với từng ngành và từng doanh nghiệp.

---

## Con người vẫn giữ vai trò trung tâm

Dù agentic AI xử lý ngày càng nhiều phần execution phức tạp, bài viết nhấn mạnh rằng vai trò con người trở nên **quan trọng hơn chứ không ít đi**. Kết quả tốt nhất đến từ việc kết hợp:

- Khả năng thực thi của AI
- Phán đoán của con người
- Khả năng nhận diện pattern giữa các bài toán tương tự
- Tư duy chiến lược và ưu tiên kinh doanh

Trong mô hình này, con người tập trung vào quyết định chiến lược còn AI đảm nhận phần lớn execution work.

---

## Vì sao cần hành động ngay

Thông điệp kết thúc mang tính cạnh tranh rõ rệt: tổ chức nào triển khai modernization bằng agentic AI sớm sẽ xây được lợi thế vận hành tích luỹ theo thời gian. Cơ hội không chỉ là giảm chi phí, mà còn là giải phóng nguồn lực khỏi legacy maintenance để chuyển sang xây dựng các năng lực AI mới.

AWS định vị **AWS Transform** là cách để:

- Giảm technical debt
- Tăng tốc migration và modernization
- Chuẩn bị toàn bộ technology stack cho làn sóng AI

Thông điệp cuối cùng là “cửa sổ” lợi thế cạnh tranh đang mở ngay lúc này, nhưng sẽ không mở mãi.

---

## Kết luận rút ra cho báo cáo thực tập

- Agentic AI khác với automation thông thường vì nó kết hợp specialized agents, orchestration và natural-language collaboration.
- AWS Transform hướng tới enterprise migration và modernization như một năng lực liên tục, không phải một dự án one-off.
- Bằng chứng thuyết phục nhất trong bài đến từ customer outcomes: planning nhanh hơn, giảm developer effort, giảm chi phí, rút ngắn testing cycles.
- Giá trị chiến lược không chỉ là tốc độ modernization, mà là dùng tốc độ đó để mở khoá khả năng cạnh tranh và triển khai AI rộng hơn trong doanh nghiệp.
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
