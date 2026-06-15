---
name: dva-quiz-web
description: >-
  Thêm/cập nhật câu hỏi trắc nghiệm tương tác lên trang web quiz DVA. Kích hoạt khi
  user yêu cầu tạo quiz, thêm câu luyện tập lên web, trang MCQ 4 đáp án, hoặc sau
  buổi luyện công thức/tính toán (RCU WCU, pricing, sizing) cần đưa thành bài tập
  có đáp án cố định. Không thay thế skill dva-exam-notes — dùng song song khi cần.
---

# DVA Practice Quiz — Trang web luyện tập

## Cấu trúc thư mục

| Vai trò | Đường dẫn |
|---------|-----------|
| **Hub** (danh sách quiz) | `dva-exam-notes/quiz/index.html` |
| **Từng chủ đề** | `dva-exam-notes/quiz/<service>-<topic>.html` |
| Ghi chú thi (khác quiz) | `dva-exam-notes/dva-notes.html` |

**Quy ước tên file quiz:** `<service>-<topic>.html` — chữ thường, nối bằng gạch ngang.

Ví dụ: `dynamodb-rcu-wcu.html`, `lambda-concurrency.html`, `s3-storage-class.html`

## Khi nào thêm lên trang quiz (KHÔNG phải mọi câu đều vào quiz)

### ✅ Nên thêm vào quiz web

| Tín hiệu | Ví dụ |
|----------|-------|
| User **yêu cầu rõ** | "tạo trang web câu hỏi", "thêm vào quiz", "luyện tập MCQ" |
| **Công thức / tính toán** có 1 đáp án số rõ | RCU, WCU, Kinesis shard, Lambda concurrent |
| **So sánh A vs B** với đáp án cố định | strong vs eventual RCU, !Ref vs !GetAtt |
| User **làm sai lặp lại** cùng pattern | Scan RCU tính tổng KB÷4, WCU eventual |
| Buổi chat có **≥5 câu luyện** cùng chủ đề | Sau block practice DynamoDB → gom vào 1 quiz file |
| User muốn **tự kiểm tra** trước thi | Drill nhanh, chấm đúng/sai ngay |

### ❌ KHÔNG thêm vào quiz (chỉ dùng dva-exam-notes)

| Tín hiệu | Lý do |
|----------|-------|
| Câu **scenario dài** nhiều bước Console/CLI | Ghi chú + lab, không MCQ 4 đáp án gọn |
| **Mở đáp án** / kiến trúc tổng thể | Không có 1 đáp án duy nhất rõ ràng |
| Chỉ giải **1 câu practice exam** lẻ | → `dva-notes.txt` + card HTML |
| User chỉ hỏi "giải thích" / "tip nhớ" | Note, không quiz |
| Nội dung **lab hands-on** | Skill `aws-dva-lab-helper` |

### Cả hai (notes + quiz)

Khi vừa là câu thi quan trọng **vừa** có dạng bài tập lặp được:

1. `dva-exam-notes` → giải thích + card ghi chú
2. `dva-quiz-web` → biến thành 1+ câu MCQ (nếu phù hợp tiêu chí ✅ ở trên)

## Workflow thêm câu vào quiz có sẵn

1. **Đọc file quiz** target (vd. `quiz/dynamodb-rcu-wcu.html`)
2. **Kiểm tra trùng** — so nội dung `q` trong mảng `QUESTIONS`; không duplicate
3. **Thêm object** vào cuối `QUESTIONS` (trước `];`):

```javascript
{
  tag: "rcu",           // slug: wcu | rcu | mix | tên service
  tagLabel: "RCU",      // hiển thị trên card
  q: "Câu hỏi đầy đủ?",
  options: ["A text", "B text", "C text", "D text"],  // đúng 4 phần tử
  correct: 1,           // index 0–3 (A=0, B=1, C=2, D=3)
  explain: "Giải thích nhiều dòng.\nDòng 2.",
  calc: "Công thức / bước tính ngắn"
}
```

4. **Cập nhật hub** `quiz/index.html` — số câu trong `.meta` (vd. `22 câu` → `23 câu`)
5. **Cập nhật link** trong `dva-notes.html` tab service tương ứng (nếu có card quiz)
6. Reply user: nêu file đã sửa + số câu mới

## Workflow tạo quiz chủ đề mới

1. **Copy template** từ `quiz/dynamodb-rcu-wcu.html` (giữ CSS/JS `render`, `onSelect`, `QUESTIONS`)
2. **Đặt tên file** `quiz/<service>-<topic>.html`
3. Sửa `<title>`, `<header>`, box công thức (nếu có), mảng `QUESTIONS`
4. Footer quiz:

```html
<a href="index.html">← Tất cả quiz</a> ·
<a href="../dva-notes.html">Ghi chú DVA</a>
```

5. **Thêm card** vào `quiz/index.html`:

```html
<a class="quiz-card" href="<file>.html">
  <span class="badge">Service</span>
  <h2>Tiêu đề quiz</h2>
  <p>Mô tả ngắn phạm vi.</p>
  <div class="meta">N câu · Mở quiz →</div>
</a>
```

6. (Tuỳ chọn) Thêm link từ tab service trong `dva-notes.html` → `quiz/index.html` hoặc file cụ thể

## Quy tắc nội dung câu hỏi

- **Tiếng Việt**, đúng logic exam DVA-C02
- **Đúng 4 đáp án** — 1 đúng, 3 sai có lý (bẫy hay gặp)
- `explain`: vì sao đúng + vì sao bẫy phổ biến sai
- `calc`: công thức / bước tính (đặc biệt câu tính toán)
- **Không** copy nguyên văn câu có bản quyền practice exam — paraphrase
- Tag CSS: thêm class `.tag-<slug>` trong `<style>` nếu cần màu mới

## Phân biệt với dva-exam-notes

| | dva-exam-notes | dva-quiz-web |
|--|----------------|--------------|
| File | `dva-notes.txt`, `dva-notes.html` | `quiz/*.html` |
| Dạng | Ghi chú, diagram, tip | MCQ click → chấm + giải thích |
| Kích hoạt | Mọi câu thi DVA | Luyện tập có đáp án cố định / user xin quiz |
| Trùng lặp | Tránh duplicate card | Tránh duplicate `q` trong cùng file quiz |

## Sau khi cập nhật

Trong reply:

- Nêu file quiz + hub đã sửa
- Số câu thêm / tổng
- Link mở local: `dva-exam-notes/quiz/index.html`

Không paste toàn bộ mảng QUESTIONS trừ khi user yêu cầu.

## Tham chiếu

- Ghi chú thi: skill `dva-exam-notes`
- Lab: skill `aws-dva-lab-helper`
