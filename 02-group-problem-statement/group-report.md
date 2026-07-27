# Group Report — Day 02

## Thành viên nhóm

| STT | Họ và tên | Mã học viên | Vai trò trong nhóm |
|---:|---|---|---|
| 1 | Nguyễn Quốc Anh | 01100 | Thành viên |
| 2 | Hoàng Bảo Huy | 01440 | Thành viên |
| 3 | Nguyễn Xuân Quang | 01776 | Trưởng nhóm |
| 4 | Lê Quý Thành | 01168 | Thành viên |

## Bối cảnh

Nhóm phân tích quy trình Chăm sóc khách hàng và Vận hành giao hàng tại một sàn thương mại điện tử hoặc nhà bán hàng lớn. Candidate cuối cùng được thu hẹp vào việc xử lý ticket về **đơn hàng bị delay hoặc có dấu hiệu thất lạc**, không bao gồm mọi câu hỏi “đơn hàng của tôi đang ở đâu” (WISMO) thông thường.

Các số liệu trong báo cáo là baseline sơ bộ được các thành viên nêu và challenge trong buổi pitching. Trong phạm vi bài mô phỏng, chúng được dùng để so sánh workflow chứ không được xem là số liệu vận hành đã xác nhận.

---

# 02 — Group Problem Statement

## Group convergence

### Nhật ký buổi pitching và hội tụ

Buổi họp bắt đầu bằng vòng pitching: mỗi người có 2–3 phút trình bày candidate mạnh nhất theo cấu trúc actor → workflow → bottleneck → impact. Sau mỗi pitch, các thành viên còn lại hỏi ngược về evidence, metric, data access và khả năng giải bằng Rule trước khi dùng AI. Nhóm chưa vote ngay mà ghi các candidate lên bảng, gom những bài có cùng pattern rồi mới shortlist.

| Người pitch | Candidate nổi bật | Actor | Bottleneck / câu hỏi bị challenge |
|---|---|---|---|
| Nguyễn Quốc Anh | Ticket WISMO; khiếu nại vỡ hỏng; đối soát lệch phí | CS, Claims, Kế toán | Ticket WISMO thường có thật sự cần AI hay dashboard tracking đã đủ? |
| Hoàng Bảo Huy | Ticket delay/thất lạc; phân loại complaint; tổng hợp SLA | CS, CS QA, Ops | “Nguyên nhân trễ” có dữ liệu nguồn hay chỉ là suy đoán của CS? |
| Nguyễn Xuân Quang | Ticket giao trễ; chuyển tuyến ticket; hồ sơ hư hỏng | CS, Shipping, Claims | Pain nằm ở soạn phản hồi hay ở dữ liệu tracking rời rạc và không đồng nhất? |
| Lê Quý Thành | Chuẩn hóa địa chỉ; điều hướng ngõ hẻm; dự đoán giao thành công/hư hỏng | Shipper, Ops, khách hàng | Có đủ dữ liệu lịch sử và phạm vi có quá lớn cho một pilot nhỏ không? |

### Diễn biến thảo luận

1. Nhóm ban đầu thấy cả WISMO và ticket delay/thất lạc đều thuộc nhóm “khách hỏi đơn ở đâu”.
2. Sau khi walkthrough hai tình huống, nhóm nhận ra WISMO thường chỉ cần trả trạng thái hiện tại, còn delay/thất lạc cần tra nhiều nguồn, so SLA và cân nhắc escalation.
3. Nhóm challenge phương án AI viết câu trả lời ngay vì dữ liệu carrier có thể chậm hoặc mâu thuẫn.
4. Nhóm thống nhất pain chính là bước nối dữ liệu và xác định hướng xử lý; viết phản hồi chỉ là bước phụ.
5. Candidate được thu hẹp thành ticket delay hoặc có dấu hiệu thất lạc cần điều tra, không bao gồm toàn bộ WISMO.

### Gom cluster

| Cluster | Candidates included | Pattern chung |
|---|---|---|
| Tra cứu và phản hồi trạng thái đơn | WISMO, delay/thất lạc, giải thích giao trễ | CS phải nối dữ liệu từ nhiều hệ thống rồi giải thích cho khách |
| Phân loại và xử lý khiếu nại | Gắn tag complaint, vỡ hỏng, root-cause | Đọc dữ liệu phi cấu trúc và chuyển đúng owner |
| Đối soát và báo cáo vận hành | Lệch phí, SLA carrier, KPI shipping | Gom dữ liệu có cấu trúc và đối chiếu theo quy tắc |
| Tối ưu giao hàng | Chuẩn hóa địa chỉ, re-routing, dự đoán giao thành công | Cần dữ liệu lớn, tích hợp vận hành và mô hình dự đoán |

### Shortlist

| Candidate | Vì sao vào shortlist | Rủi ro / điều chưa rõ |
|---|---|---|
| Ticket delay/thất lạc | Nhiều pitch hội tụ vào cùng pain; actor, workflow và metric rõ | Baseline nêu trong buổi họp chưa thống nhất; dữ liệu carrier có thể chậm hoặc mâu thuẫn |
| Phân loại complaint shipping | Tần suất cao, taxonomy có thể chuẩn hóa, pilot nhỏ được | Rule/form có thể đã giải phần lớn; chưa có tỷ lệ chuyển sai hiện tại |
| Đối soát SLA/phí vận chuyển | Impact thời gian và tiền rõ, dữ liệu có cấu trúc | Có thể chỉ cần script/rule; AI không tạo thêm nhiều giá trị |
| Dự đoán giao thành công | Có tiềm năng tác động lớn đến tỷ lệ giao thất bại | Thiếu dữ liệu, phạm vi và rủi ro thiên lệch quá lớn cho lab |

### Score để đồng thuận

Thang điểm 1–5, dùng để buộc nhóm giải thích lựa chọn, không phải kết quả đo khách quan.

| Candidate | Actor rõ | Workflow rõ | Pain có evidence | Impact đo được | Làm trong lab | So sánh R/W/A | Nhóm hiểu domain | Tổng |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Ticket delay/thất lạc | 5 | 5 | 4 | 5 | 5 | 5 | 5 | **34** |
| Phân loại complaint | 5 | 5 | 3 | 4 | 5 | 5 | 5 | **32** |
| Đối soát SLA/phí | 5 | 5 | 4 | 5 | 5 | 4 | 4 | **32** |
| Dự đoán giao thành công | 4 | 3 | 2 | 4 | 2 | 4 | 3 | **22** |

**Candidate nhóm chọn:** Xử lý ticket đơn hàng delay hoặc có dấu hiệu thất lạc.

**Vì sao chọn:**

- Nhiều pitch độc lập trong buổi họp hội tụ vào cùng pain tra cứu trạng thái và xử lý giao trễ.
- Có actor cụ thể là CS Agent và handoff rõ sang Shipping/carrier.
- Bottleneck nằm ở một chuỗi bước có thể vẽ và đo thời gian.
- Có thể dùng cùng một bộ scenario trong buổi role-play để so sánh No AI, Rule, Workflow và Agent.
- Có thể pilot bằng dữ liệu đã ẩn thông tin cá nhân mà không kết nối hệ thống thật.

**Vì sao không chọn các candidate còn lại:**

- Phân loại complaint có thể giải phần lớn bằng form, taxonomy và rule; chưa có baseline chuyển sai.
- Đối soát phù hợp với script/rule hơn LLM vì đầu ra đúng/sai khá rõ.
- Dự đoán giao thành công cần dữ liệu lịch sử lớn, có rủi ro thiên lệch với khách hàng và vượt phạm vi lab.

## Quick validation

Ngay sau khi chọn candidate, nhóm thực hiện quick validation theo hình thức **scenario walkthrough và role-play**. Nhóm chuẩn bị ba scenario ngắn: WISMO bình thường, đơn quá SLA và đơn mất checkpoint. Trong mỗi vòng, bốn thành viên lần lượt đóng vai khách hàng, CS Agent, Shipping Coordinator và người quan sát. Người quan sát ghi lại số bước, điểm phải hỏi lại, chỗ cần handoff và rủi ro nếu AI trả lời sai.

Sau mỗi vòng, từng thành viên trả lời ba câu hỏi: pain nằm ở bước nào, Rule có đủ không và bước nào bắt buộc con người quyết định. Cách validation này phù hợp với một bài mô phỏng và tạo điều kiện để mọi thành viên pitch, challenge và thay đổi quan điểm. Kết quả chỉ là tín hiệu định tính của buổi thảo luận, không đại diện cho dữ liệu vận hành thật.

| Nguồn | Số người / số mẫu | Tín hiệu xác nhận | Tín hiệu phản bác | Nhóm sửa problem thế nào |
|---|---:|---|---|---|
| Scenario walkthrough | 3 scenario | Case quá SLA và mất checkpoint có nhiều bước tra cứu/handoff hơn WISMO thường | WISMO bình thường có thể giải bằng tracking link hoặc template | Loại WISMO bình thường khỏi scope chính |
| Role-play luân phiên | 4 thành viên, 3 vòng | Vai CS phải nối WMS, carrier và SLA trước khi biết nên trả lời hay chuyển Shipping | Vai Shipping chỉ có thể xác nhận dữ kiện có trong nguồn; không phải lúc nào cũng biết nguyên nhân | AI chỉ tóm tắt dữ kiện và nêu mức không chắc chắn; CS quyết định |
| Peer challenge | 4 thành viên | Nhóm đồng thuận pain chính là nối dữ liệu và chọn hướng xử lý, không chỉ là viết câu trả lời | Baseline thời gian khác nhau vì mỗi người hình dung case khác nhau | Giữ 12–15 phút là giả định mô phỏng, không trình bày như số liệu thật |
| Dot vote cuối buổi | 4 phiếu | Workflow có AI hỗ trợ được giữ lại vì cân bằng tốc độ và kiểm soát | Không ai đề xuất cho AI tự gửi hoặc tự hoàn tiền sau khi xét rủi ro | Chọn Workflow có human review; loại Agent tự động khỏi prototype |

### Insight sau validation

```text
Pain thật không nằm ở mọi câu hỏi “đơn hàng của tôi ở đâu”.
Pain tập trung ở ticket đã quá SLA, mất checkpoint hoặc có dữ liệu mâu thuẫn,
khi CS phải nối nhiều nguồn trước khi biết nên trả lời hay escalate.
```

### Câu hỏi dùng để challenge trong role-play

1. Dữ kiện nào trong scenario được xem là sự thật đã xác nhận?
2. Nếu bỏ AI, Rule hoặc template giải được bao nhiêu bước?
3. Nếu AI nêu sai nguyên nhân, ai phát hiện và sửa?
4. Khi nào CS được trả lời và khi nào bắt buộc chuyển Shipping?
5. Future workflow có giảm thao tác nhưng vẫn giữ đủ human control không?

## Research giải pháp hiện có

| Nguồn / tool / case | Link | Họ giải quyết phần nào? | Điểm mạnh | Khoảng trống / rủi ro | Bài học cho nhóm |
|---|---|---|---|---|---|
| AfterShip Tracking API | https://www.aftership.com/docs/tracking/quickstart/api-quick-start | Lấy carrier, tracking number, checkpoints và cập nhật qua API/webhook | Giảm việc mở từng portal carrier; dữ liệu có cấu trúc | Rate limit, chi phí, độ trễ và chất lượng vẫn phụ thuộc nguồn carrier | Dùng API/rule cho bước lấy dữ liệu, không dùng LLM để đoán tracking |
| Zendesk Intelligent Triage | https://support.zendesk.com/hc/en-us/articles/4964463770650-About-intelligent-triage | Phân loại ticket theo topic, sentiment, language và entity để routing | Pattern phù hợp cho nhận diện WISMO/delay và gắn category | Cần taxonomy, dữ liệu cấu hình và kiểm tra nhãn sai | Classification là bước phụ; chưa giải quyết điều tra trạng thái đơn |
| Zendesk AI/Copilot | https://support.zendesk.com/hc/en-us/articles/10018448457498-Overview-of-Zendesk-AI-offerings | Tóm tắt ticket, hỗ trợ viết và gợi ý cho agent | Cho thấy pattern AI hỗ trợ agent trong giao diện làm việc | Draft có thể sai/không đúng policy; phụ thuộc gói sản phẩm | AI draft, người thật review phù hợp hơn tự động gửi |

**Research takeaway:**

```text
Phần lấy tracking và so SLA nên dùng API/rule vì dữ liệu có cấu trúc.
AI chỉ nên tổng hợp các checkpoint, nêu nguyên nhân khả nghi có dẫn nguồn
và draft phản hồi. CS Agent vẫn xác nhận nguyên nhân, action và nội dung gửi.
```

## Workflow before/after

### Current workflow bản nhóm

```text
CURRENT STATE — baseline tạm thời 12–15 phút/ticket

[1 Khách tạo ticket delay/thất lạc]
→ [2 CS đọc ticket và lấy mã đơn: 1']
→ [3 CS tra WMS nội bộ: 3']
→ [4 CS tra portal carrier: 4']
→ [5 CS đối chiếu checkpoint với SLA: 3']  <-- bottleneck
→ [6 CS xác định hướng xử lý/escalate: 1–2']
→ [7 CS soạn phản hồi và cập nhật CRM: 1–2']

Handoff: CS → Shipping/carrier nếu đơn mất cập nhật, quá SLA hoặc nghi thất lạc.
```

| Bước | Actor | Input | Output | Thời gian | Ghi chú |
|---|---|---|---|---:|---|
| 1–2 | Khách hàng, CS | Ticket, mã đơn | Ticket đủ mã tra cứu | ~1 phút | Có thể phải hỏi lại nếu thiếu mã |
| 3 | CS | Mã đơn | Trạng thái pick/pack/ship | ~3 phút | WMS nội bộ |
| 4 | CS | Tracking ID | Checkpoint carrier | ~4 phút | Nhiều portal khác nhau |
| 5 | CS | Checkpoint, SLA | Kết luận đúng hạn/quá hạn và nguyên nhân khả nghi | ~3 phút | Bottleneck chính |
| 6 | CS, Shipping/carrier | Kết quả đối chiếu, policy | Chờ thêm hoặc escalation | 1–2 phút | Handoff quan trọng |
| 7 | CS | Kết quả, policy | Phản hồi khách, CRM log | 1–2 phút | Con người chịu trách nhiệm |

**Bottleneck chính:** CS phải nối dữ liệu WMS và carrier rồi diễn giải checkpoint theo SLA. Dữ liệu có thể thiếu, chậm hoặc mâu thuẫn, nên không thể chỉ dựa vào câu trả lời sinh tự động.

### Future workflow bản nhóm

```text
FUTURE STATE — mục tiêu dưới 6 phút/ticket

[1 Ticket được nhận và nhận diện loại vấn đề]
→ [2 Rule/API lấy WMS + carrier checkpoints: 0.5']
→ [3 Rule tính SLA và đánh dấu ngoại lệ: 0.5']
→ [4 AI tóm tắt timeline, dẫn nguồn và draft phản hồi: 0.5']
→ [5 CS kiểm tra dữ liệu, nguyên nhân và action: 4']  <-- human boundary
→ [6 CS gửi/escalate; hệ thống auto-log CRM: 0.5']

Fallback:
- API lỗi hoặc dữ liệu mâu thuẫn → quay về workflow tra cứu thủ công.
- AI không đủ chắc → không nêu nguyên nhân; CS kiểm tra hoặc escalate.
- Draft sai policy/giọng điệu → CS xóa draft và tự viết.
```

### Before/after impact

| Metric | Trước | Sau kỳ vọng | Cách đo |
|---|---:|---:|---|
| Tổng thời gian/ticket | Giả định 12–15 phút | Kỳ vọng dưới 6 phút | So sánh thời gian giữa các vòng role-play |
| Số hệ thống CS mở thủ công | Giả định 2–4 | 1 giao diện | Đếm bước trên scenario walkthrough |
| Số bước thủ công chính | 6 | 2 | Đếm trên workflow |
| Mức rõ ràng của phản hồi | Chưa có baseline | 4/4 thành viên hiểu cùng một action | Peer review sau mỗi vòng |
| Mức đồng thuận về boundary | Chưa có baseline | 4/4 xác định đúng việc AI không được làm | Câu hỏi nhanh cuối role-play |
| Lỗi factual/policy | Chưa có baseline | 0 lỗi nghiêm trọng | Người quan sát đối chiếu output với scenario |

## Problem Statement v0

| Field | Nội dung |
|---|---|
| **Actor** | CS Agent Tier 1 xử lý ticket shipping tại sàn thương mại điện tử |
| **Workflow** | Nhận ticket → tra WMS → tra carrier → đối chiếu SLA → xác định hướng xử lý → phản hồi → cập nhật CRM |
| **Bottleneck** | Tra và đối chiếu dữ liệu rời rạc chiếm khoảng 8–10 phút trong ticket phức tạp |
| **Impact** | Ticket tồn đọng khi cao điểm, phản hồi khách chậm và có nguy cơ giảm CSAT |
| **Success Metric** | Giảm AHT từ khoảng 12–15 phút xuống dưới 6 phút; không tăng reopen rate và không giảm CSAT |
| **Boundary** | Không tự hoàn tiền, hủy đơn, kết luận thất lạc hoặc gửi phản hồi khi chưa có CS review |

### Lỗ hổng của v0

- Baseline hiện là ước lượng dùng cho mô phỏng, không phải số liệu vận hành thật.
- “Nguyên nhân delay” không phải lúc nào cũng có trong tracking.
- Chưa có baseline riêng về reopen rate, CSAT và lỗi factual.
- Data access carrier là điều kiện tiên quyết, không phải việc AI có thể tự giải quyết.

## Rule / Workflow / Agent

### Ma trận độ phù hợp

- **Độ phức tạp:** tương đối cao vì cần 3+ nguồn/bước: CRM, WMS, carrier, SLA và policy.
- **Độ mơ hồ:** trung bình đến cao ở bước diễn giải nguyên nhân và viết phản hồi.
- **AI không cần tự quyết định bước tiếp theo:** các bước chính đã biết trước; ngoại lệ được chuyển cho con người.

Kết luận: bài toán phù hợp với **Workflow có AI hỗ trợ**, chưa cần Agent.

### So sánh No AI / Rule / Workflow / Agent

| Mức | Phương án | Khi nào đủ | Rủi ro / giới hạn | Chọn? |
|---|---|---|---|---|
| **No AI / process fix** | Chuẩn hóa taxonomy, SLA, escalation checklist và mẫu phản hồi | Đủ nếu pain chủ yếu do quy trình không thống nhất | CS vẫn mở nhiều hệ thống và đọc checkpoint thủ công | Dùng làm nền tảng |
| **Rule** | API gom tracking, tính quá SLA, mapping trạng thái và chọn template | Đủ cho WISMO thường và checkpoint rõ | Không xử lý tốt log phi cấu trúc, ngoại lệ và cách giải thích theo context | Dùng cho phần có cấu trúc |
| **Workflow** | Rule/API lấy dữ liệu → AI tóm tắt/draft → CS review → gửi/escalate | Phù hợp vì đường đi cố định nhưng có bước ngôn ngữ/mơ hồ | Hallucination, bỏ sót checkpoint, draft sai policy | **Chọn** |
| **Agent** | Tự chọn tool, trao đổi với khách, escalate và quyết định action | Chỉ phù hợp khi cần lập kế hoạch động và quyền hành động rộng | Quá nhiều permission; có thể hứa sai, escalate sai hoặc gây thiệt hại tài chính | Chưa chọn |

**Vì sao không chọn mức đơn giản hơn làm toàn bộ giải pháp:** Rule giải tốt việc lấy tracking và so SLA nhưng chưa đủ cho việc tổng hợp nhiều checkpoint và draft phản hồi theo context.

**Vì sao không chọn Agent:** Workflow đã có đường đi cố định. Hệ thống không cần tự lập kế hoạch, tự hoàn tiền hoặc tự giao tiếp không giám sát.

## Problem Statement v1

| Field | Nội dung |
|---|---|
| **Actor** | CS Agent Tier 1 phụ trách ticket đơn delay hoặc có dấu hiệu thất lạc tại sàn thương mại điện tử |
| **Workflow** | Nhận ticket → lấy mã đơn → tra WMS/carrier → đối chiếu SLA → chọn hướng xử lý → phản hồi → log CRM |
| **Bottleneck** | CS mất phần lớn AHT để tra cứu và nối các checkpoint rời rạc trước khi biết nên trả lời hay escalate |
| **Impact** | Ticket tồn đọng trong peak sale, tăng thời gian phản hồi và có nguy cơ khách hỏi lại hoặc đánh giá thấp |
| **Success Metric** | Trong pilot, giảm median AHT xuống dưới 6 phút; không tăng reopen rate; không giảm CSAT; không có lỗi factual/policy nghiêm trọng |
| **Boundary** | Chỉ xử lý ticket delay/thất lạc có tracking ID; không xử lý refund/compensation tự động; không tự kết luận nguyên nhân nếu nguồn không nói rõ; không tự gửi |
| **AI intervention point** | Sau khi rule/API đã gom dữ liệu và tính SLA, trước bước CS đánh giá và viết phản hồi |
| **Mức chọn** | Workflow: rule/API + AI summary/draft + CS review |
| **Rủi ro & người thật kiểm tra** | AI có thể bịa nguyên nhân, bỏ sót checkpoint hoặc dùng sai policy. CS Agent kiểm tra tracking và nội dung; Team Lead review mẫu pilot và lỗi nghiêm trọng |

## Final decision

| Câu hỏi | Trạng thái | Ghi chú |
|---|---|---|
| Actor và workflow đã rõ chưa? | Yes | CS Tier 1, Shipping/carrier handoff rõ |
| Baseline và success metric đã đo được chưa? | Not Yet | Có estimate mô phỏng nhưng chưa có baseline vận hành |
| Có input đủ cho bài mô phỏng chưa? | Yes | Ba scenario đủ để walkthrough và challenge workflow |
| Nếu AI sai, hậu quả có chấp nhận được không? | Yes, với boundary | Chỉ draft; CS review; không tự refund/gửi |
| Có người review/owner vận hành không? | Yes | CS Agent và Team Lead |
| Có cách non-AI đơn giản hơn không? | Yes | Dashboard/API, SLA rule và template phải được thử cùng pilot |

### Decision

```text
Go với prototype thảo luận mô phỏng; Not Yet cho pilot vận hành và production.
```

### Prototype nhỏ nhất

1. Viết ba scenario ngắn: WISMO bình thường, quá SLA và mất checkpoint.
2. Chia bốn vai: khách hàng, CS Agent, Shipping Coordinator và người quan sát; đổi vai sau mỗi vòng.
3. Vòng một xử lý theo workflow hiện tại; vòng hai dùng Rule/template; vòng ba dùng Workflow có AI draft.
4. Mỗi người ghi bottleneck, số bước, điểm chưa rõ và rủi ro trong từng vòng.
5. Cả nhóm peer review output, challenge nguyên nhân/action và thống nhất boundary.
6. Chốt lại Problem Statement v1 và quyết định Go/Not Yet dựa trên thảo luận.

### Tiêu chí Go tiếp

- Workflow tương lai giảm rõ số bước tra cứu/lặp lại so với current workflow trong cả ba scenario.
- Mọi trạng thái và checkpoint trong draft đều truy ngược được về scenario.
- Không có lỗi nghiêm trọng như hứa sai ngày, kết luận thất lạc sai hoặc đề xuất sai policy.
- Cả bốn thành viên giải thích thống nhất khi nào Rule đủ và khi nào cần AI hỗ trợ.
- Cả bốn thành viên xác định đúng human boundary và fallback.

### Exit / rollback

- Nếu scenario cho thấy Rule/template đã đủ, hạ giải pháp xuống Rule và không thêm AI.
- Nếu AI draft thường xuyên thêm dữ kiện không có trong scenario, bỏ draft và quay về template.
- Nếu xuất hiện lỗi factual/policy nghiêm trọng, tắt AI draft và quay về workflow thủ công.
- Không mở quyền tự gửi, tự refund hoặc tự kết luận thất lạc trong phạm vi này.

### Lý do quyết định

Problem, actor và workflow đã đủ rõ để role-play và phản biện. Research cũng cho thấy API có thể gom tracking và AI có thể hỗ trợ triage/tóm tắt. Tuy nhiên, đây là bài mô phỏng, không có baseline vận hành. Vì vậy, quyết định hợp lý là Go với prototype thảo luận có human review và Not Yet cho triển khai thực tế.





