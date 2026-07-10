---
title: "Worklog Tuần 11"
date: 2026-06-15
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11: Xây trọn luồng OAuth Gmail — và đối mặt với sự cố khó hiểu nhất của mảng bảo mật

Tuần này là tuần trọng tâm của phần tôi phụ trách: biến cấu hình Google Cloud tuần trước thành một luồng OAuth chạy thật — từ lúc app xin kết nối đến lúc token Gmail nằm an toàn trong DynamoDB và tự làm mới khi hết hạn. Kế hoạch nghe tuyến tính, nhưng thực tế tuần này ghi dấu bằng một sự cố mà mọi thứ đều báo "thành công" trong khi hệ thống hoàn toàn không hoạt động — sự cố dạy tôi nhiều nhất về OAuth trong cả dự án.

- Viết Lambda `oauth-init`: tạo consent URL kèm các tham số quyết định (`access_type`, `prompt`, `state`).
- Viết Lambda `oauth-callback`: đổi authorization code lấy token, lưu DynamoDB, trả trang HTML cho trình duyệt.
- Giải bài toán truyền danh tính xuyên vòng redirect bằng tham số `state`, nhận diện trung thực giới hạn bảo mật của cách làm MVP.
- Xây Lambda Layer `gmail-shared` chứa logic refresh token dùng chung giữa các Lambda.
- Xử lý sự cố thực tế: OAuth "thành công" nhưng Worker gọi Gmail API nhận `403 Forbidden`.

### Các công việc cần triển khai trong tuần này:

| Thứ | Hạng mục công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Nhận callback URL thật từ bạn backend (REST API đã deploy), điền vào **Authorized redirect URIs** của Google OAuth Client. <br> - Xử lý lỗi đầu tiên: `Invalid Origin: URIs must not contain a path` — do điền nhầm redirect URI vào ô JavaScript origins. | 15/06/2026 | 15/06/2026 | https://developers.google.com/identity/protocols/oauth2/web-server |
| 3 | - Viết Lambda `oauth-init` (yêu cầu Cognito auth): dựng consent URL với scope tối thiểu, `access_type=offline` (bắt buộc để nhận refresh_token), `prompt=consent`, và `state` mã hoá userId. <br> - Tìm hiểu kỹ vì sao thiếu `prompt=consent` thì từ lần approve thứ hai Google không trả refresh_token nữa. | 16/06/2026 | 16/06/2026 | https://developers.google.com/identity/protocols/oauth2 |
| 4 | - Viết Lambda `oauth-callback` (public, Authorizer NONE — vì Google redirect không mang JWT): xác thực `state`, đổi code lấy token, lấy địa chỉ Gmail qua userinfo, ghi bảng `inboxiq-gmail-connections`. <br> - Xử lý logic giữ lại refresh_token cũ khi response mới không có (Google chỉ chắc chắn trả ở lần consent đầu). | 17/06/2026 | 17/06/2026 | https://developers.google.com/identity/protocols/oauth2/web-server |
| 5 | - Xây Lambda Layer `gmail-shared`: `getGoogleCredentials()` (đọc secret có cache) và `refreshAccessToken(userId)`. <br> - Xử lý bẫy build của SAM: `BuildMethod: nodejs20.x` tự bọc thêm lớp `nodejs/` gây lồng đôi thư mục, Lambda báo `Cannot find module` dù build thành công — sửa bằng trỏ `ContentUri` vào thẳng thư mục `nodejs/`. | 18/06/2026 | 18/06/2026 | https://docs.aws.amazon.com/serverless-application-model/ |
| 6 | - Test luồng OAuth end-to-end lần đầu với Gmail thật: consent hoàn tất, token có trong DynamoDB — nhưng Worker của bạn backend gọi Gmail API nhận `403 Forbidden`. <br> - Truy vết: phát hiện field `scope` trong DynamoDB thiếu `gmail.readonly` — chỉ có `userinfo.email openid`. | 19/06/2026 | 19/06/2026 | Nội bộ dự án |
| 7 | - Xác định nguyên nhân: cơ chế **granular permissions** của Google cho phép người dùng bỏ tick từng quyền riêng lẻ trên màn consent — token vẫn được cấp nhưng thiếu scope. <br> - Chạy lại luồng OAuth, Google hiện màn incremental consent hỏi đúng quyền còn thiếu; xác nhận scope đầy đủ và Worker gọi Gmail thành công. Sửa thêm lỗi tiếng Việt hiển thị sai ở trang callback (thiếu `charset=utf-8`). | 20/06/2026 | 20/06/2026 | https://developers.google.com/identity/protocols/oauth2/resources/granular-permissions |
| CN | - Viết tài liệu nội bộ về sự cố granular permissions và bài học "consent hoàn tất ≠ đủ quyền". <br> - Bàn giao luồng OAuth hoàn chỉnh cho bạn Flutter tích hợp phía client (deep link) tuần sau. Viết báo cáo tuần. | 21/06/2026 | 21/06/2026 | Nội bộ dự án InboxIQ |

### Kết quả đạt được tuần 11:

#### 1. Ba tham số nhỏ quyết định cả luồng OAuth

Viết `oauth-init` dạy tôi rằng OAuth "chạy được" và OAuth "chạy đúng lâu dài" khác nhau ở vài tham số ít ai để ý:

- **`access_type=offline`**: thiếu nó, Google chỉ trả access_token sống ~1 giờ — hệ thống sẽ chết lặng lẽ sau đúng một giờ kể từ khi người dùng kết nối.
- **`prompt=consent`**: thiếu nó, từ lần approve thứ hai trở đi Google không trả refresh_token nữa — loại lỗi chỉ bộc lộ khi người dùng kết nối lại, rất khó tái hiện trong test một lần.
- **`state`**: kênh duy nhất mang danh tính người dùng xuyên qua vòng redirect, vì request callback đến từ trình duyệt qua Google — không mang JWT của Cognito.

Về `state`, tôi ghi nhận trung thực một giới hạn của bản MVP: hiện chỉ mã hoá base64 chứ không ký — về lý thuyết kẻ xấu có thể tự tạo `state` giả mạo userId khác. Phương án chuẩn cho production (ký `state` bằng JWT hoặc lưu tạm vào DynamoDB kèm TTL để đối chiếu) được ghi vào phần Limitations thay vì im lặng bỏ qua.

#### 2. Sự cố granular permissions — "thành công" giả và bài học lớn nhất mảng bảo mật

Đây là sự cố đáng nhớ nhất: luồng OAuth hoàn tất trọn vẹn, token nằm trong DynamoDB, không một thông báo lỗi nào — nhưng Worker gọi Gmail API nhận `403 Forbidden`, job bị SQS retry mỗi 5 phút trong im lặng.
Manh mối phá án nằm ngay trong dữ liệu: field `scope` của record chỉ chứa `userinfo.email openid`, **thiếu `gmail.readonly`**. Nguyên nhân là cơ chế **granular permissions** tương đối mới của Google: màn consent cho phép người dùng tick chọn từng quyền riêng lẻ, và người test đã bấm "Tiếp tục" mà không tick ô quyền Gmail — Google vẫn cấp token bình thường, chỉ là token thiếu quyền. Đa số tài liệu OAuth cũ chưa nhắc đến hành vi này.
Cách xử lý hoá ra thanh lịch: chạy lại luồng OAuth, Google nhận ra app đã có một phần quyền và hiển thị màn **incremental consent** chỉ hỏi đúng quyền còn thiếu. Bài học khắc cốt: **với OAuth, luôn kiểm tra field `scope` trong token response thay vì tin vào việc luồng chạy trót lọt** — ứng dụng production nên kiểm tra scope ngay tại callback và chủ động yêu cầu re-consent khi thiếu.

#### 3. Lambda Layer — chia sẻ code đúng cách giữa các Lambda

Logic refresh token cần dùng ở cả oauth-callback lẫn Worker, nhưng SAM build và zip từng function riêng theo `CodeUri` nên function này không `import` được file của function kia — Lambda Layer là cơ chế chia sẻ chính thống. Bẫy thực tế gặp phải: `BuildMethod: nodejs20.x` tự bọc thêm một lớp `nodejs/` khi đóng gói, nếu `ContentUri` trỏ vào thư mục cha (đã chứa sẵn `nodejs/`) thì kết quả lồng đôi `nodejs/nodejs/` và Lambda runtime báo `Cannot find module '/opt/nodejs/gmail-shared.mjs'` dù build "thành công". Sửa bằng trỏ `ContentUri` sâu thêm một cấp, xoá `.aws-sam` rồi build lại vì cache có thể giữ artifact cũ.

#### 4. Những lỗi nhỏ nhưng tốn thời gian không nhỏ

Hai lỗi phụ đáng ghi lại: điền redirect URI nhầm ô trên Google Console (`Invalid Origin: URIs must not contain a path` — URI đầy đủ có path phải vào ô **Authorized redirect URIs**, không phải ô origins), và tiếng Việt hiển thị lỗi "Káº¿t ná»'i..." ở trang callback do header `Content-Type` thiếu `charset=utf-8` khiến trình duyệt tự đoán sai bảng mã.

### Đánh giá kết quả tuần 11:

- Luồng OAuth Gmail hoàn chỉnh: init → consent → callback → token trong DynamoDB → tự refresh khi hết hạn.
- Giải quyết và tài liệu hoá sự cố granular permissions — bài học có giá trị vượt phạm vi dự án, áp dụng cho mọi tích hợp OAuth với Google.
- Lambda Layer dùng chung vận hành ổn, được cả oauth-callback lẫn Worker sử dụng.
- Nhận diện và ghi nhận trung thực giới hạn bảo mật của `state` không ký — sẵn câu trả lời cho phần bảo vệ.

### Khó khăn gặp phải:

- Sự cố 403 hoàn toàn "âm thầm": không có lỗi nào ở phía OAuth, chỉ phát hiện được khi truy ngược từ triệu chứng của Worker (do bạn backend báo) về dữ liệu scope trong DynamoDB.
- Hành vi granular permissions của Google gần như vắng bóng trong các tài liệu hướng dẫn OAuth phổ biến — phải tìm đến đúng trang tài liệu chính thức chuyên biệt mới xác nhận được.
- Bẫy lồng thư mục `nodejs/nodejs/` của SAM Layer chỉ bộc lộ lúc runtime, build không hề cảnh báo.

### Hướng khắc phục và Best Practices đúc kết được:

- Luôn kiểm tra `scope` thực nhận trong token response — coi đó là một bước bắt buộc của luồng OAuth, không phải tuỳ chọn.
- Với dịch vụ bên thứ ba thay đổi hành vi thường xuyên (như Google), ưu tiên tài liệu chính thức mới nhất và ghi ngày tra cứu vào tài liệu nhóm.
- Sau khi đổi cấu trúc Layer/build, luôn xoá sạch `.aws-sam` trước khi build lại để loại trừ cache cũ.
- Mọi response HTML chứa tiếng Việt phải khai báo `charset=utf-8` tường minh.

### Định hướng cho tuần tiếp theo:

- Hỗ trợ bạn Flutter hoàn thiện phía client của luồng OAuth (deep link quay về app sau consent).
- Rà soát bảo mật tổng thể trước khi nộp: quyền IAM, vị trí lưu credential, danh sách Test users.
- Chuẩn bị phần trình bày về các quyết định bảo mật và giới hạn đã nhận diện cho báo cáo nhóm.
