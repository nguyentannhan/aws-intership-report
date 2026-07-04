---
title: "Blog 1"
date: 2026-07-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Truy cập Amazon S3 riêng tư từ VPC bằng VPC Endpoint

_Bài viết được thực hiện bởi nhóm Otokoshi trong quá trình học tập và thực hành các dịch vụ trên AWS._
Khi mới triển khai các mô hình demo trên AWS, cách đơn giản và dễ hiểu nhất để một EC2 Instance có thể upload hoặc download dữ liệu với Amazon S3 là đặt EC2 trong public subnet, gán public IP, cấu hình route đi ra Internet Gateway, sau đó sử dụng AWS CLI hoặc SDK để thao tác với S3.
Cách tiếp cận này phù hợp với môi trường học tập hoặc demo nhanh vì cấu hình không quá phức tạp. Tuy nhiên, trong các hệ thống production như backend, internal service hoặc data processing, tài nguyên thường được đặt trong **private subnet** để hạn chế truy cập trực tiếp từ Internet. Khi đó, việc để dữ liệu đi qua public internet không còn là lựa chọn tối ưu, đặc biệt nếu hệ thống yêu cầu cao về bảo mật, kiểm soát dữ liệu và chi phí vận hành.
Vì vậy, bài viết này trình bày cách thiết kế mô hình truy cập Amazon S3 riêng tư từ bên trong VPC bằng **Gateway VPC Endpoint**. Đây là giải pháp giúp EC2 trong private subnet có thể giao tiếp với S3 mà không cần sử dụng Internet Gateway hoặc NAT Gateway.

## Bối cảnh và bài toán đặt ra

Giả sử hệ thống có một backend nội bộ chạy trên EC2 Instance trong private subnet. EC2 này không có public IP và không được expose trực tiếp ra Internet nhằm đảm bảo an toàn cho hệ thống. Tuy nhiên, backend vẫn cần kết nối đến Amazon S3 để thực hiện một số tác vụ quan trọng như:
- Lưu trữ file log hệ thống.
- Xuất báo cáo định kỳ.
- Sao lưu cấu hình và dữ liệu batch lên Amazon S3.
Trong điều kiện private subnet không có đường ra Internet, EC2 sẽ không thể truy cập trực tiếp đến S3 endpoint mặc định. Một phương án thường gặp là cấu hình NAT Gateway để các tài nguyên trong private subnet có thể đi ra ngoài Internet. Tuy nhiên, NAT Gateway có thể phát sinh chi phí theo giờ chạy và chi phí xử lý dữ liệu theo dung lượng sử dụng.
Thay vì sử dụng NAT Gateway chỉ để truy cập S3, chúng ta có thể triển khai **Gateway VPC Endpoint cho Amazon S3**. Giải pháp này tạo ra một đường kết nối riêng tư giữa VPC và Amazon S3, giúp traffic không cần đi qua public Internet, đồng thời tối ưu hơn về bảo mật và chi phí.

## Kiến trúc tổng quan và luồng xử lý

Mô hình MVP (Minimum Viable Product) trong bài viết này bao gồm các thành phần cốt lõi sau:
- **EC2 Instance**: Được đặt trong private subnet và không có public IP.
- **Amazon S3 Bucket**: Đóng vai trò lưu trữ dữ liệu đích và được bật tính năng _Block Public Access_.
- **Gateway VPC Endpoint**: Điểm cuối VPC dạng Gateway dùng để kết nối riêng tư đến Amazon S3.
- **Route Table**: Bảng định tuyến của private subnet được cấu hình để điều hướng traffic đến S3 thông qua endpoint.
- **IAM & Policies**: Bao gồm IAM Role cho EC2, Endpoint Policy và Bucket Policy nhằm kiểm soát quyền truy cập ở nhiều lớp.

### Luồng xử lý dữ liệu

Quy trình xử lý có thể được hiểu theo các bước sau:
1. EC2 Instance trong private subnet gửi request đọc hoặc ghi dữ liệu đến Amazon S3.
2. Route table của private subnet nhận diện traffic có đích đến là S3 thông qua Prefix List của AWS.
3. Traffic được định tuyến qua **Gateway VPC Endpoint** thay vì đi qua Internet Gateway hoặc NAT Gateway.
4. Amazon S3 tiếp nhận request và kiểm tra quyền truy cập thông qua nhiều lớp bảo mật, bao gồm IAM Role của EC2, Endpoint Policy và Bucket Policy.
5. Nếu tất cả điều kiện bảo mật đều hợp lệ, EC2 có thể thao tác với S3 Bucket một cách riêng tư và an toàn.
Với kiến trúc này, backend trong private subnet vẫn có thể sử dụng Amazon S3 cho các tác vụ lưu trữ mà không cần mở kết nối ra public Internet.

## Lựa chọn giải pháp: VPC Endpoint vs NAT Gateway

VPC Endpoint và NAT Gateway đều có vai trò quan trọng trong kiến trúc mạng trên AWS, nhưng chúng phục vụ các mục đích khác nhau. VPC Endpoint không thay thế hoàn toàn NAT Gateway, mà phù hợp trong trường hợp tài nguyên trong VPC cần kết nối riêng tư đến một số dịch vụ AWS được hỗ trợ, chẳng hạn như Amazon S3 hoặc DynamoDB.
Bảng dưới đây thể hiện sự khác biệt cơ bản giữa Gateway VPC Endpoint và NAT Gateway:
| Tiêu chí                 | Gateway VPC Endpoint (S3/DynamoDB)                               | NAT Gateway                                                       |
| ------------------------ | ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Phạm vi kết nối**      | Chỉ kết nối riêng tư đến các dịch vụ AWS được hỗ trợ như S3, DynamoDB. | Cho phép tài nguyên trong private subnet kết nối ra Internet, ví dụ gọi API bên ngoài hoặc cập nhật package. |
| **Đường đi của traffic** | Traffic nằm trong hệ thống mạng nội bộ của AWS thông qua AWS backbone network. | Traffic đi qua môi trường public Internet. |
| **Chi phí**              | Không tính phí theo giờ chạy và không tính phí xử lý dữ liệu đối với Gateway Endpoint. | Tính phí theo giờ chạy và phí xử lý dữ liệu theo từng GB. |
| **Cấu hình Network**     | Cập nhật Route Table với target là Endpoint ID (`vpce-xxx`). | Cập nhật Route Table với target là NAT Gateway ID (`nat-xxx`). |
Từ bảng trên có thể thấy, nếu nhu cầu chính chỉ là để EC2 trong private subnet truy cập Amazon S3, Gateway VPC Endpoint là lựa chọn phù hợp hơn. Giải pháp này giúp giảm sự phụ thuộc vào public Internet, đồng thời tránh phát sinh chi phí không cần thiết từ NAT Gateway.

## Các điểm lưu ý quan trọng về bảo mật

> **Mẹo kiến trúc:** VPC Endpoint chủ yếu giải quyết bài toán đường đi của traffic ở tầng Network. Nó không tự động cấp quyền truy cập dữ liệu cho EC2. Vì vậy, để hệ thống hoạt động an toàn, cần kết hợp nhiều lớp kiểm soát quyền theo hướng **Defense in Depth**.
Trong mô hình này, có 3 lớp bảo mật quan trọng cần được cấu hình cẩn thận:
- **IAM Role của EC2**: Xác định EC2 Instance được phép thực hiện những hành động nào trên Amazon S3, ví dụ `s3:GetObject` hoặc `s3:PutObject`.
- **Endpoint Policy**: Giới hạn những hành động hoặc những bucket nào được phép truyền traffic đi qua VPC Endpoint.
- **Bucket Policy**: Kiểm soát các request truy cập vào bucket. Để tăng cường bảo mật, có thể cấu hình Bucket Policy chỉ chấp nhận request đến từ một VPC Endpoint cụ thể thông qua điều kiện `aws:sourceVpce`.
Ví dụ Bucket Policy chỉ cho phép truy cập thông qua một VPC Endpoint cụ thể:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Allow-Access-From-Specific-VPCE-Only",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ],
      "Condition": {
        "StringNotEquals": {
          "aws:sourceVpce": "vpce-1a2b3c4d"
        }
      }
    }
  ]
}
```
Cách cấu hình này giúp đảm bảo rằng request truy cập vào bucket chỉ hợp lệ khi đi qua đúng VPC Endpoint được chỉ định. Nếu request đến từ nguồn khác, kể cả khi có một số quyền truy cập nhất định, hệ thống vẫn có thể từ chối theo chính sách bảo mật đã thiết lập.

## Kinh nghiệm debug khi gặp lỗi kết nối

Trong quá trình triển khai, nếu EC2 không thể kết nối đến Amazon S3 hoặc bị từ chối truy cập, không nên chỉ tập trung kiểm tra mã nguồn. Lỗi có thể đến từ nhiều lớp khác nhau trong kiến trúc, bao gồm IAM, Route Table, DNS, Region hoặc Policy.
Một checklist cơ bản khi debug có thể gồm:
- [ ] **IAM Role**: EC2 đã được gắn đúng IAM Role chưa? Role này đã có quyền tối thiểu cần thiết theo nguyên tắc Least Privilege chưa?
- [ ] **Route Table**: Private subnet đã có route điều hướng traffic đến S3 thông qua `vpce-xxx` chưa?
- [ ] **DNS & Region**: Tên bucket và region trong code hoặc AWS CLI đã được cấu hình chính xác chưa?
- [ ] **Endpoint Policy**: Endpoint Policy có đang giới hạn sai bucket hoặc sai action không?
- [ ] **Bucket Policy**: Bucket Policy có điều kiện `Deny` nào gây chặn nhầm request không?
- [ ] **Block Public Access**: Bucket đã được bật Block Public Access chưa, và chính sách truy cập có phù hợp với mô hình private access không?
Việc kiểm tra theo từng lớp như trên giúp quá trình xử lý lỗi rõ ràng hơn. Thay vì đoán lỗi nằm ở code, người triển khai có thể xác định chính xác request đang bị chặn ở tầng network, IAM hay policy.

## Bài học rút ra

Thông qua quá trình thực hành triển khai mô hình truy cập Amazon S3 riêng tư từ VPC, nhóm rút ra một số bài học quan trọng:
1. **Private subnet không có nghĩa là tài nguyên bị cô lập hoàn toàn**
  Tài nguyên trong private subnet không được truy cập trực tiếp từ Internet, nhưng vẫn có thể giao tiếp với các dịch vụ AWS cần thiết nếu kiến trúc network được thiết kế đúng cách. Gateway VPC Endpoint giúp EC2 trong private subnet truy cập Amazon S3 một cách riêng tư, an toàn và không cần public IP.
2. **Bảo mật cần được tích hợp ngay từ thiết kế kiến trúc**
  VPC Endpoint không chỉ là một cấu hình network. Khi được kết hợp với IAM Role, Endpoint Policy và Bucket Policy, nó giúp giảm bề mặt tấn công bằng cách loại bỏ nhu cầu mở kết nối public Internet khi không cần thiết. Đây là một ví dụ thực tế của tư duy bảo mật theo hướng nhiều lớp.
3. **Một hệ thống Cloud tốt cần sự phối hợp giữa Network và Security**
  Kiến trúc Cloud không chỉ nằm ở việc tạo tài nguyên. Một hệ thống được thiết kế tốt cần hiểu rõ traffic đi qua đâu, route table điều hướng như thế nào, quyền được cấp ở lớp nào và bucket kiểm soát truy cập ra sao. Sự kết hợp giữa Network như Route Table, VPC Endpoint và Security như IAM, Bucket Policy, Logging là nền tảng để vận hành hệ thống an toàn và tối ưu.

## Kết luận

Gateway VPC Endpoint là một giải pháp phù hợp khi cần cho tài nguyên trong private subnet truy cập Amazon S3 một cách riêng tư. Thay vì để EC2 đi ra Internet thông qua NAT Gateway, mô hình này giúp traffic đến S3 được định tuyến trong mạng nội bộ của AWS, giảm rủi ro bảo mật và tối ưu chi phí vận hành.
Trong thực tế, khi thiết kế hệ thống cloud, việc lựa chọn giữa NAT Gateway và VPC Endpoint cần dựa trên nhu cầu kết nối cụ thể. Nếu hệ thống cần truy cập nhiều dịch vụ bên ngoài Internet, NAT Gateway vẫn là một thành phần cần thiết. Tuy nhiên, nếu mục tiêu chỉ là truy cập các dịch vụ AWS được hỗ trợ như Amazon S3, VPC Endpoint là một lựa chọn đơn giản, an toàn và hiệu quả hơn.
Quan trọng hơn, bài học lớn nhất từ mô hình này là: bảo mật cloud không chỉ đến từ việc đặt tài nguyên trong private subnet, mà còn đến từ cách thiết kế đường đi của traffic, cách phân quyền IAM và cách kiểm soát dữ liệu ở từng lớp của hệ thống.

![Ảnh minh họa](/images/3-BlogsTranslated/Blog1/blog1.jpg)