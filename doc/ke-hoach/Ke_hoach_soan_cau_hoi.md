# KẾ HOẠCH SOẠN NGÂN HÀNG CÂU HỎI

> Mục tiêu theo NFR-08 và KPI trong M1: **300–500 câu cho mỗi nhóm nội dung**. Kế hoạch này nhắm mức tối thiểu **300 câu TOEIC + 300 câu CNTT = 600 câu**, sẵn sàng để nạp vào hệ thống ở **cuối Sprint 3 (Tuần 13, 1/11/2026)**.
>
> Liên quan: `Ke_hoach_trien_khai.md` (Mục 5.4 Seed, Sprint 3), `../Thiet_ke_co_so_du_lieu.md` (bảng `questions`, `question_options`).

---

## 1. Nguyên tắc

1. **Tự soạn, không sao chép đề có bản quyền** (ETS, New Economy, Hackers, đề thi của trường…). Chỉ bám **định dạng** đề TOEIC, không dùng lại nội dung.
2. **AI soạn nháp, người duyệt 100%.** Dùng ChatGPT, Claude hoặc Gemini để sinh nháp theo prompt mẫu (Mục 6), sau đó tự kiểm tra từng câu theo checklist (Mục 7). Câu chưa duyệt không được đưa vào dữ liệu seed.
3. **Mỗi câu phải có đủ:** chủ đề (topic), độ khó, đáp án, **giải thích bằng tiếng Việt**. Hai tính năng AI phân tích năng lực và xem lại đáp án phụ thuộc trực tiếp vào các trường này.
4. **Soạn thẳng vào tệp mẫu Excel** (Mục 5) để nạp được ngay bằng chức năng nhập câu hỏi, không phải chuyển đổi lại.
5. **Ghi nguồn** cho mọi hình ảnh và âm thanh (cột `source`).

---

## 2. Vấn đề cần sửa trong thiết kế CSDL: nhóm câu hỏi

TOEIC Part 3, 4, 6, 7 là **nhóm câu hỏi dùng chung một đoạn hội thoại, bài nói hoặc đoạn văn** (ví dụ 1 đoạn hội thoại có 3 câu hỏi). Bảng `questions` hiện tại chỉ có `content` và `media_url` cho từng câu, nên không biểu diễn được điều này. Nếu giữ nguyên, sẽ phải chép đoạn văn/audio vào cả 3 câu, và khi làm bài không hiển thị được theo nhóm.

**Điều chỉnh (bổ sung D15 vào kế hoạch triển khai):**

| Bảng mới `question_groups` | Kiểu | Ghi chú |
|---|---|---|
| id | UUID | PK |
| topic_id | UUID | FK → topics |
| author_id | UUID | FK → teachers |
| skill, part | skill, SMALLINT | Như `questions` |
| passage | TEXT | Đoạn văn (Part 6, 7); với Part 7 nhiều đoạn thì ngăn cách bằng dấu phân đoạn |
| transcript | TEXT | Lời thoại (Part 3, 4). **Chỉ hiện ở màn xem lại, không hiện khi đang làm bài** |
| audio_url, image_url | VARCHAR(500) | Audio dùng chung, hình/biểu đồ (Part 3, 4 có câu hỏi đọc biểu đồ) |
| status, created_at, updated_at, deleted_at | | Như `questions` |

- Thêm cột `questions.group_id` (cho phép NULL) và `questions.order_in_group`.
- Khi soạn đề và rút đề theo ma trận, **nhóm được xử lý như một khối**: không được lấy lẻ từng câu trong nhóm.
- Part 1 và 2 vẫn là câu đơn. Nội dung phương án chính là lời đọc; **giao diện ẩn nội dung phương án khi đang làm bài** và chỉ hiện lại ở màn xem lại.

---

## 3. Phân bổ số lượng

### 3.1. TOEIC: 300 câu

Phân bổ để **ghép được ít nhất 1 đề full test (200 câu)**, phần còn lại dùng cho luyện tập theo Part.

| Part | Dạng | 1 đề thật | **Soạn** | Đơn vị soạn | Tài nguyên cần |
|---|---|---|---|---|---|
| 1 | Mô tả tranh (4 phương án) | 6 | **12** | 12 câu đơn | 12 ảnh + 12 audio |
| 2 | Hỏi – đáp (3 phương án) | 25 | **40** | 40 câu đơn | 40 audio |
| 3 | Hội thoại | 39 | **48** | 16 nhóm × 3 câu | 16 audio (2–3 giọng), 3 biểu đồ |
| 4 | Bài nói ngắn | 30 | **36** | 12 nhóm × 3 câu | 12 audio, 2 biểu đồ |
| 5 | Điền câu chưa hoàn chỉnh | 30 | **60** | 60 câu đơn | — |
| 6 | Điền đoạn văn | 16 | **32** | 8 nhóm × 4 câu | — |
| 7 | Đọc hiểu | 54 | **72** | 10 đoạn đơn (~40 câu) + 4 bộ đa đoạn × 5 câu (20 câu) + 4 đoạn đơn ngắn × 3 (12 câu) | — |
| | **Tổng** | 200 | **300** | Listening 136 · Reading 164 | **80 tệp audio, 12 ảnh** |

### 3.2. CNTT: 300 câu (4 môn × 75 câu)

| Môn (subject code) | Chủ đề (topic), ~12–15 câu/chủ đề |
|---|---|
| **CSDL** Cơ sở dữ liệu | Mô hình ER · Mô hình quan hệ & đại số quan hệ · SQL truy vấn · Phụ thuộc hàm & chuẩn hóa · Giao tác & điều khiển đồng thời · Chỉ mục & tối ưu truy vấn |
| **KTMT** Kiến trúc máy tính | Biểu diễn dữ liệu · Tập lệnh & chế độ địa chỉ · CPU & đường ống (pipeline) · Bộ nhớ & cache · Vào/ra & ngắt |
| **MMT** Mạng máy tính | Mô hình OSI/TCP-IP · Tầng liên kết & Ethernet · Địa chỉ IP & chia mạng con · Định tuyến · TCP/UDP · Tầng ứng dụng (HTTP, DNS…) |
| **LTHDT** Lập trình hướng đối tượng | Lớp & đối tượng · Đóng gói · Kế thừa · Đa hình & lớp trừu tượng/interface · Nguyên lý SOLID · Mẫu thiết kế cơ bản |

- **Loại câu:** khoảng 85% `SINGLE_CHOICE`, 15% `MULTIPLE_CHOICE` (4 phương án).
- **Hình thức câu:** lý thuyết, đọc code (đoạn code ngắn trong `content`), tính toán (chia subnet, địa chỉ cache, chuẩn hóa) và tình huống.

### 3.3. Tỉ lệ độ khó (áp dụng cho cả hai nhóm)

`EASY` 30% · `MEDIUM` 50% · `HARD` 20%. Mỗi chủ đề phải có đủ cả 3 mức, để chức năng rút đề theo ma trận và gợi ý bài tập theo điểm yếu có câu để chọn.

### 3.4. Chủ đề TOEIC (đơn vị thống kê điểm mạnh, điểm yếu)

Mỗi câu TOEIC gắn **1 chủ đề theo dạng kỹ năng**, không theo nội dung, để AI chỉ ra được "yếu ở đâu":

- **Listening:** P1 – Tranh người · P1 – Tranh vật/cảnh · P2 – Câu hỏi WH · P2 – Yes/No & câu hỏi lựa chọn · P2 – Câu đề nghị/yêu cầu · P3/P4 – Ý chính & mục đích · P3/P4 – Chi tiết · P3/P4 – Suy luận & hàm ý · P3/P4 – Biểu đồ/hình.
- **Reading:** P5/P6 – Từ loại · P5/P6 – Thì & dạng động từ · P5/P6 – Giới từ & liên từ · P5/P6 – Đại từ & mạo từ · P5/P6 – Mệnh đề quan hệ · P5/P6 – Từ vựng · P6 – Chèn câu · P7 – Ý chính · P7 – Chi tiết · P7 – Suy luận · P7 – Từ đồng nghĩa · P7 – Liên kết nhiều đoạn.

Bối cảnh nội dung (văn phòng, du lịch, mua sắm, nhà hàng, nhân sự, tài chính…) được ghi ở cột `tags` để trải đều, không dùng làm chủ đề.

---

## 4. Lịch thực hiện (Tuần 8 → 13)

Làm song song với code. Mỗi ngày trung bình **~20 câu**, chia 1 buổi soạn nháp bằng AI và 1 buổi duyệt.

| Tuần | Thời gian | Việc | Số câu tích lũy |
|---|---|---|---|
| 8 | 21/9 – 27/9 | Chốt tệp mẫu Excel + prompt mẫu. **Soạn thử 20 câu** (10 P5, 10 CSDL) để kiểm tra quy trình và checklist | 20 |
| 9 | 28/9 – 4/10 | CNTT: CSDL + LTHDT (150 câu) | 170 |
| 10 | 5/10 – 11/10 | CNTT: MMT + KTMT (150 câu) · TOEIC: P5 (60 câu) | 380 |
| 11 | 12/10 – 18/10 | TOEIC Reading: P6 (32) + P7 (72) | 484 |
| 12 | 19/10 – 25/10 | TOEIC Listening: viết lời thoại + câu hỏi P1–P4 (136); chọn 12 ảnh P1 | 620 |
| 13 | 26/10 – 1/11 | **Sinh 80 tệp audio** (Mục 8) · Rà soát chéo 10% số câu · Nạp thử bằng chức năng nhập · Sửa lỗi | **600 đã duyệt** |

> Tổng có dư khoảng 20 câu so với 600 để bù cho những câu bị loại khi duyệt. Nếu chậm tiến độ: giữ đủ 300 câu CNTT và các Part Reading, còn Listening thì giảm P2 xuống 25 và P3 xuống 13 nhóm (vẫn đủ 1 đề). Không được giảm dưới mức cần cho 1 đề full test.

**Theo dõi trên Jira:** tạo Epic *"Dữ liệu ngân hàng câu hỏi"*, mỗi dòng trong bảng trên là 1 task. Mỗi task có 2 trạng thái con: *Đã soạn* và *Đã duyệt*.

---

## 5. Tệp mẫu Excel

Một tệp `.xlsx` có 2 sheet và một thư mục `media/` đi kèm. Tệp này sẽ **đồng thời là định dạng của chức năng nhập câu hỏi** (`/teacher/questions/import/*`) và dữ liệu seed, nên cần chốt ngay trong Tuần 8.

**Sheet `groups`** (chỉ dùng cho Part 3, 4, 6, 7)

| Cột | Bắt buộc | Ví dụ |
|---|---|---|
| group_code | ✔ | `P3-G01` |
| subject_code | ✔ | `TOEIC` |
| topic | ✔ | `P3/P4 – Chi tiết` |
| part | ✔ | `3` |
| passage | P6, P7 | Đoạn văn |
| transcript | P3, P4 | `M: …` / `W: …` (mỗi lượt thoại một dòng) |
| audio_file | P3, P4 | `P3-G01.mp3` |
| image_file | | `P3-G01-chart.png` |
| source | khi có ảnh | `Tự soạn` / `Unsplash – tên tác giả – link` |

**Sheet `questions`**

| Cột | Bắt buộc | Ví dụ / quy ước |
|---|---|---|
| question_code | ✔ | `P5-001`, `CSDL-042` (duy nhất, dùng để đối chiếu khi sửa) |
| subject_code | ✔ | `TOEIC`, `CSDL`, `KTMT`, `MMT`, `LTHDT` |
| topic | ✔ | Đúng tên trong danh sách chủ đề |
| type | ✔ | `SINGLE_CHOICE` / `MULTIPLE_CHOICE` |
| part | TOEIC | `1`–`7` (skill suy ra từ part) |
| difficulty | ✔ | `EASY` / `MEDIUM` / `HARD` |
| group_code | nếu thuộc nhóm | `P3-G01` |
| order_in_group | nếu thuộc nhóm | `1`, `2`, `3` |
| content | ✔ | Nội dung câu hỏi (code để trong cặp ``` ``` ```) |
| option_a … option_d | ✔ (P2 chỉ A–C) | Nội dung phương án |
| correct | ✔ | `B` hoặc `A,C` (câu nhiều đáp án) |
| explanation | ✔ | Giải thích bằng tiếng Việt: vì sao đúng **và** vì sao các phương án khác sai |
| audio_file / image_file | P1, P2 | `P1-003.mp3`, `P1-003.jpg` |
| tags | | `văn phòng`, `du lịch`… |
| source | | `Tự soạn (AI hỗ trợ)` |

**Quy ước tệp media:** đặt tên trùng mã (`P1-003.jpg`); audio dạng MP3 mono 64 kbps; ảnh JPG rộng tối đa 1280px, nhỏ hơn 300KB.

---

## 6. Prompt mẫu soạn nháp bằng AI

Mỗi lần chỉ yêu cầu **10–15 câu cho 1 chủ đề**, và yêu cầu AI trả về dạng bảng có đúng thứ tự cột như tệp mẫu để dán thẳng vào Excel.

```
Bạn là giáo viên ra đề [TOEIC Reading Part 5 | môn Cơ sở dữ liệu cho sinh viên năm 3 CNTT].
Soạn 12 câu trắc nghiệm MỚI, không chép từ đề đã xuất bản, về chủ đề: [Từ loại | Chuẩn hóa – 3NF/BCNF].
Độ khó: 4 EASY, 6 MEDIUM, 2 HARD.
Yêu cầu:
- 4 phương án A–D, đúng 1 đáp án; các phương án nhiễu phải hợp lý (lỗi sai thường gặp của người học).
- Không có câu mơ hồ hay có 2 đáp án cùng chấp nhận được.
- Giải thích bằng tiếng Việt: nêu quy tắc/kiến thức, vì sao đáp án đúng, vì sao từng phương án còn lại sai.
- [TOEIC] Bối cảnh công việc/đời sống, từ vựng mức TOEIC 450–750; trải đều các bối cảnh.
Trả về bảng TSV với các cột: topic, difficulty, content, option_a, option_b, option_c, option_d, correct, explanation, tags
```

Với Part 3/4/6/7: yêu cầu AI soạn **nhóm** (lời thoại hoặc đoạn văn + 3–5 câu), trả về 2 bảng tương ứng với 2 sheet.

---

## 7. Checklist duyệt từng câu

- [ ] Đúng **một** đáp án (hoặc đúng tập đáp án với câu nhiều lựa chọn). **Tự giải lại câu trước khi xem đáp án của AI.**
- [ ] Không mơ hồ; phương án nhiễu hợp lý, không có phương án sai quá lộ.
- [ ] Nội dung chính xác về chuyên môn (với CNTT: đối chiếu giáo trình hoặc bài giảng); code chạy đúng như mô tả.
- [ ] Tiếng Anh tự nhiên, đúng ngữ pháp (có thể kiểm tra lại bằng một AI khác).
- [ ] Nhãn độ khó hợp lý; chủ đề gắn đúng.
- [ ] Giải thích đủ 3 ý: kiến thức, vì sao đúng, vì sao các phương án khác sai.
- [ ] Không trùng hoặc gần trùng câu đã có (lọc theo cột `content` trong Excel).
- [ ] Ảnh và audio đúng tên tệp, có nguồn.

**Rà soát chéo (Tuần 13):** chọn ngẫu nhiên 10% số câu, nhờ bạn cùng lớp hoặc GVHD làm thử và đối chiếu. Nếu tỉ lệ lỗi trên 5% thì duyệt lại toàn bộ chủ đề có lỗi.

---

## 8. Tài nguyên Listening (audio và ảnh)

- **Audio:** sinh bằng TTS từ cột `transcript` và nội dung phương án. Dùng **`edge-tts`** (miễn phí, có giọng Mỹ, Anh, Úc, Canada giống TOEIC) để sinh từng lượt thoại, sau đó ghép bằng **ffmpeg** (máy đã có), chèn khoảng lặng giữa các lượt và thêm câu đọc số thứ tự câu hỏi.
  - Viết script `tools/tts/generate_audio.py`: đọc sheet `groups` và `questions`, tách người nói theo tiền tố `M:` / `W:`, gán giọng khác nhau cho mỗi người nói, xuất `media/<code>.mp3`.
  - P2: đọc câu hỏi → khoảng lặng → "A. … B. … C. …" (P1 tương tự, 4 phương án).
  - Việc này làm trong Tuần 13, **sau khi lời thoại đã được duyệt**, để tránh phải sinh lại audio.
- **Ảnh P1:** lấy từ Unsplash hoặc Pexels (giấy phép miễn phí), ghi nguồn; ưu tiên ảnh có người làm hành động rõ ràng. Không dùng ảnh trích từ sách luyện thi.
- **Biểu đồ P3/P4:** tự vẽ bằng Excel hoặc Figma (lịch trình, bảng giá, sơ đồ tầng).

---

## 9. Ảnh hưởng tới kế hoạch code

| Hạng mục | Thay đổi |
|---|---|
| Sprint 0 | Thêm bảng `question_groups` + cột `questions.group_id`, `order_in_group` vào `schema.prisma` |
| Sprint 3 | Nhập câu hỏi hỗ trợ **tệp `.zip` gồm `.xlsx` + `media/`**, đọc 2 sheet `groups` và `questions`. Form câu hỏi và builder đề thi xử lý nhóm như một khối. Script seed dùng lại chính service nhập câu hỏi |
| Sprint 4 | Màn làm bài hiển thị theo nhóm: đoạn văn bên trái, câu hỏi bên phải; audio phát một lần cho cả nhóm. P1/P2 ẩn nội dung phương án. Màn xem lại hiện transcript |
| Sprint 5 | Thống kê điểm yếu theo `topic` dùng đúng danh sách chủ đề ở Mục 3.2 và 3.4 |
