# Chương 1: Đặc tả yêu cầu chức năng (FR) và phi chức năng (NFR)

**1. Yêu cầu chức năng (FR)**

| **Mã** | **Tên chức năng** | **Ưu tiên** |
|---|---|---|
| FR-01 | Đăng ký tài khoản | Bắt buộc |
| FR-02 | Đăng nhập | Bắt buộc |
| FR-03 | Quên / đặt lại mật khẩu | Bắt buộc |
| FR-04 | Quản lý hồ sơ cá nhân | Bắt buộc |
| FR-05 | Tra cứu khóa học, đề thi | Bắt buộc |
| FR-06 | Làm bài thi thử / luyện tập theo Part (đếm giờ, tự động lưu, nộp bài, chấm điểm tự động) | Bắt buộc |
| FR-07 | Xem lại kết quả & lịch sử học tập (chi tiết đáp án, giải thích, biểu đồ tiến độ) | Bắt buộc |
| FR-08 | Ôn tập Flashcard | Có thể bỏ |
| FR-09 | AI phân tích năng lực & gợi ý bài tập (kèm lộ trình cá nhân hóa; học viên vẫn tự chọn được chủ đề) | Bắt buộc |
| FR-10 | Chatbot AI trợ giảng theo ngữ cảnh bài đang làm | Nên có |
| FR-11 | Sinh câu hỏi / flashcard từ tài liệu (AI), có bước giáo viên duyệt trước khi xuất bản | Nên có / Có thể bỏ |
| FR-12 | Quản lý ngân hàng câu hỏi & đề thi | Bắt buộc |
| FR-13 | Quản lý khóa học / môn học | Bắt buộc |
| FR-14 | Quản lý người dùng & phân quyền (RBAC) | Bắt buộc |
| FR-15 | Thống kê, báo cáo & quản lý hạn mức API AI | Nên có |

**2. Yêu cầu phi chức năng (NFR)**

| **Mã** | **Yêu cầu** | **Chỉ số / Ngưỡng đo** |
|---|---|---|
| NFR-01 | Hiệu năng phản hồi API | Thời gian phản hồi trung bình (không tính thời gian gọi AI) < 500ms |
| NFR-02 | Khả năng chịu tải | Ổn định với ≥ 50 người dùng đồng thời trong môi trường kiểm thử |
| NFR-03 | Bảo mật xác thực | JWT (Access 15 phút / Refresh 7 ngày) + bcrypt + HTTPS toàn bộ kết nối + RBAC |
| NFR-04 | Bảo vệ dữ liệu cá nhân | Tuân thủ Luật BVDLCN số 91/2025/QH15 và Nghị định 356/2025/NĐ-CP; ẩn danh hóa trước khi gửi AI |
| NFR-05 | Độ tin cậy | Tự động lưu & khôi phục bài làm khi mất kết nối; có phương án dự phòng khi AI không khả dụng |
| NFR-06 | Kiến trúc & khả năng mở rộng | Phân tầng, module hóa, tuân thủ SOLID; tác vụ AI chạy bất đồng bộ qua hàng đợi |
| NFR-07 | Tương thích & khả dụng | Giao diện responsive, tương thích Chrome/Edge/Safari/Firefox bản mới |
| NFR-08 | Chất lượng dữ liệu & hiệu quả AI | 300–500 câu hỏi mỗi nhóm nội dung để kiểm thử; ≥ 70% người dùng đánh giá gợi ý AI phù hợp |
| NFR-09 | Giám sát & chi phí vận hành AI | Audit log lưu tối thiểu 6 tháng; cache kết quả AI (TTL 24 giờ) để kiểm soát chi phí gọi API |

# Chương 2: UML

## I. Sơ đồ UC

[Hình ảnh]

## 1. Student

### a, Đăng ký tài khoản

| **Name** | Đăng ký tài khoản |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép người dùng tạo tài khoản học viên để sử dụng các chức năng học tập của hệ thống như tra cứu khóa học và đề thi, làm bài thi thử, ôn tập flashcard, theo dõi tiến độ và nhận gợi ý học tập cá nhân hóa từ AI. |
| **Normal Flow** | 1. Người dùng truy cập vào trang Register của hệ thống.<br>2. Hệ thống hiển thị form đăng ký tài khoản.<br>3. Người dùng nhập các thông tin đăng ký:<br>Full Name<br>Email<br>Password<br>Confirm Password<br>4. Người dùng tích chọn đồng ý Điều khoản sử dụng và Chính sách xử lý dữ liệu cá nhân.<br>5. Người dùng nhấn nút Register.<br>6. Hệ thống kiểm tra tính hợp lệ của thông tin:<br>6.1. Kiểm tra các trường bắt buộc không được để trống.<br>6.2. Kiểm tra định dạng email hợp lệ.<br>6.3. Kiểm tra độ mạnh của mật khẩu (tối thiểu 8 ký tự, gồm chữ hoa, chữ thường và số).<br>6.4. Kiểm tra Password và Confirm Password có khớp hay không.<br>6.5. Kiểm tra người dùng đã chấp nhận điều khoản hay chưa.<br>7. Hệ thống kiểm tra email đã tồn tại trong hệ thống hay chưa.<br>8. Nếu thông tin hợp lệ, hệ thống mã hóa mật khẩu bằng thuật toán bcrypt.<br>9. Hệ thống tạo tài khoản mới với vai trò mặc định là Student và trạng thái “Chưa kích hoạt”.<br>10. Hệ thống lưu thông tin người dùng vào cơ sở dữ liệu.<br>11. Hệ thống sinh mã kích hoạt và gửi email xác thực đến địa chỉ email đã đăng ký.<br>12. Hệ thống hiển thị thông báo đăng ký thành công và hướng dẫn người dùng kiểm tra email để kích hoạt tài khoản.<br>13. Người dùng mở email và nhấn vào liên kết kích hoạt.<br>14. Hệ thống xác thực mã kích hoạt và chuyển trạng thái tài khoản sang “Đang hoạt động”.<br>15. Hệ thống chuyển người dùng đến trang Login. |
| **Alternative Flow** | 1. Thông tin không hợp lệ (bỏ trống, sai định dạng email, mật khẩu không đủ mạnh):<br>Hệ thống hiển thị thông báo lỗi tương ứng cho từng trường và giữ lại dữ liệu đã nhập (trừ trường mật khẩu).<br>Người dùng nhập lại thông tin và thực hiện lại từ bước 5.<br>2. Mật khẩu và Confirm Password không khớp:<br>Hệ thống thông báo mật khẩu xác nhận không trùng khớp.<br>Người dùng nhập lại Confirm Password.<br>3. Email đã tồn tại:<br>Hệ thống thông báo email đã được đăng ký trước đó.<br>Hệ thống gợi ý người dùng chuyển sang chức năng Đăng nhập hoặc Quên mật khẩu.<br>4. Người dùng chưa đồng ý điều khoản:<br>Hệ thống thông báo yêu cầu chấp nhận Điều khoản sử dụng và Chính sách xử lý dữ liệu cá nhân trước khi tiếp tục.<br>5. Gửi email kích hoạt thất bại:<br>Hệ thống ghi nhận log lỗi và hiển thị thông báo cho người dùng.<br>Hệ thống cho phép người dùng yêu cầu gửi lại email kích hoạt.<br>6. Mã kích hoạt hết hạn hoặc không hợp lệ:<br>Hệ thống thông báo liên kết kích hoạt không còn hiệu lực.<br>Hệ thống cho phép người dùng yêu cầu gửi lại email kích hoạt mới. |

### b, Đăng nhập

| **Name** | Đăng nhập |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên đăng nhập vào hệ thống bằng tài khoản đã đăng ký và kích hoạt, để truy cập các chức năng học tập, luyện thi và theo dõi tiến độ cá nhân. |
| **Normal Flow** | 1. Người dùng truy cập vào trang Login của hệ thống.<br>2. Hệ thống hiển thị form đăng nhập.<br>3. Người dùng nhập thông tin đăng nhập:<br>Email<br>Password<br>4. Người dùng nhấn nút Login.<br>5. Hệ thống kiểm tra tính hợp lệ của thông tin:<br>5.1. Kiểm tra các trường không được để trống.<br>5.2. Kiểm tra định dạng email hợp lệ.<br>6. Hệ thống truy vấn tài khoản tương ứng với email trong cơ sở dữ liệu.<br>7. Hệ thống so khớp mật khẩu người dùng nhập với chuỗi băm bcrypt đã lưu.<br>8. Hệ thống kiểm tra trạng thái tài khoản (đã kích hoạt và không bị khóa).<br>9. Hệ thống sinh cặp token theo chuẩn JWT: Access Token (hiệu lực 15 phút) và Refresh Token (hiệu lực 7 ngày).<br>10. Hệ thống xác định vai trò Student và tải danh sách quyền tương ứng theo cơ chế RBAC.<br>11. Hệ thống ghi nhận nhật ký đăng nhập (thời điểm, thiết bị, địa chỉ IP).<br>12. Hệ thống chuyển người dùng đến trang chủ dành cho học viên. |
| **Alternative Flow** | 1. Thông tin để trống hoặc sai định dạng:<br>Hệ thống hiển thị thông báo lỗi tương ứng cho từng trường.<br>Người dùng nhập lại thông tin.<br>2. Email không tồn tại hoặc mật khẩu không đúng:<br>Hệ thống hiển thị thông báo chung “Email hoặc mật khẩu không chính xác”, không chỉ rõ trường nào sai nhằm hạn chế nguy cơ dò tìm tài khoản.<br>Hệ thống tăng bộ đếm số lần đăng nhập sai của tài khoản.<br>3. Đăng nhập sai quá 5 lần liên tiếp:<br>Hệ thống tạm khóa chức năng đăng nhập của tài khoản trong 15 phút và hiển thị thông báo.<br>Hệ thống gợi ý người dùng sử dụng chức năng Quên mật khẩu.<br>4. Tài khoản chưa được kích hoạt:<br>Hệ thống thông báo tài khoản chưa kích hoạt và cho phép gửi lại email kích hoạt.<br>5. Tài khoản bị quản trị viên khóa:<br>Hệ thống thông báo tài khoản đã bị khóa và hướng dẫn liên hệ quản trị viên.<br>6. Người dùng chọn chức năng Quên mật khẩu:<br>Hệ thống chuyển sang luồng của Use Case Quên và đặt lại mật khẩu. |

### c, Quản lý tài khoản

[Hình ảnh]

- Đặc tả về xem và cập nhật hồ sơ cá nhân

| **Name** | Xem và cập nhật hồ sơ cá nhân |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên xem và chỉnh sửa thông tin hồ sơ cá nhân như họ tên, ảnh đại diện, trình độ hiện tại và mục tiêu điểm số. Đây cũng là nguồn dữ liệu đầu vào phục vụ chức năng gợi ý học tập cá nhân hóa của hệ thống. |
| **Normal Flow** | 1. Học viên đã đăng nhập thành công vào hệ thống.<br>2. Học viên chọn menu “Hồ sơ cá nhân”.<br>3. Hệ thống xác thực Access Token và kiểm tra quyền truy cập theo vai trò.<br>4. Hệ thống truy vấn và hiển thị thông tin hồ sơ:<br>Full Name, Email, ảnh đại diện<br>Trình độ hiện tại, mục tiêu điểm số<br>Ngày tạo tài khoản, lần đăng nhập gần nhất<br>5. Học viên nhấn nút “Chỉnh sửa”.<br>6. Hệ thống chuyển form sang chế độ cho phép nhập liệu.<br>7. Học viên cập nhật thông tin và/hoặc tải lên ảnh đại diện mới.<br>8. Học viên nhấn nút “Lưu thay đổi”.<br>9. Hệ thống kiểm tra tính hợp lệ của dữ liệu:<br>9.1. Họ tên không được để trống và không vượt quá độ dài cho phép.<br>9.2. Ảnh đại diện đúng định dạng (JPG, PNG) và không vượt quá 2MB.<br>9.3. Mục tiêu điểm số nằm trong khoảng hợp lệ của kỳ thi.<br>10. Hệ thống lưu thông tin vào cơ sở dữ liệu và ghi nhật ký thay đổi.<br>11. Hệ thống hiển thị thông báo cập nhật thành công và hiển thị lại hồ sơ với thông tin mới. |
| **Alternative Flow** | 1. Dữ liệu nhập không hợp lệ:<br>Hệ thống hiển thị thông báo lỗi tương ứng cho từng trường và giữ nguyên giá trị cũ.<br>Học viên nhập lại thông tin và thực hiện lại từ bước 8.<br>2. Ảnh đại diện sai định dạng hoặc vượt quá dung lượng:<br>Hệ thống thông báo giới hạn cho phép và yêu cầu học viên chọn lại ảnh khác.<br>3. Học viên nhấn “Hủy”:<br>Hệ thống bỏ qua toàn bộ thay đổi và quay lại chế độ xem hồ sơ.<br>4. Học viên cố gắng chỉnh sửa trường Email:<br>Hệ thống không cho phép chỉnh sửa trực tiếp vì email là định danh đăng nhập, đồng thời hiển thị hướng dẫn liên hệ quản trị viên nếu cần thay đổi.<br>5. Access Token hết hạn trong quá trình thao tác:<br>Hệ thống sử dụng Refresh Token để cấp lại Access Token và tiếp tục thao tác.<br>Nếu Refresh Token cũng hết hạn, hệ thống chuyển người dùng về trang Login.<br>6. Lỗi kết nối cơ sở dữ liệu khi lưu:<br>Hệ thống thông báo lỗi, không thay đổi dữ liệu và cho phép học viên thử lại. |

- Đặc tả về đổi mật khẩu

| **Name** | Đổi mật khẩu |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên đang đăng nhập thay đổi mật khẩu tài khoản nhằm bảo đảm an toàn thông tin cá nhân. |
| **Normal Flow** | 1. Học viên đã đăng nhập và chọn chức năng “Đổi mật khẩu” trong trang Hồ sơ cá nhân.<br>2. Hệ thống hiển thị form đổi mật khẩu gồm các trường:<br>Mật khẩu hiện tại<br>Mật khẩu mới<br>Xác nhận mật khẩu mới<br>3. Học viên nhập đầy đủ thông tin và nhấn nút “Xác nhận”.<br>4. Hệ thống kiểm tra các trường không được để trống.<br>5. Hệ thống so khớp mật khẩu hiện tại với chuỗi băm bcrypt đã lưu trong cơ sở dữ liệu.<br>6. Hệ thống kiểm tra tính hợp lệ của mật khẩu mới:<br>6.1. Kiểm tra độ mạnh của mật khẩu mới.<br>6.2. Kiểm tra mật khẩu mới khác mật khẩu hiện tại.<br>6.3. Kiểm tra mật khẩu mới và xác nhận mật khẩu mới có khớp hay không.<br>7. Hệ thống băm mật khẩu mới bằng bcrypt và cập nhật vào cơ sở dữ liệu.<br>8. Hệ thống thu hồi toàn bộ Refresh Token đang hoạt động của tài khoản để buộc đăng xuất khỏi các thiết bị khác.<br>9. Hệ thống ghi nhật ký thay đổi mật khẩu và gửi email thông báo đến học viên.<br>10. Hệ thống hiển thị thông báo đổi mật khẩu thành công và yêu cầu đăng nhập lại. |
| **Alternative Flow** | 1. Mật khẩu hiện tại không đúng:<br>Hệ thống hiển thị thông báo lỗi và không thực hiện cập nhật.<br>Hệ thống ghi nhận số lần nhập sai để phát hiện truy cập bất thường.<br>2. Mật khẩu mới không đủ mạnh:<br>Hệ thống hiển thị tiêu chí mật khẩu và yêu cầu học viên nhập lại.<br>3. Mật khẩu mới trùng với mật khẩu hiện tại:<br>Hệ thống thông báo và yêu cầu chọn mật khẩu khác.<br>4. Xác nhận mật khẩu mới không khớp:<br>Hệ thống yêu cầu học viên nhập lại trường Xác nhận mật khẩu mới.<br>5. Gửi email thông báo thất bại:<br>Hệ thống ghi log lỗi gửi email, việc đổi mật khẩu vẫn có hiệu lực. |

- Đặc tả về quên và đặt lại mật khẩu

| **Name** | Quên và đặt lại mật khẩu |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên khôi phục quyền truy cập tài khoản khi quên mật khẩu, thông qua liên kết đặt lại mật khẩu được gửi tới địa chỉ email đã đăng ký. |
| **Normal Flow** | 1. Tại trang Login, học viên chọn chức năng “Quên mật khẩu”.<br>2. Hệ thống hiển thị form nhập địa chỉ email.<br>3. Học viên nhập email và nhấn nút “Gửi yêu cầu”.<br>4. Hệ thống kiểm tra định dạng email hợp lệ.<br>5. Hệ thống kiểm tra email có tồn tại trong cơ sở dữ liệu hay không.<br>6. Hệ thống sinh token đặt lại mật khẩu có thời hạn 15 phút và lưu vào cơ sở dữ liệu.<br>7. Hệ thống gửi email chứa liên kết đặt lại mật khẩu đến học viên.<br>8. Hệ thống hiển thị thông báo đã gửi hướng dẫn đặt lại mật khẩu.<br>9. Học viên mở email và nhấn vào liên kết đặt lại mật khẩu.<br>10. Hệ thống xác thực token và hiển thị form đặt mật khẩu mới.<br>11. Học viên nhập Mật khẩu mới, Xác nhận mật khẩu mới và nhấn “Xác nhận”.<br>12. Hệ thống kiểm tra độ mạnh của mật khẩu và tính khớp giữa hai trường.<br>13. Hệ thống băm mật khẩu bằng bcrypt, cập nhật cơ sở dữ liệu và vô hiệu hóa token vừa sử dụng.<br>14. Hệ thống thu hồi các phiên đăng nhập cũ và ghi nhật ký thao tác.<br>15. Hệ thống thông báo đặt lại mật khẩu thành công và chuyển đến trang Login. |
| **Alternative Flow** | 1. Email không đúng định dạng:<br>Hệ thống hiển thị thông báo lỗi và yêu cầu nhập lại.<br>2. Email chưa được đăng ký trong hệ thống:<br>Hệ thống vẫn hiển thị thông báo chung như bước 8 nhưng không gửi email, nhằm tránh làm lộ thông tin tài khoản nào đang tồn tại.<br>3. Token đặt lại mật khẩu hết hạn hoặc đã được sử dụng:<br>Hệ thống thông báo liên kết không còn hiệu lực.<br>Hệ thống cho phép học viên gửi lại yêu cầu đặt lại mật khẩu mới.<br>4. Mật khẩu mới không hợp lệ hoặc không khớp:<br>Hệ thống hiển thị thông báo lỗi tương ứng và yêu cầu nhập lại.<br>5. Học viên gửi yêu cầu đặt lại mật khẩu nhiều lần liên tiếp:<br>Hệ thống giới hạn tối đa 3 yêu cầu trong 15 phút và thông báo khi vượt ngưỡng. |

- Đặc tả về đăng xuất

| **Name** | Đăng xuất |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên kết thúc phiên làm việc hiện tại và thu hồi token truy cập, bảo đảm an toàn tài khoản khi sử dụng trên thiết bị dùng chung. |
| **Normal Flow** | 1. Học viên đang ở trạng thái đã đăng nhập.<br>2. Học viên chọn chức năng “Đăng xuất”.<br>3. Hệ thống hiển thị hộp thoại xác nhận đăng xuất.<br>4. Học viên xác nhận đăng xuất.<br>5. Hệ thống thu hồi Refresh Token của phiên hiện tại trong cơ sở dữ liệu.<br>6. Hệ thống xóa Access Token và dữ liệu phiên lưu tại phía trình duyệt.<br>7. Hệ thống ghi nhật ký đăng xuất.<br>8. Hệ thống chuyển người dùng về trang Login. |
| **Alternative Flow** | 1. Học viên hủy thao tác ở hộp thoại xác nhận:<br>Hệ thống giữ nguyên phiên làm việc hiện tại.<br>2. Học viên đang có bài thi/bài luyện tập dở dang:<br>Hệ thống cảnh báo bài làm sẽ được lưu tạm và có thể khôi phục ở lần đăng nhập sau.<br>Hệ thống chỉ thực hiện đăng xuất khi học viên xác nhận.<br>3. Phiên đăng nhập đã hết hạn từ trước:<br>Hệ thống xóa dữ liệu phiên tại trình duyệt và chuyển về trang Login. |

### d, Tra cứu khóa học và đề thi

[Hình ảnh]

- Đặc tả về tìm kiếm khóa học và đề thi

| **Name** | Tìm kiếm khóa học và đề thi |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên tìm kiếm, lọc và duyệt danh sách khóa học, đề thi đã được xuất bản trong hệ thống theo từ khóa, kỹ năng, trình độ và các tiêu chí khác để lựa chọn nội dung học tập phù hợp. |
| **Normal Flow** | 1. Học viên truy cập trang “Khóa học” hoặc “Đề thi”.<br>2. Hệ thống hiển thị danh sách nội dung đã xuất bản, có phân trang, kèm ô tìm kiếm và bộ lọc.<br>3. Học viên nhập từ khóa và/hoặc chọn các tiêu chí lọc:<br>Kỹ năng (Listening, Reading, Speaking, Writing)<br>Trình độ, Part, thời lượng bài thi<br>Mức độ phổ biến, khóa học đã tham gia<br>4. Học viên nhấn nút “Tìm kiếm”.<br>5. Hệ thống kiểm tra và chuẩn hóa tham số tìm kiếm.<br>6. Hệ thống truy vấn cơ sở dữ liệu theo điều kiện, chỉ lấy các nội dung ở trạng thái đã xuất bản.<br>7. Hệ thống trả về kết quả kèm tổng số bản ghi, sắp xếp theo tiêu chí mặc định.<br>8. Hệ thống hiển thị danh sách kết quả gồm: tên, mô tả ngắn, kỹ năng, trình độ, số câu hỏi, thời lượng và số lượt làm bài.<br>9. Học viên có thể thay đổi tiêu chí sắp xếp hoặc chuyển trang; hệ thống tải lại kết quả tương ứng.<br>10. Học viên chọn một khóa học hoặc đề thi để xem chi tiết. |
| **Alternative Flow** | 1. Không có kết quả phù hợp:<br>Hệ thống hiển thị thông báo không tìm thấy kết quả.<br>Hệ thống gợi ý nới lỏng điều kiện lọc hoặc hiển thị các khóa học, đề thi phổ biến.<br>2. Từ khóa chứa ký tự không hợp lệ hoặc vượt quá độ dài cho phép:<br>Hệ thống chuẩn hóa từ khóa và thông báo cho học viên.<br>3. Lỗi truy vấn hoặc hệ thống quá tải:<br>Hệ thống hiển thị thông báo lỗi và cho phép học viên thử lại.<br>4. Học viên chưa đăng nhập:<br>Hệ thống vẫn cho phép tra cứu và xem thông tin cơ bản.<br>Khi học viên chọn “Vào học” hoặc “Làm bài”, hệ thống yêu cầu đăng nhập trước khi tiếp tục. |

- Đặc tả về xem chi tiết và đăng ký tham gia khóa học

| **Name** | Xem chi tiết và đăng ký tham gia khóa học |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên xem thông tin chi tiết của một khóa học và đăng ký tham gia để truy cập các bài học, đề thi thuộc khóa học đó và được hệ thống ghi nhận tiến độ. |
| **Normal Flow** | 1. Từ danh sách kết quả tra cứu, học viên chọn một khóa học.<br>2. Hệ thống truy vấn và hiển thị thông tin chi tiết của khóa học:<br>Mô tả, mục tiêu đầu ra, đối tượng phù hợp<br>Cấu trúc chương, bài học và danh sách đề thi kèm theo<br>Giáo viên phụ trách, số học viên đang tham gia<br>3. Học viên nhấn nút “Đăng ký tham gia”.<br>4. Hệ thống kiểm tra trạng thái đăng nhập của học viên.<br>5. Hệ thống kiểm tra học viên đã tham gia khóa học này hay chưa.<br>6. Hệ thống kiểm tra điều kiện tham gia: khóa học còn mở và còn chỗ (nếu có giới hạn).<br>7. Hệ thống tạo bản ghi ghi danh gắn học viên với khóa học.<br>8. Hệ thống khởi tạo dữ liệu tiến độ học tập ban đầu cho học viên trong khóa học.<br>9. Hệ thống hiển thị thông báo đăng ký thành công và bổ sung khóa học vào mục “Khóa học của tôi”. |
| **Alternative Flow** | 1. Học viên chưa đăng nhập:<br>Hệ thống chuyển đến trang Login.<br>Sau khi đăng nhập thành công, hệ thống đưa học viên trở lại đúng trang khóa học đang xem.<br>2. Học viên đã tham gia khóa học:<br>Hệ thống ẩn nút đăng ký và hiển thị nút “Tiếp tục học” kèm tiến độ hiện tại.<br>3. Khóa học đã đóng hoặc bị gỡ xuất bản:<br>Hệ thống thông báo khóa học không còn khả dụng và gợi ý các khóa học tương tự.<br>4. Khóa học đã đủ số lượng học viên:<br>Hệ thống thông báo khóa học đã hết chỗ.<br>5. Lỗi khi ghi nhận đăng ký:<br>Hệ thống hoàn tác giao dịch, thông báo lỗi và cho phép học viên thử lại. |

e, Luyện tập và làm bài thi

[Hình ảnh]

- Đặc tả về làm bài thi thử / luyện tập theo part

| **Name** | Làm bài thi thử / luyện tập theo Part |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên thực hiện bài thi thử toàn phần hoặc luyện tập theo từng Part với cơ chế đếm giờ, tự động lưu bài làm, nộp bài và chấm điểm tự động, sau đó chuyển sang xem kết quả chi tiết. |
| **Normal Flow** | 1. Học viên chọn một đề thi từ danh sách đề thi hoặc từ khóa học đã tham gia.<br>2. Hệ thống hiển thị thông tin đề thi: số câu hỏi, thời lượng, cấu trúc các Part và số lần học viên đã làm.<br>3. Học viên chọn chế độ làm bài:<br>Thi thử toàn phần: làm toàn bộ đề thi, có đếm giờ<br>Luyện tập theo Part: chọn Part cần luyện, có thể bật hoặc tắt đếm giờ<br>4. Học viên nhấn nút “Bắt đầu làm bài”.<br>5. Hệ thống kiểm tra quyền truy cập đề thi và kiểm tra học viên có bài làm dở dang hay không.<br>6. Hệ thống khởi tạo phiên làm bài, sinh mã phiên và ghi nhận thời điểm bắt đầu.<br>7. Hệ thống tải nội dung câu hỏi cùng tài nguyên đính kèm (âm thanh, hình ảnh) và hiển thị câu hỏi đầu tiên.<br>8. Hệ thống hiển thị đồng hồ đếm ngược và bảng điều hướng câu hỏi.<br>9. Học viên chọn đáp án cho từng câu, có thể đánh dấu câu cần xem lại và chuyển câu tự do qua bảng điều hướng.<br>10. Hệ thống tự động lưu tạm bài làm sau mỗi thao tác chọn đáp án hoặc theo chu kỳ định sẵn.<br>11. Học viên nhấn nút “Nộp bài”.<br>12. Hệ thống hiển thị hộp thoại xác nhận kèm số câu chưa trả lời.<br>13. Học viên xác nhận nộp bài.<br>14. Hệ thống khóa phiên làm bài, dừng đồng hồ và ghi nhận thời điểm nộp.<br>15. Hệ thống chấm điểm tự động:<br>15.1. So khớp đáp án học viên chọn với đáp án đúng của từng câu.<br>15.2. Tính số câu đúng, sai, bỏ trống theo từng Part.<br>15.3. Tính tổng điểm và điểm quy đổi theo thang điểm của kỳ thi.<br>16. Hệ thống lưu kết quả, thời gian làm bài và toàn bộ câu trả lời vào cơ sở dữ liệu.<br>17. Hệ thống cập nhật dữ liệu tiến độ học tập của học viên.<br>18. Hệ thống chuyển đến trang kết quả bài làm. |
| **Alternative Flow** | 1. Hết thời gian làm bài:<br>Hệ thống tự động nộp bài với các đáp án đã được lưu và thực hiện tiếp từ bước 14.<br>2. Học viên có bài làm dở dang của đề thi này:<br>Hệ thống hiển thị lựa chọn “Tiếp tục bài làm trước” hoặc “Bắt đầu lại từ đầu”.<br>Nếu chọn tiếp tục, hệ thống chuyển sang luồng của Use Case Khôi phục bài làm bị gián đoạn.<br>3. Mất kết nối mạng trong lúc làm bài:<br>Hệ thống lưu đáp án tạm tại phía trình duyệt và hiển thị cảnh báo mất kết nối.<br>Hệ thống tự động đồng bộ lại dữ liệu khi kết nối được khôi phục.<br>4. Học viên thoát giữa chừng hoặc đóng trình duyệt:<br>Hệ thống giữ phiên làm bài ở trạng thái “Đang làm dở” để khôi phục ở lần truy cập sau.<br>5. Không tải được tài nguyên âm thanh hoặc hình ảnh:<br>Hệ thống thông báo và cho phép tải lại tài nguyên mà không làm mất các đáp án đã chọn.<br>6. Học viên chọn “Hủy” tại hộp thoại xác nhận nộp bài:<br>Hệ thống quay lại màn hình làm bài, đồng hồ tiếp tục chạy.<br>7. Xảy ra lỗi trong quá trình chấm điểm:<br>Hệ thống ghi log lỗi, giữ nguyên bài làm đã nộp và thông báo kết quả sẽ được trả sau khi xử lý xong. |

- Đặc tả về khôi phục bài làm bị gián đoạn

| **Name** | Khôi phục bài làm bị gián đoạn |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên tiếp tục bài thi hoặc bài luyện tập bị gián đoạn do mất kết nối, đóng trình duyệt hoặc thoát giữa chừng, bảo đảm không mất dữ liệu bài làm theo yêu cầu về độ tin cậy của hệ thống. |
| **Normal Flow** | 1. Học viên đăng nhập lại và chọn đề thi đang có bài làm dở, hoặc chọn thông báo nhắc bài làm dở dang tại trang chủ.<br>2. Hệ thống truy vấn phiên làm bài ở trạng thái “Đang làm dở” của học viên với đề thi tương ứng.<br>3. Hệ thống hiển thị thông tin phiên làm bài:<br>Thời điểm bắt đầu, số câu đã trả lời<br>Thời gian còn lại của bài thi<br>4. Hệ thống hiển thị hai lựa chọn: “Tiếp tục bài làm” hoặc “Bắt đầu lại từ đầu”.<br>5. Học viên chọn “Tiếp tục bài làm”.<br>6. Hệ thống tải lại toàn bộ đáp án đã lưu tạm và trạng thái đánh dấu câu hỏi.<br>7. Hệ thống khôi phục đồng hồ đếm ngược theo thời gian còn lại đã ghi nhận.<br>8. Hệ thống hiển thị màn hình làm bài tại vị trí câu hỏi gần nhất học viên đang thực hiện.<br>9. Học viên tiếp tục làm bài và nộp bài theo luồng của Use Case Làm bài thi thử / luyện tập theo Part. |
| **Alternative Flow** | 1. Học viên chọn “Bắt đầu lại từ đầu”:<br>Hệ thống chuyển phiên cũ sang trạng thái “Đã hủy” và khởi tạo phiên làm bài mới.<br>2. Phiên làm bài đã hết thời gian trong lúc gián đoạn:<br>Hệ thống tự động nộp bài với các đáp án đã lưu và chuyển đến trang kết quả.<br>3. Đề thi bị chỉnh sửa hoặc gỡ xuất bản sau khi học viên bắt đầu làm:<br>Hệ thống giữ nguyên phiên bản đề thi tại thời điểm bắt đầu để bảo đảm tính nhất quán của bài làm.<br>4. Dữ liệu lưu tạm bị lỗi hoặc không đầy đủ:<br>Hệ thống khôi phục tối đa số câu trả lời còn hợp lệ và thông báo rõ số câu khôi phục được.<br>5. Học viên có nhiều phiên làm bài dở dang ở các đề thi khác nhau:<br>Hệ thống liệt kê danh sách các phiên để học viên lựa chọn phiên cần tiếp tục. |

### f, Theo dõi kết quả và tiến độ học tập

[Hình ảnh]

- Đặc tả về xem kết quả và giải thích đáp án

| **Name** | Xem kết quả và giải thích đáp án |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên xem kết quả chi tiết của một lần làm bài, bao gồm điểm số theo từng Part, đáp án đúng và phần giải thích cho từng câu hỏi, nhằm hiểu rõ lỗi sai và cải thiện năng lực. |
| **Normal Flow** | 1. Sau khi nộp bài, hệ thống tự động chuyển đến trang kết quả; hoặc học viên chọn một lần làm bài từ lịch sử học tập.<br>2. Hệ thống xác thực quyền truy cập và truy vấn kết quả bài làm tương ứng.<br>3. Hệ thống hiển thị thông tin tổng quan:<br>Tổng điểm và điểm quy đổi<br>Số câu đúng, sai, bỏ trống; thời gian làm bài<br>So sánh với lần làm bài gần nhất của cùng đề thi<br>4. Hệ thống hiển thị thống kê theo từng Part và theo dạng câu hỏi để chỉ ra điểm mạnh, điểm yếu.<br>5. Học viên chọn “Xem chi tiết đáp án”.<br>6. Hệ thống hiển thị lần lượt từng câu hỏi kèm: đáp án học viên đã chọn, đáp án đúng, trạng thái đúng/sai và nội dung giải thích.<br>7. Học viên có thể lọc danh sách câu hỏi theo trạng thái: chỉ câu sai, câu bỏ trống hoặc câu đã đánh dấu.<br>8. Học viên có thể lưu từ vựng hoặc câu hỏi vào bộ flashcard cá nhân.<br>9. Học viên thoát khỏi trang kết quả; hệ thống ghi nhận trạng thái đã xem kết quả. |
| **Alternative Flow** | 1. Bài làm chưa được chấm xong:<br>Hệ thống hiển thị trạng thái “Đang chấm điểm” và tự động cập nhật khi có kết quả.<br>2. Câu hỏi chưa có nội dung giải thích:<br>Hệ thống hiển thị thông báo “Chưa có giải thích cho câu hỏi này” thay vì để trống.<br>3. Học viên truy cập kết quả của bài làm không thuộc về mình:<br>Hệ thống từ chối truy cập theo phân quyền RBAC và ghi nhật ký cảnh báo.<br>4. Lỗi truy vấn dữ liệu kết quả:<br>Hệ thống thông báo lỗi và cho phép học viên tải lại trang. |

- Đặc tả về xem lịch sử làm bài và biểu đồ tiến độ

| **Name** | Xem lịch sử làm bài và biểu đồ tiến độ |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên xem toàn bộ lịch sử làm bài và theo dõi sự tiến bộ theo thời gian thông qua biểu đồ thống kê điểm số theo từng kỹ năng, từng Part và từng khóa học. |
| **Normal Flow** | 1. Học viên chọn mục “Kết quả học tập” hoặc “Tiến độ của tôi”.<br>2. Hệ thống xác thực và truy vấn lịch sử làm bài của học viên.<br>3. Hệ thống hiển thị danh sách các lần làm bài gồm: tên đề thi, chế độ làm bài, ngày làm, điểm số và thời gian làm bài.<br>4. Hệ thống hiển thị biểu đồ tiến độ:<br>Biểu đồ đường thể hiện điểm số qua các lần làm bài<br>Biểu đồ so sánh kết quả theo từng kỹ năng và từng Part<br>Tỉ lệ hoàn thành của các khóa học đã tham gia<br>5. Học viên chọn khoảng thời gian hoặc lọc theo kỹ năng, khóa học.<br>6. Hệ thống truy vấn lại dữ liệu và vẽ lại biểu đồ theo điều kiện lọc.<br>7. Học viên chọn một lần làm bài cụ thể để xem kết quả chi tiết. |
| **Alternative Flow** | 1. Học viên chưa thực hiện bài làm nào:<br>Hệ thống thông báo chưa có dữ liệu học tập.<br>Hệ thống gợi ý một số đề thi phù hợp để học viên bắt đầu.<br>2. Không có dữ liệu trong khoảng thời gian đã lọc:<br>Hệ thống hiển thị biểu đồ trống kèm thông báo và giữ nguyên bộ lọc để học viên điều chỉnh.<br>3. Số lượng bản ghi lớn:<br>Hệ thống phân trang danh sách và tổng hợp biểu đồ theo mốc thời gian (tuần, tháng) để bảo đảm hiệu năng phản hồi.<br>4. Lỗi khi tổng hợp dữ liệu thống kê:<br>Hệ thống thông báo lỗi và cho phép học viên thử lại. |

g, Học tập với Flashcard

[Hình ảnh]

- Đặc tả về ôn tập Flashcard

| **Name** | Ôn tập Flashcard |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên ôn tập từ vựng và kiến thức bằng bộ thẻ flashcard, với cơ chế ghi nhận mức độ ghi nhớ và ưu tiên hiển thị lại các thẻ chưa thuộc. |
| **Normal Flow** | 1. Học viên chọn chức năng “Flashcard”.<br>2. Hệ thống hiển thị danh sách bộ thẻ gồm bộ thẻ do hệ thống, giáo viên cung cấp và bộ thẻ cá nhân của học viên.<br>3. Học viên chọn một bộ flashcard và nhấn “Bắt đầu ôn tập”.<br>4. Hệ thống tải danh sách thẻ và xác định thứ tự hiển thị, ưu tiên các thẻ chưa thuộc và thẻ đến hạn ôn lại.<br>5. Hệ thống hiển thị mặt trước của thẻ (từ hoặc câu hỏi).<br>6. Học viên tự trả lời rồi nhấn để lật thẻ.<br>7. Hệ thống hiển thị mặt sau của thẻ (nghĩa, ví dụ, ghi chú).<br>8. Học viên đánh giá mức độ ghi nhớ: “Đã thuộc” hoặc “Chưa thuộc”.<br>9. Hệ thống ghi nhận kết quả, cập nhật trạng thái và lịch ôn tập lại của thẻ.<br>10. Hệ thống chuyển sang thẻ tiếp theo và lặp lại từ bước 5 cho đến khi hết bộ thẻ.<br>11. Hệ thống hiển thị bảng tổng kết phiên ôn tập: số thẻ đã ôn, số thẻ đã thuộc, số thẻ cần ôn lại.<br>12. Hệ thống cập nhật tiến độ ôn tập của học viên. |
| **Alternative Flow** | 1. Bộ flashcard không có thẻ nào:<br>Hệ thống thông báo bộ thẻ rỗng và gợi ý học viên thêm thẻ hoặc chọn bộ thẻ khác.<br>2. Học viên thoát giữa phiên ôn tập:<br>Hệ thống lưu tiến độ phiên ôn tập để lần sau tiếp tục từ thẻ đang dở.<br>3. Học viên chọn chế độ “Chỉ ôn các thẻ chưa thuộc”:<br>Hệ thống lọc và chỉ hiển thị các thẻ chưa thuộc trong phiên ôn tập mới.<br>4. Mất kết nối trong lúc ôn tập:<br>Hệ thống cho phép tiếp tục ôn tập với dữ liệu đã tải và đồng bộ kết quả khi có kết nối trở lại. |

- Đặc tả về tạo và quản lý bộ flashcard cá nhân

| **Name** | Tạo và quản lý bộ flashcard cá nhân |
|---|---|
| **Actor** | Student (Học viên) |
| **Description** | Cho phép học viên tự tạo, chỉnh sửa và xóa các bộ flashcard cá nhân, thêm thẻ thủ công hoặc thêm nhanh từ các câu hỏi, từ vựng gặp trong quá trình làm bài. |
| **Normal Flow** | 1. Học viên chọn “Flashcard” rồi chọn mục “Bộ thẻ của tôi”.<br>2. Hệ thống hiển thị danh sách bộ thẻ cá nhân của học viên.<br>3. Học viên nhấn nút “Tạo bộ thẻ mới”.<br>4. Hệ thống hiển thị form nhập thông tin bộ thẻ: tên bộ thẻ, mô tả, chủ đề.<br>5. Học viên nhập thông tin và nhấn “Lưu”.<br>6. Hệ thống kiểm tra tên bộ thẻ không để trống và không trùng với bộ thẻ khác của chính học viên.<br>7. Hệ thống tạo bộ thẻ và chuyển đến màn hình quản lý thẻ.<br>8. Học viên thêm thẻ mới bằng một trong hai cách:<br>Nhập thủ công mặt trước, mặt sau, ví dụ và ghi chú<br>Thêm nhanh từ trang kết quả bài thi hoặc trang chi tiết câu hỏi<br>9. Hệ thống kiểm tra dữ liệu thẻ và lưu vào bộ thẻ.<br>10. Học viên có thể sửa, xóa từng thẻ hoặc đổi tên, xóa cả bộ thẻ.<br>11. Hệ thống cập nhật cơ sở dữ liệu và hiển thị thông báo kết quả sau mỗi thao tác. |
| **Alternative Flow** | 1. Tên bộ thẻ để trống hoặc trùng với bộ thẻ đã có:<br>Hệ thống hiển thị thông báo lỗi và yêu cầu học viên nhập lại.<br>2. Nội dung thẻ thiếu mặt trước hoặc mặt sau:<br>Hệ thống không cho phép lưu và thông báo các trường bắt buộc.<br>3. Học viên xóa bộ thẻ:<br>Hệ thống hiển thị hộp thoại xác nhận và cảnh báo dữ liệu ôn tập liên quan sẽ bị xóa.<br>Hệ thống chỉ thực hiện xóa khi học viên xác nhận.<br>4. Vượt quá số lượng thẻ tối đa của một bộ:<br>Hệ thống thông báo giới hạn và gợi ý tách thành bộ thẻ mới.<br>5. Thêm thẻ có nội dung trùng với thẻ đã có trong bộ:<br>Hệ thống cảnh báo trùng lặp và cho phép học viên chọn vẫn thêm hoặc hủy thao tác. |

**2. Teacher**

a, Đăng ký

| **Name** | Đăng ký tài khoản |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép người dùng đăng ký tài khoản với vai trò giáo viên để tham gia xây dựng và quản lý nội dung học tập trên hệ thống. Tài khoản giáo viên phải được quản trị viên phê duyệt trước khi được cấp quyền quản lý nội dung. |
| **Normal Flow** | 1. Người dùng truy cập trang Register và chọn loại tài khoản “Giáo viên”.<br>2. Hệ thống hiển thị form đăng ký dành cho giáo viên.<br>3. Người dùng nhập các thông tin đăng ký:<br>Full Name, Email<br>Password, Confirm Password<br>Đơn vị công tác, chuyên môn giảng dạy<br>Tệp minh chứng năng lực, chứng chỉ chuyên môn (nếu có)<br>4. Người dùng tích chọn đồng ý Điều khoản sử dụng và Chính sách xử lý dữ liệu cá nhân.<br>5. Người dùng nhấn nút Register.<br>6. Hệ thống kiểm tra tính hợp lệ của thông tin:<br>6.1. Các trường bắt buộc không được để trống.<br>6.2. Kiểm tra định dạng email hợp lệ.<br>6.3. Kiểm tra độ mạnh của mật khẩu.<br>6.4. Kiểm tra Password và Confirm Password có khớp hay không.<br>6.5. Kiểm tra tệp minh chứng đúng định dạng và dung lượng cho phép.<br>7. Hệ thống kiểm tra email đã tồn tại trong hệ thống hay chưa.<br>8. Hệ thống mã hóa mật khẩu bằng bcrypt và tạo tài khoản với vai trò Teacher, trạng thái “Chờ phê duyệt”.<br>9. Hệ thống lưu thông tin vào cơ sở dữ liệu và gửi email xác thực địa chỉ email.<br>10. Người dùng nhấn liên kết xác thực trong email; hệ thống ghi nhận email đã được xác thực.<br>11. Hệ thống gửi thông báo yêu cầu phê duyệt hồ sơ giáo viên đến quản trị viên.<br>12. Quản trị viên xem xét hồ sơ và phê duyệt tài khoản.<br>13. Hệ thống chuyển trạng thái tài khoản sang “Đang hoạt động”, cấp quyền theo vai trò Teacher và gửi email thông báo cho giáo viên.<br>14. Hệ thống chuyển người dùng đến trang Login. |
| **Alternative Flow** | 1. Thông tin không hợp lệ (bỏ trống, sai định dạng email, mật khẩu không đủ mạnh):<br>Hệ thống hiển thị thông báo lỗi cho từng trường và giữ lại dữ liệu đã nhập (trừ trường mật khẩu).<br>Người dùng nhập lại thông tin và thực hiện lại từ bước 5.<br>2. Email đã tồn tại trong hệ thống:<br>Hệ thống thông báo và gợi ý người dùng chuyển sang chức năng Đăng nhập hoặc Quên mật khẩu.<br>3. Mật khẩu và Confirm Password không khớp:<br>Hệ thống thông báo và yêu cầu nhập lại trường Confirm Password.<br>4. Người dùng chưa đồng ý điều khoản:<br>Hệ thống yêu cầu chấp nhận Điều khoản sử dụng và Chính sách xử lý dữ liệu cá nhân trước khi tiếp tục.<br>5. Quản trị viên từ chối hồ sơ giáo viên:<br>Hệ thống gửi email thông báo kèm lý do từ chối.<br>Tài khoản chuyển sang trạng thái “Bị từ chối” và cho phép người dùng bổ sung minh chứng để gửi lại yêu cầu phê duyệt.<br>6. Người dùng đăng nhập khi tài khoản chưa được phê duyệt:<br>Hệ thống chỉ cho phép truy cập trang thông tin trạng thái hồ sơ, chưa cấp quyền quản lý nội dung.<br>7. Gửi email xác thực thất bại:<br>Hệ thống ghi log lỗi và cho phép người dùng yêu cầu gửi lại email xác thực. |

b, Đăng nhập

| **Name** | Đăng nhập |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên đăng nhập vào hệ thống bằng tài khoản đã được phê duyệt để truy cập các chức năng quản lý khóa học, ngân hàng câu hỏi và đề thi. |
| **Normal Flow** | 1. Giáo viên truy cập vào trang Login của hệ thống.<br>2. Hệ thống hiển thị form đăng nhập.<br>3. Giáo viên nhập Email và Password.<br>4. Giáo viên nhấn nút Login.<br>5. Hệ thống kiểm tra các trường không được để trống và định dạng email hợp lệ.<br>6. Hệ thống truy vấn tài khoản tương ứng với email trong cơ sở dữ liệu.<br>7. Hệ thống so khớp mật khẩu với chuỗi băm bcrypt đã lưu.<br>8. Hệ thống kiểm tra trạng thái tài khoản:<br>8.1. Email đã được xác thực.<br>8.2. Hồ sơ giáo viên đã được quản trị viên phê duyệt.<br>8.3. Tài khoản không bị khóa.<br>9. Hệ thống sinh Access Token (hiệu lực 15 phút) và Refresh Token (hiệu lực 7 ngày) theo chuẩn JWT.<br>10. Hệ thống xác định vai trò Teacher và tải danh sách quyền quản lý nội dung tương ứng theo cơ chế RBAC.<br>11. Hệ thống ghi nhật ký đăng nhập (thời điểm, thiết bị, địa chỉ IP).<br>12. Hệ thống chuyển giáo viên đến trang làm việc dành cho giáo viên, hiển thị tổng quan khóa học phụ trách, nội dung chờ duyệt và thống kê nhanh. |
| **Alternative Flow** | 1. Thông tin để trống hoặc sai định dạng:<br>Hệ thống hiển thị thông báo lỗi tương ứng và yêu cầu nhập lại.<br>2. Email không tồn tại hoặc mật khẩu không đúng:<br>Hệ thống hiển thị thông báo chung “Email hoặc mật khẩu không chính xác” nhằm hạn chế nguy cơ dò tìm tài khoản.<br>Hệ thống tăng bộ đếm số lần đăng nhập sai.<br>3. Đăng nhập sai quá 5 lần liên tiếp:<br>Hệ thống tạm khóa chức năng đăng nhập trong 15 phút và gợi ý sử dụng chức năng Quên mật khẩu.<br>4. Hồ sơ giáo viên đang chờ phê duyệt:<br>Hệ thống vẫn cho phép đăng nhập nhưng chỉ hiển thị trang trạng thái hồ sơ, chưa cấp quyền quản lý nội dung.<br>5. Hồ sơ bị từ chối hoặc tài khoản bị khóa:<br>Hệ thống thông báo trạng thái tài khoản và hướng dẫn liên hệ quản trị viên.<br>6. Giáo viên chọn chức năng Quên mật khẩu:<br>Hệ thống chuyển sang luồng của Use Case Quên và đặt lại mật khẩu. |

c, Quản lý tài khoản

[Hình ảnh]

- Đặc tả về xem và cập nhật hồ sơ giáo viên

| **Name** | Xem và cập nhật hồ sơ giáo viên |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên xem và cập nhật thông tin cá nhân cùng hồ sơ chuyên môn. Các thông tin này được hiển thị cho học viên tại trang chi tiết của những khóa học do giáo viên phụ trách. |
| **Normal Flow** | 1. Giáo viên đã đăng nhập và chọn menu “Hồ sơ cá nhân”.<br>2. Hệ thống xác thực Access Token và kiểm tra quyền truy cập.<br>3. Hệ thống truy vấn và hiển thị thông tin hồ sơ:<br>Full Name, Email, ảnh đại diện<br>Đơn vị công tác, chuyên môn giảng dạy, chứng chỉ<br>Phần giới thiệu bản thân hiển thị cho học viên<br>Thống kê: số khóa học phụ trách, số câu hỏi đã đóng góp<br>4. Giáo viên nhấn nút “Chỉnh sửa”.<br>5. Hệ thống chuyển form sang chế độ cho phép nhập liệu.<br>6. Giáo viên cập nhật thông tin, tải ảnh đại diện mới hoặc bổ sung minh chứng chuyên môn.<br>7. Giáo viên nhấn nút “Lưu thay đổi”.<br>8. Hệ thống kiểm tra tính hợp lệ của dữ liệu:<br>8.1. Họ tên không được để trống và không vượt quá độ dài cho phép.<br>8.2. Ảnh đại diện và tệp minh chứng đúng định dạng, dung lượng.<br>8.3. Phần giới thiệu không vượt quá số ký tự quy định.<br>9. Hệ thống lưu thông tin vào cơ sở dữ liệu và ghi nhật ký thay đổi.<br>10. Hệ thống hiển thị thông báo cập nhật thành công và hiển thị lại hồ sơ với thông tin mới. |
| **Alternative Flow** | 1. Dữ liệu nhập không hợp lệ:<br>Hệ thống hiển thị thông báo lỗi tương ứng và giữ nguyên giá trị cũ.<br>2. Ảnh đại diện hoặc tệp minh chứng sai định dạng, vượt dung lượng:<br>Hệ thống thông báo giới hạn cho phép và yêu cầu chọn lại tệp khác.<br>3. Giáo viên chỉnh sửa thông tin chuyên môn đã được phê duyệt:<br>Hệ thống chuyển phần thông tin chuyên môn sang trạng thái chờ quản trị viên xác nhận lại, các thông tin còn lại vẫn được cập nhật ngay.<br>4. Giáo viên nhấn “Hủy”:<br>Hệ thống bỏ qua toàn bộ thay đổi và quay lại chế độ xem hồ sơ.<br>5. Giáo viên cố gắng chỉnh sửa trường Email:<br>Hệ thống không cho phép chỉnh sửa trực tiếp vì email là định danh đăng nhập và hướng dẫn liên hệ quản trị viên nếu cần thay đổi.<br>6. Access Token hết hạn trong quá trình thao tác:<br>Hệ thống dùng Refresh Token cấp lại Access Token; nếu Refresh Token cũng hết hạn thì chuyển về trang Login. |

- Đặc tả về đổi mật khẩu

| **Name** | Đổi mật khẩu |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên đang đăng nhập thay đổi mật khẩu tài khoản nhằm bảo đảm an toàn cho dữ liệu nội dung học tập mà giáo viên đang quản lý. |
| **Normal Flow** | 1. Giáo viên đã đăng nhập và chọn chức năng “Đổi mật khẩu” trong trang Hồ sơ cá nhân.<br>2. Hệ thống hiển thị form đổi mật khẩu gồm các trường:<br>Mật khẩu hiện tại<br>Mật khẩu mới<br>Xác nhận mật khẩu mới<br>3. Giáo viên nhập đầy đủ thông tin và nhấn nút “Xác nhận”.<br>4. Hệ thống kiểm tra các trường không được để trống.<br>5. Hệ thống so khớp mật khẩu hiện tại với chuỗi băm bcrypt đã lưu.<br>6. Hệ thống kiểm tra tính hợp lệ của mật khẩu mới:<br>6.1. Đạt yêu cầu về độ mạnh của mật khẩu.<br>6.2. Khác với mật khẩu hiện tại.<br>6.3. Khớp với trường xác nhận mật khẩu mới.<br>7. Hệ thống băm mật khẩu mới bằng bcrypt và cập nhật vào cơ sở dữ liệu.<br>8. Hệ thống thu hồi toàn bộ Refresh Token đang hoạt động để buộc đăng xuất khỏi các thiết bị khác.<br>9. Hệ thống ghi nhật ký thay đổi mật khẩu và gửi email thông báo đến giáo viên.<br>10. Hệ thống hiển thị thông báo đổi mật khẩu thành công và yêu cầu đăng nhập lại. |
| **Alternative Flow** | 1. Mật khẩu hiện tại không đúng:<br>Hệ thống hiển thị thông báo lỗi, không thực hiện cập nhật và ghi nhận số lần nhập sai.<br>2. Mật khẩu mới không đủ mạnh:<br>Hệ thống hiển thị tiêu chí mật khẩu và yêu cầu nhập lại.<br>3. Mật khẩu mới trùng với mật khẩu hiện tại:<br>Hệ thống thông báo và yêu cầu chọn mật khẩu khác.<br>4. Xác nhận mật khẩu mới không khớp:<br>Hệ thống yêu cầu nhập lại trường Xác nhận mật khẩu mới.<br>5. Giáo viên đang soạn nội dung chưa lưu:<br>Hệ thống cảnh báo thao tác sẽ đăng xuất khỏi mọi phiên làm việc và đề nghị lưu nháp nội dung trước khi tiếp tục. |

- Đặc tả về quên và đặt lại mật khẩu

| **Name** | Quên và đặt lại mật khẩu |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên khôi phục quyền truy cập tài khoản khi quên mật khẩu, thông qua liên kết đặt lại mật khẩu được gửi tới địa chỉ email đã đăng ký. |
| **Normal Flow** | 1. Tại trang Login, giáo viên chọn chức năng “Quên mật khẩu”.<br>2. Hệ thống hiển thị form nhập địa chỉ email.<br>3. Giáo viên nhập email và nhấn nút “Gửi yêu cầu”.<br>4. Hệ thống kiểm tra định dạng email hợp lệ.<br>5. Hệ thống kiểm tra email có tồn tại trong cơ sở dữ liệu hay không.<br>6. Hệ thống sinh token đặt lại mật khẩu có thời hạn 15 phút và lưu vào cơ sở dữ liệu.<br>7. Hệ thống gửi email chứa liên kết đặt lại mật khẩu.<br>8. Hệ thống hiển thị thông báo đã gửi hướng dẫn đặt lại mật khẩu.<br>9. Giáo viên mở email và nhấn vào liên kết đặt lại mật khẩu.<br>10. Hệ thống xác thực token và hiển thị form đặt mật khẩu mới.<br>11. Giáo viên nhập Mật khẩu mới, Xác nhận mật khẩu mới và nhấn “Xác nhận”.<br>12. Hệ thống kiểm tra độ mạnh của mật khẩu và tính khớp giữa hai trường.<br>13. Hệ thống băm mật khẩu bằng bcrypt, cập nhật cơ sở dữ liệu và vô hiệu hóa token vừa sử dụng.<br>14. Hệ thống thu hồi các phiên đăng nhập cũ và ghi nhật ký thao tác.<br>15. Hệ thống thông báo thành công và chuyển đến trang Login. |
| **Alternative Flow** | 1. Email không đúng định dạng:<br>Hệ thống hiển thị thông báo lỗi và yêu cầu nhập lại.<br>2. Email chưa được đăng ký trong hệ thống:<br>Hệ thống vẫn hiển thị thông báo chung như bước 8 nhưng không gửi email, nhằm tránh làm lộ thông tin tài khoản đang tồn tại.<br>3. Token đặt lại mật khẩu hết hạn hoặc đã được sử dụng:<br>Hệ thống thông báo liên kết không còn hiệu lực và cho phép gửi lại yêu cầu mới.<br>4. Mật khẩu mới không hợp lệ hoặc không khớp:<br>Hệ thống hiển thị thông báo lỗi tương ứng và yêu cầu nhập lại.<br>5. Tài khoản đang ở trạng thái chờ phê duyệt:<br>Hệ thống vẫn cho phép đặt lại mật khẩu nhưng giữ nguyên trạng thái chờ phê duyệt của hồ sơ.<br>6. Giáo viên gửi yêu cầu đặt lại mật khẩu nhiều lần liên tiếp:<br>Hệ thống giới hạn tối đa 3 yêu cầu trong 15 phút và thông báo khi vượt ngưỡng. |

- Đặc tả về đăng xuất

| **Name** | Đăng xuất |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên kết thúc phiên làm việc hiện tại và thu hồi token truy cập, bảo đảm an toàn tài khoản khi sử dụng trên thiết bị dùng chung. |
| **Normal Flow** | 1. Giáo viên đang ở trạng thái đã đăng nhập.<br>2. Giáo viên chọn chức năng “Đăng xuất”.<br>3. Hệ thống hiển thị hộp thoại xác nhận đăng xuất.<br>4. Giáo viên xác nhận đăng xuất.<br>5. Hệ thống thu hồi Refresh Token của phiên hiện tại trong cơ sở dữ liệu.<br>6. Hệ thống xóa Access Token và dữ liệu phiên lưu tại phía trình duyệt.<br>7. Hệ thống ghi nhật ký đăng xuất.<br>8. Hệ thống chuyển người dùng về trang Login. |
| **Alternative Flow** | 1. Giáo viên hủy thao tác ở hộp thoại xác nhận:<br>Hệ thống giữ nguyên phiên làm việc hiện tại.<br>2. Giáo viên đang soạn đề thi hoặc câu hỏi chưa lưu:<br>Hệ thống cảnh báo nội dung chưa lưu sẽ bị mất và đề nghị lưu nháp trước khi đăng xuất.<br>3. Có tác vụ sinh nội dung bằng AI đang chạy:<br>Hệ thống thông báo tác vụ vẫn tiếp tục được xử lý nền và kết quả sẽ xuất hiện trong danh sách chờ duyệt ở lần đăng nhập sau.<br>4. Phiên đăng nhập đã hết hạn từ trước:<br>Hệ thống xóa dữ liệu phiên tại trình duyệt và chuyển về trang Login. |

d, Quản lý khóa học và môn học

[Hình ảnh]

- Đặc tả về tạo khóa học

| **Name** | Tạo khóa học |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên tạo một khóa học/môn học mới trong hệ thống, khai báo thông tin cơ bản và cấu trúc ban đầu để tổ chức nội dung học tập cho học viên. |
| **Normal Flow** | 1. Giáo viên đăng nhập và chọn mục “Quản lý khóa học”.<br>2. Hệ thống xác thực Access Token, kiểm tra vai trò Teacher và quyền tạo khóa học theo RBAC.<br>3. Hệ thống hiển thị danh sách khóa học do giáo viên phụ trách.<br>4. Giáo viên nhấn nút “Tạo khóa học mới”.<br>5. Hệ thống hiển thị form khai báo thông tin khóa học:<br>Tên khóa học, mã khóa học<br>Mô tả, mục tiêu đầu ra<br>Kỹ năng, trình độ, đối tượng phù hợp<br>Ảnh đại diện khóa học<br>6. Giáo viên nhập thông tin và nhấn “Lưu”.<br>7. Hệ thống kiểm tra tính hợp lệ của thông tin:<br>7.1. Tên khóa học và mã khóa học không được để trống.<br>7.2. Mã khóa học không trùng với khóa học đã có trong hệ thống.<br>7.3. Ảnh đại diện đúng định dạng và dung lượng cho phép.<br>8. Hệ thống tạo khóa học ở trạng thái “Bản nháp” và gán giáo viên tạo làm người phụ trách.<br>9. Hệ thống lưu thông tin vào cơ sở dữ liệu và ghi nhật ký thao tác.<br>10. Hệ thống thông báo tạo thành công và chuyển đến màn hình tổ chức nội dung khóa học. |
| **Alternative Flow** | 1. Thông tin bắt buộc để trống hoặc không hợp lệ:<br>Hệ thống hiển thị thông báo lỗi tương ứng cho từng trường và giữ lại dữ liệu đã nhập.<br>Giáo viên nhập lại thông tin và thực hiện lại từ bước 6.<br>2. Mã khóa học đã tồn tại:<br>Hệ thống thông báo trùng mã và gợi ý mã khóa học khác.<br>3. Giáo viên nhấn “Hủy”:<br>Hệ thống hủy thao tác, không tạo bản ghi và quay lại danh sách khóa học.<br>4. Tài khoản không có quyền tạo khóa học:<br>Hệ thống từ chối thao tác, hiển thị thông báo và ghi nhật ký cảnh báo.<br>5. Lỗi khi lưu dữ liệu:<br>Hệ thống hoàn tác giao dịch, thông báo lỗi và cho phép giáo viên thử lại. |

- Đặc tả về cập nhật, xóa và xuất bản khóa học

| **Name** | Cập nhật, xóa và xuất bản khóa học |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên chỉnh sửa thông tin khóa học, thay đổi trạng thái xuất bản để học viên có thể truy cập, hoặc đóng và xóa khóa học không còn sử dụng. |
| **Normal Flow** | 1. Giáo viên chọn mục “Quản lý khóa học”.<br>2. Hệ thống hiển thị danh sách khóa học do giáo viên phụ trách kèm trạng thái: Bản nháp, Đã xuất bản hoặc Đã đóng.<br>3. Giáo viên chọn một khóa học và chọn thao tác: Chỉnh sửa, Xuất bản, Gỡ xuất bản hoặc Xóa.<br>4. Trường hợp Chỉnh sửa: hệ thống hiển thị form với dữ liệu hiện tại; giáo viên chỉnh sửa và nhấn “Lưu”.<br>5. Hệ thống kiểm tra tính hợp lệ của dữ liệu và lưu thay đổi.<br>6. Trường hợp Xuất bản: hệ thống kiểm tra điều kiện xuất bản:<br>6.1. Khóa học có ít nhất một bài học hoặc một đề thi.<br>6.2. Các đề thi được gán vào khóa học đã ở trạng thái hoàn chỉnh.<br>7. Hệ thống chuyển trạng thái khóa học sang “Đã xuất bản” và hiển thị khóa học trong danh sách tra cứu của học viên.<br>8. Trường hợp Gỡ xuất bản: hệ thống chuyển trạng thái về “Bản nháp”, ẩn khỏi danh sách tra cứu nhưng giữ nguyên dữ liệu học tập của học viên đã tham gia.<br>9. Trường hợp Xóa: hệ thống hiển thị hộp thoại xác nhận kèm cảnh báo phạm vi ảnh hưởng; sau khi giáo viên xác nhận, hệ thống thực hiện xóa mềm khóa học.<br>10. Hệ thống ghi nhật ký thao tác và hiển thị thông báo kết quả. |
| **Alternative Flow** | 1. Khóa học chưa đủ điều kiện xuất bản:<br>Hệ thống liệt kê cụ thể các điều kiện còn thiếu và không thay đổi trạng thái.<br>2. Khóa học đã có học viên tham gia nhưng giáo viên yêu cầu xóa:<br>Hệ thống không cho phép xóa vĩnh viễn, chỉ cho phép đóng khóa học nhằm bảo toàn dữ liệu học tập của học viên.<br>3. Giáo viên chỉnh sửa khóa học đang ở trạng thái đã xuất bản:<br>Hệ thống cảnh báo thay đổi sẽ ảnh hưởng đến học viên đang học và yêu cầu xác nhận.<br>4. Giáo viên không phải người phụ trách khóa học:<br>Hệ thống từ chối thao tác theo phân quyền RBAC và ghi nhật ký cảnh báo.<br>5. Xung đột chỉnh sửa đồng thời:<br>Hệ thống thông báo dữ liệu đã được cập nhật bởi người dùng khác và yêu cầu tải lại trước khi lưu. |

- Đặc tả về tổ chức nội dung khóa học

| **Name** | Tổ chức nội dung khóa học |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên xây dựng cấu trúc khóa học gồm các chương, bài học, tài liệu đính kèm và gán đề thi, bộ flashcard tương ứng vào từng phần của khóa học. |
| **Normal Flow** | 1. Giáo viên mở một khóa học và chọn mục “Nội dung khóa học”.<br>2. Hệ thống hiển thị cây cấu trúc hiện tại gồm các chương và bài học.<br>3. Giáo viên chọn “Thêm chương”, nhập tên chương và thứ tự hiển thị.<br>4. Hệ thống kiểm tra tên chương không để trống và lưu chương mới.<br>5. Giáo viên chọn một chương và thêm bài học với các thông tin:<br>Tên bài học, mô tả nội dung<br>Nội dung bài giảng, tài liệu đính kèm<br>6. Hệ thống kiểm tra định dạng, dung lượng tài liệu và lưu bài học.<br>7. Giáo viên chọn “Gán đề thi”; hệ thống hiển thị danh sách đề thi khả dụng.<br>8. Giáo viên chọn đề thi cần gán và xác nhận.<br>9. Hệ thống tạo liên kết giữa đề thi và chương hoặc bài học tương ứng.<br>10. Giáo viên sắp xếp lại thứ tự chương, bài học bằng thao tác kéo thả.<br>11. Hệ thống cập nhật thứ tự, lưu vào cơ sở dữ liệu và hiển thị thông báo kết quả. |
| **Alternative Flow** | 1. Tên chương hoặc bài học trùng trong cùng khóa học:<br>Hệ thống cảnh báo trùng tên và yêu cầu giáo viên đặt tên khác.<br>2. Tài liệu vượt quá dung lượng hoặc sai định dạng cho phép:<br>Hệ thống thông báo giới hạn và không thực hiện tải lên.<br>3. Đề thi cần gán đã bị gỡ xuất bản:<br>Hệ thống không cho phép gán và thông báo lý do cho giáo viên.<br>4. Giáo viên xóa chương còn chứa bài học:<br>Hệ thống cảnh báo toàn bộ bài học bên trong sẽ bị xóa theo và yêu cầu xác nhận.<br>5. Mất kết nối trong lúc tải tài liệu:<br>Hệ thống thông báo và cho phép tải lại, không làm mất cấu trúc khóa học đã tạo. |

e, Quản lý ngân hàng câu hỏi

[Hình ảnh]

- Đặc tả về thêm câu hỏi vào ngân hàng câu hỏi

| **Name** | Thêm câu hỏi vào ngân hàng câu hỏi |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên tạo câu hỏi mới với đầy đủ nội dung, phương án trả lời, đáp án đúng, phần giải thích và các nhãn phân loại để đưa vào ngân hàng câu hỏi dùng chung cho việc soạn đề thi. |
| **Normal Flow** | 1. Giáo viên chọn mục “Ngân hàng câu hỏi”.<br>2. Hệ thống hiển thị danh sách câu hỏi kèm ô tìm kiếm và bộ lọc.<br>3. Giáo viên nhấn nút “Thêm câu hỏi”.<br>4. Hệ thống hiển thị form soạn câu hỏi.<br>5. Giáo viên chọn loại câu hỏi: trắc nghiệm một đáp án, trắc nghiệm nhiều đáp án, điền từ hoặc dạng khác.<br>6. Giáo viên nhập nội dung câu hỏi, các phương án trả lời và chọn đáp án đúng.<br>7. Giáo viên nhập phần giải thích đáp án.<br>8. Giáo viên gắn nhãn phân loại cho câu hỏi:<br>Kỹ năng, trình độ, Part<br>Chủ đề, độ khó<br>9. Giáo viên đính kèm tài nguyên âm thanh hoặc hình ảnh (nếu có).<br>10. Giáo viên nhấn “Lưu”.<br>11. Hệ thống kiểm tra tính hợp lệ của dữ liệu:<br>11.1. Nội dung câu hỏi và các phương án trả lời không được để trống.<br>11.2. Phải có ít nhất một đáp án đúng được chọn.<br>11.3. Tài nguyên đính kèm đúng định dạng và dung lượng cho phép.<br>11.4. Các nhãn phân loại bắt buộc đã được chọn đầy đủ.<br>12. Hệ thống lưu câu hỏi vào ngân hàng, ghi nhận người tạo và thời điểm tạo.<br>13. Hệ thống thông báo thành công và hiển thị câu hỏi trong danh sách. |
| **Alternative Flow** | 1. Thiếu thông tin bắt buộc:<br>Hệ thống hiển thị thông báo lỗi cho từng trường và giữ lại nội dung đã soạn.<br>2. Chưa chọn đáp án đúng:<br>Hệ thống không cho phép lưu và hiển thị cảnh báo cho giáo viên.<br>3. Hệ thống phát hiện nội dung trùng lặp cao với câu hỏi đã có:<br>Hệ thống cảnh báo trùng lặp và cho phép giáo viên chọn vẫn lưu hoặc chỉnh sửa lại.<br>4. Tải tệp âm thanh hoặc hình ảnh thất bại:<br>Hệ thống thông báo và cho phép tải lại mà không làm mất nội dung câu hỏi đã soạn.<br>5. Giáo viên chọn “Lưu nháp”:<br>Hệ thống lưu câu hỏi ở trạng thái nháp, chưa cho phép sử dụng để soạn đề thi. |

- Đặc tả về cập nhật và xóa câu hỏi

| **Name** | Cập nhật và xóa câu hỏi |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên chỉnh sửa nội dung câu hỏi đã có hoặc gỡ bỏ câu hỏi không còn phù hợp, đồng thời bảo đảm tính nhất quán của các đề thi và kết quả bài làm đã sử dụng câu hỏi đó. |
| **Normal Flow** | 1. Giáo viên tìm kiếm và chọn câu hỏi cần thao tác trong ngân hàng câu hỏi.<br>2. Hệ thống hiển thị chi tiết câu hỏi kèm thông tin quản lý:<br>Người tạo, thời điểm cập nhật gần nhất<br>Số đề thi đang sử dụng câu hỏi này<br>3. Giáo viên chọn thao tác “Chỉnh sửa” hoặc “Xóa”.<br>4. Trường hợp Chỉnh sửa: hệ thống hiển thị form với dữ liệu hiện tại; giáo viên sửa nội dung, phương án, đáp án, giải thích hoặc nhãn phân loại.<br>5. Giáo viên nhấn “Lưu thay đổi”.<br>6. Hệ thống kiểm tra tính hợp lệ của dữ liệu tương tự khi tạo câu hỏi mới.<br>7. Hệ thống kiểm tra câu hỏi có đang được sử dụng trong đề thi đã xuất bản hay không.<br>8. Hệ thống lưu phiên bản mới của câu hỏi và giữ lại lịch sử chỉnh sửa.<br>9. Trường hợp Xóa: hệ thống hiển thị hộp thoại xác nhận kèm cảnh báo phạm vi ảnh hưởng.<br>10. Sau khi giáo viên xác nhận, hệ thống thực hiện xóa mềm câu hỏi.<br>11. Hệ thống ghi nhật ký thao tác và hiển thị thông báo kết quả. |
| **Alternative Flow** | 1. Câu hỏi đang được sử dụng trong đề thi đã xuất bản:<br>Hệ thống cảnh báo thay đổi có thể ảnh hưởng đến các đề thi liên quan.<br>Hệ thống giữ nguyên phiên bản câu hỏi đã dùng cho các bài làm trước đó nhằm bảo đảm tính nhất quán của kết quả.<br>2. Giáo viên yêu cầu xóa câu hỏi đang được sử dụng:<br>Hệ thống không cho phép xóa, chỉ cho phép chuyển câu hỏi sang trạng thái “Ngừng sử dụng”.<br>3. Giáo viên không phải người tạo câu hỏi:<br>Hệ thống kiểm tra quyền và chỉ cho phép chỉnh sửa nếu giáo viên có quyền quản lý ngân hàng câu hỏi chung.<br>4. Xung đột chỉnh sửa đồng thời:<br>Hệ thống thông báo và yêu cầu tải lại dữ liệu mới nhất trước khi lưu. |

- Đặc tả về nhập câu hỏi hàng loạt từ tệp

| **Name** | Nhập câu hỏi hàng loạt từ tệp |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên nhập nhiều câu hỏi cùng lúc từ tệp Excel hoặc Word theo mẫu quy định, giúp rút ngắn thời gian xây dựng ngân hàng câu hỏi. |
| **Normal Flow** | 1. Giáo viên chọn “Ngân hàng câu hỏi” rồi chọn chức năng “Nhập từ tệp”.<br>2. Hệ thống hiển thị hướng dẫn nhập liệu và cho phép tải tệp mẫu.<br>3. Giáo viên tải tệp mẫu và điền dữ liệu câu hỏi theo đúng cấu trúc cột quy định.<br>4. Giáo viên tải tệp đã điền lên hệ thống.<br>5. Hệ thống kiểm tra định dạng và dung lượng tệp.<br>6. Hệ thống đọc, phân tích nội dung tệp và kiểm tra tính hợp lệ của từng dòng dữ liệu.<br>7. Hệ thống hiển thị bảng xem trước kết quả:<br>Số câu hỏi hợp lệ sẽ được nhập<br>Số dòng lỗi kèm mô tả lỗi cụ thể theo từng dòng<br>8. Giáo viên xem lại, chọn các câu hỏi cần nhập và nhấn “Xác nhận nhập”.<br>9. Hệ thống lưu các câu hỏi hợp lệ vào ngân hàng câu hỏi.<br>10. Hệ thống hiển thị báo cáo kết quả nhập và ghi nhật ký thao tác. |
| **Alternative Flow** | 1. Tệp sai định dạng hoặc không đúng cấu trúc mẫu:<br>Hệ thống thông báo lỗi và hướng dẫn giáo viên tải lại tệp mẫu chuẩn.<br>2. Tệp vượt quá dung lượng hoặc số dòng cho phép:<br>Hệ thống thông báo giới hạn và đề nghị chia nhỏ tệp trước khi nhập.<br>3. Một số dòng dữ liệu không hợp lệ:<br>Hệ thống chỉ nhập các dòng hợp lệ và xuất danh sách dòng lỗi để giáo viên sửa rồi nhập lại.<br>4. Phát hiện câu hỏi trùng với câu hỏi đã có trong ngân hàng:<br>Hệ thống đánh dấu trùng lặp và cho phép giáo viên chọn bỏ qua hoặc vẫn nhập.<br>5. Giáo viên hủy thao tác tại bước xem trước:<br>Hệ thống không lưu bất kỳ câu hỏi nào vào ngân hàng.<br>6. Lỗi trong quá trình xử lý tệp:<br>Hệ thống hoàn tác toàn bộ giao dịch nhập, thông báo lỗi và cho phép thử lại. |

- Đặc tả về tìm kiếm và lọc câu hỏi

| **Name** | Tìm kiếm và lọc câu hỏi |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên tra cứu nhanh câu hỏi trong ngân hàng theo nhiều tiêu chí phân loại và thực hiện các thao tác hàng loạt trên tập kết quả tìm được. |
| **Normal Flow** | 1. Giáo viên truy cập mục “Ngân hàng câu hỏi”.<br>2. Hệ thống hiển thị danh sách câu hỏi có phân trang.<br>3. Giáo viên nhập từ khóa và/hoặc chọn các tiêu chí lọc:<br>Kỹ năng, trình độ, Part, chủ đề, độ khó<br>Người tạo, trạng thái câu hỏi, nguồn gốc (thủ công hoặc do AI sinh)<br>4. Giáo viên nhấn nút “Tìm kiếm”.<br>5. Hệ thống chuẩn hóa tham số và truy vấn cơ sở dữ liệu.<br>6. Hệ thống trả về danh sách kết quả kèm tổng số bản ghi.<br>7. Hệ thống hiển thị kết quả gồm: nội dung rút gọn, loại câu hỏi, nhãn phân loại và số lần câu hỏi được sử dụng.<br>8. Giáo viên có thể chọn nhiều câu hỏi để thực hiện thao tác hàng loạt: gắn nhãn, đổi trạng thái hoặc thêm vào đề thi.<br>9. Hệ thống thực hiện thao tác và hiển thị kết quả cho giáo viên. |
| **Alternative Flow** | 1. Không có kết quả phù hợp:<br>Hệ thống thông báo không tìm thấy và gợi ý nới lỏng điều kiện lọc.<br>2. Số lượng kết quả lớn:<br>Hệ thống phân trang phía máy chủ và giới hạn số bản ghi mỗi trang để bảo đảm hiệu năng phản hồi.<br>3. Thao tác hàng loạt áp dụng lên câu hỏi đang được sử dụng:<br>Hệ thống cảnh báo, chỉ áp dụng cho các câu hỏi hợp lệ và báo cáo lại danh sách câu hỏi bị bỏ qua.<br>4. Lỗi truy vấn dữ liệu:<br>Hệ thống thông báo lỗi và cho phép giáo viên thử lại. |

f, Quản lý đề thi

[Hình ảnh]

- Đặc tả về tạo đề thi và soạn đề từ ngân hàng câu hỏi

| **Name** | Tạo đề thi và soạn đề từ ngân hàng câu hỏi |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên tạo đề thi mới và lựa chọn câu hỏi từ ngân hàng câu hỏi theo cách thủ công hoặc rút tự động theo ma trận đề đã thiết lập. |
| **Normal Flow** | 1. Giáo viên chọn mục “Quản lý đề thi” và nhấn “Tạo đề thi mới”.<br>2. Hệ thống hiển thị form khai báo thông tin đề thi: tên đề, mô tả, kỹ năng, trình độ và thang điểm.<br>3. Giáo viên nhập thông tin và nhấn “Tiếp tục”.<br>4. Hệ thống kiểm tra tính hợp lệ và tạo đề thi ở trạng thái “Bản nháp”.<br>5. Hệ thống hiển thị màn hình soạn đề với hai phương thức chọn câu hỏi:<br>Chọn thủ công từ ngân hàng câu hỏi<br>Rút tự động theo ma trận đề: số câu theo Part, chủ đề và độ khó<br>6. Giáo viên chọn phương thức và thực hiện lựa chọn câu hỏi.<br>7. Hệ thống kiểm tra số lượng câu hỏi khả dụng có đáp ứng tiêu chí đã đặt hay không.<br>8. Hệ thống đưa các câu hỏi được chọn vào đề thi và hiển thị theo từng Part.<br>9. Giáo viên sắp xếp thứ tự câu hỏi và điều chỉnh điểm số của từng câu.<br>10. Giáo viên nhấn “Lưu đề thi”.<br>11. Hệ thống kiểm tra đề thi có ít nhất một câu hỏi và tổng điểm hợp lệ.<br>12. Hệ thống lưu đề thi vào cơ sở dữ liệu và ghi nhật ký thao tác.<br>13. Hệ thống hiển thị thông báo lưu thành công. |
| **Alternative Flow** | 1. Ngân hàng câu hỏi không đủ số câu theo ma trận đề:<br>Hệ thống thông báo số câu còn thiếu theo từng tiêu chí.<br>Hệ thống cho phép giáo viên điều chỉnh ma trận đề hoặc bổ sung câu hỏi vào ngân hàng.<br>2. Giáo viên chọn câu hỏi đã có trong đề:<br>Hệ thống cảnh báo trùng lặp và không thêm câu hỏi đó vào đề thi.<br>3. Tổng điểm các câu hỏi không khớp thang điểm đã khai báo:<br>Hệ thống cảnh báo và gợi ý phương án phân bổ lại điểm.<br>4. Giáo viên thoát giữa chừng:<br>Hệ thống lưu đề thi ở trạng thái bản nháp để giáo viên tiếp tục soạn sau.<br>5. Lỗi khi lưu đề thi:<br>Hệ thống hoàn tác giao dịch, thông báo lỗi và cho phép thử lại. |

- Đặc tả về cấu hình và xem trước đề thi

| **Name** | Cấu hình và xem trước đề thi |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên thiết lập các tham số làm bài của đề thi và xem trước đề thi theo đúng giao diện học viên sẽ sử dụng, nhằm kiểm tra chất lượng trước khi xuất bản. |
| **Normal Flow** | 1. Giáo viên mở một đề thi ở trạng thái bản nháp và chọn “Cấu hình đề thi”.<br>2. Hệ thống hiển thị form cấu hình gồm các tham số:<br>Thời gian làm bài của toàn đề và của từng Part<br>Số lần làm bài tối đa cho mỗi học viên<br>Chế độ cho phép: thi thử toàn phần, luyện tập theo Part<br>Quy tắc hiển thị kết quả và giải thích đáp án sau khi nộp bài<br>Tùy chọn đảo thứ tự câu hỏi và phương án trả lời<br>3. Giáo viên thiết lập các tham số và nhấn “Lưu cấu hình”.<br>4. Hệ thống kiểm tra tính hợp lệ của cấu hình:<br>4.1. Thời gian làm bài lớn hơn 0 và phù hợp với số lượng câu hỏi.<br>4.2. Tổng thời gian các Part không vượt quá thời gian của toàn đề.<br>5. Hệ thống lưu cấu hình vào cơ sở dữ liệu.<br>6. Giáo viên chọn chức năng “Xem trước đề thi”.<br>7. Hệ thống hiển thị đề thi theo đúng giao diện học viên sẽ nhìn thấy, bao gồm đồng hồ đếm ngược và bảng điều hướng câu hỏi.<br>8. Giáo viên kiểm tra nội dung câu hỏi, tài nguyên âm thanh, hình ảnh và bố cục hiển thị.<br>9. Giáo viên thoát chế độ xem trước; hệ thống không ghi nhận phiên xem trước vào dữ liệu kết quả học tập. |
| **Alternative Flow** | 1. Tham số cấu hình không hợp lệ:<br>Hệ thống thông báo lỗi tương ứng và giữ nguyên cấu hình cũ.<br>2. Phát hiện câu hỏi lỗi khi xem trước (thiếu đáp án, tài nguyên hỏng):<br>Hệ thống hiển thị cảnh báo và cho phép giáo viên chuyển nhanh đến câu hỏi đó để chỉnh sửa.<br>3. Đề thi chưa có câu hỏi nào:<br>Hệ thống không cho phép xem trước và thông báo cần bổ sung câu hỏi.<br>4. Giáo viên thay đổi cấu hình của đề thi đã xuất bản:<br>Hệ thống cảnh báo ảnh hưởng đến các lượt làm bài mới và yêu cầu xác nhận trước khi lưu. |

- Đặc tả về xuất bản và gỡ xuất bản đề thi

| **Name** | Xuất bản và gỡ xuất bản đề thi |
|---|---|
| **Actor** | Teacher (Giáo viên) |
| **Description** | Cho phép giáo viên đưa đề thi vào sử dụng cho học viên hoặc tạm ngừng cung cấp đề thi, đồng thời bảo toàn toàn bộ kết quả bài làm đã phát sinh. |
| **Normal Flow** | 1. Giáo viên chọn đề thi cần thao tác trong danh sách đề thi.<br>2. Hệ thống hiển thị chi tiết đề thi và trạng thái hiện tại.<br>3. Giáo viên nhấn nút “Xuất bản”.<br>4. Hệ thống kiểm tra điều kiện xuất bản:<br>4.1. Đề thi có đủ câu hỏi theo cấu trúc đã khai báo.<br>4.2. Mọi câu hỏi đều có đáp án đúng và đang ở trạng thái sử dụng.<br>4.3. Cấu hình thời gian làm bài và thang điểm đã được thiết lập.<br>5. Hệ thống chuyển trạng thái đề thi sang “Đã xuất bản”.<br>6. Hệ thống hiển thị đề thi trong danh sách tra cứu của học viên và trong các khóa học có gán đề thi này.<br>7. Hệ thống ghi nhật ký thao tác kèm người thực hiện và thời điểm.<br>8. Trường hợp Gỡ xuất bản: giáo viên chọn “Gỡ xuất bản”, hệ thống hiển thị hộp thoại xác nhận.<br>9. Sau khi giáo viên xác nhận, hệ thống ẩn đề thi khỏi danh sách của học viên nhưng giữ nguyên toàn bộ kết quả bài làm đã có.<br>10. Hệ thống hiển thị thông báo kết quả thao tác. |
| **Alternative Flow** | 1. Đề thi chưa đủ điều kiện xuất bản:<br>Hệ thống liệt kê cụ thể các điều kiện chưa đạt và không thay đổi trạng thái đề thi.<br>2. Đề thi chứa câu hỏi đã bị ngừng sử dụng:<br>Hệ thống cảnh báo và yêu cầu giáo viên thay thế câu hỏi trước khi xuất bản.<br>3. Có học viên đang làm bài khi giáo viên gỡ xuất bản:<br>Hệ thống cho phép các phiên làm bài đang diễn ra hoàn tất bình thường và chỉ chặn các lượt làm bài mới.<br>4. Giáo viên không có quyền xuất bản đề thi:<br>Hệ thống từ chối thao tác và ghi nhật ký cảnh báo. |

**3. Admin**

a, Đăng nhập

| **Name** | Đăng ký tài khoản quản trị viên |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép người được cấp quyền tạo tài khoản quản trị viên thông qua mã mời do quản trị viên hiện có cấp. Hệ thống không mở đăng ký công khai cho vai trò quản trị viên nhằm bảo đảm an toàn cho toàn hệ thống. |
| **Normal Flow** | 1. Quản trị viên hiện có tạo mã mời và gửi kèm liên kết đăng ký đến địa chỉ email của người được cấp quyền.<br>2. Người được mời truy cập liên kết đăng ký tài khoản quản trị viên.<br>3. Hệ thống yêu cầu nhập mã mời.<br>4. Hệ thống kiểm tra mã mời hợp lệ và còn hiệu lực.<br>5. Hệ thống hiển thị form đăng ký gồm các thông tin:<br>Full Name, Email (đã gắn với mã mời)<br>Password, Confirm Password<br>Số điện thoại liên hệ<br>6. Người dùng nhập thông tin và tích chọn đồng ý Điều khoản sử dụng, Chính sách xử lý dữ liệu cá nhân.<br>7. Người dùng nhấn nút Register.<br>8. Hệ thống kiểm tra tính hợp lệ của thông tin:<br>8.1. Các trường bắt buộc không được để trống.<br>8.2. Email nhập vào trùng khớp với email đã được mời.<br>8.3. Mật khẩu đáp ứng chính sách bảo mật dành cho tài khoản quản trị: độ dài tối thiểu cao hơn tài khoản thường và có ký tự đặc biệt.<br>8.4. Password và Confirm Password khớp nhau.<br>9. Hệ thống mã hóa mật khẩu bằng bcrypt, tạo tài khoản với vai trò Admin và trạng thái “Đang hoạt động”.<br>10. Hệ thống vô hiệu hóa mã mời vừa được sử dụng.<br>11. Hệ thống yêu cầu thiết lập xác thực hai yếu tố trong lần đăng nhập đầu tiên.<br>12. Hệ thống ghi nhật ký kiểm toán việc tạo tài khoản quản trị viên kèm thông tin người cấp mã mời.<br>13. Hệ thống thông báo đăng ký thành công và chuyển đến trang Login. |
| **Alternative Flow** | 1. Mã mời không hợp lệ hoặc đã được sử dụng:<br>Hệ thống từ chối và thông báo người dùng liên hệ quản trị viên đã cấp mã.<br>2. Mã mời đã hết hạn:<br>Hệ thống thông báo và cho phép người dùng yêu cầu cấp lại mã mời mới.<br>3. Email nhập vào không khớp với email được mời:<br>Hệ thống từ chối đăng ký và ghi nhật ký cảnh báo truy cập bất thường.<br>4. Mật khẩu không đạt chính sách bảo mật dành cho quản trị viên:<br>Hệ thống hiển thị tiêu chí mật khẩu và yêu cầu nhập lại.<br>5. Người dùng truy cập trang đăng ký quản trị viên mà không có mã mời:<br>Hệ thống không hiển thị form đăng ký và ghi nhật ký lượt truy cập bất thường. |

b, Quản lý tài khoản

[Hình ảnh]

- Đặc tả về xem và cập nhật hồ sơ quản trị viên

| **Name** | Xem và cập nhật hồ sơ quản trị viên |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên xem và cập nhật thông tin tài khoản của chính mình, đồng thời quản lý cấu hình nhận cảnh báo hệ thống. |
| **Normal Flow** | 1. Quản trị viên đã đăng nhập và chọn menu “Hồ sơ cá nhân”.<br>2. Hệ thống xác thực và hiển thị thông tin hồ sơ:<br>Full Name, Email, số điện thoại liên hệ<br>Vai trò và phạm vi quyền đang được cấp<br>Thời điểm đăng nhập gần nhất, trạng thái xác thực hai yếu tố<br>3. Quản trị viên nhấn nút “Chỉnh sửa”.<br>4. Quản trị viên cập nhật họ tên, số điện thoại, ảnh đại diện hoặc cấu hình nhận cảnh báo hệ thống qua email.<br>5. Quản trị viên nhấn nút “Lưu thay đổi”.<br>6. Hệ thống kiểm tra tính hợp lệ của dữ liệu nhập.<br>7. Hệ thống yêu cầu nhập lại mật khẩu để xác nhận thay đổi thông tin tài khoản quản trị.<br>8. Hệ thống kiểm tra mật khẩu xác nhận.<br>9. Hệ thống lưu thay đổi vào cơ sở dữ liệu và ghi nhật ký kiểm toán.<br>10. Hệ thống hiển thị thông báo cập nhật thành công. |
| **Alternative Flow** | 1. Dữ liệu nhập không hợp lệ:<br>Hệ thống hiển thị thông báo lỗi tương ứng và giữ nguyên giá trị cũ.<br>2. Mật khẩu xác nhận không đúng:<br>Hệ thống từ chối lưu thay đổi và ghi nhật ký cảnh báo.<br>3. Quản trị viên tự thay đổi vai trò hoặc phạm vi quyền của chính mình:<br>Hệ thống không cho phép thực hiện tại màn hình hồ sơ; thao tác này phải thực hiện qua chức năng phân quyền và tuân theo ràng buộc về quản trị viên duy nhất.<br>4. Quản trị viên tắt xác thực hai yếu tố:<br>Hệ thống cảnh báo rủi ro bảo mật và chỉ cho phép nếu chính sách hệ thống không bắt buộc áp dụng.<br>5. Access Token hết hạn trong quá trình thao tác:<br>Hệ thống yêu cầu đăng nhập lại do tài khoản quản trị có thời hạn phiên ngắn. |

- Đặc tả về đổi mật khẩu

| **Name** | Đổi mật khẩu |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên thay đổi mật khẩu theo chính sách bảo mật áp dụng riêng cho tài khoản quản trị, bao gồm yêu cầu xác thực hai yếu tố và không được sử dụng lại các mật khẩu cũ. |
| **Normal Flow** | 1. Quản trị viên đã đăng nhập và chọn chức năng “Đổi mật khẩu”.<br>2. Hệ thống hiển thị form gồm: Mật khẩu hiện tại, Mật khẩu mới, Xác nhận mật khẩu mới.<br>3. Quản trị viên nhập đầy đủ thông tin và nhấn “Xác nhận”.<br>4. Hệ thống kiểm tra các trường không được để trống.<br>5. Hệ thống so khớp mật khẩu hiện tại với chuỗi băm bcrypt đã lưu.<br>6. Hệ thống kiểm tra mật khẩu mới theo chính sách dành cho quản trị viên:<br>6.1. Độ dài và độ phức tạp đạt yêu cầu.<br>6.2. Không trùng với các mật khẩu đã sử dụng gần nhất theo số lượng quy định.<br>6.3. Mật khẩu mới và xác nhận mật khẩu mới khớp nhau.<br>7. Hệ thống yêu cầu nhập mã xác thực hai yếu tố để hoàn tất thao tác.<br>8. Hệ thống băm mật khẩu mới bằng bcrypt và cập nhật vào cơ sở dữ liệu.<br>9. Hệ thống thu hồi toàn bộ Refresh Token, buộc đăng xuất khỏi mọi thiết bị.<br>10. Hệ thống ghi nhật ký kiểm toán và gửi email thông báo đến quản trị viên.<br>11. Hệ thống thông báo thành công và yêu cầu đăng nhập lại. |
| **Alternative Flow** | 1. Mật khẩu hiện tại không đúng:<br>Hệ thống thông báo lỗi, không cập nhật và ghi nhật ký cảnh báo.<br>2. Mật khẩu mới không đạt chính sách bảo mật:<br>Hệ thống hiển thị tiêu chí và yêu cầu nhập lại.<br>3. Mật khẩu mới trùng với mật khẩu đã sử dụng trước đó:<br>Hệ thống thông báo và yêu cầu chọn mật khẩu khác.<br>4. Mã xác thực hai yếu tố không hợp lệ:<br>Hệ thống hủy thao tác đổi mật khẩu và giữ nguyên mật khẩu cũ.<br>5. Đến hạn đổi mật khẩu định kỳ theo chính sách:<br>Hệ thống bắt buộc quản trị viên đổi mật khẩu ngay sau khi đăng nhập và không cho phép sử dụng các chức năng khác cho đến khi hoàn tất. |

- Đặc tả về quên và đặt lại mật khẩu

| **Name** | Quên và đặt lại mật khẩu |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên khôi phục quyền truy cập khi quên mật khẩu, với quy trình kiểm soát chặt hơn tài khoản thường do đây là tài khoản có quyền cao nhất trong hệ thống. |
| **Normal Flow** | 1. Tại trang Login, quản trị viên chọn chức năng “Quên mật khẩu”.<br>2. Hệ thống hiển thị form nhập địa chỉ email.<br>3. Quản trị viên nhập email và nhấn “Gửi yêu cầu”.<br>4. Hệ thống kiểm tra định dạng và sự tồn tại của email gắn với vai trò Admin.<br>5. Hệ thống sinh token đặt lại mật khẩu có thời hạn ngắn (10 phút).<br>6. Hệ thống gửi email chứa liên kết đặt lại mật khẩu và đồng thời gửi cảnh báo đến các quản trị viên khác.<br>7. Hệ thống hiển thị thông báo chung đã gửi hướng dẫn đặt lại mật khẩu.<br>8. Quản trị viên mở email và nhấn vào liên kết đặt lại mật khẩu.<br>9. Hệ thống xác thực token và yêu cầu nhập mã xác thực hai yếu tố hoặc mã khôi phục dự phòng.<br>10. Hệ thống kiểm tra mã và hiển thị form đặt mật khẩu mới.<br>11. Quản trị viên nhập mật khẩu mới, xác nhận mật khẩu và nhấn “Xác nhận”.<br>12. Hệ thống kiểm tra mật khẩu mới theo chính sách dành cho quản trị viên.<br>13. Hệ thống băm mật khẩu bằng bcrypt, cập nhật cơ sở dữ liệu và vô hiệu hóa token vừa sử dụng.<br>14. Hệ thống thu hồi toàn bộ phiên đăng nhập cũ và ghi nhật ký kiểm toán.<br>15. Hệ thống thông báo thành công và chuyển đến trang Login. |
| **Alternative Flow** | 1. Email không thuộc tài khoản quản trị viên:<br>Hệ thống hiển thị thông báo chung như bước 7 nhưng không gửi email đặt lại mật khẩu.<br>2. Token đặt lại mật khẩu hết hạn hoặc đã được sử dụng:<br>Hệ thống thông báo liên kết không còn hiệu lực và cho phép gửi lại yêu cầu.<br>3. Không vượt qua bước xác thực hai yếu tố:<br>Hệ thống hủy yêu cầu đặt lại mật khẩu và ghi nhật ký cảnh báo.<br>4. Quản trị viên không còn quyền truy cập email hoặc thiết bị xác thực:<br>Việc đặt lại mật khẩu phải do quản trị viên khác thực hiện thông qua chức năng quản lý người dùng.<br>5. Gửi yêu cầu đặt lại mật khẩu nhiều lần liên tiếp:<br>Hệ thống giới hạn số lần yêu cầu trong một khoảng thời gian và thông báo khi vượt ngưỡng. |

- Đặc tả về đăng xuất

| **Name** | Đăng xuất |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên kết thúc phiên làm việc và thu hồi token truy cập, đồng thời hệ thống tự động đăng xuất khi phát hiện phiên không hoạt động quá thời gian quy định. |
| **Normal Flow** | 1. Quản trị viên đang ở trạng thái đã đăng nhập.<br>2. Quản trị viên chọn chức năng “Đăng xuất”.<br>3. Hệ thống hiển thị hộp thoại xác nhận đăng xuất.<br>4. Quản trị viên xác nhận đăng xuất.<br>5. Hệ thống thu hồi Refresh Token của phiên hiện tại trong cơ sở dữ liệu.<br>6. Hệ thống xóa Access Token và dữ liệu phiên lưu tại phía trình duyệt.<br>7. Hệ thống ghi nhật ký kiểm toán thao tác đăng xuất.<br>8. Hệ thống chuyển người dùng về trang Login. |
| **Alternative Flow** | 1. Quản trị viên hủy thao tác ở hộp thoại xác nhận:<br>Hệ thống giữ nguyên phiên làm việc hiện tại.<br>2. Đang có tác vụ quản trị chạy nền (sao lưu dữ liệu, xuất báo cáo):<br>Hệ thống thông báo tác vụ vẫn tiếp tục được xử lý và kết quả được lưu lại.<br>Quản trị viên có thể xem lại kết quả ở lần đăng nhập sau.<br>3. Phiên không có thao tác nào quá thời gian quy định:<br>Hệ thống tự động đăng xuất và ghi nhật ký kiểm toán, với ngưỡng thời gian chặt hơn tài khoản thường.<br>4. Phiên đăng nhập đã hết hạn từ trước:<br>Hệ thống xóa dữ liệu phiên tại trình duyệt và chuyển về trang Login. |

c, Quản lý người dùng và phân quyền

[Hình ảnh]

- Đặc tả về tạo và cập nhật tài khoản người dùng

| **Name** | Tạo và cập nhật tài khoản người dùng |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên trực tiếp tạo tài khoản cho học viên, giáo viên hoặc quản trị viên khác và cập nhật thông tin tài khoản khi cần, phục vụ công tác quản lý người dùng của hệ thống. |
| **Normal Flow** | 1. Quản trị viên đăng nhập và chọn mục “Quản lý người dùng”.<br>2. Hệ thống xác thực và kiểm tra quyền quản trị theo cơ chế RBAC.<br>3. Hệ thống hiển thị danh sách người dùng kèm vai trò và trạng thái tài khoản.<br>4. Quản trị viên nhấn nút “Thêm người dùng”.<br>5. Hệ thống hiển thị form khai báo thông tin:<br>Họ tên, email<br>Vai trò: Student, Teacher hoặc Admin<br>Trạng thái khởi tạo của tài khoản<br>6. Quản trị viên nhập thông tin và nhấn “Lưu”.<br>7. Hệ thống kiểm tra tính hợp lệ của dữ liệu:<br>7.1. Các trường bắt buộc không được để trống.<br>7.2. Email đúng định dạng và chưa tồn tại trong hệ thống.<br>7.3. Vai trò được chọn nằm trong danh sách vai trò hợp lệ.<br>8. Hệ thống sinh mật khẩu tạm thời, băm bằng bcrypt và lưu tài khoản vào cơ sở dữ liệu.<br>9. Hệ thống gửi email thông báo kèm hướng dẫn đăng nhập lần đầu và yêu cầu đổi mật khẩu.<br>10. Trường hợp cập nhật: quản trị viên chọn một tài khoản, chỉnh sửa thông tin và nhấn “Lưu”; hệ thống kiểm tra hợp lệ rồi cập nhật dữ liệu.<br>11. Hệ thống ghi nhật ký thao tác kèm người thực hiện và thời điểm.<br>12. Hệ thống hiển thị thông báo kết quả. |
| **Alternative Flow** | 1. Email đã tồn tại trong hệ thống:<br>Hệ thống thông báo trùng email và yêu cầu nhập địa chỉ khác.<br>2. Dữ liệu nhập không hợp lệ:<br>Hệ thống hiển thị thông báo lỗi cho từng trường và giữ lại dữ liệu đã nhập.<br>3. Gửi email thông báo thất bại:<br>Hệ thống vẫn tạo tài khoản, ghi log lỗi và cho phép gửi lại thông tin đăng nhập.<br>4. Quản trị viên tự hạ quyền của chính mình:<br>Hệ thống cảnh báo nguy cơ mất quyền quản trị và yêu cầu xác nhận.<br>Hệ thống từ chối thao tác nếu đây là tài khoản quản trị viên duy nhất của hệ thống.<br>5. Lỗi khi lưu dữ liệu:<br>Hệ thống hoàn tác giao dịch và thông báo lỗi cho quản trị viên. |

- Đặc tả về khóa và mở khóa tài khoản

| **Name** | Khóa và mở khóa tài khoản |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên tạm ngừng hoặc khôi phục quyền truy cập của một tài khoản khi phát hiện vi phạm quy định sử dụng hoặc có dấu hiệu truy cập bất thường. |
| **Normal Flow** | 1. Quản trị viên truy cập danh sách người dùng.<br>2. Quản trị viên tìm kiếm và chọn tài khoản cần thao tác.<br>3. Hệ thống hiển thị chi tiết tài khoản kèm lịch sử đăng nhập gần đây.<br>4. Quản trị viên chọn chức năng “Khóa tài khoản”.<br>5. Hệ thống hiển thị hộp thoại yêu cầu nhập lý do khóa tài khoản.<br>6. Quản trị viên nhập lý do và xác nhận thao tác.<br>7. Hệ thống chuyển trạng thái tài khoản sang “Bị khóa”.<br>8. Hệ thống thu hồi toàn bộ token đang hoạt động, buộc kết thúc phiên đăng nhập của tài khoản đó.<br>9. Hệ thống gửi email thông báo đến người dùng bị khóa.<br>10. Hệ thống ghi nhật ký kèm lý do, người thực hiện và thời điểm thao tác.<br>11. Trường hợp Mở khóa: quản trị viên chọn “Mở khóa” và xác nhận; hệ thống chuyển trạng thái tài khoản về “Đang hoạt động” và gửi thông báo cho người dùng. |
| **Alternative Flow** | 1. Quản trị viên khóa tài khoản của chính mình:<br>Hệ thống từ chối thao tác và hiển thị cảnh báo.<br>2. Tài khoản là quản trị viên duy nhất của hệ thống:<br>Hệ thống không cho phép khóa nhằm bảo đảm luôn có người quản trị hệ thống.<br>3. Quản trị viên không nhập lý do khóa:<br>Hệ thống yêu cầu nhập lý do trước khi thực hiện thao tác.<br>4. Người dùng đang làm bài thi tại thời điểm bị khóa:<br>Hệ thống lưu bài làm ở trạng thái dở dang nhằm bảo toàn dữ liệu học tập.<br>5. Lỗi khi cập nhật trạng thái tài khoản:<br>Hệ thống hoàn tác thao tác và thông báo lỗi. |

- Đặc tả về gán vai trò và cấu hình quyền truy cập

| **Name** | Gán vai trò và cấu hình quyền truy cập (RBAC) |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên gán vai trò cho người dùng và thiết lập ma trận quyền truy cập của từng vai trò đối với các chức năng trong hệ thống theo mô hình phân quyền RBAC. |
| **Normal Flow** | 1. Quản trị viên chọn mục “Vai trò và phân quyền”.<br>2. Hệ thống hiển thị danh sách vai trò hiện có (Student, Teacher, Admin) kèm ma trận quyền tương ứng.<br>3. Quản trị viên chọn thao tác: gán vai trò cho người dùng hoặc cấu hình quyền của một vai trò.<br>4. Trường hợp gán vai trò: quản trị viên chọn người dùng, chọn vai trò mới và xác nhận.<br>5. Hệ thống kiểm tra tính hợp lệ của vai trò và các ràng buộc nghiệp vụ liên quan.<br>6. Hệ thống cập nhật vai trò và thu hồi token hiện tại để áp dụng quyền mới ở lần đăng nhập kế tiếp.<br>7. Trường hợp cấu hình quyền: hệ thống hiển thị danh sách chức năng theo từng module kèm các quyền xem, thêm, sửa, xóa.<br>8. Quản trị viên tích chọn hoặc bỏ chọn quyền cho vai trò và nhấn “Lưu”.<br>9. Hệ thống kiểm tra ràng buộc: vai trò Admin phải giữ tối thiểu các quyền quản trị cốt lõi.<br>10. Hệ thống lưu cấu hình phân quyền và áp dụng cho toàn bộ người dùng thuộc vai trò đó.<br>11. Hệ thống ghi nhật ký thay đổi phân quyền kèm giá trị trước và sau khi thay đổi. |
| **Alternative Flow** | 1. Thay đổi làm mất toàn bộ quyền quản trị của hệ thống:<br>Hệ thống từ chối lưu cấu hình và hiển thị cảnh báo.<br>2. Quản trị viên thay đổi vai trò của chính mình:<br>Hệ thống yêu cầu xác nhận hai bước.<br>Hệ thống không cho phép thực hiện nếu đây là quản trị viên duy nhất.<br>3. Người dùng đang đăng nhập khi bị thay đổi vai trò:<br>Hệ thống thông báo phiên hiện tại sẽ kết thúc và yêu cầu người dùng đăng nhập lại.<br>4. Xung đột cấu hình do nhiều quản trị viên thao tác đồng thời:<br>Hệ thống thông báo và yêu cầu tải lại cấu hình mới nhất trước khi lưu. |

- Đặc tả về tìm kiếm và lọc người dùng

| **Name** | Tìm kiếm và lọc người dùng |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên tra cứu nhanh tài khoản người dùng theo nhiều tiêu chí để phục vụ công tác quản lý, hỗ trợ người dùng và thống kê. |
| **Normal Flow** | 1. Quản trị viên truy cập mục “Quản lý người dùng”.<br>2. Hệ thống hiển thị danh sách người dùng có phân trang.<br>3. Quản trị viên nhập từ khóa (họ tên, email) và/hoặc chọn các tiêu chí lọc:<br>Vai trò, trạng thái tài khoản<br>Khoảng thời gian đăng ký, mức độ hoạt động<br>4. Quản trị viên nhấn nút “Tìm kiếm”.<br>5. Hệ thống chuẩn hóa tham số và truy vấn cơ sở dữ liệu.<br>6. Hệ thống trả về kết quả kèm tổng số bản ghi tìm được.<br>7. Hệ thống hiển thị danh sách gồm: họ tên, email, vai trò, trạng thái, ngày đăng ký và lần đăng nhập gần nhất.<br>8. Quản trị viên có thể sắp xếp theo cột, chuyển trang hoặc chọn một tài khoản để xem chi tiết.<br>9. Quản trị viên có thể xuất danh sách kết quả ra tệp phục vụ báo cáo. |
| **Alternative Flow** | 1. Không có kết quả phù hợp:<br>Hệ thống thông báo và gợi ý điều chỉnh điều kiện lọc.<br>2. Số lượng bản ghi lớn:<br>Hệ thống thực hiện phân trang phía máy chủ để bảo đảm thời gian phản hồi.<br>3. Xuất tệp với số lượng bản ghi vượt ngưỡng cho phép:<br>Hệ thống xử lý bất đồng bộ và gửi thông báo kèm liên kết tải về khi tệp sẵn sàng.<br>4. Lỗi truy vấn dữ liệu:<br>Hệ thống thông báo lỗi và cho phép thử lại. |

d, Thống kê và báo cáo

[Hình ảnh]

- Đặc tả về xem bảng điều khiển thống kê tổng quan

| **Name** | Xem bảng điều khiển thống kê tổng quan |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên theo dõi các chỉ số vận hành tổng quan của hệ thống qua bảng điều khiển trực quan, phục vụ việc đánh giá mức độ sử dụng và ra quyết định quản lý. |
| **Normal Flow** | 1. Quản trị viên đăng nhập và truy cập mục “Thống kê và báo cáo”.<br>2. Hệ thống xác thực quyền truy cập dữ liệu thống kê.<br>3. Hệ thống tổng hợp dữ liệu và hiển thị bảng điều khiển gồm các nhóm chỉ số:<br>Tổng số người dùng theo từng vai trò, số người dùng mới trong kỳ<br>Số lượt truy cập, số lượt làm bài, tỉ lệ hoàn thành bài thi<br>Số khóa học, số đề thi, số câu hỏi đang hoạt động<br>Số lượt gọi dịch vụ AI và chi phí ước tính trong kỳ<br>4. Hệ thống hiển thị biểu đồ xu hướng theo thời gian cho các chỉ số chính.<br>5. Quản trị viên chọn khoảng thời gian cần xem.<br>6. Hệ thống tính toán lại và cập nhật các chỉ số, biểu đồ tương ứng.<br>7. Quản trị viên chọn một chỉ số để xem báo cáo chi tiết. |
| **Alternative Flow** | 1. Chưa đủ dữ liệu trong khoảng thời gian đã chọn:<br>Hệ thống hiển thị biểu đồ trống kèm thông báo cho quản trị viên.<br>2. Khối lượng dữ liệu tổng hợp lớn:<br>Hệ thống sử dụng dữ liệu đã được tổng hợp sẵn theo chu kỳ để bảo đảm hiệu năng.<br>Hệ thống hiển thị rõ thời điểm cập nhật gần nhất của dữ liệu.<br>3. Lỗi trong quá trình tổng hợp dữ liệu:<br>Hệ thống thông báo lỗi, giữ nguyên dữ liệu của lần tổng hợp gần nhất và cho phép làm mới thủ công. |

- Đặc tả về xem báo cáo hoạt động và mức độ sử dụng nội dung

| **Name** | Xem báo cáo hoạt động và mức độ sử dụng nội dung |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên xem các báo cáo chi tiết về hoạt động học tập của học viên và mức độ sử dụng khóa học, đề thi, câu hỏi nhằm đánh giá hiệu quả nội dung của hệ thống. |
| **Normal Flow** | 1. Quản trị viên chọn mục “Báo cáo chi tiết”.<br>2. Hệ thống hiển thị danh mục các loại báo cáo:<br>Báo cáo hoạt động học viên<br>Báo cáo mức độ sử dụng khóa học, đề thi, câu hỏi<br>Báo cáo hiệu quả nội dung do AI sinh<br>3. Quản trị viên chọn loại báo cáo và thiết lập tham số: khoảng thời gian, vai trò, khóa học, kỹ năng.<br>4. Quản trị viên nhấn nút “Xem báo cáo”.<br>5. Hệ thống truy vấn và tổng hợp dữ liệu theo các tham số đã chọn.<br>6. Hệ thống hiển thị báo cáo dạng bảng kèm biểu đồ minh họa.<br>7. Quản trị viên có thể lọc sâu theo từng dòng dữ liệu để xem chi tiết.<br>8. Hệ thống ghi nhận lượt truy xuất báo cáo vào nhật ký hệ thống. |
| **Alternative Flow** | 1. Tham số báo cáo không hợp lệ (ngày bắt đầu sau ngày kết thúc):<br>Hệ thống thông báo lỗi và yêu cầu chọn lại khoảng thời gian.<br>2. Khoảng thời gian vượt quá ngưỡng cho phép:<br>Hệ thống cảnh báo và đề nghị thu hẹp khoảng thời gian truy vấn.<br>3. Không có dữ liệu phù hợp điều kiện lọc:<br>Hệ thống hiển thị báo cáo trống kèm thông báo.<br>4. Thời gian tổng hợp dữ liệu kéo dài:<br>Hệ thống xử lý bất đồng bộ và thông báo cho quản trị viên khi báo cáo sẵn sàng. |

- Đặc tả về xuất báo cáo

| **Name** | Xuất báo cáo |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên kết xuất dữ liệu báo cáo ra tệp Excel, PDF hoặc CSV để lưu trữ và sử dụng ngoài hệ thống, đồng thời bảo đảm tuân thủ quy định về bảo vệ dữ liệu cá nhân. |
| **Normal Flow** | 1. Từ màn hình báo cáo, quản trị viên nhấn nút “Xuất báo cáo”.<br>2. Hệ thống hiển thị tùy chọn định dạng xuất: Excel, PDF hoặc CSV.<br>3. Quản trị viên chọn định dạng và phạm vi dữ liệu cần xuất.<br>4. Hệ thống kiểm tra quyền xuất dữ liệu của quản trị viên.<br>5. Hệ thống kiểm tra khối lượng dữ liệu cần kết xuất.<br>6. Hệ thống tạo tệp báo cáo theo định dạng đã chọn, loại bỏ hoặc ẩn danh hóa các trường dữ liệu cá nhân nhạy cảm theo chính sách bảo vệ dữ liệu.<br>7. Hệ thống cung cấp liên kết tải tệp cho quản trị viên.<br>8. Hệ thống ghi nhật ký thao tác xuất dữ liệu kèm người thực hiện, thời điểm và phạm vi dữ liệu. |
| **Alternative Flow** | 1. Khối lượng dữ liệu lớn:<br>Hệ thống xử lý bất đồng bộ qua hàng đợi và gửi thông báo kèm liên kết tải về khi hoàn tất.<br>2. Quản trị viên không có quyền xuất dữ liệu cá nhân:<br>Hệ thống chỉ xuất dữ liệu ở dạng tổng hợp và thông báo rõ phạm vi bị giới hạn.<br>3. Lỗi trong quá trình tạo tệp:<br>Hệ thống ghi log lỗi, thông báo cho quản trị viên và cho phép thử lại.<br>4. Liên kết tải tệp đã hết hạn:<br>Hệ thống thông báo và cho phép tạo lại yêu cầu xuất báo cáo. |

e, Cấu hình hệ thống và nhật ký kiểm toán

[Hình ảnh]

- Đặc tả về cấu hình tham số hệ thống

| **Name** | Cấu hình tham số hệ thống |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên thiết lập các tham số vận hành của hệ thống về bảo mật, học tập, nội dung và thông báo mà không cần can thiệp vào mã nguồn. |
| **Normal Flow** | 1. Quản trị viên chọn mục “Cấu hình hệ thống”.<br>2. Hệ thống hiển thị các nhóm tham số:<br>Chính sách bảo mật: độ dài mật khẩu, thời hạn token, số lần đăng nhập sai tối đa<br>Tham số học tập: số lần làm bài tối đa, thời gian lưu bài làm dở dang<br>Tham số nội dung: dung lượng tệp tải lên, số thẻ tối đa của một bộ flashcard<br>Cấu hình email và thông báo hệ thống<br>3. Quản trị viên chọn nhóm tham số cần điều chỉnh.<br>4. Hệ thống hiển thị giá trị hiện tại kèm mô tả và khoảng giá trị hợp lệ của từng tham số.<br>5. Quản trị viên nhập giá trị mới và nhấn “Lưu cấu hình”.<br>6. Hệ thống kiểm tra giá trị nằm trong khoảng cho phép và không mâu thuẫn với các tham số khác.<br>7. Hệ thống lưu cấu hình và áp dụng cho các phiên làm việc mới.<br>8. Hệ thống ghi nhật ký thay đổi cấu hình kèm giá trị cũ và giá trị mới.<br>9. Hệ thống hiển thị thông báo cập nhật thành công. |
| **Alternative Flow** | 1. Giá trị nhập nằm ngoài khoảng cho phép:<br>Hệ thống thông báo lỗi kèm khoảng giá trị hợp lệ và không lưu cấu hình.<br>2. Thay đổi làm giảm mức độ an toàn xuống dưới ngưỡng tối thiểu:<br>Hệ thống từ chối lưu và hiển thị cảnh báo về rủi ro bảo mật.<br>3. Tham số thay đổi ảnh hưởng đến các phiên đang hoạt động:<br>Hệ thống cảnh báo phạm vi ảnh hưởng và yêu cầu quản trị viên xác nhận.<br>4. Quản trị viên chọn “Khôi phục mặc định”:<br>Hệ thống hiển thị hộp thoại xác nhận trước khi đặt lại toàn bộ tham số của nhóm về giá trị mặc định.<br>5. Lỗi khi lưu cấu hình:<br>Hệ thống giữ nguyên cấu hình cũ và thông báo lỗi. |

- Đặc tả về xem và tra cứu nhật ký kiểm toán

| **Name** | Xem và tra cứu nhật ký kiểm toán |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên tra cứu toàn bộ nhật ký thao tác của người dùng trên hệ thống nhằm phục vụ công tác kiểm tra, truy vết sự cố và tuân thủ quy định về bảo vệ dữ liệu cá nhân. |
| **Normal Flow** | 1. Quản trị viên chọn mục “Nhật ký hệ thống”.<br>2. Hệ thống kiểm tra quyền truy cập nhật ký kiểm toán.<br>3. Hệ thống hiển thị danh sách bản ghi nhật ký gần nhất, sắp xếp theo thời gian giảm dần.<br>4. Quản trị viên thiết lập điều kiện tra cứu:<br>Khoảng thời gian, người thực hiện<br>Loại thao tác, module, mức độ nghiêm trọng<br>5. Quản trị viên nhấn nút “Tra cứu”.<br>6. Hệ thống truy vấn và trả về kết quả kèm tổng số bản ghi.<br>7. Hệ thống hiển thị từng bản ghi gồm: thời điểm, người thực hiện, vai trò, hành động, đối tượng tác động, địa chỉ IP và kết quả thao tác.<br>8. Quản trị viên chọn một bản ghi để xem chi tiết dữ liệu trước và sau khi thay đổi.<br>9. Quản trị viên có thể kết xuất kết quả tra cứu phục vụ công tác kiểm tra. |
| **Alternative Flow** | 1. Không có bản ghi phù hợp điều kiện tra cứu:<br>Hệ thống thông báo và gợi ý mở rộng khoảng thời gian tra cứu.<br>2. Khoảng thời gian vượt quá thời hạn lưu trữ nhật ký:<br>Hệ thống thông báo thời hạn lưu trữ đang áp dụng và chỉ trả về dữ liệu còn được lưu.<br>3. Quản trị viên yêu cầu sửa hoặc xóa bản ghi nhật ký:<br>Hệ thống từ chối thao tác; nhật ký kiểm toán chỉ được phép đọc nhằm bảo đảm tính toàn vẹn dữ liệu.<br>4. Số lượng bản ghi trả về quá lớn:<br>Hệ thống phân trang và giới hạn phạm vi truy vấn của mỗi lần tra cứu. |

- Đặc tả về quản lý sao lưu và phục hồi dữ liệu

| **Name** | Quản lý sao lưu và phục hồi dữ liệu |
|---|---|
| **Actor** | Admin (Quản trị viên) |
| **Description** | Cho phép quản trị viên thiết lập lịch sao lưu tự động, tạo bản sao lưu thủ công và thực hiện phục hồi dữ liệu khi xảy ra sự cố, bảo đảm tính sẵn sàng và an toàn dữ liệu của hệ thống. |
| **Normal Flow** | 1. Quản trị viên chọn mục “Sao lưu và phục hồi”.<br>2. Hệ thống hiển thị lịch sao lưu tự động và danh sách bản sao lưu hiện có kèm thời điểm, dung lượng, trạng thái.<br>3. Quản trị viên chọn thao tác: cấu hình lịch sao lưu, tạo bản sao lưu thủ công hoặc phục hồi dữ liệu.<br>4. Trường hợp cấu hình lịch: quản trị viên thiết lập chu kỳ, thời điểm thực hiện và số bản sao lưu được giữ lại; hệ thống lưu cấu hình.<br>5. Trường hợp sao lưu thủ công: quản trị viên nhấn “Tạo bản sao lưu”; hệ thống thực hiện sao lưu bất đồng bộ và hiển thị tiến trình.<br>6. Hệ thống kiểm tra tính toàn vẹn của bản sao lưu sau khi hoàn tất.<br>7. Trường hợp phục hồi: quản trị viên chọn bản sao lưu và nhấn “Phục hồi”.<br>8. Hệ thống hiển thị cảnh báo về phạm vi ảnh hưởng và yêu cầu xác nhận hai bước.<br>9. Sau khi xác nhận, hệ thống chuyển sang chế độ bảo trì và thực hiện phục hồi dữ liệu.<br>10. Hệ thống kiểm tra kết quả phục hồi, thoát chế độ bảo trì và thông báo hoàn tất.<br>11. Hệ thống ghi nhật ký toàn bộ thao tác sao lưu và phục hồi. |
| **Alternative Flow** | 1. Dung lượng lưu trữ không đủ:<br>Hệ thống thông báo và đề nghị xóa bớt các bản sao lưu cũ.<br>2. Bản sao lưu bị lỗi hoặc không toàn vẹn:<br>Hệ thống đánh dấu bản sao lưu không hợp lệ và không cho phép sử dụng để phục hồi.<br>3. Quá trình sao lưu thất bại:<br>Hệ thống ghi log lỗi, gửi cảnh báo cho quản trị viên và giữ nguyên các bản sao lưu trước đó.<br>4. Có người dùng đang hoạt động khi thực hiện phục hồi:<br>Hệ thống thông báo trước, kết thúc các phiên đang hoạt động và hiển thị trang bảo trì.<br>5. Quá trình phục hồi thất bại:<br>Hệ thống giữ nguyên trạng thái dữ liệu trước khi phục hồi và thông báo lỗi chi tiết. |

**II. Sơ đồ Sequence**

**1. Student**

a, Đăng ký

[Hình ảnh]

b, Đăng nhập

[Hình ảnh]

c, Xem chi tiết và đăng ký tham gia khóa học

[Hình ảnh]

d, Làm bài thi thử / luyện tập theo Part

[Hình ảnh]

e, Khôi phục bài làm bị gián đoạn

[Hình ảnh]

f, Xem kết quả và giải thích đáp án

[Hình ảnh]

g, Xem lịch sử làm bài và biểu đồ tiến độ

[Hình ảnh]

**2. Teacher**

a, Đăng ký

[Hình ảnh]

b, Cập nhật, xóa và xuất bản khóa học

[Hình ảnh]

c, Thêm câu hỏi vào ngân hàng câu hỏi

[Hình ảnh]

d, Cập nhật và xóa câu hỏi

[Hình ảnh]

e, Nhập câu hỏi hàng loạt từ tệp

[Hình ảnh]

f, Tạo đề thi và soạn đề từ ngân hàng câu hỏi

[Hình ảnh]

g, Xuất bản và gỡ xuất bản đề thi

[Hình ảnh]

**3. Admin**

a, Tạo và cập nhật tài khoản người dùng

[Hình ảnh]

b, Khóa và mở khóa tài khoản

[Hình ảnh]

c, Gán vai trò và cấu hình quyền

[Hình ảnh]

d, Xem bảng điều khiển thống kê tổng quan

[Hình ảnh]

e, Xuất báo cáo

[Hình ảnh]

f, Quản lý sao lưu và phục hồi dữ liệu

[Hình ảnh]

**III. Sơ đồ Activity**

**1. Student**

a, Đăng ký

[Hình ảnh]

b, Đăng nhập

[Hình ảnh]

c, Quên và đặt lại mật khẩu

[Hình ảnh]

d, Đăng ký tham gia khóa học

[Hình ảnh]

e, Làm bài thi

[Hình ảnh]

f, Khôi phục bài làm

[Hình ảnh]

g, Xem kết quả và tiến độ

[Hình ảnh]

h, Ôn tập flashcard

[Hình ảnh]

**2. Teacher**

a, Đăng ký

[Hình ảnh]

b, Tạo khóa học

[Hình ảnh]

c, Cập nhât, xóa và xuất bản khóa học

[Hình ảnh]

d, Tổ chức nội dung khóa học

[Hình ảnh]

e, Thêm câu hỏi

[Hình ảnh]

f, Cập nhật và xóa câu hỏi

[Hình ảnh]

g, Nhập câu hỏi từ tệp

[Hình ảnh]

h, Tạo đề thi

[Hình ảnh]

i, Xuất bản đề thi

[Hình ảnh]

**3. Admin**

a, Tạo và cập nhật tài khoản

[Hình ảnh]

b, Khóa và mở khóa tài khoản

[Hình ảnh]

c, Gán vai trò và phân quyền

[Hình ảnh]

d, Xuất báo cáo

[Hình ảnh]

e, Sao lưu và phục hồi

[Hình ảnh]

**Chương 3: Thiết kế hệ thống**

**I. Sơ đồ class**

**[Hình ảnh]**
