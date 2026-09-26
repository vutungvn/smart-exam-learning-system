# CHƯƠNG 1. TỔNG QUAN KHẢO SÁT

## 1.1. Bối cảnh

Chứng chỉ tiếng Anh là điều kiện xét tốt nghiệp bắt buộc tại phần lớn các trường đại học Việt Nam. Theo thống kê, khoảng 127 cơ sở đào tạo đang sử dụng chứng chỉ TOEIC, IELTS hoặc Khung năng lực ngoại ngữ 6 bậc làm tiêu chí xét tốt nghiệp; mức chuẩn đầu ra TOEIC phổ biến dao động trong khoảng 450–550 điểm, một số chương trình kỹ thuật và chất lượng cao yêu cầu từ 600 điểm trở lên. Sinh viên không đạt chuẩn ngoại ngữ sẽ bị "treo bằng" dù đã hoàn thành đầy đủ các học phần chuyên môn và đồ án tốt nghiệp.

Thực tế cho thấy nhiều sinh viên kỹ thuật thường tập trung vào các môn chuyên ngành trong những năm đầu và chỉ bắt đầu ôn luyện ngoại ngữ khi chuẩn bị xét tốt nghiệp. Khi đó, quỹ thời gian còn lại rất hạn chế trong khi khối lượng công việc năm cuối (đồ án, thực tập, tìm việc) lại lớn nhất. Đây là nhóm người dùng có nhu cầu cao đối với một công cụ luyện thi hiệu quả, biết chỉ ra chính xác điểm yếu và tiết kiệm thời gian ôn tập.

Song song với đó, việc ôn tập các môn chuyên ngành Công nghệ thông tin (Cơ sở dữ liệu, Kiến trúc máy tính, Mạng máy tính, Lập trình hướng đối tượng…) hiện chủ yếu dựa vào tài liệu giấy, slide bài giảng và các bộ đề trắc nghiệm được chia sẻ tự phát, thiếu một nền tảng có ngân hàng câu hỏi được tổ chức và có khả năng đánh giá năng lực người học.

Trong giai đoạn 2023–2026, các mô hình ngôn ngữ lớn đã đạt tới mức độ trưởng thành cho phép tích hợp vào ứng dụng giáo dục với chi phí hợp lý, mở ra khả năng tự động hoá việc phân tích năng lực, sinh nội dung học tập và hỗ trợ giải đáp theo ngữ cảnh. Đây chính là cơ sở thực tiễn và công nghệ cho đề tài.

## 1.2. Mục đích khảo sát

- Xác định nhu cầu thực tế và những khó khăn của sinh viên trong quá trình luyện thi TOEIC và ôn tập các môn chuyên ngành CNTT.
- Phân tích các hệ thống luyện thi và học tập hiện hành để nhận diện ưu điểm cần kế thừa và khoảng trống có thể khai thác.
- Đánh giá và lựa chọn công nghệ triển khai phù hợp với năng lực của một sinh viên trong 24 tuần.
- Rà soát các ràng buộc pháp lý về bảo vệ dữ liệu cá nhân và bản quyền học liệu, làm cơ sở cho thiết kế hệ thống ở giai đoạn sau.
- Làm đầu vào cho Báo cáo phân tích hệ thống (đặc tả yêu cầu, Use Case, Activity Diagram, Sequence Diagram) ở Tuần 2–3.

## 1.3. Phạm vi khảo sát

| **Khía cạnh** | **Phạm vi xác định** |
|---|---|
| Nội dung học tập | Luyện thi TOEIC Listening & Reading (không bao gồm Speaking & Writing); các môn lý thuyết chuyên ngành CNTT theo hình thức trắc nghiệm |
| Đối tượng người dùng | Sinh viên đại học khối kỹ thuật/CNTT; giảng viên và biên tập viên nội dung; quản trị viên hệ thống |
| Nền tảng | Ứng dụng Web responsive chạy trên trình duyệt máy tính và di động (không phát triển ứng dụng di động gốc) |
| Hệ thống đối sánh | Nhóm luyện thi TOEIC (STUDY4, Santa AI TOEIC); nhóm học tập tổng quát có tích hợp AI (Quizlet, Khanmigo); nhóm LMS nội bộ (Moodle) |
| Công nghệ | Frontend, Backend, CSDL, ORM, API mô hình AI, giao tiếp thời gian thực, hàng đợi và bộ nhớ đệm |
| Pháp lý | Pháp luật Việt Nam về bảo vệ dữ liệu cá nhân; bản quyền học liệu và đề thi; điều khoản sử dụng API AI bên thứ ba |

*Bảng 1.1. Phạm vi khảo sát*

## 1.4. Phương pháp khảo sát

Báo cáo sử dụng kết hợp bốn phương pháp nhằm bảo đảm dữ liệu vừa có chiều rộng vừa có chiều sâu:

| **TT** | **Phương pháp** | **Cách thực hiện** | **Kết quả thu được** |
|---|---|---|---|
| 1 | Khảo sát bằng bảng hỏi | Phát phiếu trực tuyến qua Google Form tới sinh viên các khóa 65–68 khoa CNTT và một số khoa kỹ thuật khác | Dữ liệu định lượng về thói quen học tập, khó khăn và mức độ quan tâm tới từng tính năng |
| 2 | Phỏng vấn sâu | Phỏng vấn bán cấu trúc 3–5 sinh viên đang ôn thi TOEIC và 1–2 giảng viên | Dữ liệu định tính, làm rõ nguyên nhân phía sau các con số |
| 3 | Phân tích hệ thống hiện hành | Trực tiếp sử dụng, chụp màn hình và chấm điểm 5 hệ thống theo bộ tiêu chí thống nhất | Bảng so sánh tính năng, danh sách ưu – nhược điểm |
| 4 | Nghiên cứu tài liệu | Đọc tài liệu kỹ thuật chính thức, văn bản quy phạm pháp luật và điều khoản dịch vụ | Căn cứ lựa chọn công nghệ và danh mục ràng buộc pháp lý |

*Bảng 1.2. Các phương pháp khảo sát được sử dụng*

# CHƯƠNG 2. KHẢO SÁT NHU CẦU NGƯỜI DÙNG

## 2.1. Thiết kế phiếu khảo sát

Phiếu khảo sát gồm 6 nhóm câu hỏi, được thiết kế theo nguyên tắc đi từ thông tin nền tới hành vi, rồi mới tới đánh giá tính năng, nhằm hạn chế việc người trả lời bị định hướng bởi mô tả sản phẩm. Toàn văn phiếu khảo sát được trình bày tại Phụ lục A.

| **Nhóm** | **Chủ đề** | **Nội dung chính** | **Dạng câu hỏi** |
|---|---|---|---|
| I | Thông tin nền | Khóa, ngành, đã thi TOEIC chưa, điểm hiện tại và điểm mục tiêu | Chọn một / điền số |
| II | Thói quen luyện thi | Tần suất học, nền tảng đang dùng, thiết bị chủ yếu, thời lượng mỗi phiên học | Chọn một / chọn nhiều |
| III | Khó khăn gặp phải | Không xác định được điểm yếu, thiếu lộ trình, thiếu đề môn chuyên ngành, không hiểu vì sao sai | Chọn nhiều / xếp hạng |
| IV | Mức độ quan tâm tính năng | 10 tính năng dự kiến của hệ thống | Thang Likert 1–5 |
| V | Thái độ với AI | Mức độ tin tưởng và lo ngại khi AI xử lý dữ liệu học tập cá nhân | Thang Likert 1–5 / câu mở |
| VI | Mức độ sẵn sàng sử dụng | Ý định sử dụng, mức phí chấp nhận được (nếu có) | Chọn một / câu mở |

*Bảng 2.1. Cấu trúc phiếu khảo sát*

## 2.2. Quy mô mẫu và cách thu thập

| **Tiêu chí** | **Thiết kế** |
|---|---|
| Phương thức phát phiếu | Google Form, phát qua nhóm lớp, nhóm khoa và diễn đàn sinh viên |
| Quy mô mẫu mục tiêu | Tối thiểu 80 phản hồi hợp lệ (mục tiêu 100) |
| Phương pháp chọn mẫu | Chọn mẫu thuận tiện, có phân tầng theo khóa học (năm 2 – năm 4) |
| Thời gian thu thập | Tuần 1 của kỳ đồ án (07 ngày) |
| Phỏng vấn sâu | 03–05 sinh viên (mỗi cuộc 20–30 phút) và 01–02 giảng viên |
| Tiêu chí loại phiếu | Phiếu điền dưới 60 giây, trả lời cùng một đáp án cho toàn bộ thang Likert, hoặc thiếu nhóm câu hỏi bắt buộc |

*Bảng 2.2. Thiết kế mẫu khảo sát*

## 2.3. Kết quả khảo sát

**Lưu ý:** *Các bảng trong mục 2.3 và 2.4 là khung tổng hợp kết quả. Số liệu cụ thể được điền sau khi hoàn tất thu thập phiếu ở Tuần 1; dữ liệu thô đính kèm tại Phụ lục C. Không sử dụng số liệu giả định trong báo cáo chính thức.*

### *2.3.1. Đặc điểm mẫu khảo sát*

| **Tiêu chí phân loại** | **Nhóm** | **Số lượng** | **Tỉ lệ (%)** |
|---|---|---|---|
| Năm học | Năm 2 | … | … |
|  | Năm 3 | … | … |
|  | Năm 4 / năm cuối | … | … |
| Tình trạng chứng chỉ | Đã có chứng chỉ đạt chuẩn | … | … |
|  | Đã thi nhưng chưa đạt | … | … |
|  | Chưa từng thi | … | … |
| Thiết bị học chủ yếu | Máy tính | … | … |
|  | Điện thoại | … | … |

*Bảng 2.3. Đặc điểm mẫu khảo sát*

### *2.3.2. Mức độ quan tâm tới các tính năng dự kiến*

Thang đo Likert 5 mức (1 – Hoàn toàn không cần thiết, 5 – Rất cần thiết). Cột "Điểm TB" dùng để xếp thứ tự ưu tiên phát triển ở giai đoạn thiết kế.

| **TT** | **Tính năng** | **Điểm TB** | **Tỉ lệ 4–5 (%)** | **Xếp hạng** |
|---|---|---|---|---|
| 1 | Thi thử có đếm ngược thời gian, mô phỏng sát đề thật | … | … | … |
| 2 | Chấm điểm tự động và quy đổi thang điểm TOEIC | … | … | … |
| 3 | Xem lại chi tiết từng câu kèm giải thích đáp án | … | … | … |
| 4 | Biểu đồ phân tích điểm mạnh – điểm yếu theo chủ đề | … | … | … |
| 5 | Gợi ý bài tập theo đúng lỗ hổng kiến thức | … | … | … |
| 6 | Lộ trình học tập cá nhân hóa | … | … | … |
| 7 | Chatbot AI giải đáp thắc mắc theo ngữ cảnh bài đang làm | … | … | … |
| 8 | Ngân hàng câu hỏi các môn chuyên ngành CNTT | … | … | … |
| 9 | Tự động sinh câu hỏi từ tài liệu PDF của môn học | … | … | … |
| 10 | Flashcard ôn tập từ vựng / thuật ngữ | … | … | … |

*Bảng 2.4. Mức độ quan tâm tới các tính năng dự kiến*

## 2.4. Các khó khăn chính của người học

Tổng hợp từ nhóm câu hỏi III của phiếu khảo sát và nội dung phỏng vấn sâu, các khó khăn được phân nhóm và ánh xạ trực tiếp sang hướng giải quyết của hệ thống:

| **TT** | **Khó khăn (Pain point)** | **Tỉ lệ lựa chọn (%)** | **Hướng giải quyết của hệ thống** |
|---|---|---|---|
| P1 | Làm nhiều đề nhưng không biết mình thực sự yếu ở phần nào | … | Thống kê tỉ lệ đúng theo từng Part/chủ đề kết hợp phân tích bằng AI |
| P2 | Không biết nên ôn gì tiếp theo, học dàn trải không trọng tâm | … | Gợi ý bài tập theo lỗ hổng kiến thức; lộ trình cá nhân hóa |
| P3 | Biết đáp án đúng nhưng không hiểu vì sao mình sai | … | Giải thích chi tiết đáp án; chatbot trợ giảng theo ngữ cảnh câu hỏi |
| P4 | Thiếu ngân hàng câu hỏi trắc nghiệm cho môn chuyên ngành CNTT | … | Ngân hàng câu hỏi riêng cho môn chuyên ngành; sinh câu hỏi từ tài liệu |
| P5 | Mất bài làm khi rớt mạng hoặc đóng nhầm trình duyệt | … | Tự động lưu trạng thái bài làm; khôi phục phiên thi dang dở |
| P6 | Chi phí các nền tảng trả phí cao so với sinh viên | … | Định vị là công cụ học tập miễn phí trong phạm vi nhà trường |
| P7 | Không sắp xếp được thời gian học tập đều đặn | … | Theo dõi tiến độ, thống kê chuỗi ngày học, nhắc nhở ôn tập |

*Bảng 2.5. Khó khăn của người học và hướng giải quyết*

# CHƯƠNG 3. KHẢO SÁT HỆ THỐNG HIỆN HÀNH

## 3.1. Tiêu chí đánh giá

Năm hệ thống được khảo sát theo một bộ tiêu chí thống nhất gồm 5 nhóm, mỗi tiêu chí được chấm theo ba mức: Có đầy đủ (●), Có một phần (◎), Không có (○).

| **Nhóm tiêu chí** | **Các tiêu chí thành phần** |
|---|---|
| Nội dung | Có nội dung TOEIC LR; có nội dung môn chuyên ngành CNTT; quy mô ngân hàng câu hỏi; có giải thích đáp án |
| Thi thử | Mô phỏng sát đề thật; đếm ngược thời gian; tự động lưu bài làm; khôi phục khi mất kết nối; quy đổi thang điểm |
| Năng lực AI | Phân tích điểm mạnh – điểm yếu; gợi ý bài tập theo lỗ hổng; lộ trình cá nhân hóa; chatbot trợ giảng; sinh câu hỏi từ tài liệu; sinh flashcard |
| Quản trị | Quản lý ngân hàng câu hỏi; ma trận đề; thống kê báo cáo; phân quyền người dùng |
| Trải nghiệm & chi phí | Giao diện responsive; hỗ trợ tiếng Việt; mô hình miễn phí – trả phí; nền tảng hỗ trợ |

*Bảng 3.1. Bộ tiêu chí đánh giá hệ thống hiện hành*

## 3.2. STUDY4 (Việt Nam)

STUDY4 là nền tảng luyện thi TOEIC và IELTS trực tuyến phổ biến tại Việt Nam, đại diện cho nhóm sản phẩm nội địa có nội dung được bản địa hóa tốt.

**Đặc điểm nổi bật:**

- Giao diện làm bài mô phỏng sát định dạng thi thật, từ bố cục câu hỏi, đồng hồ đếm giờ đến cách hiển thị bài thi, giúp người học rèn kỹ năng quản lý thời gian.
- Kho đề phong phú theo các bộ đề uy tín như ETS và New Economy, mỗi bộ đề kèm đáp án được giải thích chi tiết bằng tiếng Việt.
- Bộ công cụ hỗ trợ trong lúc làm bài: highlight thông tin quan trọng, ghi chú, tạo flashcard và tra từ điển ngay trên nền tảng.
- Khóa "Complete TOEIC" cung cấp hơn 20.000 câu hỏi trắc nghiệm theo định dạng TOEIC, tất cả bài tập đều có giải thích, dịch nghĩa và transcript cho phần nghe.
- Áp dụng spaced-repetition cho việc ôn flashcard và sử dụng AI để chấm chữa phát âm cũng như bài thi Nói/Viết.
- Cung cấp thống kê kết quả theo từng tuần và từng Part, hỗ trợ người học tự nhận biết điểm mạnh và điểm yếu.

**Hạn chế so với mục tiêu đề tài:**

- Thống kê chủ yếu ở mức mô tả (tỉ lệ đúng theo Part), chưa diễn giải nguyên nhân và chưa tự động đề xuất bài tập bù đắp lỗ hổng.
- Không có nội dung dành cho các môn chuyên ngành CNTT.
- Không hỗ trợ người dùng tự tải tài liệu lên để sinh câu hỏi ôn tập.
- Các khóa học chuyên sâu và nhiều tính năng nâng cao thuộc gói trả phí.

## 3.3. Santa – AI TOEIC (Riiid / Socra AI)

Santa là hệ thống luyện thi TOEIC ứng dụng AI tiêu biểu nhất trên thị trường quốc tế, đồng thời là đối thủ tham chiếu quan trọng nhất cho phần AI của đề tài.

**Đặc điểm nổi bật:**

- Bài kiểm tra đầu vào rút gọn cho phép dự đoán điểm TOEIC chỉ trong khoảng ba phút, thay cho việc bắt người học làm trọn một đề full-test để xác định trình độ.
- Ở các phiên bản trước, hệ thống có thể ước lượng điểm số sau khi người dùng hoàn thành 6–10 câu hỏi và phân tích năng lực theo từng phần như nghe, từ vựng, ngữ pháp.
- Thuật toán được huấn luyện trên khối dữ liệu học tập rất lớn, có nhiệm vụ loại bỏ những câu hỏi không cần thiết và chỉ tập trung vào các câu có khả năng cải thiện điểm số.
- Bài giảng được đưa ra ngay tại thời điểm hệ thống phát hiện điểm yếu của người học.
- Kho nội dung lớn với hơn 8.000 câu hỏi và khoảng 450 bài giảng.

**Hạn chế so với mục tiêu đề tài:**

- Chỉ phục vụ các bài thi chuẩn hóa quốc tế, không mở rộng sang nội dung học thuật của nhà trường.
- Giao diện và nội dung chủ yếu bằng tiếng Anh/Hàn, mức độ bản địa hóa cho người Việt hạn chế.
- Phần lớn tính năng cốt lõi nằm sau gói thuê bao, chi phí cao đối với sinh viên.
- Không có chức năng trợ giảng hội thoại theo ngữ cảnh câu hỏi và không cho phép sinh nội dung từ tài liệu của người dùng.
- Người dùng phản ánh thuật toán đôi khi lặp lại những câu đã trả lời đúng và khó chủ động chọn kỹ năng muốn luyện, cho thấy cá nhân hóa hoàn toàn tự động cần đi kèm khả năng can thiệp thủ công.

## 3.4. Quizlet

Quizlet là nền tảng flashcard và học tập tổng quát có quy mô lớn, được khảo sát với vai trò tham chiếu cho nhóm tính năng sinh nội dung từ tài liệu người dùng.

**Đặc điểm nổi bật:**

- Tính năng Magic Notes cho phép dán văn bản, tải tệp PDF hoặc nhập ghi chép lớp học và tự động chuyển thành flashcard, dàn ý, tóm tắt và đề luyện tập.
- Chế độ học thích ứng áp dụng lặp lại ngắt quãng, đưa thẻ ra đúng thời điểm người học sắp quên.
- Thư viện cộng đồng rất lớn với hàng trăm triệu bộ thẻ do người dùng và giáo viên tạo.
- Quy mô khoảng 60 triệu người học mỗi tháng, cho thấy mô hình sinh nội dung tự động có sức hút thực tế.

**Hạn chế so với mục tiêu đề tài:**

- Không có chức năng thi thử mô phỏng kỳ thi chuẩn hóa với đếm giờ và quy đổi thang điểm.
- Chất lượng nội dung trong thư viện cộng đồng không đồng đều; người học có thể ghi nhớ sai nếu dùng phải bộ thẻ thiếu chính xác.
- Trợ lý hội thoại Q-Chat được nhiều nguồn ghi nhận đã ngừng cung cấp từ giữa năm 2025, cần kiểm chứng lại tại thời điểm viết báo cáo chính thức.
- Các tính năng AI nâng cao, trong đó có sinh câu hỏi từ tài liệu, thuộc gói trả phí.

## 3.5. Khanmigo (Khan Academy)

Khanmigo được khảo sát không phải với tư cách đối thủ trực tiếp, mà là hình mẫu tham chiếu cho việc thiết kế trợ lý AI trong môi trường giáo dục.

**Đặc điểm nổi bật:**

- Triết lý thiết kế đáng chú ý nhất: trợ lý không đưa thẳng đáp án mà dẫn dắt người học đi qua quá trình giải, nhằm rèn tư duy phản biện thay vì tạo thói quen phụ thuộc.
- Tích hợp chặt với kho nội dung sẵn có của Khan Academy, trợ lý luôn trả lời trong ngữ cảnh bài học cụ thể.
- Có phân hệ riêng cho giáo viên: hỗ trợ soạn bài, sinh nội dung theo chuẩn và tóm tắt tiến độ học tập của học sinh.
- Có cơ chế an toàn cho người học nhỏ tuổi như lịch sử hội thoại và cảnh báo kiểm duyệt cho phụ huynh.

**Bài học rút ra cho đề tài:**

- System prompt của chatbot trợ giảng cần ràng buộc rõ vai trò: gợi mở hướng tư duy, chỉ ra lỗi sai, nhưng không giải hộ toàn bộ bài tập.
- Trợ lý chỉ thực sự hữu ích khi được cung cấp ngữ cảnh cụ thể của câu hỏi đang xem, thay vì trả lời chung chung như một chatbot tổng quát.
- Cần lưu lịch sử hội thoại để phục vụ tra cứu và đánh giá chất lượng câu trả lời.

## 3.6. Hệ thống LMS nội bộ (Moodle)

Nhóm hệ thống LMS trong trường đại diện cho hiện trạng phục vụ ôn tập môn chuyên ngành.

**Đặc điểm:**

- Có sẵn chức năng tạo đề trắc nghiệm, giới hạn thời gian, chấm điểm tự động và quản lý ngân hàng câu hỏi theo môn.
- Phân quyền rõ ràng giữa giảng viên và sinh viên, tích hợp với hệ thống tài khoản của nhà trường.
- Dữ liệu được lưu trữ nội bộ, thuận lợi về mặt quản lý và bảo mật.

**Hạn chế:**

- Hoàn toàn không có năng lực AI: không phân tích điểm yếu, không gợi ý ôn tập, không sinh nội dung.
- Giao diện làm bài cũ, trải nghiệm trên thiết bị di động kém.
- Việc sử dụng phụ thuộc vào giảng viên có tạo đề hay không; sinh viên không thể chủ động tự luyện tập ngoài lịch của môn học.
- Không hỗ trợ nội dung luyện thi chứng chỉ ngoại ngữ.

## 3.7. Bảng so sánh tổng hợp

Ký hiệu:  ● Có đầy đủ  ·  ◎ Có một phần  ·  ○ Không có

| **Tiêu chí** | **STUDY4** | **Santa AI** | **Quizlet** | **Khanmigo** | **Moodle** |
|---|---|---|---|---|---|
| Nội dung TOEIC LR | ● | ● | ◎ | ○ | ○ |
| Nội dung môn chuyên ngành CNTT | ○ | ○ | ◎ | ◎ | ● |
| Giải thích chi tiết đáp án | ● | ● | ◎ | ● | ◎ |
| Thi thử mô phỏng sát đề thật | ● | ◎ | ○ | ○ | ◎ |
| Đếm ngược thời gian làm bài | ● | ◎ | ○ | ○ | ● |
| Tự động lưu / khôi phục bài làm | ◎ | ◎ | ○ | ○ | ◎ |
| Quy đổi thang điểm chuẩn hóa | ● | ● | ○ | ○ | ○ |
| Phân tích điểm mạnh – điểm yếu | ◎ | ● | ◎ | ◎ | ○ |
| Gợi ý bài tập theo lỗ hổng | ○ | ● | ◎ | ◎ | ○ |
| Lộ trình học tập cá nhân hóa | ◎ | ● | ◎ | ◎ | ○ |
| Chatbot trợ giảng theo ngữ cảnh | ○ | ○ | ○ | ● | ○ |
| Sinh câu hỏi từ tài liệu người dùng | ○ | ○ | ● | ◎ | ○ |
| Sinh flashcard tự động | ◎ | ○ | ● | ○ | ○ |
| Quản lý ngân hàng câu hỏi | ● | ● | ● | ● | ● |
| Thống kê, báo cáo cho quản trị | ◎ | ◎ | ◎ | ● | ● |
| Giao diện responsive | ● | ● | ● | ● | ◎ |
| Hỗ trợ tiếng Việt | ● | ○ | ◎ | ○ | ● |
| Miễn phí ở mức đủ dùng | ◎ | ○ | ◎ | ◎ | ● |

*Bảng 3.2. So sánh tổng hợp các hệ thống hiện hành*

## 3.8. Nhận xét chung

Từ bảng so sánh có thể rút ra bốn nhận định:

- **Thứ nhất,** không có hệ thống nào phục vụ đồng thời hai nhóm nội dung mà đề tài hướng tới. Các nền tảng mạnh về TOEIC thì không có nội dung chuyên ngành CNTT, còn hệ thống nội bộ của nhà trường thì ngược lại.
- **Thứ hai,** năng lực AI đang phân mảnh. Santa mạnh về phân tích năng lực và cá nhân hóa nhưng không có trợ giảng hội thoại; Quizlet mạnh về sinh nội dung từ tài liệu nhưng không có thi thử; Khanmigo mạnh về trợ giảng nhưng không phục vụ thi chứng chỉ. Chưa có sản phẩm nào gộp cả ba năng lực này trong một hệ thống.
- **Thứ ba,** các tính năng AI giá trị nhất hầu như đều nằm sau gói trả phí, tạo rào cản chi phí đối với sinh viên — nhóm người dùng bắt buộc phải đạt chuẩn ngoại ngữ để tốt nghiệp.
- **Thứ tư,** kinh nghiệm từ phản hồi người dùng Santa cho thấy cá nhân hóa hoàn toàn tự động là chưa đủ. Hệ thống cần cho phép người học tự chọn chủ đề muốn luyện bên cạnh lộ trình do AI đề xuất. Đây là yêu cầu thiết kế cần ghi nhận ngay từ giai đoạn phân tích.

# CHƯƠNG 4. KHẢO SÁT CÔNG NGHỆ

Việc lựa chọn công nghệ dựa trên bốn tiêu chí: (1) phù hợp với yêu cầu chức năng; (2) sinh viên có thể làm chủ trong thời gian đồ án; (3) tài liệu và cộng đồng hỗ trợ tốt; (4) chi phí vận hành thấp trong môi trường thử nghiệm.

## 4.1. Lựa chọn công nghệ Frontend

| **Phương án** | **Ưu điểm** | **Nhược điểm** | **Đánh giá** |
|---|---|---|---|
| React + Vite | Hệ sinh thái lớn; Vite khởi động và build rất nhanh; phù hợp ứng dụng dạng dashboard nhiều tương tác; dễ tìm tài liệu tiếng Việt | Phải tự lựa chọn thư viện routing, state, form | Lựa chọn |
| Next.js | Hỗ trợ SSR/SSG tốt cho SEO; có sẵn API routes | Phức tạp hơn cần thiết vì hệ thống chủ yếu là ứng dụng sau đăng nhập, không cần SEO; tăng chi phí triển khai | Không chọn |
| Vue 3 | Cú pháp dễ tiếp cận, tài liệu tốt | Hệ sinh thái thư viện UI cho dashboard ít phong phú hơn React | Không chọn |

*Bảng 4.1. So sánh phương án Frontend*

**Kết luận:** Chọn React 18 + Vite, kết hợp TypeScript, Tailwind CSS và Shadcn UI cho tầng giao diện; TanStack Query cho trạng thái phía máy chủ và Zustand/Redux Toolkit cho trạng thái phía client. Lý do quyết định là hệ thống có nhiều màn hình tương tác cao (giao diện làm bài, biểu đồ năng lực, trang quản trị) — đây là thế mạnh của React, trong khi yêu cầu SEO gần như không có.

## 4.2. Lựa chọn công nghệ Backend

| **Phương án** | **Ưu điểm** | **Nhược điểm** | **Đánh giá** |
|---|---|---|---|
| NestJS (Node.js + TypeScript) | Kiến trúc phân tầng module hóa sẵn có (Module – Controller – Service – Repository); hỗ trợ tiêm phụ thuộc, Guard cho RBAC, WebSocket Gateway và tích hợp Swagger tự động; dùng chung TypeScript với Frontend | Đường cong học tập ban đầu cao hơn Express thuần | Lựa chọn |
| Express.js thuần | Nhẹ, tự do, dễ bắt đầu | Phải tự dựng cấu trúc phân tầng; dễ dẫn tới mã nguồn thiếu nhất quán khi dự án lớn dần — mâu thuẫn với yêu cầu Clean/Layered Architecture của đề tài | Không chọn |
| Spring Boot (Java) | Rất trưởng thành, phù hợp hệ thống lớn | Khác ngôn ngữ với Frontend, làm tăng chi phí chuyển ngữ cảnh cho một sinh viên làm toàn bộ hệ thống | Không chọn |

*Bảng 4.2. So sánh phương án Backend*

**Kết luận:** Chọn NestJS. Đề tài yêu cầu rõ kiến trúc phân tầng, nguyên lý SOLID, RBAC, tài liệu API bằng Swagger và WebSocket — NestJS đáp ứng trực tiếp toàn bộ các yêu cầu này bằng cơ chế có sẵn, giúp giảm đáng kể khối lượng mã hạ tầng phải tự viết.

## 4.3. Cơ sở dữ liệu và ORM

| **Phương án** | **Ưu điểm** | **Nhược điểm** | **Đánh giá** |
|---|---|---|---|
| PostgreSQL | Hỗ trợ kiểu JSONB rất phù hợp để lưu kết quả phân tích của AI và nội dung câu hỏi có cấu trúc thay đổi; hàm cửa sổ (window function) mạnh cho thống kê theo chủ đề; chỉ mục GIN cho tìm kiếm | Cấu hình ban đầu phức tạp hơn MySQL đôi chút | Lựa chọn |
| MySQL | Phổ biến, dễ triển khai trên hosting giá rẻ | Hỗ trợ JSON và hàm phân tích yếu hơn PostgreSQL | Dự phòng |
| Prisma ORM | Sinh kiểu TypeScript tự động từ schema, giúp phát hiện lỗi ngay khi biên dịch; hệ thống migration rõ ràng, dễ trình bày trong báo cáo | Khó tối ưu với một số truy vấn thống kê phức tạp | Lựa chọn (kết hợp raw query khi cần) |

*Bảng 4.3. So sánh cơ sở dữ liệu và ORM*

## 4.4. Lựa chọn API mô hình AI

Đây là quyết định có ảnh hưởng lớn nhất tới chi phí vận hành và rủi ro của đề tài. Kết quả khảo sát tại thời điểm tháng 8/2026:

| **Tiêu chí** | **Google Gemini API** | **Ghi chú khi triển khai** |
|---|---|---|
| Gói miễn phí | Có gói miễn phí không giới hạn thời gian cho dòng Flash và Flash-Lite, không yêu cầu thẻ tín dụng. Các mô hình Pro đã được đưa ra khỏi gói miễn phí từ khoảng tháng 4–5/2026 | Đủ cho giai đoạn phát triển và kiểm thử của đồ án |
| Giới hạn tốc độ (free tier) | Ba loại giới hạn cùng lúc: số lượt gọi mỗi phút (RPM), mỗi ngày (RPD) và số token mỗi phút (TPM). Dòng Flash ở mức khoảng vài chục lượt/phút và khoảng 1.500 lượt/ngày | Bắt buộc phải có hàng đợi và cơ chế thử lại; con số cụ thể cần tra cứu lại trên tài liệu chính thức trước khi triển khai |
| Xử lý lỗi vượt hạn mức | Trả về HTTP 429 kèm thông tin loại giới hạn bị chạm | Áp dụng exponential backoff kèm jitter, không thử lại ngay lập tức |
| Đầu ra có cấu trúc | Hỗ trợ ràng buộc đầu ra theo JSON schema | Bắt buộc dùng cho chức năng sinh câu hỏi và phân tích năng lực để kết quả có thể kiểm tra tự động |
| Rủi ro về dữ liệu | Dữ liệu ở gói miễn phí có thể được Google sử dụng để cải thiện mô hình | Bắt buộc ẩn danh hóa dữ liệu trước khi gửi — xem Chương 5 |
| Chi phí khi lên gói trả phí | Tính theo token đầu vào và đầu ra; có cơ chế context caching giúp giảm chi phí với phần prompt lặp lại | Áp dụng cache kết quả AI ở tầng ứng dụng để giảm số lượt gọi |

*Bảng 4.4. Khảo sát Google Gemini API*

**Kết luận:** Chọn Google Gemini API dòng Flash làm nhà cung cấp chính nhờ gói miễn phí ổn định, hỗ trợ đầu ra JSON có ràng buộc schema và chi phí thấp khi mở rộng. Tuy nhiên, hệ thống phải được thiết kế theo hướng trừu tượng hóa tầng gọi AI (thông qua LangChain hoặc một lớp adapter tự viết) để có thể thay thế nhà cung cấp mà không phải sửa đổi logic nghiệp vụ. Do các giới hạn và bảng giá thay đổi thường xuyên, mọi con số nêu trên cần được kiểm chứng lại trên tài liệu chính thức tại thời điểm triển khai.

## 4.5. Giao tiếp thời gian thực, hàng đợi và bộ nhớ đệm

| **Thành phần** | **Công nghệ lựa chọn** | **Lý do** |
|---|---|---|
| Giao tiếp thời gian thực | WebSocket qua Socket.IO | Chatbot AI cần trả lời theo từng đoạn (streaming) và cập nhật tiến độ sinh câu hỏi. So với SSE, WebSocket hỗ trợ hai chiều và tích hợp sẵn trong NestJS qua Gateway |
| Hàng đợi tác vụ nền | BullMQ trên nền Redis | Các tác vụ gọi AI kéo dài (phân tích năng lực, sinh câu hỏi từ PDF) phải chạy bất đồng bộ để không làm chậm phản hồi của API; đồng thời giúp kiểm soát tốc độ gọi API AI, tránh chạm rate limit |
| Bộ nhớ đệm | Redis | Lưu kết quả phân tích AI (TTL 24 giờ), đếm số lần đăng nhập sai và theo dõi hạn mức sử dụng AI theo từng người dùng |
| Trích xuất tài liệu | pdf-parse, mammoth | Lấy văn bản từ tệp PDF và DOCX. Lưu ý: không xử lý được PDF dạng ảnh scan nếu không tích hợp OCR — cần thông báo rõ cho người dùng |

*Bảng 4.5. Thành phần hạ tầng bổ trợ*

## 4.6. Kết luận lựa chọn công nghệ

| **Tầng** | **Công nghệ** |
|---|---|
| Frontend | React 18, Vite, TypeScript, Tailwind CSS, Shadcn UI, TanStack Query, Recharts (biểu đồ năng lực) |
| Backend | Node.js, NestJS, TypeScript, Passport (JWT + Google OAuth 2.0), Socket.IO Gateway, Swagger |
| Cơ sở dữ liệu | PostgreSQL 16, Prisma ORM (migrations), Redis (cache + hàng đợi) |
| Tích hợp AI | Google Gemini API (dòng Flash), LangChain, Zod để kiểm tra JSON đầu ra |
| Hàng đợi | BullMQ |
| Kiểm thử | Jest (unit), Supertest (integration), k6 hoặc Artillery (kiểm thử tải) |
| Công cụ hỗ trợ | Visual Studio Code, Postman, Figma, Visual Paradigm, Git/GitHub, Jira, Docker Compose (môi trường phát triển) |

*Bảng 4.6. Tổng hợp công nghệ lựa chọn*

# CHƯƠNG 5. KẾT LUẬN

## 5.1. Khoảng trống của các hệ thống hiện có

Kết quả khảo sát cho thấy bốn khoảng trống mà đề tài có thể khai thác:

| **TT** | **Khoảng trống** | **Cơ hội cho đề tài** |
|---|---|---|
| G1 | Không có nền tảng nào phục vụ đồng thời luyện thi chứng chỉ ngoại ngữ và ôn tập môn chuyên ngành CNTT | Xây dựng một hệ thống hai nhóm nội dung dùng chung hạ tầng thi thử, chấm điểm và phân tích năng lực |
| G2 | Năng lực AI đang phân mảnh giữa các sản phẩm: phân tích năng lực, trợ giảng hội thoại và sinh nội dung không cùng tồn tại trong một hệ thống | Tích hợp cả ba nhóm năng lực AI trên cùng một nền tảng, chia sẻ chung nguồn dữ liệu học tập |
| G3 | Các tính năng AI có giá trị nhất đều thuộc gói trả phí, tạo rào cản với sinh viên | Định vị hệ thống là công cụ học tập phục vụ sinh viên trong phạm vi nhà trường |
| G4 | Cá nhân hóa hoàn toàn tự động khiến người học mất quyền chủ động chọn nội dung muốn luyện | Thiết kế theo mô hình kết hợp: AI đề xuất nhưng người học vẫn tự chọn được chủ đề và dạng bài |

*Bảng 6.1. Khoảng trống và cơ hội*

## 5.2. Ánh xạ nhu cầu sang chức năng hệ thống

Bảng dưới đây liên kết các khó khăn đã xác định tại Chương 2 với chức năng dự kiến và mức độ ưu tiên đã thống nhất trong Nhiệm vụ đồ án. Đây là đầu vào trực tiếp cho việc xây dựng Use Case ở Báo cáo phân tích hệ thống.

| **Nguồn** | **Nhu cầu / khoảng trống** | **Chức năng hệ thống** | **Mức ưu tiên** |
|---|---|---|---|
| P1, G2 | Không biết mình yếu ở đâu | Thống kê theo Part/chủ đề; AI phân tích điểm mạnh – điểm yếu; biểu đồ radar năng lực | Bắt buộc |
| P2, G4 | Không biết ôn gì tiếp theo | Gợi ý bài tập theo lỗ hổng kiến thức, kết hợp cho phép người học tự chọn chủ đề | Bắt buộc |
| P3 | Không hiểu vì sao sai | Xem lại chi tiết từng câu kèm giải thích; chatbot trợ giảng theo ngữ cảnh câu hỏi | Bắt buộc / Nên có |
| P4, G1 | Thiếu đề môn chuyên ngành CNTT | Ngân hàng câu hỏi riêng theo môn; sinh câu hỏi từ tài liệu PDF | Bắt buộc / Nên có |
| P5 | Mất bài làm khi rớt mạng | Tự động lưu trạng thái bài làm; khôi phục phiên thi dang dở | Bắt buộc |
| P7 | Không duy trì được thói quen học | Theo dõi tiến độ, lịch sử thi, thống kê chuỗi ngày học | Bắt buộc |
| — | Nhu cầu ghi nhớ thuật ngữ, từ vựng | Flashcard sinh tự động từ tài liệu | Có thể bỏ |
| G3 | Quản lý chi phí AI | Kiểm soát hạn mức sử dụng API AI theo người dùng | Nên có |

*Bảng 6.2. Ánh xạ nhu cầu sang chức năng hệ thống*

## 5.3. Định hướng triển khai và công việc tiếp theo

Công việc tiếp theo trong Tuần 2:

- Xây dựng đặc tả yêu cầu chức năng (FR) và phi chức năng (NFR) trên cơ sở Bảng 6.2.
- Thiết kế sơ đồ Use Case tổng quát và các sơ đồ Use Case phân rã theo phân hệ.
- Thiết kế Activity Diagram cho các luồng nghiệp vụ chính: đăng ký/đăng nhập, làm bài thi thử, AI phân tích năng lực, sinh câu hỏi từ tài liệu.
- Thiết kế Sequence Diagram tương ứng cho các luồng trên.
- Khởi tạo kho mã nguồn trên GitHub và lập kế hoạch triển khai trên Jira.
