TRƯỜNG ĐẠI HỌC XÂY DỰNG HÀ NỘI

**KHOA  CÔNG NGHỆ THÔNG TIN**

***Mẫu ĐATN-01***

**CỘNG HOÀ XÃ HỘI CHỦ NGHĨA VIỆT NAM**

**Độc lập - Tự do - Hạnh phúc**

**NHIỆM VỤ ĐỒ ÁN TỐT NGHIỆP**

*(Ban hành kèm theo Quyết định số 483/QĐ-ĐHXDHN ngày 20 tháng 3 năm 2026 của Hiệu trưởng Trường Đại học Xây dựng Hà Nội)*

| Họ và tên sinh viên: | Vũ Văn Thanh Tùng | Mã SV: | 0284367 |
|---|---|---|---|
| Lớp: | 67PM2 | Khóa: | 67 |
| Ngành/Chuyên<br>ngành: | Công nghệ phần mềm | Hệ đào tạo: | Kỹ sư |
| Tên đề tài ĐATN: | Xây dựng hệ thống luyện thi và học tập thông minh |  |  |
| Khoa: | Công nghệ thông tin | Nhóm chuyên môn: |  |
| GV hướng dẫn: | Phạm Hữu Tùng | Email/ĐT: | tungph@huce.edu.vn |
| GV đồng hướng dẫn<br>(nếu có): |  | Email/ĐT: |  |
| Doanh nghiệp/đơn<br>vị phối hợp (nếu có): |  | Địa điểm thực hiện: |  |
| Thời gian thực hiện: | 3/8/2026 - 17/1/2027 | Đợt ĐATN: | 1 |

**1. Mục tiêu và yêu cầu của ĐATN**

*Ghi chú: các nội dung dưới đây được phân theo 3 mức độ ưu tiên để đảm bảo tính khả thi trong thời gian làm đồ án (khoảng 24 tuần, 1 sinh viên thực hiện): [Bắt buộc] – phải hoàn thiện; [Nên có] – cố gắng hoàn thiện nếu tiến độ cho phép; [Có thể bỏ] – có thể cắt giảm hoặc làm ở mức tối thiểu nếu thiếu thời gian, sau khi thống nhất với GVHD.*

***a, Mục tiêu***

- **[Bắt buộc]** Xây dựng nền tảng ứng dụng Web (Web Application) hỗ trợ luyện thi và học tập, tập trung vào 2 nhóm nội dung: luyện thi chứng chỉ TOEIC (Listening & Reading) và các môn chuyên ngành CNTT (Lập trình, Cơ sở dữ liệu, Kiến trúc máy tính…).
- **[Bắt buộc]** Phát triển hệ thống hoàn chỉnh gồm giao diện người dùng (Frontend) và hệ thống máy chủ (Backend), với chức năng thi thử trực tuyến: đếm ngược thời gian, tự động lưu trạng thái làm bài, chấm điểm tự động và xem lại lịch sử.
- **[Bắt buộc]** Ứng dụng AI để tự động phân tích điểm mạnh, điểm yếu của người học dựa trên lịch sử làm bài trắc nghiệm, từ đó gợi ý bài tập ôn luyện phù hợp.
- **[Bắt buộc]** Xây dựng trang quản trị (Admin Dashboard) ở mức cơ bản: quản lý ngân hàng câu hỏi, danh mục khóa học và tài khoản người dùng.
- **[Nên có]** Hỗ trợ tính năng tự động sinh ngân hàng câu hỏi trắc nghiệm và giải thích chi tiết đáp án từ tài liệu học tập PDF/văn bản đầu vào.
- **[Nên có]** Tích hợp Trợ lý AI (Chatbot) đóng vai trò trợ giảng, giải đáp thắc mắc theo ngữ cảnh bài thi/bài học đang làm.
- **[Nên có]** Mở rộng trang quản trị với báo cáo thống kê và quản lý hạn mức sử dụng API AI.
- **[Có thể bỏ]** Tự động tạo thẻ ghi nhớ (Flashcard) thông minh từ tài liệu học tập.
- **[Có thể bỏ]** Tự động sinh lộ trình học tập cá nhân hóa hoàn toàn tự động (thay vì gợi ý bài tập theo lỗ hổng kiến thức ở mức cơ bản).
- **[Có thể bỏ]** Quản lý hạn mức sử dụng API AI chi tiết theo từng người dùng/nhóm người dùng.
- Thiết kế hệ thống theo kiến trúc phân tầng, chuẩn mô-đun hóa, đảm bảo tính bảo trì, khả năng mở rộng và hiệu năng cao (yêu cầu xuyên suốt, áp dụng cho toàn bộ hệ thống).

***b, Yêu cầu***

- **[Bắt buộc]** Giao diện ứng dụng Web thân thiện, hiện đại, tương thích đa thiết bị (Responsive Design) trên trình duyệt máy tính và di động.
- **[Bắt buộc]** Hệ thống máy chủ xử lý logic nghiệp vụ, quản lý cơ sở dữ liệu và cung cấp RESTful API ổn định, có tài liệu API rõ ràng (Swagger/Postman).
- **[Bắt buộc]** Thiết kế cơ sở dữ liệu chuẩn hóa, tối ưu truy vấn cho việc lưu trữ tiến trình học tập, lịch sử thi và ngân hàng câu hỏi.
- **[Bắt buộc]** Xác thực bảo mật bằng JWT (Access Token, Refresh Token); tuân thủ quy định về bảo vệ dữ liệu cá nhân của người dùng (theo Nghị định 13/2023/NĐ-CP), đặc biệt khi gửi dữ liệu tới API mô hình AI của bên thứ ba.
- **[Bắt buộc]** Xây dựng chức năng thi thử trực tuyến với bộ đếm ngược thời gian, chấm điểm tự động và hiển thị đáp án chi tiết.
- **[Nên có]** Đăng nhập nhanh qua Google OAuth 2.0.
- **[Nên có]** Xây dựng WebSocket phục vụ giao tiếp thời gian thực giữa client và Backend cho tính năng Chatbot AI.
- **[Nên có]** Chức năng AI phân tích biểu đồ năng lực và gợi ý bài tập theo lỗ hổng kiến thức; hiển thị đáp án kèm giải thích từ AI.
- **[Nên có]** Trang quản trị (Admin Dashboard) quản lý dữ liệu hệ thống, phân quyền người dùng và thống kê hiệu suất học tập.
- **[Có thể bỏ]** Chatbot AI trả lời theo thời gian thực cho mọi dạng câu hỏi phức tạp với độ trễ thấp trong mọi điều kiện tải.
- **[Có thể bỏ]** Kiểm soát hạn mức sử dụng AI ở mức chi tiết theo từng gói/người dùng.
- Xử lý ngoại lệ lỗi tốt và bảo mật dữ liệu người dùng (yêu cầu xuyên suốt).

***c, Tiêu chí đánh giá / KPI dự kiến***

- Thời gian phản hồi trung bình của API (không tính thời gian gọi mô hình AI) dưới 500ms.
- Ngân hàng câu hỏi ban đầu tối thiểu 300–500 câu cho TOEIC và 300–500 câu cho các môn chuyên ngành CNTT để phục vụ kiểm thử.
- Hệ thống hoạt động ổn định với tối thiểu 50 người dùng đồng thời trong môi trường kiểm thử.
- Trên 70% người dùng thử nghiệm đánh giá gợi ý bài tập/lộ trình ôn luyện của AI là phù hợp (qua khảo sát nhanh).

**2. Phạm vi, nội dung công việc và sản phẩm cần nộp**

***a, Phạm vi***

- Phạm vi nền tảng: Đề tài tập trung phát triển nền tảng Web (Web Application) chạy trên trình duyệt máy tính và thiết bị di động (Responsive Web).
- Phạm vi nội dung luyện thi: Hệ thống tập trung hỗ trợ luyện thi và học tập cho 2 nhóm nội dung chính:
- Luyện thi chứng chỉ tiếng Anh: tập trung vào cấu trúc đề thi TOEIC 2 kỹ năng (Listening & Reading).
- Luyện thi các môn chuyên ngành CNTT: tập trung vào các môn lý thuyết & bài tập trắc nghiệm như Cơ sở dữ liệu, Kiến trúc máy tính, Mạng máy tính, Lập trình hướng đối tượng...
- Phân hệ người dùng: Hệ thống gồm phân hệ cho Học viên (luyện thi, làm bài, tương tác với AI trợ giảng, xem lộ trình cá nhân hóa) và phân hệ cho Quản trị viên/Giáo viên (quản lý đề thi, ngân hàng câu hỏi, thống kê người dùng).

***b, Nội dung công việc***

- Khảo sát nhu cầu thực tế và phân tích các hệ thống luyện thi hiện hành.
- Xác định yêu cầu chức năng và phi chức năng của hệ thống.
- Thiết kế các sơ đồ UML: Use Case Diagram, Activity Diagram, Sequence Diagram, Class Diagram và sơ đồ ERD.
- Thiết kế kiến trúc hệ thống và thiết kế chi tiết giao diện người dùng (UI/UX).
- Xây dựng cơ sở dữ liệu và các RESTful API / WebSocket.
- Phát triển các phân hệ xác thực, phân quyền (Role-Based Access Control) và quản lý tài khoản người dùng.
- Phát triển phân hệ thi thử trực tuyến: làm bài, đếm ngược thời gian, nộp bài, chấm điểm tự động và xem lại lịch sử.
- Tích hợp các dịch vụ AI theo mức ưu tiên đã xác định ở Mục 1: phân tích năng lực người học, gợi ý bài tập cá nhân hóa (Bắt buộc); sinh đề/flashcard từ tài liệu và Chatbot trợ giảng (Nên có/Có thể bỏ).
- Phát triển trang quản trị hệ thống (Admin Dashboard).
- Kiểm thử chức năng, kiểm thử hiệu năng tích hợp AI, tối ưu hóa truy vấn cơ sở dữ liệu và giao diện Web.
- Đóng gói sản phẩm, hoàn thiện báo cáo đồ án và chuẩn bị slide bảo vệ.

***c, Sản phẩm cần nộp***

- Báo cáo Đồ án tốt nghiệp.
- Mã nguồn ứng dụng Frontend và mã nguồn máy chủ Backend.
- Cơ sở dữ liệu của hệ thống (File script SQL / Migrations).
- Tài liệu Phân tích & Thiết kế hệ thống (UML, ERD, API Specs).
- Tài liệu Hướng dẫn cài đặt và sử dụng hệ thống.
- Slide báo cáo thuyết trình.
- Chương trình demo chạy hoàn chỉnh.

**3. Dữ liệu đầu vào, giả thiết, tiêu chuẩn/quy chuẩn, phần mềm/công cụ sử dụng**

***a, Dữ liệu đầu vào***

- Thông tin tài khoản người dùng, hồ sơ học tập.
- Ngân hàng câu hỏi, đáp án, ma trận đề thi TOEIC và các môn chuyên ngành CNTT.
- Tài liệu bài học dạng PDF/văn bản để AI trích xuất câu hỏi và Flashcard.
- Lịch sử làm bài, thời gian hoàn thành câu hỏi và kết quả thi của người học.
- Dữ liệu prompt/ngữ cảnh gửi tới mô hình AI để phân tích điểm yếu và sinh nội dung học tập.

***b, Giả thiết***

- Người dùng có kết nối Internet ổn định và sử dụng trình duyệt web hiện đại (Chrome, Edge, Safari, Firefox).
- Các API dịch vụ AI (như Google Gemini API) hoạt động ổn định với độ trễ chấp nhận được.
- Dữ liệu ngân hàng câu hỏi ban đầu được chuẩn hóa đúng cấu trúc định dạng.
- Hệ thống thư điện tử (SMTP) hoạt động bình thường để gửi mã xác thực và thông báo.

***c, Tiêu chuẩn***

- Thiết kế API theo chuẩn RESTful, sử dụng JSON làm định dạng trao đổi dữ liệu.
- Cấu trúc mã nguồn phân tầng rõ ràng (Clean Architecture / Layered Architecture).
- Tuân thủ các nguyên lý thiết kế hướng đối tượng (SOLID).
- Bảo mật hệ thống với JWT, OAuth 2.0 và phân quyền theo vai trò (RBAC).
- Tuân thủ quy định về bảo vệ dữ liệu cá nhân người dùng (Nghị định 13/2023/NĐ-CP) khi thu thập, lưu trữ và gửi dữ liệu tới các API AI bên thứ ba.
- Áp dụng Git flow trong quản lý phiên bản mã nguồn.

***d, Phần mềm và công cụ***

- Công cụ lập trình: Visual Studio Code, Postman.
- Backend & Database: Node.js, Express.js / NestJS, TypeScript, PostgreSQL / MySQL, Prisma ORM.
- Frontend: React.js, Vite, Tailwind CSS, Shadcn UI, Redux Toolkit / TanStack Query.
- Trí tuệ nhân tạo (AI): Google Gemini API / OpenAI API, LangChain.
- Thiết kế & Quản lý: Figma, Visual Paradigm, Git/GitHub, Jira / Trello.

***e, Rủi ro dự kiến và phương án dự phòng***

- Rủi ro: API AI (Gemini/OpenAI) gián đoạn, giới hạn tốc độ (rate limit) hoặc chi phí vượt dự kiến. Phương án: cache kết quả AI, xử lý hàng đợi (queue), có thông báo/fallback cho người dùng khi AI tạm thời không khả dụng.
- Rủi ro: thiếu dữ liệu ngân hàng câu hỏi ban đầu để kiểm thử đầy đủ. Phương án: tự biên soạn bộ dữ liệu mẫu tối thiểu cho 1–2 môn/kỹ năng để kiểm thử trước, mở rộng dần trong quá trình phát triển.
- Rủi ro: khối lượng công việc vượt quá thời gian và năng lực của 1 sinh viên trong 24 tuần. Phương án: áp dụng bảng phân loại ưu tiên Bắt buộc/Nên có/Có thể bỏ tại Mục 1 để điều chỉnh phạm vi triển khai thực tế, có trao đổi định kỳ với GVHD.
- Rủi ro: rò rỉ hoặc lộ dữ liệu cá nhân người dùng khi tích hợp với API AI bên thứ ba. Phương án: ẩn danh hóa/loại bỏ thông tin định danh trước khi gửi tới AI, mã hóa dữ liệu nhạy cảm khi lưu trữ.

**4. Kế hoạch/mốc tiến độ dự kiến**

| **STT** | **Kế hoạch*** | **Thời gian (Mốc)** |
|---|---|---|
| 1 | Khảo sát nhu cầu thực tế các hệ thống EdTech/luyện thi hiện hành; xác định chi tiết các yêu cầu chức năng và phi chức năng của hệ thống. | Tuần 1 |
| 2 | Nghiên cứu các công nghệ cốt lõi (React.js, Node.js/NestJS, PostgreSQL/MySQL, Gemini API); Xây dựng sơ đồ Use Case và Activity Diagram. | Tuần 2 |
| 3 | Xây dựng sơ đồ Sequence Diagram và Class Diagram cho các luồng nghiệp vụ chính của hệ thống. | Tuần 3 |
| 4 | Thiết kế sơ đồ quan hệ dữ liệu (ERD); Thiết kế cơ sở dữ liệu chi tiết và kiến trúc phân tầng tổng thể hệ thống. | Tuần 4 |
| 5 | Xây dựng tài liệu Thiết kế API Specs (sử dụng Swagger/Postman); Khởi tạo dự án mã nguồn Frontend & Backend. | Tuần 5 |
| 6 | Thiết kế giao diện người dùng chi tiết (Wireframes/UI/UX) trên Figma cho cả phân hệ Học viên và Admin. | Tuần 6 |
| 7 | Cấu hình cơ sở dữ liệu (Migrations/Prisma ORM); Xây dựng phân hệ Xác thực (JWT Access/Refresh Token, Google OAuth 2.0). | Tuần 7 |
| 8 | Phát triển phân hệ Phân quyền người dùng (RBAC) và các API/giao diện Quản lý thông tin tài khoản, hồ sơ cá nhân. | Tuần 8 |
| 9 | Phát triển mô-đun Quản lý danh mục khóa học, môn học chuyên ngành CNTT và cấu trúc đề thi TOEIC (Listening & Reading). | Tuần 9 |
| 10 | Xây dựng chức năng Quản lý ngân hàng câu hỏi trắc nghiệm, ma trận đề thi và nhập/xuất dữ liệu đề thi. | Tuần 10 |
| 11 | Phát triển giao diện và logic làm bài thi trực tuyến: đếm ngược thời gian, tự động lưu trạng thái làm bài, xử lý sự cố kết nối. | Tuần 11 |
| 12 | Xây dựng logic nộp bài, chấm điểm tự động, lưu lịch sử thi và phát triển giao diện Xem lại chi tiết kết quả làm bài. | Tuần 12 |
| 13 | Tích hợp Google Gemini API/LangChain vào Backend; Thiết kế Prompt & ngữ cảnh cho Trợ lý AI (Chatbot). | Tuần 13 |
| 14 | Phát triển giao diện Chatbot AI trợ giảng hỗ trợ giải đáp thắc mắc và hướng dẫn phương pháp giải theo thời gian thực (WebSocket/API). | Tuần 14 |
| 15 | Phát triển thuật toán/mô-đun AI phân tích lịch sử thi, đánh giá điểm mạnh/điểm yếu và hiển thị biểu đồ năng lực người học. | Tuần 15 |
| 16 | Xây dựng chức năng AI đề xuất bài tập ôn luyện theo lỗ hổng kiến thức và tự động tạo lộ trình học tập cá nhân hóa (nếu còn thời gian). | Tuần 16 |
| 17 | Phát triển chức năng AI trích xuất văn bản từ tài liệu PDF/văn bản đầu vào để tự động sinh ngân hàng câu hỏi/đề thi (Nên có). | Tuần 17 |
| 18 | Xây dựng tính năng AI tự động tạo thẻ ghi nhớ (Flashcard) từ tài liệu (Có thể bỏ nếu thiếu thời gian). | Tuần 18 |
| 19 | Phát triển trang Quản trị (Admin Dashboard): Quản lý người dùng, phân quyền, cấu hình hệ thống. | Tuần 19 |
| 20 | Phát triển các biểu đồ thống kê báo cáo hiệu suất học tập, lượt thi thử và quản lý toàn bộ dữ liệu hệ thống. | Tuần 20 |
| 21 | Tích hợp toàn bộ hệ thống (Frontend - Backend - AI Services); Thực hiện Kiểm thử đơn vị (Unit Test) và Kiểm thử tích hợp (Integration Test). | Tuần 21 |
| 22 | Rà soát bảo mật, tối ưu hóa các truy vấn CSDL, xử lý ngoại lệ lỗi và tối ưu hiệu năng/độ trễ tích hợp AI. | Tuần 22 |
| 23 | Viết và hoàn thiện Báo cáo Đồ án tốt nghiệp, Tài liệu Hướng dẫn cài đặt/sử dụng và Đóng gói mã nguồn/CSDL. | Tuần 23 |
| 24 | Thiết kế Slide thuyết trình, chuẩn bị chương trình Demo chạy hoàn chỉnh và Tổng duyệt/Chủ trì bảo vệ trước Hội đồng. | Tuần 24 |

** Khối lượng/sản phẩm dự kiến đạt được tại các mốc. Các mốc liên quan tới tính năng AI ở mức [Nên có]/[Có thể bỏ] (Tuần 16–18) có thể điều chỉnh linh hoạt theo tiến độ thực tế.*

**Sinh viên			    GV hướng dẫn				Chủ tịch Hội đồng**

(Ký, ghi rõ họ tên)		(Ký, ghi rõ họ tên)                                       (Ký, ghi rõ họ tên)
