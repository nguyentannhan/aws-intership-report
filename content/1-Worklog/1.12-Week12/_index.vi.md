---
title: "Worklog Tuần 12"
date: 2026-06-22
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu tuần 12: Khép vòng OAuth về tận tay người dùng và rà soát bảo mật toàn hệ thống

Backend OAuth đã xong từ tuần trước, nhưng "xong" mới tính đến ranh giới server — người dùng thật còn cần một chặng cuối: sau khi cấp quyền trên trình duyệt, họ phải được đưa **về lại app** một cách mượt mà. Tuần này tôi phối hợp với bạn Flutter khép nốt chặng đó (deep link), xử lý một hành vi chặn bất ngờ của Chrome, rồi dành nửa cuối tuần cho việc mà mảng bảo mật nào cũng phải làm trước khi nộp: rà soát lại toàn bộ hệ thống bằng con mắt của kẻ tấn công — và trung thực ghi nhận cả một sơ suất của chính nhóm mình.
- Vá trang callback để trình duyệt đưa người dùng về app qua deep link `inboxiq://`, xử lý hành vi chặn auto-redirect của Chrome.
- Hỗ trợ test luồng OAuth trọn vẹn trên thiết bị thật cùng bạn Flutter.
- Quản lý danh sách Test users trên Google Cloud khi nhóm cần test với nhiều tài khoản Gmail.
- Rà soát bảo mật toàn hệ thống: JWT hai tầng (REST vs WebSocket), vị trí mọi credential, quyền IAM.
- Xử lý sự cố quy trình: phát hiện Google Client Secret từng bị lộ trong ảnh chụp màn hình trao đổi nhóm — đánh giá rủi ro và lập kế hoạch rotate.

### Các công việc cần triển khai trong tuần này:

| Thứ | Hạng mục công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | - Test luồng OAuth trên thiết bị thật cùng bạn Flutter: consent hoàn tất nhưng trình duyệt đứng yên ở trang callback, không quay về app. <br> - Truy nguyên nhân: Chrome chặn tự động điều hướng sang scheme lạ (`inboxiq://`) khi không có user gesture trực tiếp — cơ chế chống mã độc. | 22/06/2026 | 22/06/2026 | Nội bộ dự án |
| 3 | - Vá `callbackHandler`: trang HTML trả về thêm `meta refresh` tự chuyển sau 1 giây kèm link dự phòng "Bấm vào đây nếu không tự chuyển". <br> - Deploy, test lại cùng bạn Flutter: deep link hoạt động trọn vẹn, app nhận callback và cập nhật trạng thái kết nối. | 23/06/2026 | 23/06/2026 | Nội bộ dự án |
| 4 | - Hướng dẫn quy trình thêm tài khoản Gmail mới vào Test users trên OAuth consent screen để nhóm test với nhiều hộp thư. <br> - Xác minh cơ chế Google "nhớ" quyền đã cấp: tài khoản đã consent trước đó không bị hỏi lại, luồng đi thẳng về callback. | 24/06/2026 | 24/06/2026 | https://support.google.com/cloud/ |
| 5 | - Rà soát mô hình xác thực hai tầng: REST xác thực JWT từng request qua Cognito Authorizer; WebSocket xác thực một lần lúc `$connect` qua Lambda Authorizer với token trên query string. <br> - Kiểm tra Lambda Authorizer verify đúng chữ ký và audience của JWT với User Pool. | 25/06/2026 | 25/06/2026 | https://docs.aws.amazon.com/apigateway/ |
| 6 | - Kiểm kê vị trí mọi credential trong hệ thống: OpenAI key và Google Client Secret trong Secrets Manager, không có key nào trong code/Git/biến môi trường plaintext. <br> - Phát hiện trong lúc rà soát: Google Client Secret từng xuất hiện trong một ảnh chụp màn hình trao đổi nội bộ nhóm — đánh giá mức độ phơi nhiễm và phương án xử lý. | 26/06/2026 | 26/06/2026 | Nội bộ dự án |
| 7 | - Lập kế hoạch rotate Google OAuth Client Secret: tạo secret mới trên Google Console, cập nhật Secrets Manager, vô hiệu secret cũ — xếp lịch thực hiện sau đợt demo để không gián đoạn chấm điểm, ghi rõ vào danh sách bảo trì. <br> - Rà soát cuối: xác nhận `state` không ký đã ghi trong Limitations, danh sách Test users đúng phạm vi. | 27/06/2026 | 27/06/2026 | https://developers.google.com/identity/protocols/oauth2 |
| CN | - Tổng hợp "hồ sơ bảo mật" của hệ thống: mô hình xác thực, vị trí credential, các giới hạn đã nhận diện và kế hoạch xử lý — bàn giao cho thành viên tài liệu đưa vào báo cáo. <br> - Viết báo cáo tổng kết tuần. | 28/06/2026 | 28/06/2026 | Nội bộ dự án InboxIQ |

### Kết quả đạt được tuần 12:

#### 1. Chặng cuối của OAuth — khi trình duyệt trở thành "bên thứ tư" khó tính

Luồng OAuth có ba bên quen thuộc (app, backend, Google), nhưng tuần này lộ ra bên thứ tư ít ai tính đến: **chính trình duyệt**. Chrome trên Android chặn tự động điều hướng sang scheme không phải http/https khi thiếu thao tác trực tiếp của người dùng — một cơ chế chống mã độc hợp lý, nhưng khiến trang callback "thành công" của chúng tôi giam người dùng lại ở trình duyệt.

Giải pháp chọn mang tính phòng thủ kép: `meta refresh` thử tự chuyển sau 1 giây (đủ cho các trình duyệt dễ tính), cộng link dự phòng bấm tay (đường thoát chắc chắn cho mọi trình duyệt). Nguyên tắc rút ra cho mọi luồng đi qua trình duyệt: không bao giờ đặt cược trải nghiệm vào một hành vi tự động mà mình không kiểm soát — luôn có đường thủ công dự phòng.

Một quan sát thú vị khi test nhiều lần: Google "nhớ" quyền đã cấp — tài khoản từng consent không bị hỏi lại, luồng lướt thẳng về callback trong chưa đầy hai giây. Đây chính là mặt còn lại của cơ chế incremental consent đã gặp tuần trước.

#### 2. Mô hình xác thực hai tầng — mỗi giao thức một cách verify

Rà soát lại giúp tôi trình bày mạch lạc điểm hay bị hỏi: cùng một JWT của Cognito nhưng hai tầng API xác thực theo hai cách khác nhau, vì bản chất giao thức khác nhau. REST xác thực **từng request** qua Cognito Authorizer (mỗi lời gọi mang token trong header). WebSocket xác thực **một lần duy nhất lúc `$connect`** qua Lambda Authorizer, token truyền trên query string — vì WebSocket handshake từ mobile không gửi được custom header, và sau khi kết nối đã mở thì các message tiếp theo chạy trên chính kênh đã xác thực đó. Hai cách làm khác nhau nhưng cùng một nguồn sự thật: chữ ký và audience của JWT đều verify với đúng User Pool.

#### 3. Rà soát credential — và bài học từ sơ suất của chính mình

Kiểm kê cho kết quả tốt: mọi credential đều nằm đúng chỗ trong Secrets Manager, không có key nào trong code, Git hay biến môi trường plaintext. Nhưng chính lúc rà soát, tôi phát hiện một sơ suất quy trình: Google Client Secret từng **lộ trong một ảnh chụp màn hình** khi nhóm trao đổi xin hỗ trợ kỹ thuật — không phải lỗi kiến trúc, mà là lỗi thói quen chụp ảnh không che thông tin nhạy cảm.

Đánh giá rủi ro thực tế: mức phơi nhiễm hẹp (ảnh trong kênh trao đổi riêng), nhưng nguyên tắc bảo mật không cho phép "chắc là không sao" — secret đã lộ là secret phải thay. Kế hoạch rotate được lập cụ thể (tạo secret mới → cập nhật Secrets Manager → vô hiệu secret cũ) và xếp lịch sau đợt demo để không gián đoạn chấm điểm, ghi minh bạch vào danh sách bảo trì của báo cáo. Bài học quy trình cho cả nhóm: mọi ảnh chụp màn hình trước khi gửi đi — kể cả nội bộ — phải che credential; thói quen này rẻ hơn nhiều so với chi phí rotate và rủi ro đi kèm.

#### 4. "Hồ sơ bảo mật" — di sản của mảng này cho báo cáo

Sản phẩm cuối tuần là một tài liệu tổng hợp: mô hình xác thực hai tầng, bản đồ vị trí credential, và — quan trọng không kém — danh sách giới hạn đã nhận diện kèm kế hoạch xử lý (`state` chưa ký, secret cần rotate, consent screen ở chế độ Testing). Triết lý xuyên suốt: một báo cáo bảo mật đáng tin không phải báo cáo không có điểm yếu, mà là báo cáo biết rõ điểm yếu của mình nằm ở đâu.

### Đánh giá kết quả tuần 12:

- Luồng OAuth khép trọn vòng về tận app trên thiết bị thật — mảng tôi phụ trách hoàn thành mục tiêu kỹ thuật.
- Mô hình xác thực hai tầng được rà soát và tài liệu hoá mạch lạc, sẵn sàng cho phần bảo vệ.
- Phát hiện và xử lý minh bạch sự cố lộ secret bằng đánh giá rủi ro và kế hoạch rotate cụ thể.
- Bàn giao "hồ sơ bảo mật" đầy đủ cho thành viên tài liệu.

### Khó khăn gặp phải:

- Hành vi chặn redirect của Chrome không có thông báo lỗi, không có log — chỉ có một trang web đứng im, phải suy luận và thử nghiệm để xác nhận.
- Cân bằng giữa nguyên tắc bảo mật (rotate secret ngay khi lộ) và thực tế vận hành (không làm gián đoạn hệ thống giữa đợt chấm điểm) — quyết định cuối cùng phải kèm đánh giá rủi ro rõ ràng chứ không chọn bừa.
- Việc thừa nhận sơ suất của chính nhóm vào báo cáo đòi hỏi vượt qua tâm lý "giấu đi cho đẹp" — nhưng minh bạch mới là thái độ đúng của người làm bảo mật.

### Hướng khắc phục và Best Practices đúc kết được:

- Mọi luồng phụ thuộc trình duyệt phải có đường dự phòng thủ công bên cạnh cơ chế tự động.
- Che mọi credential trong ảnh chụp màn hình trước khi chia sẻ, kể cả trong kênh nội bộ.
- Secret đã phơi nhiễm — dù hẹp đến đâu — đều phải rotate; điều được phép cân nhắc chỉ là thời điểm, kèm đánh giá rủi ro bằng văn bản.
- Báo cáo bảo mật nên liệt kê giới hạn đã biết kèm kế hoạch xử lý, thay vì trình bày một hệ thống "hoàn hảo" thiếu thuyết phục.

### Định hướng sau dự án:

- Thực hiện rotate Google Client Secret theo kế hoạch ngay sau đợt chấm điểm.
- Nâng cấp cơ chế `state` từ base64 thuần sang có ký (JWT) hoặc lưu tạm DynamoDB kèm TTL nếu dự án tiếp tục phát triển.
- Tách IAM Role dùng chung thành role riêng cho từng Lambda theo chuẩn Least Privilege tuyệt đối.
- Nghiên cứu quy trình Google verification nếu muốn mở app cho người dùng ngoài danh sách Test users.
