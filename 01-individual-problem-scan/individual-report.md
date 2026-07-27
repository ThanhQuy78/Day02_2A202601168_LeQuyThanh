# 01 — Individual Problem Scan

## Scan rộng

Dựa trên dữ liệu 10 problems trong lĩnh vực vận chuyển và logistics, dưới đây là bảng phân tích chi tiết.

| # | Lăng kính | Problem quan sát được | Ai chịu ảnh hưởng? | Dấu hiệu thật |
|---|---|---|---|---|
| 1 | Lặp lại / Tốn thời gian | Chuẩn hóa địa chỉ tiếng Việt phi cấu trúc | Hệ thống định tuyến, Shipper | Địa chỉ "ngõ, ngách, hẻm" không map được trên API bản đồ quốc tế, trả về sai tọa độ. |
| 2 | Pain từ người khác | Điều hướng đến địa chỉ khó tìm bằng dữ liệu kinh nghiệm của shipper khác | Shipper | Shipper phải gọi khách nhiều lần, loay hoay tìm nhà trong ngõ sâu do bản đồ không hiện. |
| 3 | AI có thể tốt hơn | Dự đoán xác suất giao thành công theo từng khách hàng cụ thể | Điều phối viên, Nền tảng | Tỷ lệ bom hàng/hoàn hàng cao, mất công Shipper đi giao nhưng khách vắng nhà/không nghe máy. |
| 4 | Tốn thời gian | Kiểm tra đóng gói bằng computer vision trước khi xuất kho | Nhân viên kho, Merchant | Lỗi nhầm/thiếu hàng từ gốc, quá trình soát lỗi thủ công chậm hoặc dễ sai sót. |
| 5 | Lặp lại | Đối chiếu hàng lúc giao (chống giao nhầm/thiếu) bằng hình ảnh | Shipper, Khách hàng | Khách khiếu nại nhận sai/thiếu hàng dù mã vận đơn đã quét thành công. |
| 6 | Pain từ người khác | Root-cause classification cho khiếu nại (quy trách nhiệm đúng khâu) | CSKH, Đội vận hành đa bên | Xử lý cảm tính, đổ lỗi qua lại giữa merchant - kho - shipper mất nhiều thời gian. |
| 7 | AI có thể tốt hơn | Re-routing động khi có ngoại lệ tại điểm giao | Shipper, Hệ thống điều phối | Shipper tự quyết định tuyến đường cảm tính khi khách đổi giờ chót, làm hỏng lộ trình tối ưu. |
| 8 | AI có thể tốt hơn | Dự đoán rủi ro hư hỏng hàng hóa trong quá trình vận chuyển | Quản lý kho, Nền tảng | Hàng hỏng chỉ được phát hiện sau khi khách phản ánh, tỷ lệ đền bù cao. |
| 9 | Pain từ người khác | Cân bằng tải công bằng cho shipper | Shipper, Quản lý nhân sự | Shipper phàn nàn do liên tục bị dồn đơn khó, nguy cơ kiệt sức và bỏ việc cao. |
| 10 | Tốn thời gian | Tự động phân xử tranh chấp và quyết định bồi thường đa bên | CSKH, Pháp chế | Thương lượng thủ công kéo dài, thiếu bằng chứng tổng hợp định lượng để ra quyết định. |

Vì sao phần scan này mạnh:
- Tập trung sâu vào một domain cụ thể (Logistics/Delivery tại Việt Nam).
- Các problem đều đi từ nỗi đau thực tế (pain-points) của những vai trò cụ thể trong chuỗi cung ứng.
- Có dấu hiệu nhận biết rõ ràng, không bị giả định chung chung (như "bom hàng", "lạc trong ngõ ngách").

## Top 3

| Rank | Problem | Vì sao chọn | Điều còn chưa chắc |
|---|---|---|---|
| 1 | Dự đoán xác suất giao thành công theo khách hàng | Impact lớn tới chi phí vận hành (giảm chi phí giao lại, hoàn hàng), có tính khả thi về mặt dữ liệu cá nhân hóa. | Dữ liệu lịch sử khách hàng (giờ có nhà, độ tin cậy SĐT) có đủ để xây mô hình chuẩn xác không. |
| 2 | Điều hướng đến địa chỉ khó tìm bằng dữ liệu shipper | Giải quyết trực tiếp pain-point rất đặc thù của giao thông ngõ hẻm Việt Nam mà API quốc tế chưa làm tốt. | Cơ chế thu thập dữ liệu (crowd-sourced) từ cộng đồng shipper có đủ tạo động lực cho họ đóng góp hay không. |
| 3 | Dự đoán rủi ro hư hỏng hàng hóa khi vận chuyển | Chuyển đổi từ xử lý hậu quả (đền bù) sang phòng ngừa chủ động (cảnh báo đóng gói thêm lớp/chuyển xe). | Cần tích hợp rất nhiều nguồn dữ liệu (loại hàng, thời tiết, độ xóc nảy, phương tiện) khá phức tạp để dự đoán tốt. |

## Problem Card #1 — Dự đoán xác suất giao thành công

**Problem 1 câu:**  
Hệ thống không nhận diện được các đơn hàng có rủi ro thất bại cao (do khách vắng nhà, bom hàng) trước khi xuất kho, dẫn đến lãng phí chi phí vận chuyển, công sức của shipper và làm chậm vòng quay hàng hóa.

**Actor:**  
Hệ thống điều phối (Dispatcher) và Shipper chịu trách nhiệm tuyến đường.

**Thời điểm / bối cảnh:**  
Ngay sau khi đơn hàng được tạo và trước thời điểm phân bổ vào tuyến đường (routing) của Shipper.

**Current workflow:**
1. Hệ thống tiếp nhận đơn hàng.
2. Tối ưu tuyến đường và gán đơn cho Shipper.
3. Shipper di chuyển đến địa chỉ giao.
4. Giao thất bại (Khách bom hàng / Không nghe máy / Đi vắng).
5. Shipper mang hàng về bưu cục, ghi nhận trạng thái rớt đơn (chờ giao lại hoặc hoàn kho).

**Bottleneck:**  
Bước 4 — Việc nhận biết đơn hàng không thể giao thành công chỉ diễn ra *sau khi* Shipper đã mất công và chi phí di chuyển đến tận nơi.

**Impact:**  
Lãng phí chi phí vận chuyển (xăng xe, khấu hao), giảm hiệu suất làm việc của Shipper (chở hàng nhưng không có doanh thu), tăng chi phí lưu kho đảo chiều (logistics thu hồi).

**Success metric:**  
Giảm tỷ lệ đơn hoàn (Return to Origin) và đơn cần giao lại (Redelivery) xuống X%, mà không làm tăng thời gian chờ xử lý đơn trước khi xuất kho.

**Non-AI alternative:**  
Hệ thống Rule-based đơn giản: Tạo Blacklist từ các SĐT từng giao thất bại trên 3 lần để cảnh báo thủ công, hoặc yêu cầu tổng đài gọi xác nhận 100% mọi đơn hàng (chi phí cao).

**AI hypothesis:**  
Mô hình AI dự đoán xác suất giao thành công dựa trên đặc trưng người dùng (lịch sử nhận hàng, khung giờ vàng có nhà, điểm tin cậy SĐT). Các đơn bị gắn cờ "Rủi ro cao" sẽ đi qua một bước phụ (như Auto-call hoặc SMS xác nhận trước) trước khi quyết định xuất kho.

**Quick gut:**  
Workflow.

### Draft current workflow

```text
CURRENT STATE

[1 Nhận đơn hàng từ merchant]
→ [2 Tối ưu tuyến và gán cho Shipper]
→ [3 Shipper di chuyển đến địa chỉ]
→ [4 Giao thất bại (bom hàng/đi vắng)]  <-- Bottleneck (mất thời gian & chi phí)
→ [5 Xử lý hoàn/lưu kho]
```

### Draft future workflow

```text
FUTURE STATE

[1 Nhận đơn hàng từ merchant]
→ [2 AI tính xác suất thành công (Scoring)] <-- AI intervention
→ [3 Đơn rủi ro cao: Auto-Call/SMS xác nhận] <-- Workflow step
→ [4 Tối ưu tuyến đường (chỉ các đơn an toàn/đã xác nhận)]
→ [5 Shipper đi giao (tỷ lệ thành công cao hơn)]

Fallback: Nếu AI dự đoán sai (gắn cờ nhầm khách tốt), khách vẫn có thể xác nhận qua SMS tự động và hàng vẫn được giao bình thường.
```

## Problem Cards #2 và #3 — tóm tắt

| Card | Actor | Bottleneck | Metric | Quick gut | Vì sao chưa chọn làm #1 |
|---|---|---|---|---|---|
| Điều hướng địa chỉ khó tìm | Shipper | Loay hoay tìm nhà ngõ ngách, phải gọi hỏi khách nhiều lần mất thời gian. | Giảm thời gian giao trung bình/đơn; Giảm số cuộc gọi. | Workflow / Agent | Xây dựng dữ liệu crowd-sourced tốn nhiều thời gian, rủi ro data rác cao, chỉ giải quyết được phần ngọn (khi shipper đã tới nơi). |
| Dự đoán rủi ro hỏng hàng | Quản lý kho / Vận hành | Hàng bị hỏng trong lúc chuyển đi nhưng không được cảnh báo để gia cố trước. | Giảm tỷ lệ đơn khiếu nại hỏng vỡ; Giảm chi phí đền bù. | Workflow | Đòi hỏi tích hợp dữ liệu quá trình đóng gói và điều kiện vật lý vận chuyển (rung lắc, thời tiết) khá phức tạp để làm pilot ngay. |