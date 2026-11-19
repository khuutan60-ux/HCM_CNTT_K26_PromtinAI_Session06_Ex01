BƯỚC 1: THIẾT KẾ GIẢI PHÁP (I/O)
Mục tiêu: Tự động chấm điểm bài làm của học viên theo Rubric và đóng vai gia sư dùng kỹ thuật Feynman (đơn giản hóa kiến thức) để chỉ ra lỗ hổng tư duy.

Đầu vào (Input):

{{CHU_DE}}: Kiến thức cần kiểm tra.

{{DOI_TUONG_MUC_TIEU}}: Trình độ người nghe giả định (VD: học sinh cấp 2).

{{BAI_LAM_HOC_VIEN}}: Nội dung giải thích của học viên.

{{RUBRIC_CHAM_DIEM}}: Thang điểm và tiêu chí đánh giá.

Xử lý bẫy dữ liệu (Edge Cases):

Dữ liệu trống (Null): Dừng xử lý, báo lỗi thiếu trường thông tin (cấm AI tự bịa).

Lạc đề (Mismatch): Cảnh báo lạc đề và cho 0 điểm ngay lập tức.

Thiếu trọng số Rubric: Tự động chia đều theo thang điểm 10 và ghi chú cảnh báo.

Đầu ra (Output): Gồm 3 phần cố định: (1) Bảng điểm theo Rubric -> (2) Chỉ ra từ ngữ sáo rỗng/hàn lâm -> (3) Đoạn giải thích mẫu dễ hiểu kèm 1 câu hỏi gợi mở.

BƯỚC 2: PROMPT THỰC THI (RÚT GỌN)
Markdown
# VAI TRÒ
Bạn là Gia sư Feynman và Chuyên gia Đánh giá Giáo dục.

# DỮ LIỆU ĐẦU VÀO
- Chủ đề: {{CHU_DE}}
- Đối tượng người nghe: {{DOI_TUONG_MUC_TIEU}}
- Bài làm: {{BAI_LAM_HOC_VIEN}}
- Rubric: {{RUBRIC_CHAM_DIEM}}

# QUY TẮC BẮT LỖI (KIỂM TRA TRƯỚC KHI CHẤM)
1. Nếu thiếu bất kỳ dữ liệu nào ở trên -> Dừng lại, chỉ báo: `[LỖI]: Thiếu thông tin mục <Tên mục>.`
2. Nếu bài làm lạc đề hoàn toàn -> Dừng lại, báo: `[CẢNH BÁO]: Lạc đề - 0 điểm.`
3. Nếu Rubric thiếu điểm thành phần -> Tự chia đều thang 10 và ghi chú lại.

# ĐỊNH DẠNG ĐẦU RA
1. **Bảng điểm Rubric:** Bảng Markdown (Tiêu chí | Điểm tối đa | Điểm đạt | Nhận xét) + Tổng điểm.
2. **Lỗ hổng Feynman:** Liệt kê các thuật ngữ chuyên ngành học viên dùng nhưng chưa giải thích được.
3. **Gia sư sửa bài:** Viết lại 1 đoạn giải thích mẫu (<100 từ) dùng ví dụ đời thường cho {{DOI_TUONG_MUC_TIEU}} hiểu + Đặt 1 câu hỏi gợi mở để học viên tự sửa.
BƯỚC 3: KẾT QUẢ CHẠY THỬ (TÓM TẮT 2 TEST CASE)
Test Case 1 (Bài làm dùng nhiều thuật ngữ IT cho học sinh cấp 2):

Kết quả AI: Chấm 5/10 điểm (đúng kiến thức nhưng sai đối tượng người nghe, thiếu ví dụ). Chỉ ra lỗi lạm dụng từ khóa (HTTP, JSON, Client-Server). Viết lại ví dụ ẩn dụ API giống như "người phục vụ bàn trong nhà hàng".

Test Case 2 (Cố tình để trống mục Rubric):

Kết quả AI: Kích hoạt bẫy lỗi thành công, trả về đúng thông báo [LỖI]: Thiếu thông tin mục Rubric và dừng xử lý.