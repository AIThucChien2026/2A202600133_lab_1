# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> sau bốn phản hồi tôi thấy quy luật là khi temperature càng thấp (0.0), phản hồi càng mang tính xác định (deterministic), chính xác và lặp lại giống nhau. Khi temperature tăng lên (0.5 đến 1.0), câu trả lời trở nên đa dạng, tự nhiên và sáng tạo hơn. Tuy nhiên, nếu temperature lên quá cao (như 1.5), mô hình bắt đầu sinh thông tin lộn xộn, lan man, hoặc thậm chí bịa đặt (hallucination).

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Nên đặt temperature ở mức rất thấp (khoảng 0.0 đến 0.3). Vì chatbot hỗ trợ khách hàng cần độ chính xác tối đa. Nó phải trả lời nhất quán và trung thực theo tài liệu gốc của công ty. Bất kỳ sự "sáng tạo" nào (như báo sai giá, quy định đổi trả,...) đều có thể gây hậu quả nghiêm trọng về cả uy tín và tổn thất cho doanh nghiệp.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> Tổng lượng token mỗi ngày = 10.000 user * 3 lần gọi * 350 token = 10.500.000 tokens (output).
> Chi phí GPT-4o = (10.500.000 / 1000) * $0.010 = $105/ngày.
> Chi phí GPT-4o-mini = (10.500.000 / 1000) * $0.0006 = $6,3/ngày.
> Tỷ lệ: $105 / $6,3 ≈ 16,67. Vậy GPT-4o đắt hơn GPT-4o-mini khoảng 16,67 lần cho hệ thống này.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> - **GPT-4o xứng đáng:** Ở những trường hợp cần khả năng suy luận logic chuyên sâu, phức tạp, hoặc yêu cầu độ chính xác cực cao như: chẩn đoán bệnh án y tế, phân tích điều khoản hợp đồng phòng rủi ro pháp lý, hoặc sinh cấu trúc mã code phần mềm quy mô lớn.
> - **GPT-4o-mini là lựa chọn tốt hơn:** Xử lý tác vụ logic đơn giản nhưng số lượng yêu cầu gọi vào (volume) cực kì lớn: ví dụ phân loại nội dung đánh giá mua hàng (sentiment analysis), phân loại email rác, tóm tắt/dịch thuật văn bản hàng loạt cho website thương mại điện tử.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> **Streaming cực kỳ quan trọng** trong các ứng dụng có giao diện đàm thoại (như giao diện ChatGPT hay chatbot website). Việc hiển thị ngay lập tức từng từ (token) đang được stream về giúp giảm cảm giác chờ đợi phản hồi (perceived latency) của người dùng từ vài giây (nếu đợi gen xong cả câu) xuống mức gần như tức thời. Ngược lại, **non-streaming lại phù hợp hơn** cho các quy trình xử lý theo lô dưới nền (background batch-processing) như đọc một file dữ liệu dài rồi trả ra chỉ định dạnh chuẩn JSON nguyên vẹn, hoặc quy trình lưu và sinh ra kết quả báo cáo PDF cuối ngày — những hệ thống này chỉ lấy kết quả khi chuỗi gen hoàn thành 100%.


## Danh Sách Kiểm Tra Nộp Bài
- [x] Tất cả tests pass: `pytest tests/ -v`
- [x] `call_openai` đã triển khai và kiểm thử
- [x] `call_openai_mini` đã triển khai và kiểm thử
- [x] `compare_models` đã triển khai và kiểm thử
- [x] `streaming_chatbot` đã triển khai và kiểm thử
- [x] `retry_with_backoff` đã triển khai và kiểm thử
- [x] `batch_compare` đã triển khai và kiểm thử
- [x] `format_comparison_table` đã triển khai và kiểm thử
- [x] `exercises.md` đã điền đầy đủ
- [x] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 
