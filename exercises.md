# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tăng temperature từ 0.0 lên 1.5, câu trả lời trở nên đa dạng và sáng tạo hơn. Temperature thấp (0.0) cho kết quả ổn định, ít thay đổi giữa các lần gọi, trong khi temperature cao (1.5) có thể tạo ra cách diễn đạt mới lạ nhưng đôi khi dài dòng hoặc kém chính xác hơn. Temperature chủ yếu ảnh hưởng đến mức độ ngẫu nhiên khi sinh văn bản.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Em sẽ chọn temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời nhất quán, chính xác và hạn chế việc mô hình tự suy diễn hoặc trả lời quá sáng tạo, điều rất quan trọng trong các tình huống hỗ trợ người dùng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**

> Theo bảng giá API, GPT-4o thường đắt hơn GPT-4o-mini khoảng 8–10 lần (tùy input/output token). Với workload khoảng 30.000 request/ngày và 350 output token mỗi request, sự chênh lệch chi phí sẽ rất đáng kể. GPT-4o phù hợp với các tác vụ yêu cầu lập luận phức tạp như phân tích tài liệu pháp lý hoặc y tế, trong khi GPT-4o-mini phù hợp cho chatbot FAQ, phân loại văn bản hoặc tóm tắt nội dung với chi phí thấp.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Hai phản hồi có sự khác biệt rõ rệt về cách diễn đạt. Persona "giáo viên tiểu học" sử dụng câu ngắn, từ ngữ đơn giản và nhiều ví dụ gần gũi để trẻ em dễ hiểu. Trong khi đó, persona "chuyên gia tài chính" dùng nhiều thuật ngữ kỹ thuật, giải thích chi tiết hơn về cơ chế đồng thuận, phân tán dữ liệu và tính bảo mật. Điều này cho thấy system prompt định hướng rất mạnh về phong cách, mức độ chuyên sâu và đối tượng mà mô hình hướng đến.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Kết quả đếm bằng tiktoken thường cao hơn ước lượng theo công thức "số từ / 0.75" khoảng 10–20% (tùy đoạn văn). Nguyên nhân là tiếng Việt có nhiều dấu thanh, từ ghép và ký tự Unicode khiến tokenizer phải chia thành nhiều token hơn. Vì vậy, hai đoạn văn có cùng số từ nhưng tiếng Việt thường tiêu tốn nhiều token hơn tiếng Anh.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt hữu ích khi mô hình tạo câu trả lời dài hoặc cần thời gian suy luận, vì người dùng nhìn thấy kết quả xuất hiện ngay lập tức nên cảm giác phản hồi nhanh hơn và giảm thời gian chờ. Ngược lại, non-streaming phù hợp với các tác vụ ngắn như phân loại văn bản, trả về JSON hoặc các API backend cần nhận toàn bộ kết quả trước khi tiếp tục xử lý.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm tải cho hệ thống bằng cách tăng dần khoảng thời gian chờ giữa các lần retry, tạo điều kiện để server phục hồi khi đang quá tải. Nếu tất cả client đều retry sau đúng 1 giây, chúng có thể đồng loạt gửi yêu cầu trở lại, tạo ra hiện tượng "retry storm" khiến server tiếp tục quá tải và khó phục hồi hơn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> "Bạn là trợ lý AI hỗ trợ học lập trình Python và Machine Learning. Hãy trả lời bằng tiếng Việt, giải thích ngắn gọn, rõ ràng, ưu tiên ví dụ minh họa và hướng dẫn từng bước khi người dùng gặp lỗi. Nếu không chắc chắn, hãy nói rõ mức độ không chắc chắn thay vì suy đoán."
EM yêu cầu "trả lời bằng tiếng Việt" để phù hợp với người dùng mục tiêu và "giải thích ngắn gọn" để tránh câu trả lời lan man. Ngoài ra, việc yêu cầu mô hình thừa nhận khi không chắc chắn giúp tăng độ tin cậy và giảm nguy cơ tạo thông tin sai.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất hiện nay là trợ lý chỉ nhớ được lịch sử hội thoại ngắn và không có bộ nhớ dài hạn nên dễ quên thông tin quan trọng của người dùng. Một cải thiện phù hợp là bổ sung cơ chế lưu lịch sử hoặc vector database để truy xuất các cuộc hội thoại liên quan. Khi có câu hỏi mới, hệ thống sẽ tìm kiếm các đoạn hội thoại phù hợp và đưa vào prompt trước khi gọi API, giúp câu trả lời nhất quán và có tính cá nhân hóa hơn.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
