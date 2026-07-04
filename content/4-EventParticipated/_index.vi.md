---
title: "Event 2"
date: 2026-05-23
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “FCAJ Community Day - May 2026 Edition”

## 1. Mục tiêu của sự kiện

Sự kiện **FCAJ Community Day - May 2026 Edition** được tổ chức nhằm cập nhật cho sinh viên và người làm công nghệ những xu hướng mới trong ngành CNTT, đặc biệt là tác động mạnh mẽ của AI đối với thị trường lao động, kỹ thuật phát triển phần mềm và cách doanh nghiệp ứng dụng AI vào thực tế.

Các mục tiêu chính của sự kiện gồm:

- Cập nhật bức tranh tổng quan về thị trường CNTT Việt Nam trong giai đoạn AI phát triển nhanh.
- Chia sẻ cách xây dựng Prompt hiệu quả, tối ưu ngữ cảnh và kiểm soát độ ngẫu nhiên khi làm việc với LLM.
- Giới thiệu Amazon Q Business, QuickSight và MCP Server trong việc xây dựng trợ lý phân tích dữ liệu cho doanh nghiệp.
- Chia sẻ kinh nghiệm thực tế từ Hackathon và cách thiết kế hệ thống Multi-Agent theo tiêu chuẩn Enterprise.
- Cung cấp góc nhìn thực chiến về ứng dụng AI Agent trong các lĩnh vực yêu cầu tính bảo mật và độ tin cậy cao như tài chính - ngân hàng.

## 2. Danh sách diễn giả

Sự kiện có sự tham gia của nhiều diễn giả đến từ các doanh nghiệp công nghệ, ngân hàng và cộng đồng AWS:

- **Anh Nguyễn Gia Hưng**: Solutions Architect tại AWS Việt Nam và Founder của FCAJ, chia sẻ về định hướng thị trường CNTT trong thời đại AI.
- **Anh Tịnh Trương**: Platform Engineer tại Gothamic X, trình bày về tối ưu ngữ cảnh và kiểm soát tính ngẫu nhiên của LLM.
- **Anh Hải Anh**: Solutions Architect tại Pacific Việt Nam, chia sẻ về Amazon Q Business và MCP.
- **Anh Nguyễn Hấn Thịnh**: DevOps Engineer, chia sẻ về Flat-rate Pricing và các lớp bảo mật nâng cao của Amazon CloudFront.
- **Bạn Uyển, bạn Mách và chị Thảo**: Nhóm Winner Hackathon, chia sẻ hành trình phát triển dự án UTMorpho trong 36 giờ.
- **Diễn giả FinTech**: Cố vấn giải pháp FinTech tại VPBank, chia sẻ về kiến trúc Multi-Agent trong nghiệp vụ ngân hàng.

## 3. Nội dung nổi bật của sự kiện

### 3.1. Thị trường CNTT và cơ hội nghề nghiệp trong thời đại AI

Một trong những nội dung mở đầu đáng chú ý là góc nhìn về sự thay đổi của thị trường CNTT khi AI ngày càng phổ biến. Diễn giả đưa ra ví dụ về nghịch lý “đèn LED”. Khi chi phí chiếu sáng giảm mạnh nhờ đèn LED, nhu cầu sử dụng ánh sáng không giảm mà còn tăng lên. Tương tự, khi AI làm cho việc viết code trở nên nhanh và rẻ hơn, nhu cầu phát triển phần mềm có thể sẽ tăng mạnh hơn trước.

AI không làm phần mềm biến mất, mà khiến nhiều người có thể tạo ra sản phẩm hơn. Ngay cả những người không học chuyên ngành CNTT như bác sĩ, luật sư hoặc người làm kinh doanh cũng có thể sử dụng AI để tham gia Hackathon hoặc xây dựng MVP. Điều này mở ra nhiều cơ hội mới nhưng cũng tạo ra thách thức lớn về chất lượng phần mềm.

Một vấn đề được nhấn mạnh là thị trường sẽ xuất hiện nhiều sản phẩm được tạo ra nhanh bằng AI nhưng thiếu ổn định, khó bảo trì hoặc chứa nhiều lỗi. Từ đó, nhu cầu về các kỹ sư có khả năng sửa lỗi, tối ưu hệ thống, tái cấu trúc code và xây dựng nền tảng vận hành bền vững sẽ tăng lên.

Đặc biệt, vai trò **Platform Engineering** sẽ ngày càng quan trọng. Các tổ chức lớn cần những kỹ sư có thể xây dựng Internal Developer Platform, tự động hóa hạ tầng và giúp đội ngũ phát triển phần mềm làm việc hiệu quả hơn.

Đối với thị trường Việt Nam, nhiều tập đoàn công nghệ lớn vẫn tiếp tục mở Technical Hub để tận dụng nguồn nhân lực trẻ. Vì vậy, kỹ sư Việt Nam cần đầu tư nhiều hơn vào tiếng Anh, kiến thức nghiệp vụ thực tế, khả năng thiết kế sản phẩm hoàn chỉnh và tư duy triển khai hệ thống production-ready thay vì chỉ dừng lại ở các bài demo đơn giản.

### 3.2. Tối ưu Context và kiểm soát Randomness của LLM

Phần chia sẻ về LLM giúp người tham gia hiểu rõ hơn cách AI xử lý thông tin và vì sao đôi khi cùng một câu hỏi nhưng AI lại trả lời khác nhau.

Một lỗi phổ biến khi sử dụng AI là đưa quá nhiều chủ đề không liên quan vào cùng một cuộc hội thoại. Ví dụ, trong một phiên chat vừa hỏi về du lịch, vừa sửa CV, vừa nhờ viết code. Điều này làm Context Window bị thay đổi liên tục, khiến AI khó giữ trọng tâm và dễ tạo ra câu trả lời sai hoặc không nhất quán.

Diễn giả cũng giải thích rằng LLM hoạt động như một **probabilistic engine**, tức là mô hình dự đoán từ tiếp theo dựa trên xác suất. Mỗi từ có một điểm số gọi là **logit**, sau đó mô hình sẽ chọn từ phù hợp để tạo câu trả lời.

Thông số **Temperature** có vai trò kiểm soát mức độ sáng tạo hoặc ổn định của đầu ra:

- Khi Temperature cao, câu trả lời có thể sáng tạo hơn nhưng cũng dễ thay đổi hơn.
- Khi Temperature thấp, đặc biệt là `Temperature = 0`, mô hình sẽ ưu tiên chọn từ có xác suất cao nhất để tạo ra kết quả ổn định hơn.
- Với các tác vụ cần cấu trúc chính xác như JSON, HTML hoặc code, nên dùng Temperature thấp để giảm lỗi định dạng.

Tuy nhiên, một điểm rất quan trọng là ngay cả khi đặt `Temperature = 0`, kết quả từ các API Cloud như OpenAI hoặc Amazon Bedrock vẫn có thể khác nhau giữa các lần chạy. Nguyên nhân đến từ cách GPU xử lý song song, sai số làm tròn số thập phân và các kỹ thuật tối ưu inference của nhà cung cấp Cloud.

Vì vậy, nếu doanh nghiệp cần kiểm soát tuyệt đối độ nhất quán của mô hình, việc tự host model trên hạ tầng local có thể là lựa chọn phù hợp hơn.

### 3.3. Amazon Q Business, QuickSight và MCP trong phân tích dữ liệu

Một nội dung đáng chú ý khác là cách Amazon Q Business và QuickSight hỗ trợ người dùng doanh nghiệp phân tích dữ liệu nhanh hơn. Thay vì yêu cầu người dùng phải có kiến thức chuyên sâu về BI, hệ thống có thể nhận dữ liệu thô như file Excel, sau đó tự động phân tích và tạo dashboard trực quan.

Điều này giúp rút ngắn thời gian từ dữ liệu thô đến báo cáo phân tích. Người dùng chỉ cần đặt câu hỏi hoặc đưa yêu cầu bằng ngôn ngữ tự nhiên, hệ thống có thể hỗ trợ tạo biểu đồ, tóm tắt xu hướng và đưa ra các góc nhìn phân tích cần thiết.

Bên cạnh đó, diễn giả cũng giới thiệu **MCP - Model Context Protocol**. Đây là cách giúp AI Agent không chỉ dừng lại ở việc trả lời văn bản mà còn có thể thực hiện hành động. Thông qua MCP Server, AI có thể kết nối với nhiều hệ thống khác nhau như Jira, Confluence, Microsoft Cloud, Google Suite hoặc các công cụ nội bộ của doanh nghiệp.

Nhờ đó, AI Agent có thể trở thành một “cánh tay nối dài” thực sự, hỗ trợ các tác vụ như đặt lịch họp, gửi email, tra cứu tài liệu, phân tích dữ liệu và tương tác với các hệ thống doanh nghiệp.

### 3.4. Case Study UTMorpho - Dự án thắng Hackathon trong 36 giờ

Nhóm thắng Hackathon đã chia sẻ hành trình phát triển dự án **UTMorpho** trong 36 giờ liên tục. Điểm hay của dự án là nhóm không chọn một ý tưởng quá vĩ mô, mà tập trung giải quyết một nỗi đau rất thực tế của lập trình viên khi làm giao diện với AI.

Thông thường, khi AI tạo ra một giao diện HTML/CSS, nếu muốn chỉnh sửa một chi tiết nhỏ như vị trí nút, kích thước khối hoặc màu sắc, người dùng thường phải yêu cầu AI tạo lại gần như toàn bộ giao diện. Việc này tốn thời gian, tốn token và dễ làm mất cấu trúc ban đầu.

UTMorpho giải quyết vấn đề này bằng cách xây dựng một hệ thống Multi-Agent trên AWS Serverless. Hệ thống gồm 3 Agent chính:

- **Vision Agent**: Đọc hình ảnh hoặc bản vẽ phác thảo từ camera, sau đó chuyển thông tin thành cấu trúc JSON.
- **Layout Agent**: Tiếp nhận JSON và tính toán bố cục, kích thước, CSS và cấu trúc layout.
- **Design Agent**: Chuyển kết quả từ Layout Agent thành mã nguồn HTML/CSS hoàn chỉnh theo thời gian thực.

Một điểm nổi bật của UTMorpho là có editor trực quan, cho phép lập trình viên kéo thả và chỉnh sửa component ngay trên giao diện. Sau khi hoàn thành, hệ thống có thể xuất link HTML public để nhóm hoặc đồng nghiệp cùng xem và review.

Dự án cho thấy nếu biết chia bài toán đúng cách, Multi-Agent có thể hỗ trợ rất tốt trong việc biến ý tưởng thành sản phẩm nhanh mà vẫn giữ được tính kiểm soát.

### 3.5. Multi-Agent trong nghiệp vụ ngân hàng

Phần cuối của sự kiện trình bày một bài toán thực tế trong lĩnh vực tài chính - ngân hàng: đánh giá tín dụng cho startup.

Trong mô hình truyền thống, doanh nghiệp muốn vay vốn thường cần cung cấp báo cáo tài chính trong nhiều năm và tài sản thế chấp rõ ràng. Tuy nhiên, nhiều startup chỉ mới hoạt động trong vài tháng, chưa có báo cáo tài chính dài hạn và tài sản chủ yếu là ý tưởng, công nghệ hoặc dữ liệu. Vì vậy, họ rất khó tiếp cận nguồn vốn theo cách đánh giá truyền thống.

Giải pháp được chia sẻ là thiết kế hệ thống **Multi-Agent** để phân tích nhiều nguồn dữ liệu phi truyền thống. Mỗi Agent đảm nhận một vai trò chuyên biệt:

- **Credit Committee Agent**: Đóng vai trò điều phối, phân công nhiệm vụ và tổng hợp báo cáo cuối cùng.
- **Financial Analyst Agent**: Phân tích báo cáo tài chính ngắn hạn, dòng tiền và khả năng vận hành của doanh nghiệp.
- **Market Research Agent**: Đánh giá thị trường, đối thủ cạnh tranh, quy mô thị trường và xu hướng ngành.
- **Team Evaluator Agent**: Phân tích năng lực đội ngũ sáng lập thông qua hồ sơ công nghệ, mạng xã hội và các hoạt động công khai.
- **Risk Assessor Agent**: Kiểm soát rủi ro, lọc dữ liệu đầu vào và đầu ra, ngăn rò rỉ thông tin nhạy cảm và phòng chống Prompt Injection.

Trong môi trường ngân hàng, yếu tố bảo mật và tuân thủ là cực kỳ quan trọng. Vì vậy, Risk Assessor Agent đóng vai trò then chốt để đảm bảo hệ thống AI không vượt khỏi phạm vi cho phép và không làm lộ dữ liệu nhạy cảm.

## 4. Những kiến thức học được

### 4.1. Tư duy thiết kế

Sau sự kiện, tôi hiểu rằng khi xây dựng một giải pháp công nghệ cho doanh nghiệp, điều quan trọng không phải chỉ là công nghệ đó có mới hay không. Quan trọng hơn là giải pháp đó có giải quyết đúng vấn đề hay không.

Một sản phẩm công nghệ cần trả lời được các câu hỏi:

- Ai là người sử dụng?
- Họ sử dụng để làm gì?
- Vì sao họ cần giải pháp này?
- Khi nào là thời điểm phù hợp để triển khai?

Tôi cũng học được tư duy **Separation of Concerns** trong thiết kế Multi-Agent. Mỗi Agent chỉ nên phụ trách một nhiệm vụ cụ thể, có vai trò rõ ràng và phạm vi xử lý giới hạn. Điều này giúp tránh quá tải Context Window và giúp hệ thống dễ kiểm soát hơn.

### 4.2. Kiến trúc kỹ thuật

Về mặt kỹ thuật, tôi hiểu rõ hơn cách kiểm soát tính ổn định của LLM thông qua các tham số như `Temperature`, `Top P` và `Repeat Penalty`. Tùy theo loại đầu ra mong muốn, người dùng cần chọn cấu hình phù hợp. Ví dụ, với nội dung sáng tạo có thể tăng Temperature, còn với JSON hoặc code nên giảm Temperature để giữ cấu trúc chính xác.

Tôi cũng được tiếp cận kiến thức về hạ tầng mạng của CloudFront, AWS Backbone và các điểm hiện diện PoP. CloudFront không chỉ giúp tăng tốc phân phối nội dung mà còn hỗ trợ bảo vệ Origin Server trước các dạng tấn công lớn như DDoS hoặc Syn Flood thông qua cơ chế xử lý request và Syn Proxy.

Ngoài ra, sự kiện cũng nhấn mạnh vai trò của **Terraform** và Infrastructure as Code. Thay vì cấu hình thủ công trên AWS Console, kỹ sư nên quản lý hạ tầng bằng mã nguồn để dễ tái lập, theo dõi phiên bản và triển khai trên nhiều Region khác nhau.

### 4.3. Chiến lược hiện đại hóa

Một bài học quan trọng là các dự án AI trong doanh nghiệp không thể chỉ dừng lại ở demo. Để được triển khai thật, dự án cần chứng minh được giá trị bằng số liệu cụ thể.

Doanh nghiệp thường quan tâm đến các chỉ số như:

- Dự án giúp tiết kiệm bao nhiêu chi phí mỗi năm.
- Thời gian hoàn vốn là bao lâu.
- Năng suất tăng bao nhiêu phần trăm.
- Rủi ro vận hành có được giảm không.

Nói cách khác, một giải pháp AI muốn thuyết phục ban lãnh đạo cần có ROI rõ ràng.

Bên cạnh đó, kiểm thử liên tục cũng là yếu tố bắt buộc. Một hệ thống AI có thể sinh ra kết quả sai định dạng hoặc xử lý ngoài dự đoán. Vì vậy, cần test kỹ ở nhiều tầng, đặc biệt là các downstream services, để đảm bảo hệ thống vẫn hoạt động ổn định khi AI tạo ra đầu ra không như mong muốn.

## 5. Ứng dụng vào học tập và công việc

Từ những kiến thức thu được, tôi có thể áp dụng vào quá trình học tập và làm việc như sau:

- **Cấu hình Temperature phù hợp hơn**: Khi cần AI tạo JSON, HTML hoặc code, tôi sẽ ưu tiên dùng `Temperature = 0` hoặc `0.1` để giảm sai lệch cấu trúc.
- **Chia nhỏ bài toán khi làm việc với AI**: Thay vì yêu cầu một khung chat xử lý toàn bộ quy trình, tôi sẽ tách bài toán thành các nhiệm vụ nhỏ hơn và kiểm tra kết quả từng phần.
- **Áp dụng tư duy Multi-Agent**: Với các dự án phức tạp, tôi có thể chia vai trò như phân tích yêu cầu, thiết kế layout, sinh code và review để giảm lỗi.
- **Tăng cường bảo mật dữ liệu**: Khi làm việc với AI trong môi trường doanh nghiệp, cần có các lớp lọc dữ liệu đầu vào và đầu ra để tránh lộ thông tin nhạy cảm.
- **Tìm hiểu Terraform và Infrastructure as Code**: Tôi sẽ từng bước chuyển từ cấu hình thủ công sang quản lý hạ tầng bằng mã nguồn để dễ kiểm soát và tái triển khai.
- **Tập trung vào nền tảng Backend**: AI là công cụ hỗ trợ, nhưng tư duy hệ thống, kiến thức API, cấu trúc dữ liệu và kỹ nghệ phần mềm vẫn là yếu tố quan trọng nhất đối với lập trình viên.

## 6. Trải nghiệm cá nhân tại sự kiện

Đối với tôi, **FCAJ Community Day - May 2026 Edition** là một sự kiện rất đáng nhớ vì nội dung không chỉ mang tính lý thuyết mà còn gắn với các bài toán thực tế trong doanh nghiệp.

### 6.1. Học hỏi từ các diễn giả có kinh nghiệm

Các diễn giả đến từ AWS Việt Nam, VPBank, VIB, Pacific Việt Nam và Gothamic X đã mang lại nhiều góc nhìn thực tế. Tôi hiểu rõ hơn sự khác biệt giữa một dự án cá nhân hoặc đồ án nhỏ với một sản phẩm thật phục vụ số lượng lớn người dùng.

Đặc biệt, trong lĩnh vực ngân hàng, hệ thống không chỉ cần chạy được mà còn phải an toàn, ổn định, tuân thủ quy định và bảo vệ dữ liệu người dùng.

### 6.2. Tiếp cận các mô hình kỹ thuật hiện đại

Sự kiện giúp tôi hình dung rõ hơn cách Multi-Agent có thể phối hợp trong một hệ thống thực tế. Mỗi Agent đảm nhận một nhiệm vụ riêng, từ phân tích tài chính, nghiên cứu thị trường, đánh giá đội ngũ cho đến kiểm soát rủi ro.

Tôi cũng hiểu thêm rằng dù đặt Temperature bằng 0, AI vẫn có thể sinh kết quả khác nhau do các yếu tố hạ tầng và tối ưu inference phía sau. Điều này giúp tôi có cái nhìn thực tế hơn, không còn nghĩ rằng AI luôn cho kết quả tuyệt đối giống nhau.

### 6.3. Trải nghiệm công cụ hiện đại

Bản demo Amazon Q Business và QuickSight cho thấy việc phân tích dữ liệu đang trở nên dễ tiếp cận hơn rất nhiều. Người dùng không cần viết nhiều câu lệnh phức tạp mà có thể dùng ngôn ngữ tự nhiên để yêu cầu hệ thống phân tích, trực quan hóa và tạo dashboard.

Ngoài ra, MCP cũng mở ra một hướng đi mới cho AI Agent, giúp AI có thể kết nối với nhiều công cụ và thực hiện hành động thay vì chỉ trả lời văn bản.

### 6.4. Kết nối và trao đổi

Sự kiện tạo ra không gian cởi mở để sinh viên gặp gỡ, trao đổi và kết nối với những người có cùng định hướng. Các hoạt động giao lưu, đặt câu hỏi và Lucky Draw giúp không khí trở nên gần gũi, khuyến khích người tham gia chủ động hơn.

Tôi nhận thấy rằng việc tham gia cộng đồng công nghệ không chỉ giúp học thêm kiến thức mới mà còn mở rộng network, tìm kiếm cơ hội hợp tác và rèn luyện sự tự tin.

## 7. Bài học rút ra

Sau sự kiện, tôi rút ra một số bài học quan trọng:

- AI làm cho việc phát triển phần mềm trở nên nhanh và rẻ hơn, nhưng cũng làm tăng nhu cầu về những kỹ sư có tư duy hệ thống tốt.
- Không nên sử dụng AI theo kiểu sao chép kết quả mà không hiểu bản chất, vì điều đó có thể gây rủi ro lớn khi đưa vào môi trường production.
- Một hệ thống công nghệ tốt không chỉ là hệ thống chạy được, mà còn phải an toàn, đáng tin cậy, dễ bảo trì và tạo ra giá trị thật cho người dùng.
- Trong doanh nghiệp, công nghệ cần đi kèm với hiệu quả kinh doanh, khả năng đo lường ROI và quy trình kiểm thử nghiêm túc.
- Muốn phát triển lâu dài trong ngành CNTT, cần tập trung vào nền tảng kỹ thuật, tiếng Anh, kiến thức nghiệp vụ và khả năng làm sản phẩm thực tế.

## 8. Một số hình ảnh tham gia sự kiện

![Ảnh minh chứng: tham gia event](</images/4-EventParticipated/Event2/event2(0).jpg>)

    >Tổng kết lại, **FCAJ Community Day - May 2026 Edition** đã mang lại cho tôi nhiều kiến thức giá trị về AI, Prompt Engineering, Multi-Agent, Cloud, bảo mật và tư duy nghề nghiệp. Sự kiện giúp tôi hiểu rằng trong thời đại AI, người kỹ sư không chỉ cần biết sử dụng công cụ mới mà còn phải có tư duy hệ thống, trách nhiệm với sản phẩm và khả năng tạo ra giá trị thực tế cho doanh nghiệp.