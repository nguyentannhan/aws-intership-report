---
title: "Event 4"
date: 2026-06-27
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Bài thu hoạch “FCAJ Community Day - June 2026 Edition”

### 1. Mục tiêu của sự kiện

- Giúp người tham gia có cái nhìn rõ hơn về lộ trình phát triển nghề nghiệp trong lĩnh vực Cloud/DevOps, đặc biệt trong bối cảnh AI đang tạo ra nhiều thay đổi trên thị trường lao động.
- Chia sẻ cách xây dựng hệ thống trợ lý giọng nói ứng dụng AI, có khả năng xử lý tiếng Việt và áp dụng vào các bài toán thực tế trong ngành Ngân hàng.
- Giới thiệu hướng tiếp cận tự động hóa quy trình vận hành, giám sát và xử lý sự cố hạ tầng thông qua DevOps AI Agent.
- Trình bày cách ứng dụng Amazon Q Business, giao thức MCP và mô hình kết nối riêng tư nhằm nâng cao hiệu quả quản trị nhân sự, đồng thời đảm bảo an toàn dữ liệu nội bộ.

### 2. Danh sách diễn giả

- **Anh Steve Trần** - Founder của Cloud Thinker, cựu Solution Architect tại Amazon/AWS Việt Nam.
- **Anh Hiếu Nghị** - Cloud Specialist đến từ Renova Cloud.
- **Anh Kiệt** - Kỹ sư giải pháp đến từ Student Video Group.
- **Anh Trung** - Founder và CEO của R AI, từng sáng lập startup công nghệ tại Mỹ và được mua lại bởi công ty con của Google.
- **Anh Nguyên Nguyễn & Chị Bảo** - Cloud Engineer đến từ Cloud Kinetics.
- **Anh Trường (Owen) & Chị Minh Anh** - Solution Architects đến từ Noventic.
- **Bạn Toàn Nguyễn** - AWS Security Builder.

### 3. Nội dung nổi bật của sự kiện

#### 3.1. Định hướng Career Path và nền tảng "Agentic Ops" cho hạ tầng Cloud

- **Sự thay đổi của thị trường tuyển dụng**: Thị trường việc làm dành cho lập trình viên đang có sự dịch chuyển rõ rệt. Nhiều doanh nghiệp, từ tập đoàn lớn đến startup, có xu hướng giảm tuyển dụng các vị trí phổ thông và ưu tiên tìm kiếm những nhân sự có kinh nghiệm, biết tận dụng AI để tăng tốc độ phát triển sản phẩm và triển khai mã nguồn.
- **Vai trò của con người trong hạ tầng Production**: Đối với hệ thống Cloud Infrastructure, đặc biệt là môi trường Production, mỗi phút gián đoạn đều có thể gây ra thiệt hại lớn về tài chính và uy tín doanh nghiệp. Vì vậy, AI không thể hoàn toàn thay thế con người mà chủ yếu đóng vai trò hỗ trợ. Doanh nghiệp vẫn cần đội ngũ kỹ sư có chuyên môn cao để đưa ra quyết định chính xác trong các tình huống sự cố quan trọng.
- **Giải pháp Agentic Platform của Cloud Thinker**: Cloud Thinker giới thiệu định hướng xây dựng một hệ điều hành Agent, có khả năng hiểu được kiến trúc hệ thống từ tầng mã nguồn cho đến tầng logic nghiệp vụ. Nền tảng này hỗ trợ kỹ sư đọc log, phân tích nguyên nhân sự cố trong thời gian ngắn, tự động rà soát mã nguồn, đánh giá lỗi và tối ưu chi phí Cloud theo hướng FinOps. Điểm đáng chú ý là hệ thống không bị phụ thuộc cố định vào một nhà cung cấp hạ tầng duy nhất.

#### 3.2. Kiến trúc trợ lý giọng nói Voice AI Agent cho thị trường tiếng Việt

- **Thách thức về ngôn ngữ tiếng Việt**: Các mô hình Speech-to-Speech hiện đại trên thế giới hiện nay thường hỗ trợ tiếng Anh tốt hơn so với tiếng Việt. Vì tiếng Việt là ngôn ngữ có nhiều đặc trưng về dấu, vùng miền và ngữ cảnh giao tiếp, việc áp dụng trực tiếp các mô hình quốc tế vào thị trường Việt Nam vẫn còn nhiều hạn chế.
- **Mô hình Voice Agent 3 tầng: STT → LLM → TTS**: Để triển khai Voice Bot tổng đài cho các ngân hàng lớn như VPBank hoặc VIB, giải pháp phù hợp là chia quy trình xử lý thành 3 lớp chính:
  1. *Speech-to-Text (STT)*: Hệ thống tiếp nhận âm thanh theo thời gian thực và chuyển đổi giọng nói thành văn bản dạng streaming
  2. *Large Language Model (LLM)*: Văn bản sau khi được chuyển đổi sẽ được đưa vào mô hình ngôn ngữ lớn để xử lý nghiệp vụ, hiểu yêu cầu của khách hàng và tạo phản hồi ở dạng text. Lớp này giúp doanh nghiệp kiểm soát nội dung phản hồi, hạn chế tình trạng AI trả lời sai ngữ cảnh, đồng thời hỗ trợ gọi các công cụ hệ thống như khóa thẻ ngân hàng, xác thực CCCD hoặc kiểm tra thông tin khách hàng.
  3. *Text-to-Speech (TTS)*: Phản hồi văn bản tiếp tục được chuyển thành giọng nói tự nhiên. Hệ thống có thể xử lý cách xưng hô theo giới tính như “Anh/Chị”, đồng thời áp dụng cơ chế turn-taking để xử lý các tình huống người dùng nói ngắt quãng hoặc chen ngang trong cuộc hội thoại.

#### 3.3. Tự động hóa xử lý sự cố Production bằng DevOps AI Agent

- **Khó khăn trong vận hành hệ thống lớn**: Khi hệ thống gặp sự cố như ứng dụng bị sập, độ trễ tăng cao hoặc dịch vụ phản hồi bất thường, đội ngũ DevOps thường phải mất nhiều thời gian để kiểm tra. Nguyên nhân là log, trace và các dữ liệu giám sát thường nằm rải rác ở nhiều nơi, khiến quá trình tìm lỗi và khôi phục hệ thống bị kéo dài.
- **Quy trình 4 bước của DevOps Agent**:
  - *Bước 1 - Phân loại sự cố*: AI nhận tín hiệu trigger từ cảnh báo CloudWatch, sau đó tự động thu thập và gom nhóm các log liên quan đến sự cố.
  - *Bước 2 - Điều tra nguyên nhân*: Dựa trên sơ đồ topology của hệ thống đã được học trước đó, AI đưa ra các giả thuyết lỗi và tiến hành phân tích để tìm nguyên nhân gốc rễ.
  - *Bước 3 - Đề xuất phương án khắc phục*: AI tạo ra một kịch bản xử lý sự cố theo hướng an toàn. Tuy nhiên, trước khi thực thi trên môi trường Production, phương án vẫn cần có sự xác nhận của con người theo cơ chế Human-in-the-loop để hạn chế rủi ro.
  - *Bước 4 - Cải tiến dài hạn*: Sau khi xử lý sự cố, hệ thống tiếp tục đề xuất các phương án tối ưu để tránh việc lỗi tương tự lặp lại trong tương lai.
- **Chi phí vận hành**: Dịch vụ được tính phí dựa trên thời gian chạy thực tế của Agent. Mức chi phí được chia sẻ khoảng **0.083 USD/giây** cho toàn bộ quá trình tính toán, dựng topology và xử lý incident.

#### 3.4. Ứng dụng Amazon Q Business và giao thức MCP trong quản trị nhân sự

- **Vấn đề trong quy trình tuyển dụng truyền thống**: Bộ phận HR thường mất nhiều thời gian khi phải đọc và lọc CV thủ công. Quy trình này có thể kéo dài từ 1 đến 2 tháng, dễ bỏ sót ứng viên phù hợp và đôi khi kết quả đánh giá còn bị ảnh hưởng bởi cảm tính thay vì dựa trên dữ liệu rõ ràng.
- **Xây dựng HR Assistant bằng Quick Desktop**: Diễn giả trình bày cách nạp các file Markdown có cấu trúc vào hệ thống để AI tự động cấu hình kỹ năng xử lý. Công cụ có thể OCR và phân tích sâu dữ liệu CV, kể cả các file scan hoặc hình ảnh. Sau đó, AI đối chiếu thông tin ứng viên với yêu cầu trong JD để chấm điểm, phân loại ứng viên theo các mức như Strong, Good hoặc Low, đồng thời hỗ trợ đồng bộ lịch phỏng vấn và soạn email gửi ứng viên.

#### 3.5. Giải pháp Private Security và bảo mật kết nối MCP Server

- **Nguy cơ rò rỉ dữ liệu trong môi trường Enterprise**: Khi các AI Agent được kết nối với các nền tảng bên thứ ba như Gmail, Jira, GitHub hoặc Zalo thông qua Public Endpoint, doanh nghiệp có thể đối mặt với nhiều rủi ro bảo mật như Prompt Injection, Man-in-the-middle hoặc rò rỉ dữ liệu nội bộ.
- **Kiến trúc kết nối bảo mật nội bộ thông qua VPC Connection**:
  - Toàn bộ MCP Server được triển khai bên trong **Private Subnet** của doanh nghiệp, hạn chế truy cập trực tiếp từ môi trường Internet công cộng.
  - Sử dụng **VPC Connection** kết hợp với **Interface Endpoint** để Amazon Q Business có thể truy vấn dữ liệu nội bộ một cách riêng tư, không cần đi qua Internet công khai.
  - Luồng traffic được phân phối thông qua **Application Load Balancer (ALB)**, kết hợp chứng chỉ TLS từ AWS Certificate Manager (ACM) để mã hóa dữ liệu truyền tải.
  - Việc xác thực người dùng được thực hiện thông qua **Amazon Cognito**, giúp kiểm soát quyền truy cập an toàn hơn.
  - *Chi phí ước tính*: Mô hình hạ tầng private này có chi phí khoảng **250 - 350 USD/tháng**, bao gồm các thành phần như Route 53 Resolver, ALB, EC2 và các dịch vụ liên quan. Đây là mức chi phí cần thiết để đảm bảo luồng dữ liệu nội bộ được bảo vệ chặt chẽ hơn.

### 4. Những kiến thức học được

#### 4.1. Tư Duy Thiết Kế

- **Tư duy cộng tác giữa AI và con người**: AI nên được xem là công cụ hỗ trợ và khuếch đại năng lực làm việc của con người, thay vì là giải pháp thay thế hoàn toàn. Đặc biệt, với các tác vụ liên quan đến sửa mã nguồn hoặc can thiệp trực tiếp vào hạ tầng Production, con người vẫn phải là người kiểm duyệt cuối cùng.
- **Tư duy bảo mật theo hướng Zero Trust**: Khi triển khai AI trong các lĩnh vực nhạy cảm như Ngân hàng, Tài chính hoặc Y tế, yếu tố quan trọng nhất không chỉ là hệ thống hoạt động được, mà còn là đảm bảo bảo mật, quyền riêng tư và tuân thủ các yêu cầu pháp lý.

#### 4.2. Kiến Trúc Kỹ Thuật

- Hiểu rõ mô hình **Voice Bot 3 lớp**, bao gồm STT, LLM và TTS, cũng như vai trò của từng lớp trong việc xử lý hội thoại bằng tiếng Việt.
- Nhận thức được tầm quan trọng của việc xây dựng dữ liệu huấn luyện đa dạng, đặc biệt là dữ liệu giọng nói vùng miền, nhằm cải thiện trải nghiệm người dùng cuối tại thị trường Việt Nam.
- Nắm được cách AI được phân quyền và cô lập môi trường hoạt động thông qua khái niệm **Agent Space** trong hệ sinh thái DevOps Agent.
- Hiểu cách thiết kế mô hình **VPC Connection** kết hợp với MCP Server để tạo ra một đường truyền dữ liệu khép kín, an toàn và phù hợp với môi trường doanh nghiệp trên AWS.

#### 4.3. Chiến Lược Hiện Đại Hóa

- **Hiện đại hóa quy trình theo hướng nền tảng**: Việc đưa AI Agent vào doanh nghiệp lớn không chỉ đơn giản là thêm một công cụ mới. Doanh nghiệp cần tái cấu trúc một phần lớn quy trình vận hành truyền thống, đồng thời có lộ trình chuyển đổi phù hợp thông qua các dịch vụ Managed Service để người dùng có thể làm quen và áp dụng từng bước.

### 5. Ứng dụng vào học tập và công việc

- **Tối ưu CV để phù hợp với hệ thống AI Filter**: Khi viết CV, cần trình bày thông tin rõ ràng, có cấu trúc và sử dụng các từ khóa kỹ thuật sát với yêu cầu trong JD. Điều này giúp CV dễ được các hệ thống sàng lọc tự động bằng AI nhận diện và đánh giá chính xác hơn.
- **Thực hành với DevOps AI Agent**: Có thể tận dụng chương trình dùng thử miễn phí 2 tháng của DevOps Agent trên AWS Console để trải nghiệm việc cấu hình, ánh xạ topology và thử phân tích các sự cố mô phỏng trên những cluster nhỏ.
- **Tìm hiểu thêm về giao thức MCP**: Nghiên cứu cách xây dựng một MCP Server cơ bản trên môi trường local để hiểu cách các công cụ AI có thể kết nối với dữ liệu cá nhân hoặc dữ liệu doanh nghiệp theo cách có kiểm soát.
- **Tập trung vào kiến thức nền tảng**: Không nên phụ thuộc hoàn toàn vào các công cụ AI sinh mã tự động. Thay vào đó, cần tiếp tục rèn luyện các kiến thức cốt lõi như Backend, mạng máy tính, JWT, bảo mật, Terraform và Infrastructure as Code để có thể làm chủ hệ thống khi xảy ra sự cố thực tế.

### 6. Trải nghiệm cá nhân tại sự kiện

Tham gia sự kiện **“FCAJ Community Day - June 2026 Edition”** tại không gian văn phòng tầng 26 và 36 là một trải nghiệm học tập và kết nối rất ý nghĩa. Sự kiện giúp tôi có thêm góc nhìn thực tế hơn về cách GenAI đang được ứng dụng trong các môi trường Enterprise lớn, đặc biệt ở những lĩnh vực yêu cầu tính bảo mật và độ ổn định cao.

#### 6.1. Học hỏi từ các diễn giả có kinh nghiệm

Tôi có cơ hội lắng nghe những chia sẻ thực tế từ các founder, chuyên gia Cloud và kỹ sư nhiều kinh nghiệm như anh Steve Trần từ Cloud Thinker hay anh Trung từ R AI. Những câu chuyện về hành trình phát triển sự nghiệp, đặc biệt là quá trình vươn lên vị trí Solution Architect tại Amazon, đã mang lại cho tôi nhiều động lực trong việc tự học và định hướng nghề nghiệp sau này.

#### 6.2. Quan sát các demo kỹ thuật thực tế

Một trong những điểm nổi bật của sự kiện là các phần live demo. Tôi được quan sát hệ thống Voice Agent phản hồi thông tin sản phẩm Apple trên nền tảng Bedrock Agent Core, cũng như demo DevOps Agent tự động quét và dựng bản đồ hơn 300 mối quan hệ tài nguyên trong hệ thống chỉ trong khoảng 15 phút.
Các phần trình bày bằng flow diagram cũng giúp tôi hiểu rõ hơn cách xử lý ngôn ngữ trong môi trường ngân hàng, nơi yêu cầu tính chính xác, an toàn và kiểm soát thông tin rất cao.

#### 6.3. Tiếp cận các công cụ hiện đại

Thông qua phần trình bày về Amazon Quick Desktop, tôi hiểu hơn cách một người dùng không chuyên về kỹ thuật vẫn có thể khai thác dữ liệu và thực hiện các công việc phân tích cơ bản. Công cụ này giúp tự động hóa nhiều tác vụ lặp lại trong bộ phận quản trị nhân sự, từ đọc CV, đánh giá ứng viên cho đến hỗ trợ sắp xếp lịch phỏng vấn.

#### 6.4. Kết nối và trao đổi trong cộng đồng

Không khí sự kiện diễn ra rất cởi mở và sôi nổi. Các hoạt động hỏi đáp trực tiếp, trao đổi với diễn giả và minigame giúp người tham gia dễ dàng kết nối với nhau hơn. Đây cũng là cơ hội tốt để sinh viên có thể tiếp cận gần hơn với cộng đồng kỹ sư, chuyên gia và những người đang làm việc thực tế trong ngành.

#### 7. Bài học rút ra

- Sau sự kiện, tôi nhận ra rằng công nghệ không nên được theo đuổi một cách mù quáng. Điều quan trọng hơn là phải hiểu bài toán nghiệp vụ, hiểu nhu cầu thật sự của doanh nghiệp và lựa chọn công nghệ phù hợp để giải quyết vấn đề đó.
- Bên cạnh đó, mọi giải pháp kiến trúc đều có sự đánh đổi. Một hệ thống dù hiện đại đến đâu cũng cần cân bằng giữa hiệu năng, độ trễ, bảo mật, khả năng mở rộng và chi phí vận hành.

#### 8. Một số hình ảnh khi tham gia sự kiện

![Ảnh minh chứng: tham gia event](</images/4-EventParticipated/Event4/event4(0).jpg>)

> Tóm lại, buổi sinh hoạt Community Day tháng 6 đã mang lại cho tôi nhiều kiến thức thực tế về Agentic Ops, Voice AI Agent, DevOps AI Agent và bảo mật AI trong môi trường doanh nghiệp. Quan trọng hơn, sự kiện giúp tôi hình thành tư duy hệ thống rõ ràng hơn, hiểu được vai trò của AI trong công việc hiện đại và có thêm sự tự tin cho định hướng nghề nghiệp trong tương lai.