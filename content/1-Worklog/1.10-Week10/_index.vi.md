---
title: "Worklog Tuần 10"
date: 2026-06-08
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10: Dựng lớp danh tính và bảo mật — nền móng vô hình của cả hệ thống

Trong phân công nhóm InboxIQ, tôi phụ trách mảng xác thực và bảo mật: Cognito, IAM, Secrets Manager, và toàn bộ luồng OAuth với Google. Đây là phần "vô hình" — người dùng không thấy, demo không khoe được — nhưng mọi thành phần khác đều đứng trên nó: app cần Cognito để biết ai đang đăng nhập, Lambda cần IAM Role để được phép chạm vào tài nguyên, Worker cần secret để gọi OpenAI. Tuần này mục tiêu là dựng xong toàn bộ lớp nền đó trước khi hai bạn backend và Flutter cần đến.

- Tạo và cấu hình Cognito User Pool: chính sách mật khẩu, xác thực qua email, App Client cho mobile.
- Hiểu và chọn đúng authentication flow cho App Client, nắm rõ vì sao mobile app là "public client" không dùng client secret.
- Thiết kế IAM Role cho Lambda theo nguyên tắc Least Privilege.
- Thiết lập Secrets Manager lưu OpenAI API key, bàn giao cách đọc secret an toàn cho bạn backend.
- Tạo Google Cloud project, bật Gmail API, cấu hình OAuth consent screen ở chế độ Testing với scope tối thiểu.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Hạng mục công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Họp nhóm chốt kiến trúc; nhận mảng xác thực/bảo mật. <br> - Nghiên cứu tổng quan Amazon Cognito: User Pool vs Identity Pool, xác định dự án chỉ cần User Pool (xác thực người dùng, cấp JWT). | 08/06/2026 | 08/06/2026 | https://docs.aws.amazon.com/cognito/ |
| 3 | - Tạo User Pool: đăng nhập bằng email, chính sách mật khẩu, xác thực email khi đăng ký. <br> - Tạo App Client `inboxiq-flutter-client`: không bật client secret (mobile là public client, không giữ bí mật an toàn được), chọn authentication flow phù hợp. | 09/06/2026 | 09/06/2026 | https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-client-apps.html |
| 4 | - Tạo user test, xác nhận luồng đăng nhập bằng AWS CLI (`initiate-auth`) trả về đủ bộ token (idToken, accessToken, refreshToken). <br> - Bàn giao Pool ID, App Client ID và flow đã bật cho bạn Flutter, kèm lưu ý đối chiếu `authenticationFlowType` phía client với cấu hình thật. | 10/06/2026 | 10/06/2026 | https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/ |
| 5 | - Thiết kế IAM Role `inboxiq-lambda-role` cho các Lambda: quyền DynamoDB, SQS, Secrets Manager, CloudWatch Logs, X-Ray và `execute-api:ManageConnections` (cho push WebSocket sau này) — không cấp quyền thừa. <br> - Tìm hiểu trade-off giữa một role dùng chung (đơn giản cho MVP) và role riêng từng hàm (chuẩn Least Privilege tuyệt đối). | 11/06/2026 | 11/06/2026 | https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html |
| 6 | - Tạo secret `inboxiq/openai-api-key` trong Secrets Manager dạng JSON. <br> - Nghiên cứu và viết hướng dẫn nội bộ: đọc secret trong Lambda với cache ở global scope, tuyệt đối không đặt key vào biến môi trường dạng plaintext hay commit vào Git. | 12/06/2026 | 12/06/2026 | https://docs.aws.amazon.com/secretsmanager/ |
| 7 | - Tạo Google Cloud project, bật Gmail API. <br> - Cấu hình OAuth consent screen: chế độ Testing, scope tối thiểu (`gmail.readonly`, `userinfo.email`), thêm email test vào danh sách Test users. | 13/06/2026 | 13/06/2026 | https://developers.google.com/identity/protocols/oauth2 |
| CN | - Tạo OAuth Client ID (loại Web application) trên Google Cloud, lưu Client ID/Secret an toàn chờ có callback URL thật từ backend tuần sau. <br> - Viết báo cáo tổng kết tuần, tài liệu bàn giao lớp xác thực cho cả nhóm. | 14/06/2026 | 14/06/2026 | Nội bộ dự án InboxIQ |

---

### Kết quả đạt được tuần 10:

#### 1. Cognito — hiểu vì sao "thuê" thay vì "tự xây" hệ thống đăng nhập

Trước khi làm, tôi từng nghĩ tự viết đăng nhập bằng JWT + bcrypt cũng không khó. Sau một tuần đọc tài liệu Cognito mới thấy phần khó không nằm ở "đăng nhập chạy được" mà ở mọi thứ xung quanh: lưu mật khẩu an toàn, chống brute-force, luồng quên mật khẩu, xác thực email, làm mới token, thu hồi token. Cognito đảm nhiệm trọn vòng đời đó — nhóm không phải giữ một dòng mật khẩu nào trong cơ sở dữ liệu của mình, đồng nghĩa loại bỏ hẳn một lớp rủi ro rò rỉ.
Quyết định quan trọng nhất khi tạo App Client: **không bật client secret**. Lý do nằm ở bản chất mobile app — mã APK nằm trong tay người dùng, mọi "bí mật" nhúng trong app đều có thể bị dịch ngược, nên chuẩn OAuth xếp mobile vào nhóm *public client* không được phép giữ secret. Đây là điểm tôi ghi rõ vào tài liệu nhóm vì rất dễ bị hỏi khi bảo vệ báo cáo.

#### 2. IAM Role — Least Privilege là một chuỗi cân nhắc, không phải checkbox

Thiết kế role cho Lambda buộc tôi trả lời từng câu hỏi cụ thể: Worker cần đọc/ghi bảng nào? Producer có cần quyền gọi Secrets Manager không (không — chỉ Worker cần)? Quyền `execute-api:ManageConnections` để push WebSocket phạm vi tới đâu? Phương án chốt cho MVP là một role dùng chung `inboxiq-lambda-role` với tập quyền tối thiểu của hợp các nhu cầu — kèm ghi chú trung thực trong tài liệu: chuẩn Least Privilege tuyệt đối là role riêng cho từng hàm, đây là hạng mục cải tiến nếu dự án lên production thật.

#### 3. Secrets Manager — chỗ đứng đúng của những thứ không được lộ

Nguyên tắc chốt với cả nhóm: API key và mọi credential chỉ tồn tại ở đúng một nơi là Secrets Manager — không trong biến môi trường plaintext, không trong code, không trong Git. Kèm theo là hướng dẫn đọc secret có cache ở global scope của Lambda (giảm cả độ trễ lẫn chi phí, vì Secrets Manager tính phí theo lượt gọi API) mà bạn backend đã áp dụng ngay trong Worker.

#### 4. Google OAuth consent screen — thế giới bên ngoài AWS

Lần đầu làm việc với Google Cloud Console, điểm đáng chú ý nhất là chế độ **Testing** của consent screen: app chưa qua kiểm duyệt của Google chỉ cho phép những tài khoản trong danh sách Test users (tối đa 100) đăng nhập. Với scope đọc Gmail — thuộc nhóm sensitive/restricted — quy trình xác minh để mở cho mọi người dùng rất nặng (privacy policy công khai, video demo, có thể cần business verification), hoàn toàn không phù hợp phạm vi đồ án. Quyết định: giữ chế độ Testing, ghi rõ giới hạn này vào phần Limitations của báo cáo ngay từ đầu thay vì để người chấm tự phát hiện.

### Đánh giá kết quả tuần 10:

- Lớp xác thực Cognito hoàn chỉnh và đã kiểm chứng bằng CLI, bàn giao đủ thông tin cho bạn Flutter tích hợp.
- IAM Role thiết kế có cân nhắc, ghi lại rõ trade-off để trả lời khi bảo vệ.
- Secrets Manager vận hành cùng quy tắc quản lý credential thống nhất toàn nhóm.
- Google Cloud project sẵn sàng, chỉ chờ callback URL thật từ backend để hoàn tất cấu hình OAuth Client.

### Khó khăn gặp phải:

- Giao diện Cognito Console mới thay đổi nhiều so với tài liệu/hướng dẫn cũ trên mạng, nhiều mục đổi tên và vị trí, phải dò theo tài liệu chính thức mới nhất.
- Phân định ranh giới quyền IAM cho `execute-api:ManageConnections` khá rối vì WebSocket API lúc này chưa tồn tại (backend dựng tuần sau) — phải thiết kế "đón đầu" dựa trên kiến trúc đã chốt.
- Các khái niệm OAuth (client type, scope, consent, incremental authorization) nhiều và dễ lẫn, đặc biệt khi tài liệu Google và tài liệu AWS dùng thuật ngữ hơi khác nhau cho cùng một thứ.

### Hướng khắc phục và Best Practices đúc kết được:

- Luôn làm theo tài liệu chính thức phiên bản mới nhất thay vì blog/hướng dẫn cũ, đặc biệt với các Console thay đổi giao diện thường xuyên.
- Ghi lại mọi trade-off bảo mật (role chung vs role riêng, Testing vs Production consent) ngay lúc quyết định, kèm lý do — vừa trung thực trong báo cáo vừa sẵn câu trả lời khi bị hỏi.
- Vẽ sơ đồ luồng OAuth ra giấy trước khi cấu hình bất cứ thứ gì trên Console — các khái niệm trừu tượng trở nên rõ ràng hơn nhiều khi thấy đường đi của từng token.

### Định hướng cho tuần tiếp theo:

- Viết hai Lambda OAuth (init + callback) và Lambda Layer dùng chung cho logic refresh token — phần việc nặng nhất của mảng tôi phụ trách.
- Giải bài toán truyền danh tính người dùng xuyên qua vòng redirect của Google (tham số `state`).
- Phối hợp với bạn backend gắn callback URL thật vào Google OAuth Client.
