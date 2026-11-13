# Bài tập tổng hợp: Cẩm nang sử dụng AI hiệu quả cho tân sinh viên

**Nội dung áp dụng:** hiểu bản chất LLM, kiểm soát token, chống ảo giác và cấu trúc prompt CREATE, kết hợp kỹ thuật chia nhỏ prompt (Prompt Chaining).
---

## (1) Sơ đồ xâu chuỗi prompt

```
┌────────────────────────────────────────────────────┐
│ P0. ĐỊNH HƯỚNG: vai trò, đối tượng, giọng văn,     │
│     dàn ý 3 phần, giới hạn độ dài                  │
└─────────────────────────┬──────────────────────────┘
                          ▼
┌────────────────────────────────────────────────────┐
│ P1. PHẦN 1: Cách AI đoán từ (LLM, token)           │
│ Đầu ra → NGƯỜI KIỂM TRA → rút tóm tắt 3 dòng       │
└─────────────────────────┬──────────────────────────┘
                          ▼  (chuyển sang: dàn ý + tóm tắt P1)
┌────────────────────────────────────────────────────┐
│ P2. PHẦN 2: 3 cách nhận diện ảo giác               │
│ Đầu ra → NGƯỜI KIỂM TRA → rút tóm tắt 3 dòng       │
└─────────────────────────┬──────────────────────────┘
                          ▼  (chuyển sang: dàn ý + tóm tắt P1, P2)
┌────────────────────────────────────────────────────┐
│ P3. PHẦN 3: Cấu trúc prompt CREATE                 │
│ Đầu ra → NGƯỜI KIỂM TRA                            │
└─────────────────────────┬──────────────────────────┘
                          ▼
┌────────────────────────────────────────────────────┐
│ P4. GHÉP & HIỆU CHỈNH: nối 3 phần, thêm mở bài,    │
│     kết bài, thống nhất giọng văn                  │
│ → NGƯỜI RÀ SOÁT CUỐI (sự kiện, lỗi, ảo giác)       │
│ → CẨM NANG HOÀN CHỈNH                              │
└────────────────────────────────────────────────────┘
```

**Vì sao chia nhỏ và có bước tóm tắt:**

- Mỗi prompt chỉ làm một việc nên kết quả đúng trọng tâm hơn.
- Chỉ mang theo bản tóm tắt thay vì toàn bộ nội dung cũ giúp tiết kiệm token và tránh AI quên đầu hội thoại.
- Người kiểm tra sau mỗi bước chính là chốt Human-in-the-loop chống ảo giác.

---

## (2) Nội dung các prompt thành phần

### P0: Định hướng

```
Bạn là mentor lập trình thân thiện. Cùng tôi viết "Cẩm nang học lập trình
cùng AI an toàn và hiệu quả" cho sinh viên năm nhất, gồm 3 phần:
(1) Cách AI đoán từ, (2) 3 cách nhận diện ảo giác, (3) Khung CREATE.
Tiếng Việt, câu ngắn, giải thích bằng ví dụ đời thường, tổng dưới 700 từ.
Chưa viết nội dung, chỉ đề xuất dàn ý mỗi phần 1-2 dòng.
```

### P1: Phần 1 (Cách AI đoán từ)

```
[C] Mentor lập trình viết cho tân sinh viên.
[R] Viết Phần 1: LLM hoạt động thế nào (đoán token tiếp theo theo xác suất,
    không tra cứu sự thật, giới hạn cửa sổ ngữ cảnh, code đúng "vẻ" chưa chắc đúng).
[E] Ví dụ: gợi ý từ khi nhắn tin.
[A] Dưới 200 từ, không công thức toán.
[T] 2 đoạn ngắn + hộp "Mẹo kiểm soát token" (3 gạch đầu dòng).
[E] Kết bằng 1 câu dặn dễ nhớ.
```

### P2: Phần 2 (3 cách nhận diện ảo giác)

```
[C] Mentor lập trình viết cho tân sinh viên.
[R] Viết Phần 2: 3 cách nhận diện ảo giác của AI khi học lập trình.
[E] Mỗi cách có 1 ví dụ ngắn (vd: AI bịa tên hàm).
[A] Dưới 220 từ, không bịa số liệu. Giữ giọng văn theo tóm tắt Phần 1.
[T] Mỗi cách gồm: tên, dấu hiệu, việc cần làm.
[E] Thêm 1 dòng "Dấu hiệu đáng ngờ chung".
Tóm tắt Phần 1: <dán>
```

### P3: Phần 3 (Khung CREATE)

```
[C] Mentor lập trình viết cho tân sinh viên.
[R] Viết Phần 3: giới thiệu khung CREATE và 1 prompt mẫu nhờ AI giải thích lỗi Python.
[E] Đưa cặp so sánh: prompt mơ hồ và prompt theo CREATE.
[A] Dưới 250 từ. Prompt mẫu phải yêu cầu AI nói rõ chỗ không chắc chắn.
[T] Bảng (chữ cái | ý nghĩa kèm ví dụ) rồi khối prompt mẫu.
[E] Kết bằng lời nhắc: luôn tự kiểm tra kết quả.
Tóm tắt Phần 1-2: <dán>
```

### P4: Ghép và hiệu chỉnh

```
Ghép 3 phần dưới đây thành cẩm nang liền mạch: thêm tiêu đề, mở bài 2-3 câu,
kết bài 3 dòng; thống nhất giọng văn, bỏ ý trùng. Giữ nguyên nội dung kỹ thuật,
không thêm số liệu hay nguồn mới. Tổng dưới 700 từ. Cuối cùng liệt kê chỗ
bạn kém chắc chắn.
<dán 3 phần đã duyệt>
```

---

## (3) Cẩm nang hoàn chỉnh

# Cẩm nang học lập trình cùng AI: An toàn và hiệu quả

*Dành cho tân sinh viên. AI là trợ giảng nhanh nhưng đôi khi nói rất tự tin điều sai. Cẩm nang này giúp bạn dùng nó đúng cách.*

## Phần 1: AI đoán từ như thế nào?

Hãy nghĩ đến tính năng gợi ý từ khi bạn gõ tin nhắn. Mô hình ngôn ngữ lớn (LLM) như ChatGPT hay Claude cũng làm việc tương tự, nhưng ở quy mô khổng lồ. Nó chia văn bản thành những "mảnh chữ" gọi là **token**, rồi liên tục đoán mảnh tiếp theo có xác suất cao nhất dựa trên hàng tỷ câu nó đã học.

Điều này có nghĩa là AI **không tra cứu sự thật** mà tạo ra câu trả lời *nghe có vẻ hợp lý*. Với lập trình, đoạn code AI viết có thể trông rất chuẩn nhưng vẫn chạy sai.

> **Mẹo kiểm soát token**
>
> - Mỗi cuộc trò chuyện có giới hạn độ dài (cửa sổ ngữ cảnh). Chat quá dài, AI dễ quên phần đầu.
> - Mỗi chủ đề mới nên mở cuộc trò chuyện mới.
> - Hỏi ngắn, rõ, chỉ dán đoạn code liên quan thay vì cả dự án.

*Nhớ: AI đoán chữ giỏi, nhưng không đảm bảo đúng.*

## Phần 2: 3 cách nhận diện ảo giác

**Cách 1: Kiểm chứng chéo.**

- *Dấu hiệu:* AI nêu khẳng định cụ thể, nhưng bạn chưa thấy ở đâu khác.
- *Việc cần làm:* đối chiếu với tài liệu chính thức hoặc giáo trình.

**Cách 2: Chạy thử và tra cứu tên.**

- *Dấu hiệu:* hàm, thư viện hoặc lệnh có tên lạ (ví dụ AI "chế" ra một hàm không có trong Python).
- *Việc cần làm:* chạy thử code, tìm tên đó trong tài liệu chính thức.

**Cách 3: Hỏi lại và xem độ nhất quán.**

- *Dấu hiệu:* hỏi cùng câu theo cách khác thì ra câu trả lời khác.
- *Việc cần làm:* yêu cầu AI nêu mức độ chắc chắn và chỗ chưa chắc; nếu không giải thích được, hãy nghi ngờ.

*Dấu hiệu đáng ngờ chung:* giọng quá tự tin, số liệu và trích dẫn quá cụ thể nhưng không kiểm chứng được.

## Phần 3: Viết prompt tốt với khung CREATE

| Chữ | Ý nghĩa | Ví dụ |
|---|---|---|
| **C**haracter | Vai trò cho AI | "Bạn là giảng viên Python" |
| **R**equest | Việc cần làm | "Giải thích lỗi trong code dưới đây" |
| **E**xamples | Ví dụ mẫu | "Trả lời giống ví dụ này..." |
| **A**djustments | Ràng buộc | "Dưới 150 từ, không dùng thuật ngữ khó" |
| **T**ype of output | Định dạng | "Gồm: nguyên nhân, cách sửa, code đã sửa" |
| **E**xtras | Bổ sung | "Nói rõ chỗ bạn không chắc chắn" |

**Prompt mơ hồ:** "Sửa code giúp mình."

**Prompt theo CREATE:**

```
Bạn là giảng viên Python cho người mới học. Hãy giải thích vì sao đoạn code
sau báo lỗi IndexError và cách sửa. Dưới 150 từ, ngôn ngữ dễ hiểu.
Trình bày theo: nguyên nhân, cách sửa, code đã sửa. Cuối cùng nói rõ
nếu có chỗ bạn không chắc chắn. <dán code>
```

*Nhớ: prompt tốt giúp AI trả lời đúng hơn, nhưng bạn vẫn phải tự kiểm tra kết quả.*

## Kết bài

AI là người bạn học nhanh nhưng không phải người thầy không bao giờ sai. Hiểu cách nó hoạt động, biết nhận ra ảo giác và viết prompt rõ ràng sẽ giúp bạn học nhanh hơn mà vẫn tự chủ.

---
