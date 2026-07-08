---
title: "Event 2"
date: 2026-06-13
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “FCAJ Community Day - June 2026 Edition”

## 1. Mục tiêu của sự kiện

Sự kiện **FCAJ Community Day - June 2026 Edition** được tổ chức nhằm mang đến cho người tham gia những góc nhìn thực tế về kiến trúc hệ thống, dữ liệu, DevOps và định hướng phát triển nghề nghiệp trong ngành công nghệ hiện đại. Nội dung sự kiện không chỉ tập trung vào lý thuyết mà còn đi sâu vào các bài toán triển khai thật trong doanh nghiệp và cộng đồng công nghệ.
Các mục tiêu chính của sự kiện gồm:
- Giới thiệu cách thiết kế một hệ thống có khả năng chịu tải lớn, thông qua bài toán xây dựng dịch vụ rút gọn URL trên nền tảng AWS.
- Chia sẻ vai trò, kỹ năng và tư duy cần có của một **Data Analytics Engineer** khi làm việc trong các tập đoàn đa quốc gia.
- Giúp người tham gia hiểu rõ hơn về công việc thực tế của **DevOps Engineer**, bao gồm thị trường tuyển dụng, mức thu nhập và các năng lực nền tảng cần xây dựng lâu dài.
- Truyền cảm hứng về hành trình phát triển bản thân, đóng góp cho cộng đồng và tư duy làm đúng việc trong lĩnh vực công nghệ.

## 2. Danh sách diễn giả

Sự kiện có sự tham gia của nhiều diễn giả đến từ các lĩnh vực khác nhau trong ngành công nghệ:
- **Anh Đinh Trung Kiên**: Lead Developer tại Startup.
- **Bạn Nguyễn Minh Thọ**: Student & Cloud Contributor.
- **Anh Dat Pham**: Data Analytics Engineer tại tập đoàn đa quốc gia.
- **Anh Cường Nguyễn**: Process Engineer.
- **Anh Trong H. Truong**: DevOps Engineer tại Endava Vietnam.
- **Anh Danh Hoàng Hiếu Nghị**: AI Engineer, AWS Community Builder & SBG Leader.

## 3. Nội dung nổi bật của sự kiện

### 3.1. Kiến trúc hệ thống rút gọn URL quy mô lớn trên AWS

Một trong những phần chia sẻ nổi bật của sự kiện là bài toán xây dựng hệ thống rút gọn URL có khả năng mở rộng lớn. Đây là một bài toán tưởng chừng đơn giản, nhưng khi triển khai ở quy mô lớn lại phát sinh nhiều vấn đề về hiệu năng, bảo mật, độ trễ và khả năng chịu tải.
Ở cách làm truyền thống, hệ thống thường tạo mã rút gọn ngay tại thời điểm người dùng gửi yêu cầu. Cách tiếp cận này dễ gặp nhiều hạn chế như:
- Độ trễ cao khi phải xử lý và kiểm tra dữ liệu theo thời gian thực.
- Nguy cơ trùng mã rút gọn nếu cơ chế sinh mã không được kiểm soát tốt.
- Khó mở rộng khi lượng truy cập tăng đột biến.
- Dễ xuất hiện điểm nghẽn hoặc điểm lỗi đơn nhất trong hệ thống.
Để giải quyết vấn đề này, diễn giả giới thiệu mô hình **Key Generation Service (KGS)**. Thay vì sinh mã khi có yêu cầu, hệ thống sẽ chuẩn bị sẵn một lượng lớn short code trước đó.
Một số điểm quan trọng trong kiến trúc gồm:
- **Tách riêng luồng đọc và luồng ghi**: Read path và Write path được thiết kế độc lập để tối ưu theo từng loại tải khác nhau.
- **Sinh mã trước thay vì xử lý on-demand**: Các container chạy trên **Amazon ECS** sẽ tạo sẵn short code và đẩy vào hàng đợi của **Amazon ElastiCache for Redis** bằng lệnh `LPUSH key_queue`.
- **Rút mã nhanh khi có yêu cầu**: Khi người dùng cần rút gọn URL, Backend Spring Boot chạy trên ECS chỉ cần dùng lệnh `RPOP` để lấy một mã có sẵn từ Redis, sau đó ghi dữ liệu vào **Amazon DynamoDB** với mã đó làm khóa chính.
- **Giảm nguy cơ trùng lặp**: Vì mã đã được sinh và kiểm soát trước, hệ thống hạn chế được rủi ro collision trong quá trình tạo URL.
- **Áp dụng Cache-aside Pattern**: Khi người dùng truy cập link rút gọn, hệ thống sẽ ưu tiên kiểm tra dữ liệu trong Redis. Nếu dữ liệu chưa có trong cache, hệ thống mới truy vấn xuống DynamoDB.
- **Bảo vệ hệ thống từ lớp biên**: AWS WAF và Amazon CloudFront được đặt gần người dùng để chặn các request nguy hiểm trước khi chúng đi vào lõi hệ thống.
Qua phần này, tôi hiểu rằng một hệ thống đơn giản về mặt chức năng vẫn có thể trở nên rất phức tạp khi phải phục vụ lượng truy cập lớn. Vì vậy, việc thiết kế kiến trúc cần quan tâm đến hiệu năng, bảo mật, khả năng mở rộng và độ ổn định ngay từ đầu.

### 3.2. Công việc và tư duy phát triển của Data Analytics Engineer

Phần chia sẻ về **Data Analytics Engineer** giúp tôi có cái nhìn rõ hơn về công việc dữ liệu trong doanh nghiệp thực tế. Tùy vào lĩnh vực hoạt động của công ty, vai trò dữ liệu có thể có nhiều trọng tâm khác nhau.
Trong các công ty công nghệ hoặc thương mại điện tử, Data Analytics Engineer thường tham gia xây dựng báo cáo định kỳ, dashboard theo dõi vận hành và các chỉ số kinh doanh. Mục tiêu là phát hiện bất thường, tìm nguyên nhân gốc rễ và hỗ trợ các phòng ban tối ưu hiệu suất.
Ví dụ, tại các doanh nghiệp như Kamereo, dữ liệu có thể được dùng để theo dõi hiệu quả vận hành, phân tích GMV, tối ưu đơn hàng hoặc hỗ trợ ra quyết định trong chuỗi cung ứng.
Trong khi đó, tại các tập đoàn sản xuất như Colgate-Palmolive, dữ liệu có thể đến từ máy móc, dây chuyền, thiết bị IoT hoặc hệ thống vận hành nhà máy. Khi đó, Data Analytics Engineer không chỉ làm báo cáo mà còn góp phần tìm ra cơ hội tiết kiệm chi phí, tối ưu hiệu suất thiết bị và nâng cao năng lực sản xuất.
Diễn giả cũng nhấn mạnh 4 kỹ năng quan trọng đối với người làm dữ liệu:
- **Tư duy phản biện**: Không chỉ nhìn số liệu bề mặt mà phải biết đặt câu hỏi và kiểm tra nguyên nhân phía sau.
- **Kỹ năng giao tiếp**: Có khả năng làm việc với nhiều phòng ban để hiểu bài toán và truyền đạt kết quả phân tích.
- **Data Storytelling**: Biết biến dữ liệu thành câu chuyện dễ hiểu, có ý nghĩa và hỗ trợ quyết định.
- **Khả năng giải quyết vấn đề thực tế**: Không phân tích dữ liệu chỉ để tạo biểu đồ, mà phải hướng đến hành động và giá trị cho doanh nghiệp.
Một điểm tôi ấn tượng là lộ trình phát triển nghề nghiệp gồm nhiều cấp độ:
`Follower` → `Learner` → `Problem Solver` → `System Thinker` → `Super Star`
Lộ trình này cho thấy người làm công nghệ không nên chỉ quan tâm đến chức danh. Điều quan trọng hơn là năng lực giải quyết vấn đề, khả năng hiểu toàn cảnh hệ thống và mức độ đóng góp thật cho tổ chức.
Ngoài ra, văn hóa **No-Blame Post-Mortem** cũng được đề cập như một điểm rất đáng học hỏi trong các tập đoàn lớn. Khi hệ thống gặp sự cố, mục tiêu không phải là tìm người để đổ lỗi, mà là cùng nhau phân tích nguyên nhân, tìm lỗ hổng trong quy trình hoặc kiến trúc và cải thiện hệ thống để tránh lỗi tương tự trong tương lai.

### 3.3. Góc nhìn thực tế về DevOps Engineer

Phần chia sẻ về DevOps giúp tôi hiểu rõ hơn rằng DevOps không chỉ là người viết CI/CD pipeline, cấu hình Docker, Kubernetes hay trực hệ thống khi có sự cố. Trên thực tế, phạm vi công việc của DevOps thay đổi tùy theo quy mô công ty, mức độ trưởng thành hạ tầng và cách tổ chức đội ngũ kỹ thuật.
Diễn giả cũng đưa ra góc nhìn về thị trường DevOps tại Việt Nam trong giai đoạn 2025 - 2026. DevOps Engineer và Cloud Engineer vẫn là những vị trí có nhu cầu tuyển dụng cao, đặc biệt khi nhiều doanh nghiệp chuyển dịch hệ thống lên Cloud và cần tự động hóa quy trình vận hành.
Mức thu nhập của DevOps cũng được đánh giá khá tốt trên thị trường. Ở cấp Junior, mức lương có thể dao động khoảng **16 - 28 triệu VND/tháng**. Với cấp Lead hoặc Expert, mức thu nhập có thể lên đến **65 - 100 triệu VND/tháng**, tùy theo năng lực, kinh nghiệm và quy mô doanh nghiệp.
Tuy nhiên, điều quan trọng nhất không phải là chạy theo công cụ mới. Diễn giả nhấn mạnh tư duy **“Fundamentals Stay”**. Công cụ có thể thay đổi theo thời gian, nhưng kiến thức nền tảng sẽ luôn có giá trị.
Một DevOps Engineer giỏi cần xây dựng chắc các nền tảng như:
- Linux và hệ điều hành.
- Networking cơ bản.
- Ngôn ngữ lập trình như Python hoặc Golang.
- Tư duy hệ thống.
- Khả năng đặt câu hỏi “Tại sao” trước khi đi tìm cách làm.
Tôi nhận ra rằng nếu chỉ học thuộc cú pháp của từng công cụ, người học sẽ dễ bị tụt lại khi công nghệ thay đổi. Ngược lại, nếu nắm chắc nền tảng, người kỹ sư có thể thích nghi với nhiều công cụ và môi trường khác nhau.

## 4. Những kiến thức học được

### 4.1. Tư duy thiết kế

Sau sự kiện, tôi học được rằng thiết kế hệ thống không chỉ là ghép các dịch vụ lại với nhau. Người kỹ sư cần hiểu rõ đặc điểm tải, điểm nghẽn, hành vi người dùng và rủi ro vận hành để đưa ra kiến trúc phù hợp.
Một bài học quan trọng là tư duy **Pre-computation**. Thay vì để người dùng chờ hệ thống xử lý các tác vụ nặng theo thời gian thực, ta có thể chuẩn bị trước tài nguyên hoặc dữ liệu cần thiết. Cách làm này giúp giảm độ trễ và cải thiện trải nghiệm người dùng.
Tôi cũng hiểu rõ hơn giá trị của văn hóa **No-Blame**. Khi hệ thống gặp sự cố, điều quan trọng không phải là chỉ trích cá nhân, mà là nhìn nhận lỗi như một cơ hội để cải thiện quy trình, kiến trúc và năng lực vận hành của cả đội ngũ.

### 4.2. Kiến trúc kỹ thuật

Về mặt kỹ thuật, tôi hiểu rõ hơn cách triển khai **Key Generation Service** trong hệ thống URL Shortener. Việc kết hợp Redis để lưu short code trong bộ nhớ và DynamoDB để lưu dữ liệu chính giúp hệ thống vừa nhanh, vừa có khả năng mở rộng tốt.
Tôi cũng hiểu hơn vai trò của các dashboard trong vận hành doanh nghiệp. Những chỉ số như Fulfillment, Last Mile Cost hay Fill Rate không chỉ là con số, mà còn phản ánh hiệu quả vận hành và giúp doanh nghiệp đưa ra quyết định chính xác hơn.
Ngoài ra, sự kiện giúp tôi phân biệt rõ hơn các lớp bảo vệ trong kiến trúc Cloud. Sự kết hợp giữa AWS WAF, CloudFront và AWS KMS giúp hệ thống vừa giảm tải, vừa tăng khả năng bảo mật và bảo vệ dữ liệu.

### 4.3. Chiến lược hiện đại hóa

Một bài học khác là sự chuyển dịch từ “làm được” sang “làm đúng chuẩn”. Trong môi trường doanh nghiệp hoặc hệ thống lớn, sản phẩm không chỉ cần chạy được mà còn phải đáp ứng tiêu chuẩn vận hành, bảo mật và tuân thủ.
Với chuỗi cung ứng vật lý, doanh nghiệp cần quan tâm đến các tiêu chuẩn như GMP, GSP hoặc GDP. Còn với chuỗi cung ứng số và hạ tầng Cloud, các tiêu chuẩn như ISO 27001, SOC 2 hoặc GDPR trở thành yếu tố rất quan trọng.
Điều này giúp tôi hiểu rằng kỹ sư công nghệ không chỉ cần biết xây dựng tính năng, mà còn phải hiểu tiêu chuẩn, quy trình và trách nhiệm khi đưa hệ thống vào môi trường thực tế.

## 5. Ứng dụng vào học tập và công việc

Từ những kiến thức thu được, tôi có thể áp dụng vào quá trình học tập và làm việc như sau:
- **Áp dụng Cache-aside vào các dự án Backend**: Khi xây dựng API, tôi có thể bổ sung Redis Cache phía trước database để giảm tải truy vấn và tăng tốc độ phản hồi.
- **Rèn luyện tư duy System Thinker**: Khi thay đổi một chức năng hoặc cấu hình hệ thống, tôi cần đánh giá tác động đến module khác, chi phí Cloud và hiệu năng tổng thể.
- **Tập trung học nền tảng DevOps**: Thay vì chỉ học công cụ, tôi cần đào sâu Linux, Networking, Git branching strategies và các nguyên lý vận hành hệ thống.
- **Làm việc theo tinh thần “Đúng việc”**: Tôi cần rèn luyện sự tự quản trị, làm việc có trách nhiệm và hướng đến việc giải quyết các bài toán thật của xã hội.
- **Thực hành và chia sẻ lại**: Tôi nên tham gia nhiều hơn vào các hoạt động hands-on lab, thử nghiệm công nghệ mới và đóng góp kiến thức cho cộng đồng.

## 6. Trải nghiệm cá nhân tại sự kiện

Tham gia **FCAJ Community Day - June 2026 Edition** vào ngày 13/06/2026 là một trải nghiệm rất đáng nhớ đối với tôi. Sự kiện mang lại nhiều kiến thức thực tế về Cloud, DevOps, dữ liệu và cách phát triển bản thân trong ngành công nghệ.

### 6.1. Học hỏi từ các diễn giả có kinh nghiệm

Các diễn giả đã chia sẻ nhiều câu chuyện thực tế, giúp tôi hiểu rõ hơn khoảng cách giữa việc học công nghệ trên lý thuyết và việc áp dụng vào môi trường doanh nghiệp thật.
Một câu nói khiến tôi ấn tượng là không nên dùng AI để “tắt não”, mà nên dùng AI để nâng cao năng lực của bản thân. Điều này giúp tôi nhìn lại cách học của mình trong thời đại AI và hiểu rằng công cụ chỉ thật sự có giá trị khi người dùng vẫn giữ được tư duy nền tảng.

### 6.2. Tiếp cận kiến trúc kỹ thuật thực tế

Phần phân tích kiến trúc AWS chịu tải cao giúp tôi hình dung rõ hơn cách một request đi qua nhiều lớp trong hệ thống. Từ Route 53, ALB, các dịch vụ chạy trên Fargate cho đến Redis và DynamoDB, mỗi thành phần đều có vai trò riêng trong việc đảm bảo hiệu năng và độ sẵn sàng cao.
Nhờ đó, tôi hiểu rõ hơn rằng một hệ thống High Availability không chỉ nằm ở việc dùng Cloud, mà nằm ở cách thiết kế luồng xử lý, phân tách trách nhiệm và chuẩn bị phương án dự phòng cho từng lớp.

### 6.3. Trải nghiệm công cụ và dashboard doanh nghiệp

Tôi cũng được tiếp cận với các mẫu dashboard vận hành trong doanh nghiệp. Qua đó, tôi nhận ra rằng dữ liệu thô nếu được xử lý và trình bày đúng cách có thể trở thành công cụ hỗ trợ ra quyết định rất mạnh.
Dashboard không chỉ dùng để hiển thị số liệu, mà còn giúp người quản lý nhìn ra vấn đề, theo dõi xu hướng và đưa ra hành động phù hợp.

### 6.4. Kết nối và trao đổi

Không khí sự kiện rất cởi mở và gần gũi. Bên cạnh các phần trình bày trực tiếp, sự kiện còn có sự kết nối trực tuyến qua Google Meet với nhiều thành viên cùng tham gia. Điều này giúp tôi có thêm cơ hội lắng nghe kinh nghiệm từ các AWS Community Builders và mở rộng network chuyên môn.
Tôi nhận thấy rằng việc tham gia cộng đồng công nghệ là một cách rất tốt để học hỏi, cập nhật kiến thức và tìm thêm động lực phát triển bản thân.

## 7. Bài học rút ra

Sau sự kiện, tôi rút ra một số bài học quan trọng:
- Công cụ và framework có thể thay đổi liên tục, nhưng kiến thức nền tảng và tư duy hệ thống sẽ luôn giữ vai trò cốt lõi.
- Muốn phát triển bền vững trong ngành công nghệ, người kỹ sư cần học bằng cách thực hành, xây dựng sản phẩm thật và liên tục cải thiện kỹ năng.
- AI nên được sử dụng như một công cụ hỗ trợ tư duy, không phải là cách để thay thế hoàn toàn việc học và suy nghĩ của con người.
- Một hệ thống tốt không chỉ cần chạy được, mà còn phải ổn định, bảo mật, dễ mở rộng và đáp ứng được các tiêu chuẩn thực tế.
- Việc chia sẻ lại kiến thức cho cộng đồng giúp bản thân học sâu hơn và tạo ra nhiều giá trị hơn cho những người xung quanh.

## 8. Một số hình ảnh tham gia sự kiện

![Ảnh minh chứng: tham gia event](<images/4-EventParticipated/Event2/event2(0).jpg>)

>Tóm lại, **FCAJ Community Day - June 2026 Edition** đã mang đến cho tôi nhiều góc nhìn thực tế và giá trị về Cloud, DevOps, Data Analytics và tư duy nghề nghiệp. Sự kiện không chỉ bổ sung kiến thức cho báo cáo thực tập mà còn giúp tôi định hình rõ hơn con đường trở thành một kỹ sư công nghệ có nền tảng vững, làm việc có trách nhiệm và hướng đến chuẩn mực quốc tế.