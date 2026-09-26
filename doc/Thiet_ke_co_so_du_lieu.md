# THIẾT KẾ CƠ SỞ DỮ LIỆU

Phần này trình bày thiết kế cơ sở dữ liệu của hệ thống, được xây dựng trực tiếp từ sơ đồ lớp ở mục I. Mỗi lớp thực thể (Entity Layer) được chuyển thành bảng, các quan hệ kết hợp, hợp thành và tổng quát hóa được chuyển thành khóa ngoại hoặc bảng trung gian theo các quy tắc ánh xạ nêu tại mục 3. Kết quả gồm 26 bảng, 16 kiểu liệt kê và 31 chỉ mục bổ sung. Toàn bộ lược đồ đã được cài đặt và kiểm thử trên PostgreSQL 16; script tạo cơ sở dữ liệu được đính kèm trong tệp database_schema.sql.

## 1. Lựa chọn hệ quản trị cơ sở dữ liệu

Theo kết quả khảo sát công nghệ, hệ thống sử dụng **PostgreSQL 16** làm hệ quản trị cơ sở dữ liệu quan hệ, kết hợp **Prisma ORM** để ánh xạ mô hình dữ liệu vào mã nguồn NestJS và quản lý migration. Các tính năng của PostgreSQL được khai thác trong thiết kế gồm:

- **Kiểu liệt kê (ENUM)** để giới hạn miền giá trị của các thuộc tính trạng thái, tương ứng một – một với các lớp «enumeration» trong sơ đồ lớp.
- **Ràng buộc CHECK** để kiểm tra miền giá trị của từng cột và mối quan hệ giữa các cột trong cùng một dòng (ví dụ bài đã chấm phải có điểm).
- **Chỉ mục một phần (partial index)** để cài đặt các quy tắc nghiệp vụ có điều kiện, ví dụ mỗi học viên chỉ có một bài làm dở với mỗi đề thi.
- **Extension pg_trgm với chỉ mục GIN** phục vụ tìm kiếm khóa học, đề thi, câu hỏi theo từ khóa và phát hiện câu hỏi trùng lặp.
- **Trigger** tự động cập nhật cột updated_at mỗi khi bản ghi thay đổi.

Bên cạnh PostgreSQL, **Redis** được dùng để lưu các dữ liệu tạm thời có thời hạn sống như refresh token, token xác thực và bộ đếm đăng nhập sai (trình bày tại mục 11).

## 2. Nguyên tắc và quy ước thiết kế

Để lược đồ nhất quán và dễ bảo trì, thiết kế tuân theo các quy ước đặt tên và quy ước kỹ thuật sau:

***Bảng 3.1.*** *Quy ước đặt tên và quy ước kỹ thuật*

| **Nội dung** | **Quy ước áp dụng** |
|---|---|
| Tên bảng | snake_case, danh từ số nhiều. Ví dụ: lớp ExamAttempt → bảng exam_attempts |
| Tên cột | snake_case, chuyển từ tên thuộc tính camelCase. Ví dụ: orderIndex → order_index |
| Khóa chính | Cột id kiểu UUID, giá trị mặc định gen_random_uuid(). Bảng con của kế thừa dùng user_id; bảng trung gian dùng khóa chính kép |
| Khóa ngoại | Đặt tên <thực thể>_id, ví dụ course_id; riêng các vai trò đặc thù đặt theo ngữ nghĩa: teacher_id, author_id, owner_id |
| Tên ràng buộc | pk_<bảng>, fk_<bảng>_<cột>, uq_<bảng>_<cột>, ck_<bảng>_<nội dung>, ix_<bảng>_<cột> |
| Tính tùy chọn | Thuộc tính có bội số [0..1] trong sơ đồ lớp → cột cho phép NULL; các thuộc tính còn lại → NOT NULL |
| Thời gian | Kiểu TIMESTAMPTZ, lưu theo UTC; giao diện chuyển sang múi giờ Việt Nam khi hiển thị |
| Cột kỹ thuật | created_at, updated_at cho các bảng dữ liệu chính; deleted_at (xóa mềm) cho courses, questions, exams |

Khóa chính dùng UUID thay vì số tự tăng vì thuộc tính id trong sơ đồ lớp có kiểu String, đồng thời UUID không để lộ số lượng bản ghi và không cho phép dò tìm tài nguyên qua API bằng cách tăng dần mã. Các kiểu dữ liệu trong sơ đồ lớp được chuyển sang kiểu của PostgreSQL như sau:

***Bảng 3.2.*** *Ánh xạ kiểu dữ liệu từ sơ đồ lớp sang PostgreSQL*

| **Kiểu trong sơ đồ lớp** | **Kiểu PostgreSQL** | **Ghi chú** |
|---|---|---|
| String (id) | UUID | Sinh tự động bằng gen_random_uuid() |
| String (ngắn) | VARCHAR(n) | Mã, tên, tiêu đề, email, URL; n chọn theo nghiệp vụ |
| String (dài) | TEXT | Nội dung câu hỏi, bài giảng, giải thích, mô tả |
| Int | INTEGER / SMALLINT | SMALLINT cho giá trị nhỏ như part, order_index của phương án |
| Float | NUMERIC(p,s) | Điểm số và phần trăm cần chính xác tuyệt đối, không dùng số thực dấu phẩy động |
| Boolean | BOOLEAN | Luôn có giá trị mặc định |
| DateTime | TIMESTAMPTZ | Có múi giờ |
| «enumeration» | ENUM | Mỗi lớp liệt kê là một kiểu ENUM (mục 7) |
| String[] (đa trị) | Bảng riêng | Tách bảng để đạt dạng chuẩn 1 (mục 3.4) |

## 3. Chuyển đổi sơ đồ lớp sang mô hình quan hệ

### 3.1. Quy tắc ánh xạ

***Bảng 3.3.*** *Quy tắc chuyển các thành phần của sơ đồ lớp*

| **Thành phần sơ đồ lớp** | **Cách chuyển sang mô hình quan hệ** |
|---|---|
| Lớp thực thể | Một bảng; mỗi thuộc tính là một cột |
| Kết hợp một – nhiều | Khóa ngoại đặt ở bảng phía "nhiều"; NOT NULL khi bội số phía "một" là 1, cho phép NULL khi là 0..1 |
| Kết hợp nhiều – nhiều | Bảng trung gian chứa hai khóa ngoại, khóa chính kép từ hai khóa ngoại đó |
| Hợp thành (◆) | Khóa ngoại NOT NULL ở bảng thành phần, ON DELETE CASCADE: xóa tổng thể thì xóa luôn thành phần |
| Kết hợp thường | Khóa ngoại ON DELETE RESTRICT: không cho xóa bản ghi đang được tham chiếu |
| Tổng quát hóa | Mỗi lớp một bảng; bảng lớp con có khóa chính đồng thời là khóa ngoại tới bảng lớp cha (mục 3.2) |
| Thuộc tính đa trị | Bảng riêng liên kết với bảng chủ |
| Lớp «enumeration» | Kiểu ENUM của PostgreSQL |
| Phương thức | Không ánh xạ; ngoại lệ là phương thức cần lưu trạng thái mà lớp chưa có thuộc tính tương ứng (mục 3.4) |
| Bội số tối thiểu 1..* | Khóa ngoại không biểu diễn được; kiểm tra ở tầng Service khi xuất bản (mục 8.3) |

### 3.2. Ánh xạ quan hệ tổng quát hóa User

Lớp trừu tượng User có ba lớp con Student, Teacher và Admin. Có ba chiến lược ánh xạ kế thừa phổ biến:

***Bảng 3.4.*** *So sánh các chiến lược ánh xạ kế thừa*

| **Chiến lược** | **Ưu điểm** | **Nhược điểm** |
|---|---|---|
| Một bảng cho cả cây kế thừa (Single Table) | Truy vấn đơn giản, không cần phép nối | Nhiều cột NULL (thuộc tính của giáo viên vô nghĩa với học viên); không đặt được NOT NULL cho thuộc tính riêng của lớp con |
| Mỗi lớp cụ thể một bảng (Concrete Table) | Không có cột NULL thừa | Lặp các cột chung ở ba bảng; email không thể UNIQUE trên toàn hệ thống; đăng nhập phải truy vấn ba bảng |
| Mỗi lớp một bảng (Class Table) — được chọn | Không lặp dữ liệu, không cột NULL thừa; khóa ngoại trỏ đúng loại người dùng | Cần phép nối khi đọc thông tin đầy đủ của một loại người dùng |

Thiết kế chọn chiến lược **mỗi lớp một bảng**: bảng users chứa các thuộc tính chung và là nơi duy nhất phục vụ đăng nhập; các bảng students, teachers, admins có khóa chính user_id đồng thời là khóa ngoại tới users.id với quan hệ một – một. Lợi ích quan trọng là các khóa ngoại trỏ đúng loại người dùng: courses.teacher_id, questions.author_id, exams.author_id tham chiếu tới teachers, còn enrollments.student_id, exam_attempts.student_id tham chiếu tới students. Nhờ vậy cơ sở dữ liệu tự bảo đảm chỉ giáo viên mới có thể là người phụ trách khóa học hoặc tác giả câu hỏi, và chỉ học viên mới có thể ghi danh hay làm bài.

Loại người dùng được xác định bởi vai trò (users.role_id). Khi tạo tài khoản, UserService ghi đồng thời bản ghi trong users và bản ghi trong bảng lớp con tương ứng trong cùng một giao dịch.

### 3.3. Ánh xạ các quan hệ trong sơ đồ lớp

***Bảng 3.5.*** *Ánh xạ các quan hệ của sơ đồ lớp sang khóa ngoại và bảng trung gian*

| **Quan hệ trong sơ đồ lớp** | **Loại** | **Cài đặt trong CSDL** |
|---|---|---|
| Permission 0..* — 0..* Role | Nhiều – nhiều | Bảng trung gian role_permissions, khóa chính (role_id, permission_id) |
| Role 1 — 0..* User | Một – nhiều | users.role_id NOT NULL → roles.id |
| User ◁— Student / Teacher / Admin | Tổng quát hóa | students.user_id, teachers.user_id, admins.user_id vừa là khóa chính vừa là khóa ngoại → users.id |
| Subject 1 ◆— 0..* Topic | Hợp thành | topics.subject_id → subjects.id, ON DELETE CASCADE |
| Subject 1 — 0..* Course | Một – nhiều | courses.subject_id → subjects.id |
| Teacher 1 — 0..* Course (manages) | Một – nhiều | courses.teacher_id → teachers.user_id |
| Course 1 ◆— 1..* Chapter | Hợp thành | chapters.course_id → courses.id, ON DELETE CASCADE |
| Chapter 1 ◆— 1..* Lesson | Hợp thành | lessons.chapter_id → chapters.id, ON DELETE CASCADE |
| Course 0..* — 0..* Exam (assigned to) | Nhiều – nhiều | Bảng trung gian course_exams, khóa chính (course_id, exam_id) |
| Student 1 — 0..* Enrollment | Một – nhiều | enrollments.student_id → students.user_id |
| Course 1 — 0..* Enrollment | Một – nhiều | enrollments.course_id → courses.id; UNIQUE (student_id, course_id) |
| Enrollment.completeLesson(lesson) | Nhiều – nhiều (ẩn) | Bảng lesson_completions, khóa chính (enrollment_id, lesson_id) |
| Topic 1 — 0..* Question | Một – nhiều | questions.topic_id → topics.id |
| Teacher 1 — 0..* Question (creates) | Một – nhiều | questions.author_id → teachers.user_id |
| Question 1 ◆— 1..* QuestionOption | Hợp thành | question_options.question_id → questions.id, ON DELETE CASCADE |
| Teacher 1 — 0..* Exam (creates) | Một – nhiều | exams.author_id → teachers.user_id |
| Exam 1 ◆— 1..* ExamSection | Hợp thành | exam_sections.exam_id → exams.id, ON DELETE CASCADE |
| ExamSection 1 ◆— 1..* ExamQuestion | Hợp thành | exam_questions.section_id → exam_sections.id, ON DELETE CASCADE |
| Question 1 — 0..* ExamQuestion (used in) | Một – nhiều | exam_questions.question_id → questions.id, ON DELETE RESTRICT |
| Student 1 — 0..* ExamAttempt (takes) | Một – nhiều | exam_attempts.student_id → students.user_id |
| Exam 1 — 0..* ExamAttempt | Một – nhiều | exam_attempts.exam_id → exams.id |
| ExamAttempt 1 ◆— 0..* AttemptAnswer | Hợp thành | attempt_answers.attempt_id → exam_attempts.id, ON DELETE CASCADE |
| ExamQuestion 1 — 0..* AttemptAnswer (answered by) | Một – nhiều | attempt_answers.exam_question_id → exam_questions.id, ON DELETE RESTRICT |
| AttemptAnswer.selectedOptionIds : String[] | Thuộc tính đa trị | Bảng attempt_answer_options, khóa chính (attempt_answer_id, option_id) |
| User 1 — 0..* FlashcardDeck (owns) | Một – nhiều | flashcard_decks.owner_id → users.id |
| FlashcardDeck 1 ◆— 0..* Flashcard | Hợp thành | flashcards.deck_id → flashcard_decks.id, ON DELETE CASCADE |
| Flashcard 1 ◆— 0..* FlashcardProgress | Hợp thành | flashcard_progresses.flashcard_id → flashcards.id, ON DELETE CASCADE |
| Student 1 — 0..* FlashcardProgress (reviews) | Một – nhiều | flashcard_progresses.student_id → students.user_id; UNIQUE (student_id, flashcard_id) |

### 3.4. Các thành phần phát sinh khi chuyển đổi

Ngoài các bảng tương ứng trực tiếp với lớp thực thể, quá trình chuyển đổi phát sinh thêm các thành phần sau:

- **role_permissions** và **course_exams**: bảng trung gian của hai quan hệ nhiều – nhiều Role – Permission và Course – Exam.
- **attempt_answer_options**: tách từ thuộc tính đa trị selectedOptionIds : String[] của AttemptAnswer. Nếu lưu dạng mảng, cột vi phạm dạng chuẩn 1 và không thể đặt khóa ngoại tới question_options, dẫn tới nguy cơ lưu mã phương án không tồn tại.
- **lesson_completions**: phương thức Enrollment.completeLesson(lesson) và updateProgress() cần biết học viên đã hoàn thành những bài học nào, nhưng lớp Enrollment chỉ có thuộc tính progressPercent. Bảng này lưu quan hệ nhiều – nhiều ẩn giữa Enrollment và Lesson. Để sơ đồ lớp và CSDL nhất quán, nên bổ sung vào sơ đồ lớp quan hệ Enrollment "0..*" — "0..*" Lesson : completes.
- **Cột kỹ thuật**: created_at, updated_at trên các bảng dữ liệu chính phục vụ truy vết và sắp xếp; deleted_at trên courses, questions, exams phục vụ xóa mềm theo yêu cầu của các use case Cập nhật, xóa và xuất bản khóa học và Cập nhật và xóa câu hỏi.

## 4. Sơ đồ quan hệ thực thể (ERD)

Sơ đồ quan hệ thực thể được vẽ theo ký pháp chân quạ (Crow's Foot). Ký hiệu || nghĩa là đúng một, o| là không hoặc một, o{ là không hoặc nhiều. Trong mỗi thực thể, phần trên đường kẻ là khóa chính, dấu * đánh dấu cột NOT NULL; nhãn «PK», «FK» chỉ khóa chính và khóa ngoại. Do số lượng bảng lớn, sơ đồ tổng thể chỉ hiển thị các cột khóa; chi tiết từng nhóm được trình bày trong các sơ đồ tiếp theo, trong đó các bảng màu xám là bảng thuộc nhóm khác được tham chiếu tới.

[Hình ảnh]

***Hình 3.1.*** *Sơ đồ ERD tổng thể của hệ thống*

[Hình ảnh]

***Hình 3.2.*** *Sơ đồ ERD nhóm Người dùng và phân quyền*

[Hình ảnh]

***Hình 3.3.*** *Sơ đồ ERD nhóm Môn học và khóa học*

[Hình ảnh]

***Hình 3.4.*** *Sơ đồ ERD nhóm Ngân hàng câu hỏi, đề thi và làm bài*

[Hình ảnh]

***Hình 3.5.*** *Sơ đồ ERD nhóm Flashcard*

## 5. Danh sách các bảng

***Bảng 3.6.*** *Danh sách các bảng trong cơ sở dữ liệu*

| **STT** | **Tên bảng** | **Lớp tương ứng** | **Nhóm chức năng** |
|---|---|---|---|
| 1 | roles | Role | Người dùng và phân quyền |
| 2 | permissions | Permission | Người dùng và phân quyền |
| 3 | role_permissions | (Role – Permission) | Người dùng và phân quyền |
| 4 | users | User | Người dùng và phân quyền |
| 5 | students | Student | Người dùng và phân quyền |
| 6 | teachers | Teacher | Người dùng và phân quyền |
| 7 | admins | Admin | Người dùng và phân quyền |
| 8 | subjects | Subject | Môn học và khóa học |
| 9 | topics | Topic | Môn học và khóa học |
| 10 | courses | Course | Môn học và khóa học |
| 11 | chapters | Chapter | Môn học và khóa học |
| 12 | lessons | Lesson | Môn học và khóa học |
| 13 | course_exams | (Course – Exam) | Môn học và khóa học |
| 14 | enrollments | Enrollment | Môn học và khóa học |
| 15 | lesson_completions | (Enrollment.completeLesson) | Môn học và khóa học |
| 16 | questions | Question | Ngân hàng câu hỏi, đề thi và làm bài |
| 17 | question_options | QuestionOption | Ngân hàng câu hỏi, đề thi và làm bài |
| 18 | exams | Exam | Ngân hàng câu hỏi, đề thi và làm bài |
| 19 | exam_sections | ExamSection | Ngân hàng câu hỏi, đề thi và làm bài |
| 20 | exam_questions | ExamQuestion | Ngân hàng câu hỏi, đề thi và làm bài |
| 21 | exam_attempts | ExamAttempt | Ngân hàng câu hỏi, đề thi và làm bài |
| 22 | attempt_answers | AttemptAnswer | Ngân hàng câu hỏi, đề thi và làm bài |
| 23 | attempt_answer_options | (AttemptAnswer.selectedOptionIds) | Ngân hàng câu hỏi, đề thi và làm bài |
| 24 | flashcard_decks | FlashcardDeck | Flashcard |
| 25 | flashcards | Flashcard | Flashcard |
| 26 | flashcard_progresses | FlashcardProgress | Flashcard |

Các tên lớp đặt trong ngoặc đơn là bảng phát sinh khi chuyển đổi, không có lớp thực thể tương ứng trực tiếp (xem mục 3.4).

## 6. Đặc tả chi tiết các bảng

Mỗi bảng được đặc tả gồm: tên cột, kiểu dữ liệu, khả năng nhận giá trị NULL, ràng buộc và ý nghĩa. Cột Ràng buộc liệt kê khóa chính (PK), khóa ngoại (FK) kèm hành vi khi xóa nếu khác RESTRICT, ràng buộc duy nhất (UNIQUE), giá trị mặc định (DEFAULT) và ràng buộc kiểm tra (CHECK).

### 6.1. Nhóm người dùng và phân quyền

**Bảng roles**  *(lớp Role)*

Lưu các vai trò trong hệ thống (Student, Teacher, Admin) phục vụ cơ chế phân quyền RBAC.

***Bảng 3.7.*** *Đặc tả bảng roles*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã vai trò |
| code | VARCHAR(50) | Không | UNIQUE | Mã định danh vai trò, ví dụ STUDENT, TEACHER, ADMIN |
| name | VARCHAR(100) | Không |  | Tên hiển thị của vai trò |
| description | TEXT | Có |  | Mô tả vai trò |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |

**Bảng permissions**  *(lớp Permission)*

Lưu danh mục quyền; mỗi quyền là một cặp phân hệ (module) và hành động (action).

***Bảng 3.8.*** *Đặc tả bảng permissions*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã quyền |
| code | VARCHAR(100) | Không | UNIQUE | Mã quyền dạng module:action, ví dụ question:create |
| module | VARCHAR(50) | Không | UNIQUE (module, action) | Phân hệ chức năng mà quyền áp dụng |
| action | permission_action | Không | UNIQUE (module, action) | Hành động được phép |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |

**Bảng role_permissions**  *(lớp (Role – Permission))*

Bảng trung gian thể hiện quan hệ nhiều – nhiều giữa Role và Permission.

***Bảng 3.9.*** *Đặc tả bảng role_permissions*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| role_id | UUID | Không | PK; FK → roles(id), ON DELETE CASCADE | Vai trò được cấp quyền |
| permission_id | UUID | Không | PK; FK → permissions(id), ON DELETE CASCADE | Quyền được cấp |
| granted_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cấp quyền |

**Bảng users**  *(lớp User)*

Lưu thông tin chung của mọi người dùng (lớp trừu tượng User). Thông tin riêng của từng loại người dùng nằm ở bảng students, teachers, admins.

***Bảng 3.10.*** *Đặc tả bảng users*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã người dùng |
| role_id | UUID | Không | FK → roles(id) | Vai trò của người dùng (Role 1 – 0..* User) |
| email | VARCHAR(255) | Không | UNIQUE theo lower(email); CHECK định dạng | Email đăng nhập |
| password_hash | VARCHAR(255) | Không |  | Chuỗi băm bcrypt của mật khẩu |
| full_name | VARCHAR(100) | Không | CHECK (length(trim(full_​name)) > 0) | Họ và tên |
| avatar_url | VARCHAR(500) | Có |  | Đường dẫn ảnh đại diện |
| status | user_status | Không | DEFAULT 'PENDING_​VERIFICATION' | Trạng thái tài khoản |
| email_verified_at | TIMESTAMPTZ | Có |  | Thời điểm xác thực email |
| last_login_at | TIMESTAMPTZ | Có |  | Thời điểm đăng nhập gần nhất |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |

**Bảng students**  *(lớp Student)*

Lưu thông tin riêng của học viên; khóa chính đồng thời là khóa ngoại tới users (kế thừa).

***Bảng 3.11.*** *Đặc tả bảng students*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| user_id | UUID | Không | PK; FK → users(id), ON DELETE CASCADE | Mã người dùng (kế thừa từ users) |
| current_level | level | Có |  | Trình độ hiện tại |
| current_score | INTEGER | Có | CHECK (current_​score BETWEEN 0 AND 990) | Điểm hiện tại (thang TOEIC) |
| target_score | INTEGER | Có | CHECK (target_​score BETWEEN 0 AND 990) | Điểm mục tiêu (thang TOEIC) |

**Bảng teachers**  *(lớp Teacher)*

Lưu thông tin riêng của giáo viên và trạng thái phê duyệt hồ sơ.

***Bảng 3.12.*** *Đặc tả bảng teachers*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| user_id | UUID | Không | PK; FK → users(id), ON DELETE CASCADE | Mã người dùng (kế thừa từ users) |
| organization | VARCHAR(255) | Không |  | Đơn vị công tác |
| specialization | VARCHAR(255) | Không |  | Chuyên môn giảng dạy |
| is_approved | BOOLEAN | Không | DEFAULT false | Hồ sơ đã được quản trị viên phê duyệt hay chưa |

**Bảng admins**  *(lớp Admin)*

Lưu thông tin riêng của quản trị viên.

***Bảng 3.13.*** *Đặc tả bảng admins*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| user_id | UUID | Không | PK; FK → users(id), ON DELETE CASCADE | Mã người dùng (kế thừa từ users) |
| phone | VARCHAR(20) | Không | CHECK (phone ~ '^\+?[0-9]{9,15}$') | Số điện thoại liên hệ |
| two_factor_enabled | BOOLEAN | Không | DEFAULT false | Bật xác thực hai lớp hay không |

### 6.2. Nhóm môn học và khóa học

**Bảng subjects**  *(lớp Subject)*

Lưu các môn học / nhóm nội dung: TOEIC và các môn chuyên ngành CNTT.

***Bảng 3.14.*** *Đặc tả bảng subjects*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã môn học |
| code | VARCHAR(50) | Không | UNIQUE | Mã môn học, ví dụ TOEIC, CSDL |
| name | VARCHAR(150) | Không |  | Tên môn học |
| category | subject_category | Không |  | Nhóm môn học |
| is_active | BOOLEAN | Không | DEFAULT true | Môn học còn được sử dụng hay không |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |

**Bảng topics**  *(lớp Topic)*

Lưu các chủ đề thuộc một môn học; là tiêu chí phân loại câu hỏi và đơn vị thống kê điểm mạnh, điểm yếu.

***Bảng 3.15.*** *Đặc tả bảng topics*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã chủ đề |
| subject_id | UUID | Không | FK → subjects(id), ON DELETE CASCADE; UNIQUE (subject_​id, name) | Môn học chứa chủ đề (hợp thành) |
| name | VARCHAR(150) | Không | UNIQUE (subject_​id, name) | Tên chủ đề |
| description | TEXT | Có |  | Mô tả chủ đề |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |

**Bảng courses**  *(lớp Course)*

Lưu thông tin khóa học do giáo viên tạo và phụ trách.

***Bảng 3.16.*** *Đặc tả bảng courses*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã khóa học |
| subject_id | UUID | Không | FK → subjects(id) | Môn học của khóa học |
| teacher_id | UUID | Không | FK → teachers(user_​id) | Giáo viên phụ trách (Teacher manages Course) |
| code | VARCHAR(50) | Không | UNIQUE | Mã khóa học |
| title | VARCHAR(255) | Không |  | Tên khóa học |
| description | TEXT | Không |  | Mô tả, mục tiêu đầu ra |
| level | level | Không |  | Trình độ phù hợp |
| max_students | INTEGER | Có | CHECK (max_​students > 0) | Số học viên tối đa; NULL nếu không giới hạn |
| status | course_status | Không | DEFAULT 'DRAFT' | Trạng thái khóa học |
| published_at | TIMESTAMPTZ | Có |  | Thời điểm xuất bản |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |
| deleted_at | TIMESTAMPTZ | Có |  | Thời điểm xóa mềm; NULL nghĩa là bản ghi còn hiệu lực |

Ràng buộc mức bảng: ck_courses_published_at — khóa học đã xuất bản phải có thời điểm xuất bản.

**Bảng chapters**  *(lớp Chapter)*

Lưu các chương của khóa học.

***Bảng 3.17.*** *Đặc tả bảng chapters*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã chương |
| course_id | UUID | Không | FK → courses(id), ON DELETE CASCADE; UNIQUE (course_​id, title) | Khóa học chứa chương (hợp thành) |
| title | VARCHAR(255) | Không | UNIQUE (course_​id, title) | Tên chương |
| order_index | INTEGER | Không | CHECK (order_​index >= 0) | Thứ tự hiển thị trong khóa học |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |

**Bảng lessons**  *(lớp Lesson)*

Lưu các bài học thuộc một chương.

***Bảng 3.18.*** *Đặc tả bảng lessons*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã bài học |
| chapter_id | UUID | Không | FK → chapters(id), ON DELETE CASCADE; UNIQUE (chapter_​id, title) | Chương chứa bài học (hợp thành) |
| title | VARCHAR(255) | Không | UNIQUE (chapter_​id, title) | Tên bài học |
| content | TEXT | Không |  | Nội dung bài giảng |
| material_url | VARCHAR(500) | Có |  | Đường dẫn tài liệu đính kèm |
| order_index | INTEGER | Không | CHECK (order_​index >= 0) | Thứ tự hiển thị trong chương |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |

**Bảng course_exams**  *(lớp (Course – Exam))*

Bảng trung gian thể hiện quan hệ nhiều – nhiều giữa Course và Exam (đề thi được gán vào khóa học).

***Bảng 3.19.*** *Đặc tả bảng course_exams*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| course_id | UUID | Không | PK; FK → courses(id), ON DELETE CASCADE | Khóa học |
| exam_id | UUID | Không | PK; FK → exams(id) | Đề thi được gán |
| assigned_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm gán đề thi |

**Bảng enrollments**  *(lớp Enrollment)*

Lưu việc ghi danh của học viên vào khóa học và tiến độ học tập tương ứng.

***Bảng 3.20.*** *Đặc tả bảng enrollments*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã ghi danh |
| student_id | UUID | Không | FK → students(user_​id); UNIQUE (student_​id, course_​id) | Học viên ghi danh |
| course_id | UUID | Không | FK → courses(id); UNIQUE (student_​id, course_​id) | Khóa học được ghi danh |
| status | enrollment_status | Không | DEFAULT 'ACTIVE' | Trạng thái ghi danh |
| progress_percent | NUMERIC(5,2) | Không | DEFAULT 0; CHECK (progress_​percent BETWEEN 0 AND 100) | Phần trăm hoàn thành khóa học (giá trị dẫn xuất) |
| enrolled_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm ghi danh |
| completed_at | TIMESTAMPTZ | Có |  | Thời điểm hoàn thành khóa học |

Ràng buộc mức bảng: ck_enrollments_completed — ghi danh đã hoàn thành phải có thời điểm hoàn thành; ck_enrollments_dates — thời điểm hoàn thành không trước thời điểm ghi danh.

**Bảng lesson_completions**  *(lớp (Enrollment.completeLesson))*

Lưu các bài học mà học viên đã hoàn thành trong một lần ghi danh. Bảng phát sinh từ phương thức Enrollment.completeLesson(lesson) để có dữ liệu tính progressPercent.

***Bảng 3.21.*** *Đặc tả bảng lesson_completions*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| enrollment_id | UUID | Không | PK; FK → enrollments(id), ON DELETE CASCADE | Lần ghi danh |
| lesson_id | UUID | Không | PK; FK → lessons(id), ON DELETE CASCADE | Bài học đã hoàn thành |
| completed_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm hoàn thành bài học |

### 6.3. Nhóm ngân hàng câu hỏi, đề thi và làm bài

**Bảng questions**  *(lớp Question)*

Lưu câu hỏi trong ngân hàng câu hỏi dùng chung.

***Bảng 3.22.*** *Đặc tả bảng questions*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã câu hỏi |
| topic_id | UUID | Không | FK → topics(id) | Chủ đề phân loại câu hỏi |
| author_id | UUID | Không | FK → teachers(user_​id) | Giáo viên tạo câu hỏi |
| type | question_type | Không |  | Loại câu hỏi |
| content | TEXT | Không |  | Nội dung câu hỏi |
| explanation | TEXT | Có |  | Giải thích đáp án |
| skill | skill | Có |  | Kỹ năng TOEIC; NULL với câu hỏi môn CNTT |
| part | SMALLINT | Có |  | Part TOEIC (1–7); NULL với câu hỏi môn CNTT |
| difficulty | difficulty | Không |  | Độ khó |
| media_url | VARCHAR(500) | Có |  | Đường dẫn tệp âm thanh / hình ảnh |
| status | question_status | Không | DEFAULT 'DRAFT' | Trạng thái câu hỏi |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |
| deleted_at | TIMESTAMPTZ | Có |  | Thời điểm xóa mềm; NULL nghĩa là bản ghi còn hiệu lực |

Ràng buộc mức bảng: ck_questions_skill_part — part 1–4 thuộc Listening, Part 5–7 thuộc Reading.

**Bảng question_options**  *(lớp QuestionOption)*

Lưu các phương án trả lời của câu hỏi; với câu điền từ, các phương án có is_correct = true là các đáp án được chấp nhận.

***Bảng 3.23.*** *Đặc tả bảng question_options*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã phương án |
| question_id | UUID | Không | FK → questions(id), ON DELETE CASCADE; UNIQUE (question_​id, label) | Câu hỏi chứa phương án (hợp thành) |
| label | VARCHAR(10) | Không | UNIQUE (question_​id, label) | Nhãn phương án (A, B, C, D…) |
| content | TEXT | Không |  | Nội dung phương án |
| is_correct | BOOLEAN | Không | DEFAULT false | Là đáp án đúng hay không |
| order_index | SMALLINT | Không | CHECK (order_​index >= 0) | Thứ tự hiển thị |

**Bảng exams**  *(lớp Exam)*

Lưu thông tin và cấu hình làm bài của đề thi.

***Bảng 3.24.*** *Đặc tả bảng exams*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã đề thi |
| author_id | UUID | Không | FK → teachers(user_​id) | Giáo viên tạo đề thi |
| title | VARCHAR(255) | Không |  | Tên đề thi |
| description | TEXT | Có |  | Mô tả đề thi |
| level | level | Không |  | Trình độ |
| scoring_scale | scoring_scale | Không |  | Thang điểm |
| duration_minutes | INTEGER | Không | CHECK (duration_​minutes > 0) | Thời gian làm bài toàn đề (phút) |
| max_attempts | INTEGER | Có | CHECK (max_​attempts > 0) | Số lần làm tối đa; NULL nếu không giới hạn |
| allow_part_practice | BOOLEAN | Không | DEFAULT true | Cho phép luyện tập theo Part |
| shuffle_questions | BOOLEAN | Không | DEFAULT false | Đảo thứ tự câu hỏi |
| status | exam_status | Không | DEFAULT 'DRAFT' | Trạng thái đề thi |
| published_at | TIMESTAMPTZ | Có |  | Thời điểm xuất bản gần nhất |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |
| deleted_at | TIMESTAMPTZ | Có |  | Thời điểm xóa mềm; NULL nghĩa là bản ghi còn hiệu lực |

Ràng buộc mức bảng: ck_exams_published_at — đề thi đã xuất bản phải có thời điểm xuất bản.

**Bảng exam_sections**  *(lớp ExamSection)*

Lưu các phần (Part) của đề thi.

***Bảng 3.25.*** *Đặc tả bảng exam_sections*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã phần thi |
| exam_id | UUID | Không | FK → exams(id), ON DELETE CASCADE | Đề thi chứa phần thi (hợp thành) |
| title | VARCHAR(255) | Không |  | Tên phần thi |
| part_number | SMALLINT | Có | CHECK (part_​number BETWEEN 1 AND 7) | Số Part TOEIC tương ứng |
| duration_minutes | INTEGER | Có | CHECK (duration_​minutes > 0) | Thời gian riêng của phần (phút) |
| order_index | SMALLINT | Không | CHECK (order_​index >= 0) | Thứ tự phần trong đề |

**Bảng exam_questions**  *(lớp ExamQuestion)*

Lưu các câu hỏi được đưa vào một phần thi cùng thứ tự và điểm số.

***Bảng 3.26.*** *Đặc tả bảng exam_questions*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã câu hỏi trong đề |
| section_id | UUID | Không | FK → exam_​sections(id), ON DELETE CASCADE; UNIQUE (section_​id, question_​id) | Phần thi chứa câu hỏi (hợp thành) |
| question_id | UUID | Không | FK → questions(id); UNIQUE (section_​id, question_​id) | Câu hỏi trong ngân hàng |
| order_index | INTEGER | Không | CHECK (order_​index >= 0) | Thứ tự câu trong phần thi |
| points | NUMERIC(6,2) | Không | DEFAULT 1; CHECK (points >= 0) | Điểm của câu hỏi |

**Bảng exam_attempts**  *(lớp ExamAttempt)*

Lưu mỗi lượt làm bài của học viên, trạng thái phiên làm bài và kết quả tổng hợp.

***Bảng 3.27.*** *Đặc tả bảng exam_attempts*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã lượt làm bài |
| student_id | UUID | Không | FK → students(user_​id) | Học viên làm bài |
| exam_id | UUID | Không | FK → exams(id) | Đề thi |
| mode | attempt_mode | Không |  | Chế độ làm bài |
| status | attempt_status | Không | DEFAULT 'IN_​PROGRESS' | Trạng thái lượt làm bài |
| started_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm bắt đầu |
| expires_at | TIMESTAMPTZ | Có |  | Hạn nộp bài; NULL nếu tắt đếm giờ |
| remaining_seconds | INTEGER | Có | CHECK (remaining_​seconds >= 0) | Thời gian còn lại, dùng khi khôi phục bài |
| submitted_at | TIMESTAMPTZ | Có |  | Thời điểm nộp bài |
| correct_count | INTEGER | Không | DEFAULT 0; CHECK (correct_​count >= 0) | Số câu đúng (giá trị dẫn xuất) |
| wrong_count | INTEGER | Không | DEFAULT 0; CHECK (wrong_​count >= 0) | Số câu sai (giá trị dẫn xuất) |
| skipped_count | INTEGER | Không | DEFAULT 0; CHECK (skipped_​count >= 0) | Số câu bỏ trống (giá trị dẫn xuất) |
| raw_score | NUMERIC(7,2) | Có |  | Điểm thô |
| scaled_score | NUMERIC(7,2) | Có |  | Điểm quy đổi theo thang của đề |

Ràng buộc mức bảng: ck_attempts_expires — hạn nộp sau thời điểm bắt đầu; ck_attempts_submitted — thời điểm nộp không trước thời điểm bắt đầu; ck_attempts_status_submitted — bài đã nộp hoặc đã chấm phải có thời điểm nộp; ck_attempts_status_graded — bài đã chấm phải có điểm.

**Bảng attempt_answers**  *(lớp AttemptAnswer)*

Lưu câu trả lời của học viên cho từng câu hỏi trong một lượt làm bài; được cập nhật liên tục khi tự động lưu.

***Bảng 3.28.*** *Đặc tả bảng attempt_answers*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã câu trả lời |
| attempt_id | UUID | Không | FK → exam_​attempts(id), ON DELETE CASCADE; UNIQUE (attempt_​id, exam_​question_​id) | Lượt làm bài (hợp thành) |
| exam_question_id | UUID | Không | FK → exam_​questions(id); UNIQUE (attempt_​id, exam_​question_​id) | Câu hỏi trong đề được trả lời |
| text_answer | TEXT | Có |  | Câu trả lời dạng văn bản (câu điền từ) |
| is_flagged | BOOLEAN | Không | DEFAULT false | Học viên đánh dấu để xem lại |
| is_correct | BOOLEAN | Có |  | Kết quả chấm; NULL khi chưa chấm |
| points_earned | NUMERIC(6,2) | Có | CHECK (points_​earned >= 0) | Điểm đạt được |
| time_spent_seconds | INTEGER | Không | DEFAULT 0; CHECK (time_​spent_​seconds >= 0) | Thời gian làm câu (giây) |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |

**Bảng attempt_answer_options**  *(lớp (AttemptAnswer.selectedOptionIds))*

Lưu các phương án học viên đã chọn. Tách từ thuộc tính mảng selectedOptionIds để đạt dạng chuẩn 1 và có khóa ngoại tới question_options.

***Bảng 3.29.*** *Đặc tả bảng attempt_answer_options*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| attempt_answer_id | UUID | Không | PK; FK → attempt_​answers(id), ON DELETE CASCADE | Câu trả lời |
| option_id | UUID | Không | PK; FK → question_​options(id) | Phương án được chọn |

### 6.4. Nhóm flashcard

**Bảng flashcard_decks**  *(lớp FlashcardDeck)*

Lưu các bộ flashcard của hệ thống, của giáo viên hoặc của cá nhân học viên.

***Bảng 3.30.*** *Đặc tả bảng flashcard_decks*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã bộ thẻ |
| owner_id | UUID | Không | FK → users(id), ON DELETE CASCADE; UNIQUE (owner_​id, title) | Người sở hữu bộ thẻ |
| title | VARCHAR(255) | Không | UNIQUE (owner_​id, title) | Tên bộ thẻ |
| description | TEXT | Có |  | Mô tả bộ thẻ |
| type | deck_type | Không |  | Loại bộ thẻ |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |

**Bảng flashcards**  *(lớp Flashcard)*

Lưu các thẻ trong một bộ flashcard.

***Bảng 3.31.*** *Đặc tả bảng flashcards*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã thẻ |
| deck_id | UUID | Không | FK → flashcard_​decks(id), ON DELETE CASCADE | Bộ thẻ chứa thẻ (hợp thành) |
| front | TEXT | Không |  | Mặt trước (từ / câu hỏi) |
| back | TEXT | Không |  | Mặt sau (nghĩa / đáp án) |
| example | TEXT | Có |  | Ví dụ minh họa |
| note | TEXT | Có |  | Ghi chú |
| created_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm tạo bản ghi |
| updated_at | TIMESTAMPTZ | Không | DEFAULT now() | Thời điểm cập nhật gần nhất (tự cập nhật bằng trigger) |

**Bảng flashcard_progresses**  *(lớp FlashcardProgress)*

Lưu mức độ ghi nhớ và lịch ôn lại của từng học viên đối với từng thẻ.

***Bảng 3.32.*** *Đặc tả bảng flashcard_progresses*

| **Tên cột** | **Kiểu dữ liệu** | **Null** | **Ràng buộc** | **Mô tả** |
|---|---|---|---|---|
| id | UUID | Không | PK; DEFAULT gen_​random_​uuid() | Mã tiến độ ôn thẻ |
| flashcard_id | UUID | Không | FK → flashcards(id), ON DELETE CASCADE; UNIQUE (student_​id, flashcard_​id) | Thẻ được ôn (hợp thành) |
| student_id | UUID | Không | FK → students(user_​id), ON DELETE CASCADE; UNIQUE (student_​id, flashcard_​id) | Học viên ôn thẻ |
| status | card_memory_status | Không | DEFAULT 'NEW' | Mức độ ghi nhớ |
| review_count | INTEGER | Không | DEFAULT 0; CHECK (review_​count >= 0) | Số lần đã ôn |
| last_reviewed_at | TIMESTAMPTZ | Có |  | Thời điểm ôn gần nhất |
| next_review_at | TIMESTAMPTZ | Có |  | Thời điểm cần ôn lại |

## 7. Các kiểu liệt kê (ENUM)

Mỗi lớp «enumeration» trong sơ đồ lớp được cài đặt thành một kiểu ENUM của PostgreSQL, khai báo tương ứng bằng enum trong Prisma. So với lưu chuỗi tự do, kiểu ENUM ngăn ghi giá trị ngoài miền cho phép ngay tại tầng cơ sở dữ liệu và chỉ tốn 4 byte mỗi giá trị.

***Bảng 3.33.*** *Danh sách kiểu liệt kê*

| **Kiểu ENUM** | **Lớp tương ứng** | **Giá trị** | **Ý nghĩa** |
|---|---|---|---|
| user_​status | UserStatus | PENDING_VERIFICATION, PENDING_APPROVAL, ACTIVE, LOCKED | Trạng thái tài khoản người dùng |
| permission_​action | PermissionAction | VIEW, CREATE, UPDATE, DELETE | Hành động của một quyền |
| subject_​category | SubjectCategory | TOEIC, IT_SUBJECT | Nhóm môn học |
| level | Level | BEGINNER, INTERMEDIATE, ADVANCED | Trình độ (học viên, khóa học, đề thi) |
| course_​status | CourseStatus | DRAFT, PUBLISHED, CLOSED | Trạng thái khóa học |
| enrollment_​status | EnrollmentStatus | ACTIVE, COMPLETED, CANCELLED | Trạng thái ghi danh |
| question_​type | QuestionType | SINGLE_CHOICE, MULTIPLE_CHOICE, FILL_IN_BLANK | Loại câu hỏi |
| question_​status | QuestionStatus | DRAFT, ACTIVE, INACTIVE | Trạng thái câu hỏi |
| skill | Skill | LISTENING, READING | Kỹ năng TOEIC |
| difficulty | Difficulty | EASY, MEDIUM, HARD | Độ khó câu hỏi |
| exam_​status | ExamStatus | DRAFT, PUBLISHED, UNPUBLISHED | Trạng thái đề thi |
| scoring_​scale | ScoringScale | TOEIC_990, SCALE_10, SCALE_100 | Thang điểm của đề thi |
| attempt_​mode | AttemptMode | FULL_TEST, PART_PRACTICE | Chế độ làm bài |
| attempt_​status | AttemptStatus | IN_PROGRESS, SUBMITTED, GRADED, CANCELLED | Trạng thái lượt làm bài |
| deck_​type | DeckType | SYSTEM, TEACHER, PERSONAL | Loại bộ flashcard |
| card_​memory_​status | CardMemoryStatus | NEW, LEARNING, MEMORIZED | Mức độ ghi nhớ thẻ |

## 8. Ràng buộc toàn vẹn và quy tắc nghiệp vụ

### 8.1. Ràng buộc cài đặt tại cơ sở dữ liệu

Ngoài ràng buộc khóa chính, khóa ngoại và các ràng buộc CHECK trên từng cột đã nêu trong mục 6, cơ sở dữ liệu cài đặt các ràng buộc liên quan tới nhiều cột hoặc nhiều dòng sau:

***Bảng 3.34.*** *Các ràng buộc nhiều cột và ràng buộc có điều kiện*

| **Tên ràng buộc** | **Bảng** | **Biểu thức** | **Ý nghĩa** |
|---|---|---|---|
| ck_​courses_​published_​at | courses | status <> 'PUBLISHED' OR published_at IS NOT NULL | Khóa học đã xuất bản phải có thời điểm xuất bản |
| ck_​enrollments_​completed | enrollments | status <> 'COMPLETED' OR completed_at IS NOT NULL | Ghi danh đã hoàn thành phải có thời điểm hoàn thành |
| ck_​enrollments_​dates | enrollments | completed_at IS NULL OR completed_at >= enrolled_at | Thời điểm hoàn thành không trước thời điểm ghi danh |
| ck_​questions_​skill_​part | questions | part IS NULL OR (skill = 'LISTENING' AND part BETWEEN 1 AND 4) OR (skill = 'READING' AND part BETWEEN 5 AND 7) | Part 1–4 thuộc Listening, Part 5–7 thuộc Reading |
| ck_​exams_​published_​at | exams | status <> 'PUBLISHED' OR published_at IS NOT NULL | Đề thi đã xuất bản phải có thời điểm xuất bản |
| ck_​attempts_​expires | exam_​attempts | expires_at IS NULL OR expires_at > started_at | Hạn nộp sau thời điểm bắt đầu |
| ck_​attempts_​submitted | exam_​attempts | submitted_at IS NULL OR submitted_at >= started_at | Thời điểm nộp không trước thời điểm bắt đầu |
| ck_​attempts_​status_​submitted | exam_​attempts | status NOT IN ('SUBMITTED','GRADED') OR submitted_at IS NOT NULL | Bài đã nộp hoặc đã chấm phải có thời điểm nộp |
| ck_​attempts_​status_​graded | exam_​attempts | status <> 'GRADED' OR raw_score IS NOT NULL | Bài đã chấm phải có điểm |
| uq_​users_​email_​lower | users | UNIQUE INDEX (lower(email)) | Email duy nhất, không phân biệt chữ hoa, chữ thường |
| uq_​attempts_​one_​in_​progress | exam_​attempts | UNIQUE INDEX (student_id, exam_id) WHERE status = 'IN_PROGRESS' | Mỗi học viên chỉ có một bài làm dở với mỗi đề thi (UC Làm bài thi, Khôi phục bài làm) |

Các ràng buộc duy nhất trên tổ hợp cột gồm: permissions (module, action); topics (subject_id, name); chapters (course_id, title); lessons (chapter_id, title); enrollments (student_id, course_id) — một học viên không ghi danh hai lần vào cùng khóa học; question_options (question_id, label); exam_questions (section_id, question_id) — không thêm trùng câu hỏi trong một phần thi; attempt_answers (attempt_id, exam_question_id) — mỗi câu chỉ có một câu trả lời trong một lượt làm bài; flashcard_decks (owner_id, title) — tên bộ thẻ không trùng trong phạm vi một người dùng; flashcard_progresses (student_id, flashcard_id).

### 8.2. Hành vi khi xóa dữ liệu

Hành vi ON DELETE của từng khóa ngoại được chọn theo loại quan hệ trong sơ đồ lớp. Mọi khóa ngoại dùng ON UPDATE CASCADE; vì khóa chính là UUID không thay đổi nên thực tế không phát sinh cập nhật dây chuyền.

***Bảng 3.35.*** *Chính sách xóa của các khóa ngoại*

| **Hành vi** | **Áp dụng cho** | **Lý do** |
|---|---|---|
| CASCADE | role_​permissions.role_​id → roles<br>role_​permissions.permission_​id → permissions<br>students.user_​id → users<br>teachers.user_​id → users<br>admins.user_​id → users<br>topics.subject_​id → subjects<br>chapters.course_​id → courses<br>lessons.chapter_​id → chapters<br>course_​exams.course_​id → courses<br>lesson_​completions.enrollment_​id → enrollments<br>lesson_​completions.lesson_​id → lessons<br>question_​options.question_​id → questions<br>exam_​sections.exam_​id → exams<br>exam_​questions.section_​id → exam_​sections<br>attempt_​answers.attempt_​id → exam_​attempts<br>attempt_​answer_​options.attempt_​answer_​id → attempt_​answers<br>flashcard_​decks.owner_​id → users<br>flashcards.deck_​id → flashcard_​decks<br>flashcard_​progresses.flashcard_​id → flashcards<br>flashcard_​progresses.student_​id → students | Quan hệ hợp thành, quan hệ kế thừa và bảng trung gian: thành phần không có ý nghĩa khi tổng thể bị xóa |
| RESTRICT | users.role_​id → roles<br>courses.subject_​id → subjects<br>courses.teacher_​id → teachers<br>course_​exams.exam_​id → exams<br>enrollments.student_​id → students<br>enrollments.course_​id → courses<br>questions.topic_​id → topics<br>questions.author_​id → teachers<br>exams.author_​id → teachers<br>exam_​questions.question_​id → questions<br>exam_​attempts.student_​id → students<br>exam_​attempts.exam_​id → exams<br>attempt_​answers.exam_​question_​id → exam_​questions<br>attempt_​answer_​options.option_​id → question_​options | Bản ghi được tham chiếu là dữ liệu gốc hoặc dữ liệu lịch sử: không cho xóa khi còn được sử dụng, nhằm bảo toàn kết quả học tập |

Chuỗi ràng buộc này bảo đảm tính nhất quán theo đúng yêu cầu của các use case. Ví dụ, không thể xóa vật lý câu hỏi đang nằm trong đề thi (exam_questions.question_id RESTRICT), không thể xóa phần thi đã có bài làm (attempt_answers.exam_question_id RESTRICT chặn thao tác CASCADE từ exam_sections), không thể xóa khóa học đã có học viên (enrollments.course_id RESTRICT). Với các trường hợp này, hệ thống dùng xóa mềm (deleted_at) hoặc chuyển trạng thái (CLOSED, INACTIVE, UNPUBLISHED).

### 8.3. Quy tắc nghiệp vụ kiểm soát tại tầng ứng dụng

Một số quy tắc không thể hoặc không nên cài đặt bằng ràng buộc khai báo, vì phụ thuộc vào nhiều bảng hoặc vào thời điểm thực hiện nghiệp vụ. Các quy tắc này được kiểm soát trong tầng Service, thực hiện trong giao dịch cơ sở dữ liệu:

***Bảng 3.36.*** *Quy tắc nghiệp vụ kiểm soát tại tầng Service*

| **Quy tắc** | **Cách thực hiện** |
|---|---|
| Bội số 1..*: khóa học có ít nhất một chương, đề thi có ít nhất một phần thi và một câu hỏi, câu hỏi có ít nhất một đáp án đúng | Kiểm tra trong Course.publish(), Exam.publish() và khi lưu câu hỏi (QuestionService.create/update) |
| Khóa học không vượt quá max_students | EnrollmentService.enroll() khóa dòng khóa học bằng SELECT … FOR UPDATE, đếm số ghi danh ACTIVE rồi mới chèn, tránh hai học viên cùng chiếm chỗ cuối |
| Học viên không làm quá max_attempts lần | ExamAttemptService.start() đếm số lượt SUBMITTED/GRADED của học viên với đề trước khi tạo lượt mới |
| Một câu hỏi không xuất hiện hai lần trong cùng một đề (khác phần thi) | ExamService.addQuestions() kiểm tra trên toàn bộ exam_questions của đề |
| Phương án được chọn phải thuộc đúng câu hỏi đang trả lời | ExamAttemptService.saveAnswer() đối chiếu option_id với question_id của exam_question |
| Loại người dùng khớp với vai trò | UserService tạo bản ghi users và bản ghi bảng con theo role trong cùng giao dịch |
| Kết quả bài làm cũ không bị thay đổi khi sửa câu hỏi | Câu hỏi đã có câu trả lời tham chiếu chỉ được sửa giải thích và nhãn phân loại; muốn sửa nội dung hoặc phương án, giáo viên nhân bản thành câu hỏi mới và chuyển câu cũ sang INACTIVE |
| Bài làm hết giờ được tự động nộp | Tác vụ nền quét exam_attempts có status = IN_PROGRESS và expires_at < now() (dùng chỉ mục ix_attempts_expiring) |
| Dữ liệu đã xóa mềm không hiển thị | Mọi truy vấn danh sách lọc deleted_at IS NULL (cấu hình mặc định trong tầng Repository) |

## 9. Chỉ mục và tối ưu truy vấn

PostgreSQL tự tạo chỉ mục cho khóa chính và ràng buộc UNIQUE nhưng không tự tạo chỉ mục cho khóa ngoại. Vì vậy mọi cột khóa ngoại đều được đánh chỉ mục, trừ khi cột đó đã là cột đầu tiên của một chỉ mục khác. Ngoài ra, các chỉ mục sau được thiết kế cho những truy vấn thường xuyên nhằm đáp ứng yêu cầu NFR-01 (thời gian phản hồi trung bình dưới 500 ms):

***Bảng 3.37.*** *Danh sách chỉ mục bổ sung*

| **Tên chỉ mục** | **Bảng** | **Cột / biểu thức** | **Loại** | **Mục đích** |
|---|---|---|---|---|
| uq_​users_​email_​lower | users | lower(email) | UNIQUE<br>B-tree | Email duy nhất, không phân biệt hoa thường; tra cứu khi đăng nhập |
| ix_​users_​role_​id | users | role_​id | B-tree | Khóa ngoại; lọc người dùng theo vai trò |
| ix_​users_​status | users | status | B-tree | Lọc người dùng theo trạng thái (trang quản trị) |
| ix_​role_​permissions_​permission_​id | role_​permissions | permission_​id | B-tree | Khóa ngoại; tìm các vai trò có một quyền |
| ix_​courses_​subject_​id | courses | subject_​id | B-tree | Khóa ngoại; lọc khóa học theo môn |
| ix_​courses_​teacher_​id | courses | teacher_​id | B-tree | Khóa ngoại; danh sách khóa học của giáo viên |
| ix_​courses_​status_​published | courses | status, published_​at DESC<br>WHERE deleted_at IS NULL | B-tree | Danh sách khóa học đã xuất bản cho học viên |
| ix_​courses_​title_​trgm | courses | title | GIN | Tìm kiếm khóa học theo từ khóa |
| ix_​chapters_​course_​order | chapters | course_​id, order_​index | B-tree | Lấy chương theo thứ tự |
| ix_​lessons_​chapter_​order | lessons | chapter_​id, order_​index | B-tree | Lấy bài học theo thứ tự |
| ix_​course_​exams_​exam_​id | course_​exams | exam_​id | B-tree | Khóa ngoại; các khóa học đang dùng một đề thi |
| ix_​enrollments_​course_​id | enrollments | course_​id | B-tree | Khóa ngoại; đếm học viên của khóa học |
| ix_​lesson_​completions_​lesson_​id | lesson_​completions | lesson_​id | B-tree | Khóa ngoại |
| ix_​questions_​topic_​id | questions | topic_​id | B-tree | Khóa ngoại; lọc câu hỏi theo chủ đề |
| ix_​questions_​author_​id | questions | author_​id | B-tree | Khóa ngoại; câu hỏi của giáo viên |
| ix_​questions_​filter | questions | skill, part, difficulty, status<br>WHERE deleted_at IS NULL | B-tree | Bộ lọc ngân hàng câu hỏi và rút đề theo ma trận |
| ix_​questions_​content_​trgm | questions | content | GIN | Tìm theo từ khóa và phát hiện câu hỏi trùng lặp |
| ix_​exams_​author_​id | exams | author_​id | B-tree | Khóa ngoại; đề thi của giáo viên |
| ix_​exams_​status_​published | exams | status, published_​at DESC<br>WHERE deleted_at IS NULL | B-tree | Danh sách đề thi đã xuất bản |
| ix_​exams_​title_​trgm | exams | title | GIN | Tìm kiếm đề thi theo từ khóa |
| ix_​exam_​sections_​exam_​order | exam_​sections | exam_​id, order_​index | B-tree | Lấy các phần thi theo thứ tự |
| ix_​exam_​questions_​question_​id | exam_​questions | question_​id | B-tree | Khóa ngoại; kiểm tra câu hỏi đang được sử dụng |
| uq_​attempts_​one_​in_​progress | exam_​attempts | student_​id, exam_​id<br>WHERE status = 'IN_PROGRESS' | UNIQUE<br>B-tree | Mỗi học viên chỉ có một bài làm dở với mỗi đề |
| ix_​attempts_​student_​started | exam_​attempts | student_​id, started_​at DESC | B-tree | Lịch sử làm bài và biểu đồ tiến độ |
| ix_​attempts_​exam_​id | exam_​attempts | exam_​id | B-tree | Khóa ngoại; thống kê theo đề thi |
| ix_​attempts_​expiring | exam_​attempts | expires_​at<br>WHERE status = 'IN_PROGRESS' | B-tree | Tác vụ nền tự động nộp bài hết giờ |
| ix_​attempt_​answers_​exam_​question_​id | attempt_​answers | exam_​question_​id | B-tree | Khóa ngoại; thống kê tỉ lệ đúng từng câu |
| ix_​attempt_​answer_​options_​option_​id | attempt_​answer_​options | option_​id | B-tree | Khóa ngoại |
| ix_​flashcards_​deck_​id | flashcards | deck_​id | B-tree | Khóa ngoại; lấy thẻ của bộ |
| ix_​flashcard_​progresses_​flashcard_​id | flashcard_​progresses | flashcard_​id | B-tree | Khóa ngoại |
| ix_​flashcard_​progresses_​due | flashcard_​progresses | student_​id, next_​review_​at | B-tree | Lấy các thẻ đến hạn ôn của học viên |

## 10. Chuẩn hóa dữ liệu

Lược đồ được thiết kế đạt dạng chuẩn 3 (3NF):

- **Dạng chuẩn 1 (1NF):** mọi cột chứa giá trị nguyên tố, không có nhóm lặp. Thuộc tính đa trị selectedOptionIds được tách thành bảng attempt_answer_options.
- **Dạng chuẩn 2 (2NF):** các bảng có khóa chính đơn (UUID) hiển nhiên đạt 2NF. Các bảng có khóa chính kép (role_permissions, course_exams, lesson_completions, attempt_answer_options) chỉ chứa thêm cột thời điểm (granted_at, assigned_at, completed_at) phụ thuộc vào toàn bộ khóa.
- **Dạng chuẩn 3 (3NF):** không có thuộc tính không khóa nào phụ thuộc bắc cầu vào khóa chính. Ví dụ, môn học của một câu hỏi được suy ra qua chủ đề (questions.topic_id → topics.subject_id) nên bảng questions không lưu subject_id; đề thi của một câu hỏi trong đề được suy ra qua phần thi nên exam_questions không lưu exam_id.

Một số cột lưu giá trị dẫn xuất được giữ lại có chủ đích (phi chuẩn hóa có kiểm soát), tương ứng với các thuộc tính đã có trong sơ đồ lớp:

***Bảng 3.38.*** *Các cột lưu giá trị dẫn xuất*

| **Cột** | **Dẫn xuất từ** | **Lý do giữ lại** | **Cách đồng bộ** |
|---|---|---|---|
| enrollments.progress_​percent | Số dòng lesson_completions / tổng số bài học của khóa | Hiển thị tiến độ ở danh sách "Khóa học của tôi" mà không phải đếm lại | Cập nhật trong cùng giao dịch với completeLesson() |
| exam_​attempts.correct_​count, wrong_​count, skipped_​count | Đếm attempt_answers theo is_correct | Lịch sử làm bài và biểu đồ tiến độ đọc nhiều lượt làm bài cùng lúc | Ghi một lần bởi GradingService khi chấm bài |
| exam_​attempts.raw_​score, scaled_​score | Tổng points_earned và bảng quy đổi của ScoringStrategy | Điểm là kết quả tại thời điểm chấm; không được thay đổi khi sau này giáo viên sửa điểm câu hỏi hoặc bảng quy đổi | Ghi một lần bởi GradingService khi chấm bài |

## 11. Dữ liệu lưu ngoài cơ sở dữ liệu quan hệ

Sơ đồ lớp không có lớp thực thể cho token và các bộ đếm bảo mật; các dữ liệu này do TokenService và AuthService quản lý. Đây là dữ liệu ngắn hạn, cần tự hết hạn và được truy cập với tần suất cao, nên được lưu trên Redis với thời hạn sống (TTL) thay vì tạo bảng trong PostgreSQL. Tệp đa phương tiện do FileStorageService lưu trên kho tệp, cơ sở dữ liệu chỉ lưu đường dẫn.

***Bảng 3.39.*** *Dữ liệu lưu trên Redis và kho tệp*

| **Dữ liệu** | **Nơi lưu / khóa** | **Thời hạn** | **Sử dụng** |
|---|---|---|---|
| Refresh token | Redis: refresh:{userId}:{tokenId} | 7 ngày | Đăng xuất xóa khóa của phiên hiện tại; đổi hoặc đặt lại mật khẩu xóa toàn bộ khóa của người dùng |
| Token xác thực email | Redis: verify:{tokenHash} → userId | 24 giờ | AuthService.verifyEmail(); dùng một lần |
| Token đặt lại mật khẩu | Redis: reset:{tokenHash} → userId | 15 phút | AuthService.resetPassword(); dùng một lần |
| Số lần đăng nhập sai | Redis: login_fail:{email} | 15 phút | Tạm khóa đăng nhập khi sai quá 5 lần liên tiếp |
| Số yêu cầu đặt lại mật khẩu | Redis: reset_limit:{email} | 15 phút | Giới hạn tối đa 3 yêu cầu |
| Ảnh đại diện, tài liệu bài học, âm thanh, hình ảnh câu hỏi | Kho tệp (FileStorageService) | Lâu dài | CSDL lưu URL trong avatar_url, material_url, media_url |

Token chỉ được lưu dưới dạng chuỗi băm SHA-256, không lưu token gốc, để kẻ tấn công đọc được Redis cũng không dùng được token.

## 12. Dữ liệu khởi tạo

Sau khi tạo lược đồ, hệ thống nạp dữ liệu khởi tạo (seed) để có thể vận hành và kiểm thử:

- **roles:** ba vai trò STUDENT, TEACHER, ADMIN.
- **permissions** và **role_permissions:** các quyền theo từng phân hệ (user, course, question, exam, flashcard, statistic) kết hợp với các hành động VIEW, CREATE, UPDATE, DELETE, được gán cho từng vai trò.
- **users** và **admins:** một tài khoản quản trị viên mặc định ở trạng thái ACTIVE; mật khẩu buộc đổi ở lần đăng nhập đầu tiên.
- **subjects** và **topics:** môn TOEIC và các môn chuyên ngành CNTT (Cơ sở dữ liệu, Kiến trúc máy tính, Mạng máy tính, Lập trình hướng đối tượng) cùng danh sách chủ đề của từng môn.
- **questions** và **question_options:** 300–500 câu hỏi cho mỗi nhóm nội dung theo chỉ tiêu NFR-08, nạp bằng chức năng nhập câu hỏi từ tệp.

## 13. Hạn chế và hướng mở rộng

Cơ sở dữ liệu được thiết kế bám sát sơ đồ lớp hiện tại, do đó chưa có bảng cho một số dữ liệu được nhắc tới trong đặc tả yêu cầu: nhật ký kiểm toán (NFR-09 và bước "ghi nhật ký" trong nhiều use case), dữ liệu phục vụ các chức năng AI (FR-09 phân tích năng lực và gợi ý, FR-10 chatbot, FR-11 sinh câu hỏi, FR-15 hạn mức API AI), cấu hình tham số hệ thống, thông tin sao lưu, lịch sử phiên bản câu hỏi và liên kết giữa đề thi với môn học. Khi các lớp tương ứng được bổ sung vào sơ đồ lớp, cơ sở dữ liệu được mở rộng bằng cách thêm bảng và khóa ngoại mới trỏ tới các bảng hiện có, không phải thay đổi cấu trúc 26 bảng đã thiết kế.
