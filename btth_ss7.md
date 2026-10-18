# KẾ HOẠCH SPRINT TÍCH HỢP CHO DỰ ÁN CAMPUSFOOD
**(INTEGRATED SPRINT PLANNING & TRACKING PACK)**

* **Dự án:** Ứng dụng đặt món trực tuyến và nhận món tại 2 căn tin trường CampusFood
* **Đội phát triển (Scrum Team):** Lan (Frontend), Minh (Backend), Ngọc (QA/BA), Phúc (Tích hợp)
* **Khung thời gian:** Sprint 10 ngày làm việc (2 tuần)
* **Tiêu chuẩn hoàn thành (Definition of Done - DoD):** Đạt 100% Acceptance Criteria; hoàn thành rà soát mã nguồn (Code Review); vượt qua kiểm thử luồng chính và luồng ngoại lệ; triển khai thành công lên môi trường Staging/thử nghiệm; không còn lỗi tồn đọng ở mức Critical hoặc High.

---

## 1. CHUẨN HÓA ƯỚC LƯỢNG (ESTIMATION STANDARDIZATION)

Nhóm áp dụng kỹ thuật **Planning Poker** với dãy số Fibonacci chuẩn ($1, 2, 3, 5, 8, 13$).
* **Câu chuyện chuẩn tham chiếu (Baseline Reference Story):** **CF01** (*Xem thực đơn/còn món*) được toàn đội thống nhất làm chuẩn tương đương **$3\text{ Story Points}$**.
* **Nguyên tắc vận hành:** Product Owner (PO) chỉ giải thích rõ yêu cầu nghiệp vụ, không áp đặt điểm số; nhóm Developers thảo luận dựa trên độ phức tạp, rủi ro và nỗ lực công việc; tuyệt đối không lấy trung bình cộng cơ học và không quy đổi $1\text{ SP} = x\text{ giờ}$.

### 1.1. Bảng phân loại và phân tích 4 PBI có chênh lệch phiếu / thiếu dữ liệu

| Mã PBI | Tên PBI | Kết quả bỏ phiếu | Phân loại trạng thái | Căn cứ & Phân tích chênh lệch phiếu | Đề xuất giải pháp chuẩn mực |
|---|---|:---:|---|---|---|
| **CF03** | Chọn điểm nhận | $3 / 5 / 5 / 8$ | **Cần thảo luận lại & Chốt phạm vi** | • Phiếu $3\text{ SP}$: Thành viên chỉ tính trường hợp nhận tại 2 quầy căn tin trong trường.<br>• Phiếu $8\text{ SP}$: Thành viên lo ngại phạm vi mở rộng giao ngoài trường (thiếu quy tắc bán kính, điểm giao mơ hồ, rủi ro định vị GPS).<br>• Bản chất không nghẽn về mặt kỹ thuật mà do ranh giới nghiệp vụ chưa được PO đóng băng. | PO chốt dứt khoát phạm vi cho bản thử nghiệm: **CHỈ cho phép nhận tại 2 căn tin nội bộ trường (Căn tin A và Căn tin B)**, loại bỏ hoàn toàn các điểm giao ngoài trường sang giai đoạn sau. Sau khi làm rõ, nhóm thống nhất chốt **$3\text{ SP}$** (tương đương CF01). |
| **CF04** | Nhận đơn / Cập nhật trạng thái giao | $5 / 8 / 8 / 13$ | **Cần thảo luận kỹ thuật & Thống nhất kiến trúc** | • Phiếu $5\text{ SP}$: Đánh giá đơn giản chỉ là cập nhật cột trạng thái trong Database.<br>• Phiếu $13\text{ SP}$: Phúc và Minh lo ngại việc cập nhật Real-time (WebSocket) đồng thời giữa sinh viên, 2 căn tin và người giao.<br>• PBI phụ thuộc chặt chẽ vào CF02 và CF03. | Minh và Lan thống nhất kiến trúc cho bản thử nghiệm: Sử dụng cơ chế kéo dữ liệu định kỳ (**Short-polling** mỗi $15\text{ giây}$) thay vì dựng hệ thống WebSocket phức tạp. Phân tích thấy nỗ lực kỹ thuật cao hơn CF02 ($5\text{ SP}$) nhưng khả thi; nhóm thống nhất chốt **$8\text{ SP}$**. |
| **CF06** | Thanh toán QR | $5 / 8 / 13 / 13$ | **Thiếu dữ liệu bên thứ ba / Rủi ro cao** | • Phiếu $5\text{ SP}$: Lan xem như việc hiển thị ảnh mã QR tĩnh.<br>• Phiếu $13\text{ SP}$: Phúc và Minh chỉ ra việc thiếu SLA của đối tác cổng QR, rủi ro mất kết nối mạng, xử lý Webhook bất đồng bộ, đối soát thất bại và tiền trừ nhưng đơn không tạo được. | Tách làm 2 phần:<br>1) Tạo một **Technical Spike ($4\text{ giờ}$)** để Phúc kiểm tra tài liệu API sandbox và xác nhận cam kết SLA của cổng thanh toán.<br>2) Đối với bản thử nghiệm tại 2 căn tin, ưu tiên dùng mã VietQR quét chuyển khoản trực tiếp đối chiếu tại quầy, hoặc chốt **$13\text{ SP}$** và dời sang Sprint sau khi đã có SLA. |
| **CF08** | Hoàn tiền quán hủy đơn | $13\text{ (Tạm chốt)}$ | **Thiếu dữ liệu / Chưa đạt Definition of Ready (DoR)** | • PBI bị ghi chú "Thiếu AC" (Acceptance Criteria).<br>• Nghiệp vụ hoàn tiền liên quan trực tiếp đến tài chính, thời gian xử lý của ngân hàng và cơ chế xác thực nhưng chưa có bất kỳ kịch bản nghiệm thu cụ thể nào. | **Kiên quyết KHÔNG đưa vào Sprint Backlog**. Theo quy tắc DoR, một hạng mục thiếu tiêu chí nghiệm thu thì nhóm không thể ước lượng chính xác và không có căn cứ kiểm thử (vi phạm tiêu chuẩn INVEST). Trả về Product Backlog để PO hoàn thiện kịch bản AC trước. |

---

## 2. SPRINT GOAL VÀ PHẠM VI SPRINT BACKLOG

### 2.1. Tuyên ngôn Sprint Goal duy nhất
> **Sprint Goal:**  
> *"Hoàn thành luồng đặt món trực tuyến và nhận món trực tiếp tại 2 căn tin trường, có thông báo trạng thái đơn hàng tin cậy, giúp sinh viên và nhân viên căn tin vận hành thử nghiệm an toàn mà không bị tắc nghẽn."*

### 2.2. Lựa chọn phạm vi Sprint Backlog và Lý do loại trừ
Phạm vi được xây dựng để tạo ra một luồng sản phẩm khép kín, dùng thử được ngay tại 2 quầy căn tin:

* **Các PBI được chọn vào Sprint Backlog:**
  1. **CF01 (Xem thực đơn/còn món - $3\text{ SP}$):** PBI đầu vào, trạng thái sẵn sàng, giúp sinh viên kiểm tra món ăn có sẵn.
  2. **CF02 (Tạo đơn/mã xác nhận - $5\text{ SP}$):** Xương sống của quy trình, phụ thuộc sau CF01, sinh mã đơn để đối chiếu.
  3. **CF03 (Chọn điểm nhận - Đã tinh gọn còn $3\text{ SP}$):** Giới hạn nghiêm ngặt tại Căn tin A và Căn tin B trong trường.
  4. **CF04 (Nhận đơn/cập nhật trạng thái giao - $8\text{ SP}$):** Kết nối giữa nhà bếp và sinh viên để cập nhật tiến độ chế biến/giao nhận.
  * **Tổng điểm cam kết:** **$19\text{ SP}$** (hoặc tinh gọn xuống **$16\text{ SP}$** nếu tách CF04 thành $5\text{ SP}$ nhận đơn tại quầy và $3\text{ SP}$ giao hàng tách riêng).

* **Các PBI bị loại bỏ khỏi Sprint và lý do:**
  * **Loại CF06 (Thanh toán QR - $13\text{ SP}$):** Thiếu SLA từ đối tác ngân hàng/cổng thanh toán. Nếu cố nhận sẽ làm vỡ Sprint vì phụ thuộc rủi ro bên ngoài. Bản thử nghiệm tại 2 quầy căn tin sẽ dùng hình thức thanh toán tại quầy hoặc quét mã QR chuyển khoản trực tiếp.
  * **Loại CF07 (Biên nhận điện tử - $3\text{ SP}$):** Phụ thuộc kỹ thuật vào CF06; khi chưa kết nối cổng QR tự động thì chưa thể xuất biên nhận thanh toán điện tử.
  * **Loại CF08 (Hoàn tiền quán hủy - $13\text{ SP}$):** Thiếu Acceptance Criteria, vi phạm Definition of Ready.
  * **Loại CF09 (Đánh giá người giao - $3\text{ SP}$):** Thiếu quy tắc sửa/xóa, không nằm trong luồng cốt lõi dùng thử tại quầy.
  * **Loại CF05 (Xem trạng thái giao - $5\text{ SP}$):** Để ở dạng tính năng mở rộng (Stretch item), chỉ thực hiện nếu 4 PBI trên hoàn thành sớm.

---

## 3. TÍNH TOÁN CAPACITY VÀ PHÂN RÃ CÔNG VIỆC (TASK BREAKDOWN)

### 3.1. Bảng tính Capacity trước và sau biến động nhân sự
Đội gồm 4 thành viên làm việc trong Sprint 10 ngày. Giờ tiêu chuẩn: $6\text{ giờ/ngày}$ (riêng Ngọc là $5\text{ giờ/ngày}$ do phụ trách thêm việc BA/nghiệp vụ).

| Thành viên | Chuyên môn chính | Tổng giờ lý thuyết | Gián đoạn công việc | Net Capacity ban đầu | Biến động (Minh nghỉ thêm 2 ngày) | Net Capacity sau biến động |
|---|---|---|---|:---:|---|:---:|
| **Lan** | Frontend Developer | $10\text{ ngày} \times 6\text{h} = 60\text{h}$ | Hỗ trợ vận hành: $6\text{h}$ | **$54\text{ giờ}$** | Không đổi | **$54\text{ giờ}$** |
| **Minh** | Backend Developer | $8\text{ ngày} \times 6\text{h} = 48\text{h}$ | Không có: $0\text{h}$ | **$48\text{ giờ}$** | Nghỉ thêm $2\text{ ngày}$ ($-12\text{h}$) | **$36\text{ giờ}$** *(Giảm $25\%$)* |
| **Ngọc** | QA / Business Analyst | $10\text{ ngày} \times 5\text{h} = 50\text{h}$ | Họp hành/Làm mịn: $5\text{h}$ | **$45\text{ giờ}$** | Không đổi | **$45\text{ giờ}$** |
| **Phúc** | Integration Engineer | $9\text{ ngày} \times 6\text{h} = 54\text{h}$ | Hỗ trợ tích hợp cũ: $8\text{h}$ | **$46\text{ giờ}$** | Không đổi | **$46\text{ giờ}$** |
| **TOÀN ĐỘI** | **Scrum Team** | **$212\text{ giờ}$** | **$19\text{ giờ}$** | **$193\text{ giờ}$** | **Giảm $12\text{ giờ}$ của Minh** | **$181\text{ giờ}$** *(Giảm $6.2\%$)* |

* **Phân tích tác động nút thắt cổ chai (Bottleneck Analysis):**  
  Mặc dù tổng số giờ toàn đội chỉ giảm $6.2\%$ (từ $193\text{h}$ xuống $181\text{h}$), nhưng **Backend** là khâu cốt lõi xử lý dữ liệu lại bị sụt giảm tới **$25\%$** (từ $48\text{h}$ xuống $36\text{h}$). Toàn bộ các User Story (tạo đơn, cập nhật trạng thái) đều phụ thuộc vào API Backend của Minh. Nếu Minh bị quá tải, Lan (Frontend) và Ngọc (QA) sẽ rơi vào tình trạng nhàn rỗi chờ API, dẫn đến nguy cơ dồn ứ công việc vào cuối Sprint.  
  *Biện pháp:* Phúc (Tích hợp) có năng lực kỹ thuật cần được phân công chia lửa viết một số endpoint API phụ trợ và chuẩn bị trước dữ liệu giả lập (Mock data) để Lan lập trình giao diện song song.

---

### 3.2. Phân rã công việc chi tiết (Task Breakdown) cho 3 PBI cốt lõi
Nhiệm vụ được đặt tên theo quy tắc `[Động từ hành động] + [Đối tượng cụ thể]`, gắn rõ đầu ra và người phụ trách, không quy đổi cơ học $1\text{ SP} = x\text{ giờ}$:

| Mã Task | Thuộc PBI | Mô tả công việc kỹ thuật | Người phụ trách | Đầu ra kiểm chứng | Ước lượng |
|:---:|:---:|---|---|---|:---:|
| **TASK-01** | CF01 ($3\text{ SP}$) | Thiết kế bảng Database danh mục, món ăn và số lượng khả dụng | Minh (Backend) | Schema DB và Script migration trên Staging | $6\text{ giờ}$ |
| **TASK-02** | CF01 ($3\text{ SP}$) | Xây dựng RESTful API tra cứu thực đơn và kiểm tra trạng thái món | Minh (Backend) | API Endpoint `/menu` kèm tài liệu Swagger | $6\text{ giờ}$ |
| **TASK-03** | CF01 ($3\text{ SP}$) | Phát triển UI hiển thị danh sách món theo 2 căn tin, nhãn hết món | Lan (Frontend) | Giao diện web/mobile responsive hiển thị đúng thực đơn | $8\text{ giờ}$ |
| **TASK-04** | CF01 ($3\text{ SP}$) | Viết kịch bản và thực hiện kiểm thử chức năng lọc thực đơn, hết món | Ngọc (QA) | Test log execution, 0 bug Critical/High | $4\text{ giờ}$ |
| **TASK-05** | CF02 ($5\text{ SP}$) | Thiết kế API tạo đơn hàng, sinh mã xác nhận 6 số và trừ tồn kho | Minh (Backend) | API Endpoint `/orders` và logic khóa tạm thời món ăn | $10\text{ giờ}$ |
| **TASK-06** | CF02 ($5\text{ SP}$) | Xây dựng giao diện giỏ hàng, nút đặt món và màn hình mã đơn | Lan (Frontend) | Giao diện giỏ hàng và màn hình hiển thị mã đơn | $8\text{ giờ}$ |
| **TASK-07** | CF02 ($5\text{ SP}$) | Tích hợp kiểm tra ràng buộc dữ liệu đầu vào và kiểm thử tải đơn | Phúc (Tích hợp) | Script test tải đơn hàng đồng thời không trùng mã | $6\text{ giờ}$ |
| **TASK-08** | CF02 ($5\text{ SP}$) | Kiểm thử các trường hợp lỗi: đặt món đã hết, mất kết nối khi đặt | Ngọc (QA) | Báo cáo kiểm thử đạt 100% kịch bản tiêu chí AC | $6\text{ giờ}$ |
| **TASK-09** | CF03 ($3\text{ SP}$) | Cấu hình danh mục 2 điểm nhận cố định (Căn tin A, Căn tin B) | Phúc (Tích hợp) | Cơ sở dữ liệu lưu chính xác mã căn tin nhận món | $4\text{ giờ}$ |
| **TASK-10** | CF03 ($3\text{ SP}$) | Xây dựng component lựa chọn căn tin nhận món trước khi bấm đặt | Lan (Frontend) | UI radio button chọn căn tin A/B, tự động lưu lựa chọn | $6\text{ giờ}$ |
| **TASK-11** | CF03 ($3\text{ SP}$) | Kiểm thử hiển thị đúng địa điểm nhận trên đơn hàng của căn tin | Ngọc (QA) | Biên bản xác nhận đơn hàng hiển thị đúng quầy nhận | $3\text{ giờ}$ |

---

## 4. PHÂN TÍCH VELOCITY VÀ QUYẾT ĐỊNH PHẠM VI SPRINT

### 4.1. Phân tích Velocity quá khứ và Quy tắc tính điểm
* **Dữ liệu lịch sử:** Sprint 3 đạt $18\text{ SP}$; Sprint 4 đạt $21\text{ SP}$; Sprint 5 đạt $15\text{ SP}$.
* **Giải thích quy tắc tính điểm Sprint 5:** Ở Sprint 5, nhóm có 1 PBI $8\text{ SP}$ đã hoàn thành $75\%$ khối lượng công việc. Tuy nhiên, theo nguyên tắc Scrum và Definition of Done, công việc chưa hoàn thành $100\%$ tiêu chuẩn chất lượng thì **tuyệt đối KHÔNG được tính điểm một phần vào Velocity**. Do đó, Velocity Sprint 5 chỉ ghi nhận đúng $15\text{ SP}$ hoàn tất trọn vẹn.
* **Đánh giá xu hướng:** Vận tốc đang suy giảm rõ rệt (từ $21\text{ SP}$ xuống $15\text{ SP}$). Vận tốc trung bình 3 Sprint gần nhất là:
  $$\text{Velocity Trung bình} = \frac{18 + 21 + 15}{3} = 18\text{ SP}$$

### 4.2. So sánh hai phương án lựa chọn phạm vi Sprint Backlog

| Tiêu chí đánh giá | Phương án A (Dựa chủ yếu vào Velocity lịch sử) | Phương án B (Dựa trên Capacity thực tế, DoR và Rủi ro) |
|---|---|---|
| **1. Cơ sở lựa chọn phạm vi** | Lấy mốc Velocity trung bình quá khứ ($18 - 19\text{ SP}$) để nhận việc cho đầy Sprint (chọn CF01, CF02, CF03, CF04 và cố nhận thêm CF05 hoặc một phần CF06). | Đối soát Capacity thực tế sau biến động (Minh nghỉ 2 ngày, Backend còn $36\text{h}$), loại trừ PBI thiếu DoR (CF08) và rủi ro bên thứ ba (CF06). Chỉ chọn CF01 ($3\text{ SP}$) + CF02 ($5\text{ SP}$) + CF03 ($3\text{ SP}$) + CF04 ($8\text{ SP}$) = $19\text{ SP}$ (hoặc $16\text{ SP}$ cốt lõi nếu chia nhỏ CF04). |
| **2. Độ tin cậy dự báo** | **THẤP:** Vận tốc quá khứ được xác lập khi đội đủ 4 người làm việc toàn thời gian; không phản ánh tình trạng hiện tại khi thành viên cốt lõi nghỉ phép. | **CAO:** Dựa trên số giờ công thực tế từng chuyên môn (chỉ có $36\text{h}$ Backend) và chỉ nhận các PBI đã rõ ràng về tiêu chí kỹ thuật. |
| **3. Ứng phó gián đoạn / Nghỉ phép** | **KÉM:** Bỏ qua việc Minh nghỉ 2 ngày và Phúc mất $8\text{h}$ bảo trì hệ thống cũ, dẫn đến quá tải nghiêm trọng ở khâu Backend. | **TỐT:** Phân bổ lại vai trò, chuyển bớt phần việc cấu hình dữ liệu sang cho Phúc, bảo đảm tiến độ không bị nghẽn cục bộ. |
| **4. Mức độ bảo vệ Sprint Goal** | **RẤT NGUY HIỂM:** Nhận quá nhiều việc khiến đội làm dàn trải mỗi thứ một ít; nguy cơ đến cuối Sprint không có PBI nào hoàn thành trọn vẹn $100\%$ DoD. | **RẤT CAO:** Tập trung toàn bộ nguồn lực vào đúng 1 mục tiêu: hoàn thành luồng đặt và nhận món an toàn tại 2 căn tin. |
| **5. Áp lực tâm lý & Nguy cơ Gaming số liệu** | **TIÊU CỰC:** Ép đội phải đạt mốc $18 - 20\text{ SP}$ sẽ dẫn đến việc Developers hạ thấp tiêu chuẩn kiểm thử, bỏ qua code review hoặc tự thổi phồng điểm Story Point để đạt chỉ tiêu. | **TÍCH CỰC:** Tạo không gian tâm lý an toàn, tập trung vào chất lượng sản phẩm thực tế thay vì chạy theo số điểm hình thức. |

### 4.3. Quyết định lựa chọn và Giả định đi kèm
* **QUYẾT ĐỊNH:** Nhóm chính thức lựa chọn **PHƯƠNG ÁN B**. Phạm vi cam kết chính thức gồm: **CF01 ($3\text{ SP}$), CF02 ($5\text{ SP}$), CF03 ($3\text{ SP}$) và phần cốt lõi của CF04 ($5\text{ SP}$ nhận đơn tại căn tin)** $\rightarrow$ Tổng cam kết an toàn là **$16\text{ SP}$** (phù hợp với xu hướng vận tốc $15\text{ SP}$ và Capacity thực tế $36\text{h}$ của Backend).
* **Các giả định đi kèm:**
  1. Hai căn tin A và B phối hợp thử nghiệm đồng ý sử dụng thiết bị máy tính bảng/màn hình có sẵn tại quầy để nhận thông báo đơn.
  2. Sinh viên dùng thử chấp nhận thanh toán tiền mặt hoặc quét mã QR chuyển khoản trực tiếp tại quầy căn tin trong đợt thử nghiệm đầu tiên.
  3. Phúc hoàn thành $8\text{ giờ}$ xử lý tích hợp cũ đúng hạn và có thể dành trọn vẹn thời gian còn lại ($38\text{h}$) để hỗ trợ Backend cùng Minh.

---

## 5. THEO DÕI BIỂU ĐỒ BURNDOWN VÀ ĐIỀU CHỈNH THÍCH NGHI

### 5.1. Dữ liệu tiến độ Sprint Burndown (Ngày 0 đến Ngày 10)

| Ngày | N0 | N1 | N2 | N3 | N4 | N5 | N6 | N7 | N8 | N9 | N10 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Lý tưởng (SP)** | 21.0 | 18.9 | 16.8 | 14.7 | 12.6 | 10.5 | 8.4 | 6.3 | 4.2 | 2.1 | 0.0 |
| **Thực tế (SP)** | **21.0** | **21.0** | **18.0** | **18.0** | **23.0** | **18.0** | **18.0** | **13.0** | **8.0** | **5.0** | **5.0** |

### 5.2. Biểu đồ mô phỏng đường Burndown thực tế và lý tưởng

```text
SP Còn Lại
 24 |                 [N4: +5 SP Scope Creep do thêm mã KM]
 23 |                   * (23)
 21 | *-----* (21)       \        [Đường Lý tưởng: . . . .]
 18 | .     . \ *-----*   \ *-----* (18) [N5-N6: Cổng QR chậm + Họp không Action Item]
 15 | .     .   .     .     .     .
 13 | .     .   .     .     .     . \ * (13)
  8 | .     .   .     .     .     .   . \ * (8)
  5 | .     .   .     .     .     .   .   . \ *-----* (5) [N9-N10: Kẹt 5 SP chưa đạt DoD]
  0 |_+_____+___+_____+_____+_____+___+___+___+_____+_____+_
     N0    N1  N2    N3    N4    N5  N6  N7  N8    N9   N10   (Ngày)
```

### 5.3. Điều tra 3 tín hiệu bất thường và Đề xuất hành động thích nghi
1. **Tín hiệu 1 (Ngày 4 - Tăng đột biến từ $18$ lên $23\text{ SP}$):**
   * *Hiện tượng:* Khối lượng công việc còn lại bất ngờ nhảy vọt từ $18\text{ SP}$ lên $23\text{ SP}$, đi ngược với nguyên lý burndown.
   * *Giả thuyết & Dữ liệu kiểm tra:* Hiện tượng phình to phạm vi (**Scope Creep**) do PO đề nghị đưa thêm tính năng "Mã khuyến mãi $5\text{ SP}$" vào giữa Sprint mà không đàm phán rút bớt việc cũ ra.
   * *Hành động khắc phục:* Scrum Master can thiệp nhắc nhở nguyên tắc bất biến của Sprint Goal; kiên quyết loại bỏ tính năng $5\text{ SP}$ này ra khỏi Sprint hiện tại để trả về Product Backlog cho kỳ tới. **Tuyệt đối không sửa số liệu ngày 0 để làm đẹp biểu đồ**.
2. **Tín hiệu 2 (Ngày 5 đến Ngày 6 - Đường nằm ngang ở $18\text{ SP}$):**
   * *Hiện tượng:* Từ ngày 5 sang ngày 6, đường thực tế đi ngang hoàn toàn ở mức $18\text{ SP}$ mặc dù các thành viên vẫn làm việc.
   * *Giả thuyết & Dữ liệu kiểm tra:* Dữ liệu ghi nhận cổng QR bị chậm phản hồi, gây tắc nghẽn khâu tích hợp. Đồng thời, ngày 6 nhóm có họp đột xuất nhưng không ghi lại biên bản/quyết định (họp hình thức, thiếu Action Items rõ ràng khiến không ai chịu trách nhiệm gỡ rối).
   * *Hành động khắc phục:* Lập tức tổ chức phiên làm việc nhanh $15\text{ phút}$ có ghi nhận biên bản hành động cụ thể: Phúc liên hệ đối tác cổng QR yêu cầu hỗ trợ kỹ thuật; Minh và Lan thiết lập cơ chế giả lập (**Mock Service**) để đội Frontend và QA tiếp tục kiểm thử luồng nghiệp vụ mà không bị chặn.
3. **Tín hiệu 3 (Ngày 9 đến Ngày 10 - Kẹt cứng ở mức $5\text{ SP}$):**
   * *Hiện tượng:* Từ ngày 9 đến ngày 10, biểu đồ dừng lại ở mức $5\text{ SP}$ và kết thúc Sprint mà không chạm mốc 0.
   * *Giả thuyết & Dữ liệu kiểm tra:* Có 1 PBI $5\text{ SP}$ đã hoàn thành mã nguồn nhưng còn tồn đọng lỗi ở khâu kiểm thử hoặc chưa hoàn thành Code Review, do đó không đạt Definition of Done.
   * *Hành động khắc phục:* Minh bạch hoàn toàn số liệu tại buổi Sprint Review: Thừa nhận PBI này chưa Done và chuyển toàn bộ $5\text{ SP}$ này về lại Product Backlog để PO tái ưu tiên. Tuyệt đối không cho nghiệm thu dở dang để lấy điểm.

---

## 6. BẢNG XỬ LÝ NGOẠI LỆ VÀ THAY ĐỔI TRONG SPRINT

| Tình huống ngoại lệ | Phân tích tác động | Quy trình xử lý chuẩn mực | Tác động đến Goal, DoD, Velocity, Burndown | Cách công khai tại Sprint Review |
|---|---|---|---|---|
| **1. Cổng QR bị lỗi sau khi sinh viên đã tạo đơn hàng thành công** | Luồng thanh toán online bị đứt gãy; sinh viên có nguy cơ bị trừ tiền mà đơn không được xác nhận tại căn tin. | • Lập trình viên bổ sung cơ chế Fallback ngay lập tức: cho phép sinh viên chuyển sang phương thức "Thanh toán tiền mặt/chuyển khoản tại quầy" để đơn vẫn được giữ chỗ.<br>• Thiết lập trạng thái đơn "Chờ thanh toán tại quầy". | • **Sprint Goal:** Được bảo vệ (sinh viên vẫn đặt và nhận được món).<br>• **DoD:** Vẫn giữ vững (phải test kịch bản lỗi thanh toán).<br>• **Velocity:** Không đổi.<br>• **Burndown:** Không đổi. | Minh bạch trình bày tại Sprint Review: Cho các bên liên quan thấy cơ chế chịu lỗi của hệ thống giúp vận hành thực tế không bị tê liệt khi đối tác ngân hàng gặp sự cố. |
| **2. Phát sinh đơn hàng thiếu SĐT hoặc chọn địa điểm ngoài trường** | Dữ liệu đơn hàng không hợp lệ, người giao hàng/căn tin không thể liên lạc với sinh viên. | • Thêm ngay ràng buộc kiểm tra dữ liệu đầu vào (Validation) tại giao diện giỏ hàng: bắt buộc nhập SĐT 10 số hợp lệ.<br>• Khóa cứng danh sách địa điểm nhận chỉ gồm: Quầy Căn tin A và Quầy Căn tin B. Không cho phép nhập địa chỉ tự do. | • **Sprint Goal:** Không bị ảnh hưởng.<br>• **DoD:** Đạt chuẩn (đã bổ sung kịch bản kiểm thử form).<br>• **Velocity:** Không đổi.<br>• **Burndown:** Không đổi. | Demo trực tiếp tính năng chặn đơn lỗi tại buổi Review; chứng minh tính an toàn và chặt chẽ của dữ liệu đơn hàng chuyển tới căn tin. |
| **3. PO đề nghị thêm tính năng "Mã khuyến mãi" $5\text{ SP}$ vào Ngày 4** | Gây phình to phạm vi (Scope Creep), làm loãng sự tập trung của nhóm vào mục tiêu cốt lõi, nguy cơ vỡ tiến độ. | • Scrum Master giải thích nguyên tắc Timebox: Không nhận thêm việc làm nguy hại Sprint Goal.<br>• Nếu PO khẩn thiết muốn làm mã khuyến mãi, PO bắt buộc phải thương lượng rút bớt một PBI có giá trị tương đương ($5\text{ SP}$) ra khỏi Sprint Backlog.<br>• Nếu không đổi, đưa vào Product Backlog cho Sprint sau. | • **Sprint Goal:** Được bảo vệ tuyệt đối.<br>• **DoD:** Không bị hạ chuẩn.<br>• **Velocity:** Phản ánh đúng năng lực.<br>• **Burndown:** Loại bỏ đường nhảy vọt bất thường. | Báo cáo tại Sprint Review rằng nhóm đã giữ vững cam kết giao đúng tính năng cốt lõi đã hứa và tính năng khuyến mãi sẽ được ưu tiên xem xét ở Sprint tới. |
| **4. Đến buổi Review, một PBI đạt $80\%$ nhưng chưa kịp chạy kiểm thử** | Vi phạm nghiêm trọng Definition of Done; tiềm ẩn lỗi tiềm tàng nếu cố tình đưa ra thị trường. | • Kiên quyết KHÔNG nghiệm thu PBI này tại Sprint Review.<br>• Không demo tính năng dở dang trước các bên liên quan.<br>• Chuyển toàn bộ PBI về Product Backlog để PO tái đánh giá và Developers ước lượng lại phần việc còn lại trong Sprint Planning tới. | • **Sprint Goal:** Đánh giá xem có đạt mức tối thiểu không.<br>• **DoD:** Được bảo vệ 100%.<br>• **Velocity:** Nhận 0 điểm cho PBI này (Velocity giảm).<br>• **Burndown:** Dừng ở mức $5\text{ SP}$. | Minh bạch công bố tại buổi Review: Nêu rõ lý do tính năng chưa đáp ứng 100% chuẩn kiểm thử chất lượng, thể hiện tính trung thực và trách nhiệm kỹ thuật của đội ngũ. |