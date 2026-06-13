---
name: dva-exam-notes
description: >-
  Ghi chú và cập nhật ôn thi AWS DVA-C02. Khi user hỏi/giải câu hỏi thi DVA,
  practice exam, đáp án DVA, tip nhớ DVA, hoặc AWS Developer Associate exam —
  giải thích câu hỏi rồi append note mới vào dva-notes.txt và dva-notes.html.
  Dùng cùng aws-dva-lab-helper cho lab hands-on.
---

# DVA Exam Notes — Ghi chú ôn thi

## File mục tiêu (BẮT BUỘC cập nhật)

| File | Đường dẫn |
|------|-----------|
| TXT | `dva-exam-notes/dva-notes.txt` |
| HTML | `dva-exam-notes/dva-notes.html` |

Đọc file hiện tại trước khi thêm — **không lặp** câu đã có (so khớp theo nội d
ung câu hỏi).

## Khi nào kích hoạt skill

- User gửi ảnh/câu hỏi practice DVA hoặc DVA-C02
- User hỏi "giải thích câu này", "đáp án đúng", "tip nhớ DVA"
- User yêu cầu tổng hợp / note lại kiến thức thi DVA

## Workflow mỗi câu hỏi

1. **Giải thích** cho user (bối cảnh → đáp án → vì sao sai các option khác)
2. **Kiểm tra trùng** trong `dva-notes.txt` — nếu đã có, chỉ bổ sung tip mới (không duplicate)
3. **Cập nhật TXT** — thêm entry vào đúng section service
4. **Cập nhật HTML** — thêm `<div class="card">` vào section tương ứng
5. **Cập nhật ngày** trong header TXT và meta HTML (`Cập nhật: YYYY-MM-DD`)

## Format entry TXT

```
[Câu] <tóm tắt câu hỏi 1 dòng>
  Yêu cầu đề: <bối cảnh đề hỏi gì — bullet ngắn>
  Yêu cầu đề: <đề cần giải quyết vấn đề gì>
  Đáp án: <đáp án đúng>
  Tip: <công thức nhớ / từ khóa bẫy>
  Lỗi hay nhầm: <option user hay chọn sai> (tùy chọn)
```

**Yêu cầu đề:** Mô tả đề thường hỏi gì — bối cảnh, constraint, đề cần gì (không chỉ đáp án). Giúp nhận dạng pattern khi gặp câu tương tự.

Đặt dưới section service phù hợp (API Gateway, Lambda, S3, …). Nếu service mới → tạo section mới cả TXT lẫn HTML.

## Format entry HTML

File HTML dùng **tab theo service** (theme sáng). Thêm card vào `<div id="slug" class="tab-panel">` tương ứng:

```html
<div class="card">
  <div class="q">Tóm tắt / tag câu (1 dòng)</div>
  <div class="req">
    <div class="req-title">Yêu cầu đề thường gặp</div>
    <ul>
      <li>Bối cảnh: <em>đề mô tả tình huống gì</em></li>
      <li>Đề hỏi: <em>cần cấu hình / chọn gì</em></li>
      <li>Bẫy: option hay chọn sai (nếu có)</li>
    </ul>
  </div>
  <div class="a">Đáp án đúng</div>
  <div class="diagram">...</div>
  <div class="tips"><span class="tip">Tip nhớ</span></div>
</div>
```

**Thứ tự card:** `q` (tag ngắn) → `req` (yêu cầu đề) → `a` → `diagram` → `tips`

**Sơ đồ:** Với câu có luồng hoạt động (auth, deploy, queue, failover…), thêm block `.diagram` + Mermaid (`flowchart` hoặc `sequenceDiagram`) để user hình dung — không chỉ giải thích chữ.

Nếu thêm service mới:
- Thêm `<button class="tab-btn" data-tab="slug">` trong `.tabs-bar`
- Thêm `<div id="slug" class="tab-panel">` với `.panel-header` + cards trong `<main>`
- Cập nhật badge số câu trong `.panel-header` nếu có

## Mapping service → section id HTML

| Service | id |
|---------|-----|
| API Gateway | api-gateway |
| Lambda | lambda |
| S3 | s3 |
| CloudFront | cloudfront |
| ECS/Fargate/ECR | ecs |
| Elastic Beanstalk | beanstalk |
| EC2/ASG/ELB | ec2 |
| RDS | rds |
| DynamoDB | dynamodb |
| SQS | sqs |
| Kinesis Streams/Firehose | kinesis |
| EFS/EBS | storage |
| ElastiCache Redis/Memcached | elasticache |
| VPC | vpc |
| IAM/Security | iam |
| CI/CD | cicd |
| CloudFormation | cloudformation |
| CFN/CloudTrail/SFN | other |
| X-Ray | xray |
| Kiến trúc | arch |

## Quy tắc nội dung

- Viết **tiếng Việt**, ngắn gọn, đúng logic exam
- Mỗi entry: **1 câu hỏi = 1 card** — không gộp nhiều câu không liên quan
- Tip dùng **công thức nhớ** hoặc **từ khóa trigger** (vd: "no stale" → Write Through)
- Nếu user chọn sai → thêm vào section "Lỗi hay nhầm" (TXT §21 + HTML `#mistakes`) khi pattern lặp lại
- Phân biệt hay bẫy → cập nhật tab `#compare` nếu có cặp mới quan trọng
- Lỗi hay nhầm → cập nhật tab `#mistakes` (thêm `<span class="item">`)

## Sau khi cập nhật file

Trong reply cho user:
- Giải thích đầy đủ câu hỏi
- Nói ngắn: "Đã cập nhật note vào `dva-exam-notes/dva-notes.txt` và `dva-notes.html`"
- Không cần paste toàn bộ file

## Tham chiếu

- Lab hands-on: skill `aws-dva-lab-helper`
- Chi tiết lab index: `.cursor/skills/aws-dva-lab-helper/labs-index.md`
