---
title: "Blog 3"
date: 2026-07-07
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Hiện đại hóa ứng dụng và cơ sở dữ liệu với GenAI trên AWS

Trong quá trình phát triển phần mềm, nhiều doanh nghiệp hiện nay vẫn đang vận hành các hệ thống cũ theo kiến trúc monolithic. Ở giai đoạn đầu, mô hình này thường khá dễ triển khai, dễ quản lý và phù hợp với quy mô nhỏ. Tuy nhiên, khi hệ thống bắt đầu mở rộng, lượng người dùng tăng lên và yêu cầu kinh doanh thay đổi liên tục, kiến trúc monolithic dần bộc lộ nhiều hạn chế như thời gian phát hành chậm, khó mở rộng từng phần, chi phí vận hành cao và dễ gây ảnh hưởng dây chuyền khi có sự cố xảy ra.
Vì vậy, hiện đại hóa ứng dụng không chỉ là việc thay đổi công nghệ hay nâng cấp mã nguồn, mà còn là quá trình thay đổi cách tư duy trong thiết kế, vận hành và phát triển hệ thống.
Bài blog này được tổng hợp dựa trên nội dung từ nhóm Otokoshi, tập trung vào giải pháp **GenAI-powered App & Database Modernization**. Thay vì chỉ dừng lại ở việc “nâng cấp code”, quá trình hiện đại hóa là sự kết hợp giữa **Domain-Driven Design (DDD)**, **microservices**, **event-driven architecture**, **serverless computing** và các công cụ AI như **Amazon Q Developer** hoặc **AWS Transform**. Những công cụ này có thể hỗ trợ phân tích, chuyển đổi và tối ưu hệ thống từ tầng kiến trúc cho đến mã nguồn và cơ sở dữ liệu.

## Hướng dẫn kiến trúc

Điểm thay đổi quan trọng trong quá trình hiện đại hóa là chuyển từ một khối monolithic lớn sang hệ sinh thái gồm nhiều dịch vụ nhỏ hơn, tức là microservices. Các service này được thiết kế độc lập, kết nối lỏng lẻo với nhau thông qua các luồng sự kiện theo mô hình event-driven.
Thay vì viết lại toàn bộ hệ thống từ đầu, cách tiếp cận an toàn hơn là bóc tách từng phần dựa trên ranh giới nghiệp vụ. Nhờ vậy, doanh nghiệp có thể giảm rủi ro trong quá trình chuyển đổi, đồng thời từng bước đưa các thành phần phù hợp sang mô hình serverless hoặc microservices.

**Kiến trúc giải pháp hiện đại hóa có thể được hình dung như sau:**

![Hình 1. Sự dịch chuyển kiến trúc từ Monolithic sang Microservices kết hợp Event-driven và Serverless](images/3-BlogsTranslated/Blog3/blog3(0).jpg)

> *Hình 1. Sự dịch chuyển kiến trúc từ Monolithic sang Microservices kết hợp Event-driven và Serverless.*

Khi xác định ranh giới cho các thành phần trong hệ thống mới, hệ thống thường có những đặc điểm sau:
* Bắt đầu từ business domain, tức là nghiệp vụ kinh doanh, trước khi lựa chọn công nghệ triển khai.
* Mỗi chức năng quan trọng được tách thành một service độc lập và có khả năng tự vận hành.
* Các service giao tiếp linh hoạt, bất đồng bộ thông qua event thay vì phụ thuộc hoàn toàn vào API call trực tiếp.
* Mã nguồn có thể được triển khai và vận hành mà không cần quản lý trực tiếp máy chủ vật lý hoặc máy ảo bên dưới.
Khi xây dựng chiến lược bóc tách hệ thống monolithic, cần cân nhắc một số yếu tố quan trọng:
* **Nghiệp vụ**: Xác định các domain chính của hệ thống như quản lý đơn hàng, thanh toán, khách hàng hoặc kho hàng.
* **Kiến trúc**: Sử dụng Bounded Context để chia nhỏ hệ thống, từ đó quyết định phần nào tiếp tục giữ lại và phần nào nên tách thành microservice.
* **Lộ trình**: Ưu tiên hiện đại hóa từng phần thay vì thay đổi toàn bộ hệ thống trong một lần, nhằm giảm rủi ro và dễ kiểm soát tiến độ.

## Lựa chọn công nghệ và mô hình tính toán

| Khía cạnh hệ thống / Mức độ kiểm soát | Các dịch vụ AWS phù hợp / Mô hình tính toán |
| --- | --- |
| Cần kiểm soát chặt chẽ hệ điều hành, runtime và cấu hình | Amazon Elastic Compute Cloud (Amazon EC2) |
| Ứng dụng container, microservices và cần orchestration | Amazon Elastic Container Service (ECS), Amazon EKS |
| Chạy container nhưng không muốn quản lý máy chủ bên dưới | AWS Fargate |
| Event-driven, xử lý tác vụ ngắn, API nhỏ, automation | AWS Lambda |

## Domain-Driven Design (DDD)

![Quy trình hiện đại hóa ứng dụng 5 bước](images/3-BlogsTranslated/Blog3/blog3(1).jpg)

Một sai lầm khá phổ biến khi hiện đại hóa hệ thống là bắt đầu bằng câu hỏi: “Nên dùng Lambda, ECS hay Kubernetes?”. Tuy nhiên, câu hỏi quan trọng hơn cần được đặt ra trước tiên là: “Hệ thống này đang phục vụ nghiệp vụ gì?”.
**Domain-Driven Design (DDD)** giúp đội ngũ kỹ thuật và đội ngũ nghiệp vụ có thể trao đổi với nhau bằng một ngôn ngữ chung. Thông qua các buổi *event storming*, nhóm phát triển có thể xác định rõ:
* Những sự kiện nghiệp vụ quan trọng và các actor tham gia.
* Timeline của các bước xử lý chính trong hệ thống.
* Các bounded context cần được bóc tách để phục vụ cho quá trình hiện đại hóa.
Tuy nhiên, DDD cũng có một số thách thức nhất định. Phương pháp này đòi hỏi sự phối hợp liên tục giữa *domain expert* (chuyên gia nghiệp vụ) và *software expert* (chuyên gia phần mềm). Hai bên cần cùng nhau phân tích, trao đổi và làm rõ sự phức tạp của nghiệp vụ trước khi bắt đầu triển khai code.

## Event-Driven Architecture

Event-Driven Architecture giúp giải quyết bài toán giao tiếp giữa các thành phần trong hệ thống. Nếu các microservices liên tục gọi nhau bằng API đồng bộ, hệ thống rất dễ rơi vào tình trạng phụ thuộc chéo. Khi một service gặp lỗi hoặc bị gián đoạn, các service khác cũng có thể bị ảnh hưởng theo.
Với mô hình event-driven, cách xử lý sẽ linh hoạt hơn:
* Một service, ví dụ như Order Service, chỉ cần phát ra sự kiện **OrderCreated**.
* Các service khác như Payment, Inventory hoặc Notification sẽ độc lập lắng nghe sự kiện đó.
* Việc điều phối sự kiện có thể được thực hiện thông qua các dịch vụ như **Amazon EventBridge**, **Amazon SNS** hoặc **Amazon SQS**.

> Cách tiếp cận này giúp hệ thống linh hoạt hơn, dễ mở rộng hơn và giảm sự phụ thuộc trực tiếp giữa các thành phần. Nhờ đó, kiến trúc tổng thể trở nên loosely-coupled và ổn định hơn khi vận hành ở quy mô lớn.

## Compute Evolution: Chuyển dịch sang Serverless

Một hướng đi quan trọng trong hiện đại hóa ứng dụng là chuyển dần sang mô hình serverless. Với mô hình này, đội ngũ phát triển không cần dành quá nhiều thời gian cho việc quản lý hạ tầng bên dưới.
Các công việc như duy trì máy chủ, scaling, patching hoặc quản lý capacity sẽ được AWS đảm nhiệm. Nhờ vậy, lập trình viên có thể tập trung nhiều hơn vào logic nghiệp vụ và tốc độ phát triển tính năng.
Một dịch vụ tiêu biểu trong hướng tiếp cận này là **AWS Lambda**:
1. Phù hợp với các workload theo mô hình event-driven.
2. Tối ưu chi phí nhờ khả năng tự động mở rộng và chỉ tính phí theo mức sử dụng thực tế.
3. Giúp lập trình viên tập trung vào xử lý nghiệp vụ thay vì phải quản lý server.

## Vai trò của GenAI trong giải pháp

### 1. Amazon Q Developer và AWS Transform

GenAI không thể thay thế hoàn toàn vai trò của kiến trúc sư phần mềm. Tuy nhiên, trong quá trình hiện đại hóa hệ thống, GenAI có thể đóng vai trò như một trợ lý kỹ thuật mạnh mẽ, giúp rút ngắn thời gian phân tích, chuyển đổi và tối ưu mã nguồn.

![Sự hỗ trợ của GenAI thông qua Amazon Q Developer và AWS Transform](images/3-BlogsTranslated/Blog3/blog3(2).jpg)

* **Amazon Q Developer**: Hỗ trợ phân tích codebase cũ, gợi ý cách bóc tách module, tạo tài liệu kỹ thuật, đề xuất test case và hỗ trợ refactor. Ví dụ, công cụ này có thể giúp nâng cấp phiên bản Java, giải thích dependency hoặc đề xuất cách cải thiện cấu trúc mã nguồn.
* **AWS Transform**: Là dịch vụ agentic AI hỗ trợ hiện đại hóa ở quy mô lớn, đặc biệt phù hợp với các workload phức tạp như Windows, mainframe hoặc VMware.
Ví dụ dưới đây minh họa một bản kế hoạch chuyển đổi dạng diff do AI đề xuất. Mục tiêu là thay thế lời gọi API đồng bộ trong hệ thống cũ bằng mô hình phát sự kiện thông qua EventBridge:

```diff
- // Legacy Monolithic Synchronous Call
- paymentService.processPayment(order.getId(), order.getTotal());
- inventoryService.deductItems(order.getItems());

+ // Modern Event-Driven Architecture with EventBridge
+ PutEventsRequestEntry eventEntry = PutEventsRequestEntry.builder()
+    .source("com.ecommerce.orders")
+    .detailType("OrderCreated")
+    .detail(orderJson)
+    .eventBusName("EcommerceEventBus")
+    .build();
+ eventBridgeClient.putEvents(PutEventsRequest.builder().entries(eventEntry).build());
```