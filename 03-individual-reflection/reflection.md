# 03 — Individual Reflection

**Họ và tên:** Lê Quý Thành  
**Mã học viên:** 01168  
**Vai trò:** Thành viên

## Đóng góp của Thành trong nhóm

| Hoạt động | Thành đã làm gì? | Kết quả |
|---|---|---|
| Scan cá nhân | Đưa ra 10 vấn đề chuyên sâu về địa chỉ, điều hướng, dự đoán giao thành công, hư hỏng và tranh chấp | Nhóm có thêm các candidate impact lớn để kiểm tra tính khả thi và data access |
| Pitch | Pitch bài dự đoán xác suất giao thành công theo từng khách hàng | Candidate được đưa vào shortlist để so sánh với bài ticket delay/thất lạc |
| Challenge | Nêu rủi ro thiếu dữ liệu lịch sử, dữ liệu địa chỉ phi cấu trúc và scope quá lớn | Nhóm không chọn bài dự đoán cho prototype của lab dù impact tiềm năng cao |
| Gom cluster | Góp phần hình thành cluster tối ưu giao hàng | Nhóm phân biệt bài toán vận hành dài hạn với bài toán CS có workflow tuyến tính, dễ thử nghiệm |
| Workflow | Challenge workflow ticket delay ở các nhánh dữ liệu mâu thuẫn và carrier không cập nhật | Nhóm bổ sung fallback về tra cứu thủ công và escalation |
| Research | Đóng góp góc nhìn về phương án Rule và giới hạn dữ liệu trước khi dùng AI | Nhóm ưu tiên API/rule cho dữ liệu có cấu trúc và không để AI đoán tracking |
| Rule / Workflow / Agent | Phản biện quyền tự quyết của Agent trong tác vụ có ảnh hưởng khách hàng và tài chính | Nhóm giữ AI ở vai trò tóm tắt/draft, không tự hoàn tiền hoặc kết luận thất lạc |
| Validation | Tham gia vai người quan sát/challenge trong scenario walkthrough | Nhóm bổ sung mức không chắc chắn, human boundary và fallback |
| Decision | Challenge điều kiện input và rủi ro trước khi Go | Nhóm chọn Go với prototype thảo luận, Not Yet cho triển khai thực tế |

## Bảng dùng AI trong reflection

| Phase | Tôi dùng AI để làm gì? | AI hữu ích ở đâu? | AI sai/hời hợt ở đâu? | Tôi sửa gì |
|---|---|---|---|---|
| Scan | Phân nhóm các problem logistics theo bốn lăng kính | Giúp nhìn ra pattern địa chỉ, dự đoán, hư hỏng và phân xử | Có thể mô tả các ý tưởng như thể dữ liệu đã sẵn sàng | Tôi bổ sung điều chưa chắc về input và khả năng mô phỏng |
| Problem Card | Phản biện candidate dự đoán giao thành công | Giúp liệt kê actor, input và metric tiềm năng | Dễ đề xuất mô hình phức tạp trước khi có baseline | Tôi không coi “xây model” là problem statement và đưa thiếu dữ liệu thành rủi ro chính |
| Workflow | Chuyển mô tả thành các bước trước/sau | Giúp xác định AI intervention point | Có xu hướng để AI tự thay đổi tuyến hoặc hủy đơn | Tôi thêm human approval và fallback an toàn |
| Research | Gợi ý case tối ưu giao hàng và validation địa chỉ | Giúp mở rộng phương án đã có trên thị trường | Có số liệu hiệu quả không kèm nguồn đáng tin cậy | Tôi bỏ số liệu không verify và chỉ giữ pattern giải pháp |
| Problem Statement | Phản biện impact, metric và boundary | Chỉ ra impact lớn nhưng metric chưa chắc đo được trong lab | Có thể đánh đồng tiềm năng với feasibility | Tôi tách rõ bài đáng giải quyết và bài có thể role-play ngay |
| Rule / Workflow / Agent | So sánh quyền tự động hóa | Giúp nhận diện rủi ro khi Agent hành động tự chủ | Có xu hướng ưu tiên Agent cho bài nhiều nhánh | Tôi yêu cầu con người phê duyệt các action ảnh hưởng khách hàng hoặc chi phí |
| Decision | Gợi ý câu hỏi input readiness và rollback | Giúp nhóm đặt điều kiện Go/Not Yet rõ hơn | Dễ đưa tiêu chí production vào một bài mô phỏng | Tôi chỉ đồng ý prototype thảo luận và giữ Not Yet cho triển khai thật |

## Bài học của Thành

- Problem có impact lớn chưa chắc là candidate phù hợp nhất nếu thiếu input và không thể mô phỏng rõ trong phạm vi lab.
- Dự đoán giao thành công có rủi ro thiên lệch nếu sử dụng lịch sử khách hàng mà không kiểm tra chất lượng và fairness.
- Challenge về data access giúp nhóm tránh chọn Agent hoặc mô hình dự đoán chỉ vì nghe hiện đại.
- Workflow ticket delay/thất lạc phù hợp hơn cho role-play vì actor, bước nghẽn và human boundary đều rõ.
- Fallback phải là một phần của future workflow, không phải ghi chú thêm sau khi chọn giải pháp.
- Quyết định đúng có thể là Go với scope nhỏ, trong khi vẫn để một số điều kiện ở trạng thái Not Yet.

Nếu làm lại:

```text
Tôi sẽ chuẩn bị một scenario cho bài dự đoán giao thành công rồi để nhóm role-play và challenge data access trước khi shortlist. Với bài được chọn, tôi sẽ tiếp tục đóng vai người quan sát để ghi rõ chỗ AI suy đoán quá dữ kiện và đề xuất fallback phù hợp.
```



