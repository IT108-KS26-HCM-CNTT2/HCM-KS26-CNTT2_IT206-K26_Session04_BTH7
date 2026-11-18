

## 1. Kiến trúc giải pháp kết hợp 3 kỹ thuật

Để lập kế hoạch tổ chức sự kiện **AI Innovation Day** (150 sinh viên, 1 ngày, 15.000.000 VNĐ, 10 nhân sự) mà không bị lố ngân sách hay thiếu người chạy chương trình, em thiết kế một hệ thống gồm **4 Prompt chạy nối tiếp nhau**:

1. **Chain-of-Thought (Suy luận từng bước):** Áp dụng ở **Prompt 1** (tính toán khả thi nguồn lực) và **Prompt 4** (cộng trừ kiểm tra ngân sách, đối chiếu chéo nhân sự và tìm lỗi hổng).
2. **Prompt Chaining (Chuỗi Prompt):** Chia công việc thành 4 bước nối tiếp. Kết quả của bước trước được đóng gói vào các thẻ dữ liệu (`[KHA_THI]`, `[LICH_TRINH]`, `[NGAN_SACH]`, `[NHAN_SU]`) làm đầu vào bắt buộc cho bước sau.
3. **Context Management (Quản lý ngữ cảnh):** Áp dụng ở **Prompt 3** để nén toàn bộ dữ liệu đã chốt thành một **Bản tóm tắt ngữ cảnh (Context Snapshot)** gọn nhẹ. Khi mở một khung chat mới (hội thoại mới) để chạy **Prompt 4**, chỉ cần dán bản tóm tắt này vào là AI nhớ hết mà không bị quên hay sai lệch thông số ban đầu.

## 2. Bộ Prompt hoàn chỉnh (Sử dụng cả 3 kỹ thuật)

### Prompt 1: Phân tích tính khả thi (Kỹ thuật: Chain-of-Thought)

```
Bạn là cố vấn tổ chức sự kiện sinh viên tại trường đại học. Hãy đánh giá tính khả thi của sự kiện "AI Innovation Day" dựa trên thông số cố định dưới đây.

[THONG_SO_CO_DINH]:
- Tên sự kiện: AI Innovation Day
- Quy mô: 150 sinh viên tham dự.
- Thời gian: 1 ngày (sáng và chiều).
- Ngân sách tối đa: 15.000.000 đồng (tuyệt đối không vượt quá).
- Nhân sự ban tổ chức: 10 người.
- Mục tiêu: Giúp sinh viên hiểu và trải nghiệm ứng dụng AI trong học tập và công việc.

Hãy suy nghĩ từng bước một (Chain-of-Thought) trước khi kết luận:
- Bước 1 (Tính toán chi phí bình quân): Lấy 15.000.000 đồng chia cho 150 sinh viên để xem mức chi tối đa trên mỗi người là bao nhiêu, từ đó xác định khả năng thuê địa điểm (nên tận dụng hội trường trường hay thuê ngoài), ăn uống, quà tặng.
- Bước 2 (Tính toán tải công việc nhân sự): Lấy 150 sinh viên chia cho 10 nhân sự tổ chức để xem tỷ lệ phục vụ (1 nhân sự lo bao nhiêu khách), đánh giá xem có đủ người chia ca trực hay không.
- Bước 3 (Đánh giá khả thi & Rủi ro): Kết luận sự kiện có khả thi không và chỉ ra 3 rủi ro lớn nhất cần đề phòng khi chạy chương trình.

Xuất kết quả ngắn gọn trong thẻ [PHAN_TICH_KHA_THI].
```

### Prompt 2: Lập lịch trình, dự toán ngân sách & phân công nhân sự (Kỹ thuật: Prompt Chaining)

```
Bạn là Trưởng ban tổ chức sự kiện. Dựa vào thông số cố định và kết quả phân tích khả thi ở bước trước, hãy lập kế hoạch triển khai chi tiết.

Dữ liệu đầu vào bắt buộc:
[THONG_SO_CO_DINH]: AI Innovation Day | 150 SV | 1 ngày | Ngân sách <= 15.000.000 VNĐ | 10 nhân sự | Mục tiêu: Hiểu & trải nghiệm AI trong học tập, công việc.
{{OUTPUT_PROMPT_1}}

Yêu cầu thực hiện (bắt buộc liên kết chặt chẽ giữa lịch trình, tiền và người):
1. Xây dựng [LICH_TRINH] chi tiết trong 1 ngày (từ 07:30 đến 16:30), có cả phần chia sẻ kiến thức và khu vực/khung giờ trải nghiệm thực hành AI.
2. Lập bảng [NGAN_SACH] chi tiết từng hạng mục (địa điểm, thiết bị, in ấn, teabreak/nước uống, khách mời, dự phòng). Bắt buộc tổng cộng phải <= 15.000.000 đồng.
3. Lập bảng [NHAN_SU] phân công nhiệm vụ cụ thể cho đúng 10 thành viên (đánh số từ NV01 đến NV10) khớp với các hoạt động trong lịch trình.
```

### Prompt 3: Lập kế hoạch truyền thông & Đóng gói tóm tắt ngữ cảnh (Kỹ thuật: Prompt Chaining + Context Management)

```
Bạn là Trưởng ban Truyền thông & Thư ký dự án. Dựa vào toàn bộ kế hoạch từ Bước 1 và Bước 2, hãy thực hiện 2 nhiệm vụ sau:

Dữ liệu đầu vào từ các bước trước:
{{OUTPUT_PROMPT_2}}

Nhiệm vụ 1: Xây dựng [KE_HOACH_TRUYEN_THONG] trước sự kiện (trong vòng 10 ngày trước ngày diễn ra) để thu hút đủ 150 sinh viên đăng ký (nêu rõ kênh đăng, nội dung chính và người phụ trách lấy từ danh sách 10 nhân sự ở Bước 2).
Nhiệm vụ 2 (Context Management): Tạo một [CONTEXT_SNAPSHOT] (Bản tóm tắt ngữ cảnh chuẩn hóa, dưới 250 từ) gói gọn toàn bộ thông số cốt lõi, tổng tiền đã chia, cơ cấu 10 nhân sự và lịch trình chính để tôi copy sang một cuộc hội thoại hoàn toàn mới nhằm kiểm tra và hiệu chỉnh kế hoạch mà không bị mất dữ liệu.
```

### Prompt 4: Kiểm tra tính nhất quán & Hiệu chỉnh kế hoạch ở hội thoại mới (Kỹ thuật: Context Management + Chain-of-Thought)

*(Lưu ý: Mở một khung chat AI mới tinh và dán Prompt này kèm Context Snapshot ở Bước 3 vào)*

```
Bạn là Kiểm toán viên dự án sự kiện (QA Reviewer). Dưới đây là bản tóm tắt ngữ cảnh của dự án sự kiện sinh viên được chuyển từ phiên làm việc trước sang.

Dữ liệu ngữ cảnh dự án:
{{CONTEXT_SNAPSHOT_TU_PROMPT_3}}

Hãy suy nghĩ từng bước một (Chain-of-Thought) để rà soát và hiệu chỉnh kế hoạch:
- Bước 1 (Kiểm tra ràng buộc cứng): Đối chiếu xem quy mô có đúng 150 SV, nhân sự đúng 10 người, thời gian 1 ngày và tổng ngân sách có vượt quá 15.000.000 đồng không.
- Bước 2 (Kiểm tra tính liên kết & Xung đột): Tìm xem có nhân sự nào bị phân công làm 2 việc cùng một khung giờ không? Lịch trình có phần nào thiếu kinh phí trong bảng dự toán không? Mục tiêu "trải nghiệm AI" đã có đủ thời lượng và thiết bị chưa?
- Bước 3 (Báo cáo & Hiệu chỉnh): Xuất báo cáo chỉ rõ các điểm bất hợp lý phát hiện được và đưa ra phương án điều chỉnh cụ thể để bản kế hoạch hoàn thiện 100%.
```

## 3. Kết quả chạy thử thực tế toàn bộ quy trình

### 3.1. Bản phân tích tính khả thi (Output Prompt 1)

`[PHAN_TICH_KHA_THI]`

* **Bước 1 (Tính toán chi phí bình quân):**
  * Ngân sách bình quân: $15.000.000 / 150 = 100.000\text{ VNĐ/sinh viên}$ cho cả ngày.
  * *Nhận định:* Mức chi này **khả thi** nếu tận dụng hội trường của trường đại học (miễn phí hoặc phí tượng trưng) và không bao ăn trưa trọn gói cho 150 người (chỉ phục vụ nước uống + bánh ngọt nhẹ teabreak). Nếu thuê hội trường ngoài sẽ vỡ ngân sách ngay.
* **Bước 2 (Tính toán tải công việc nhân sự):**
  * Tỷ lệ phục vụ: $150 / 10 = 15\text{ sinh viên / 1 nhân sự}$.
  * *Nhận định:* Tỷ lệ $1:15$ là **khả thi** cho sự kiện hội thảo kết hợp workshop, nhưng đòi hỏi 10 nhân sự phải kiêm nhiệm theo từng khung giờ (lúc đầu giờ tập trung check-in, vào chương trình thì chuyển sang hỗ trợ kỹ thuật và điều phối).
* **Bước 3 (Kết luận & 3 Rủi ro lớn nhất):**
  * **Kết luận:** Sự kiện **HOÀN TOÀN KHẢ THI** nếu tổ chức tại hội trường trường và tối ưu chi phí ăn uống.
  * **3 Rủi ro cần đề phòng:** (1) Mạng Wi-Fi hội trường bị sập khi 150 sinh viên cùng truy cập công cụ AI; (2) Phát sinh chi phí in ấn, quà tặng vượt trần 15 triệu; (3) Quá tải khâu check-in đầu giờ sáng nếu bố trí ít người trực cổng.

### 3.2. Lịch trình sự kiện theo khung giờ (Output Prompt 2 - Phần 1)

`[LICH_TRINH]` (Tổng thời gian: 1 ngày)

| Khung giờ | Nội dung hoạt động | Hình thức / Chi tiết | Phụ trách chính |
| :--- | :--- | :--- | :--- |
| **07:30 – 08:15** | Đón khách & Check-in | Quét mã QR điểm danh, phát thẻ tên & tài liệu hướng dẫn | NV01, NV02, NV03 |
| **08:15 – 08:30** | Khai mạc chương trình | Văn nghệ mở màn, tuyên bố lý do, giới thiệu khách mời | NV04 (MC) |
| **08:30 – 10:00** | **Phiên 1 (Talkshow):** Ứng dụng AI trong học tập đại học | Diễn giả chia sẻ cách dùng AI tóm tắt tài liệu, học ngoại ngữ, nghiên cứu | NV05, NV06 (Kỹ thuật) |
| **10:00 – 10:20** | Teabreak & Giao lưu | Nghỉ giải lao, dùng bánh ngọt, nước suối và tham quan gian trải nghiệm | NV07, NV08 |
| **10:20 – 11:45** | **Phiên 2 (Workshop):** Thực hành Prompting giải quyết bài tập | Sinh viên thực hành trực tiếp trên laptop/điện thoại theo đề bài mẫu | Toàn bộ team hỗ trợ |
| **11:45 – 13:30** | Nghỉ trưa tự túc | Ban tổ chức ăn trưa tại chỗ, trực bảo vệ thiết bị hội trường | NV09, NV10 trực ca |
| **13:30 – 15:15** | **Phiên 3 (Thực chiến):** Ứng dụng AI vào công việc & làm CV | Thực hành tạo slide, viết CV, xử lý dữ liệu bằng công cụ AI | NV05, NV06, NV09 |
| **15:15 – 16:15** | **Mini-Challenge:** Thi đấu giải nhanh bài toán bằng AI | Chia đội thi đấu trực tiếp tại hội trường, chấm điểm & trao giải | NV01, NV04, NV10 |
| **16:15 – 16:30** | Tổng kết & Bế mạc | Trao giải thưởng Mini-Challenge, chụp ảnh lưu niệm, gửi form khảo sát | Toàn bộ 10 nhân sự |

### 3.3. Bảng dự toán ngân sách (Output Prompt 2 - Phần 2)

`[NGAN_SACH]` (Ràng buộc: $\le 15.000.000\text{ VNĐ}$)

| STT | Hạng mục chi phí | Đơn vị tính | Số lượng | Đơn giá (VNĐ) | Thành tiền (VNĐ) | Ghi chú |
| :---: | :--- | :--- | :---: | :---: | :---: | :--- |
| 1 | Hỗ trợ phí vệ sinh & điện nước hội trường | Trọn gói | 1 | $1.500.000$ | $1.500.000$ | Xin mượn hội trường của trường |
| 2 | Backdrop, Standee, thẻ đeo BTC & đại biểu | Trọn gói | 1 | $2.200.000$ | $2.200.000$ | In ấn bộ nhận diện sự kiện |
| 3 | Nước suối & bánh ngọt Teabreak | Suất | 160 | $25.000$ | $4.000.000$ | 150 SV + 10 BTC (nghỉ giữa giờ sáng) |
| 4 | Quà cảm ơn & hỗ trợ chi phí diễn giả | Người | 2 | $1.500.000$ | $3.000.000$ | 2 diễn giả chia sẻ sáng & chiều |
| 5 | Giải thưởng cuộc thi Mini-Challenge | Gói | 1 | $1.500.000$ | $1.500.000$ | 1 giải Nhất, 1 giải Nhì, 1 giải Ba |
| 6 | Cơm trưa & nước uống cho 10 nhân sự BTC | Suất | 10 | $50.000$ | $500.000$ | Hỗ trợ BTC trực trưa tại hội trường |
| 7 | Thuê bộ phát Wi-Fi 4G tốc độ cao dự phòng | Thiết bị | 2 | $400.000$ | $800.000$ | Chống nghẽn mạng khi 150 SV dùng AI |
| 8 | Quỹ dự phòng phát sinh | Trọn gói | 1 | $1.500.000$ | $1.500.000$ | Khoảng 10% tổng ngân sách |
| | **TỔNG CỘNG** | | | | **$15.000.000$** | **Đạt chuẩn (Không vượt 15 triệu)** |

### 3.4. Bảng phân công 10 nhân sự (Output Prompt 2 - Phần 3)

`[NHAN_SU]` (Đúng 10 thành viên CLB)

| Mã NS | Vai trò / Vị trí | Nhiệm vụ cụ thể trước và trong sự kiện |
| :---: | :--- | :--- |
| **NV01** | Trưởng ban tổ chức (Project Lead) | Điều phối chung toàn bộ tiến độ, làm việc với nhà trường xin hội trường, xử lý sự cố phát sinh. |
| **NV02** | Phụ trách Truyền thông 1 (Content) | Viết bài đăng Fanpage, lên nội dung truyền thông trước sự kiện, quản lý form đăng ký 150 sinh viên. |
| **NV03** | Phụ trách Truyền thông 2 (Media) | Thiết kế ấn phẩm (poster, backdrop), chụp ảnh và quay video recap trong suốt ngày diễn ra sự kiện. |
| **NV04** | MC & Điều phối sân khấu | Dẫn dắt toàn bộ chương trình từ 08:15 đến 16:30, giữ nhịp độ và khuấy động không khí cuộc thi chiều. |
| **NV05** | Phụ trách Kỹ thuật 1 (Âm thanh/Ánh sáng) | Kiểm tra micro, máy chiếu, màn hình LED, kết nối slide cho diễn giả. |
| **NV06** | Phụ trách Kỹ thuật 2 (Mạng & Công cụ AI) | Lắp đặt 2 bộ phát Wi-Fi dự phòng, chuẩn bị sẵn tài khoản AI mẫu và hỗ trợ kỹ thuật lúc workshop. |
| **NV07** | Phụ trách Hậu cần 1 (Ăn uống/Teabreak) | Đặt và bày trí bánh ngọt, nước suối cho 160 người lúc 10:00 và đặt cơm trưa cho 10 nhân sự BTC. |
| **NV08** | Phụ trách Hậu cần 2 (Vật tư/Quà tặng) | Nhận đồ in ấn (standee, thẻ tên), chuẩn bị quà cảm ơn diễn giả và giải thưởng Mini-Challenge. |
| **NV09** | Phụ trách Đối ngoại & Diễn giả | Mời và đón tiếp 2 diễn giả, nhắc lịch trình, gửi tài liệu trước cho diễn giả chuẩn bị. |
| **NV10** | Thủ quỹ & Quản lý Check-in | Giữ ngân sách 15 triệu, thu chi có hóa đơn, quản lý bàn check-in quét QR đầu giờ sáng. |

### 3.5. Kế hoạch truyền thông trước sự kiện (Output Prompt 3 - Phần 1)

`[KE_HOACH_TRUYEN_THONG]` (Triển khai 10 ngày trước sự kiện)

* **Ngày D-10 đến D-8 (Mở màn & Khơi gợi nhu cầu):**
  * *Kênh:* Fanpage CLB, Group sinh viên các khoa.
  * *Nội dung:* Đăng bài khảo sát & minigame nhỏ "Sinh viên dùng AI để học tập hay... đối phó deadline?", hé lộ poster sự kiện *AI Innovation Day*.
  * *Phụ trách:* NV02 (viết bài), NV03 (thiết kế poster).
* **Ngày D-7 đến D-4 (Mở cổng đăng ký & Giới thiệu diễn giả):**
  * *Kênh:* Fanpage CLB, bảng tin điện tử khoa, nhờ giảng viên chia sẻ trên lớp.
  * *Nội dung:* Tung link đăng ký chính thức (giới hạn 150 suất miễn phí), giới thiệu profile 2 diễn giả và tiết lộ các công cụ AI thực chiến sẽ được hướng dẫn.
  * *Phụ trách:* NV02 (quản lý form đăng ký), NV09 (xác nhận thông tin diễn giả).
* **Ngày D-3 đến D-1 (Đẩy mạnh chuyển đổi & Chốt danh sách):**
  * *Kênh:* Fanpage CLB, Email/Zalo nhắc lịch.
  * *Nội dung:* Đăng bài đếm ngược "Chỉ còn 20 suất cuối cùng", hé lộ giải thưởng cuộc thi *Mini-Challenge* 1,5 triệu đồng. Gửi email xác nhận kèm mã QR check-in cho 150 sinh viên đăng ký thành công.
  * *Phụ trách:* NV02, NV10 (xuất mã QR check-in).

### 3.6. Bản tóm tắt ngữ cảnh dự án (Output Prompt 3 - Phần 2)

```
[CONTEXT_SNAPSHOT - DỰ ÁN AI INNOVATION DAY]
1. Ràng buộc gốc: Quy mô 150 SV | Thời gian 1 ngày (07:30-16:30) | Ngân sách tối đa 15.000.000 VNĐ | Nhân sự 10 người (NV01-NV10) | Mục tiêu: Hiểu & trải nghiệm AI trong học tập, công việc.
2. Lịch trình chính: Sáng (07:30 Check-in; 08:30 Talkshow AI học tập; 10:00 Teabreak; 10:20 Workshop thực hành); Trưa (11:45-13:30 nghỉ tự túc); Chiều (13:30 Thực chiến AI công việc/CV; 15:15 Thi đấu Mini-Challenge; 16:15 Bế mạc).
3. Ngân sách đã chia (Tổng 15.000.000 VNĐ): Hội trường 1.5tr; In ấn 2.2tr; Teabreak 160 suất 4.0tr; Diễn giả 3.0tr; Giải thưởng 1.5tr; Cơm trưa BTC 0.5tr; Thuê 2 cục phát Wi-Fi 0.8tr; Dự phòng 1.5tr.
4. Phân công 10 người: NV01 (Trưởng BTC), NV02 (Content), NV03 (Media), NV04 (MC), NV05 (Kỹ thuật âm thanh), NV06 (Kỹ thuật mạng/AI), NV07 (Hậu cần ăn uống), NV08 (Hậu cần vật tư/quà), NV09 (Đối ngoại/Diễn giả), NV10 (Thủ quỹ/Check-in).
5. Truyền thông: Chạy trước 10 ngày trên Fanpage & Group trường để gom đủ 150 đơn đăng ký kèm mã QR.
```

### 3.7. Báo cáo kiểm tra và hiệu chỉnh kế hoạch ở hội thoại mới (Output Prompt 4)

* **Bước 1 (Kiểm tra ràng buộc cứng):**
  * Quy mô: Đúng $150$ sinh viên.
  * Nhân sự: Đúng $10$ người (NV01 đến NV10).
  * Thời gian: Đúng $1$ ngày ($07:30 - 16:30$).
  * Ngân sách: $1.5 + 2.2 + 4.0 + 3.0 + 1.5 + 0.5 + 0.8 + 1.5 = 15.0\text{ triệu đồng}$ (Khớp chính xác trần $15.000.000\text{ VNĐ}$, không vượt ngân sách).
* **Bước 2 & 3 (Phát hiện điểm bất hợp lý & Phương án hiệu chỉnh):**
  1. **Xung đột nhân sự ở khâu Check-in đầu giờ (07:30 – 08:15):**
     * *Vấn đề phát hiện:* Trong lịch trình ghi NV01, NV02, NV03 trực check-in, nhưng trong bảng phân công thì NV10 mới là người quản lý mã QR check-in, còn NV03 phải bận chụp ảnh đón khách. Hơn nữa, chỉ có 3 người quét QR cho 150 sinh viên trong 45 phút (trung bình 15 giây/người) rất dễ gây ùn tắc cổng hội trường.
     * *Hiệu chỉnh:* Chuyển **NV10** (người nắm danh sách QR) và **NV08** (người phát thẻ đeo) ra bàn check-in cùng **NV02** (tổng 3 người chuyên quét mã & phát thẻ), giải phóng NV01 để bao quát chung và NV03 đi chụp ảnh.
  2. **Thiếu sót trong ngân sách cho hoạt động trải nghiệm AI:**
     * *Vấn đề phát hiện:* Nhiều công cụ AI mạnh hiện nay yêu cầu tài khoản trả phí (Plus/Pro) để demo mượt mà cho sinh viên trải nghiệm trong phiên Workshop và Mini-Challenge, nhưng bảng dự toán chưa có khoản này.
     * *Hiệu chỉnh:* Trích $500.000\text{ VNĐ}$ từ **Quỹ dự phòng ($1.500.000\text{ VNĐ}$)** để mua/thuê 3–4 tài khoản AI bản quyền dùng chung cho các đội thi đấu buổi chiều. Quỹ dự phòng còn lại $1.000.000\text{ VNĐ}$ vẫn an toàn, tổng ngân sách vẫn giữ nguyên $15.000.000\text{ VNĐ}$.
  3. **Rủi ro tỷ lệ "ảo" khi truyền thông miễn phí:**
     * *Vấn đề phát hiện:* Sự kiện sinh viên miễn phí thường có tỷ lệ đăng ký nhưng không đến tham dự (no-show) khoảng $20\% - 30\%$. Nếu chỉ nhận đúng 150 đơn đăng ký thì ngày diễn ra có thể chỉ còn khoảng 110 người.
     * *Hiệu chỉnh:* Ở khâu truyền thông (Prompt 3), cho phép mở form nhận tối đa **180–190 đơn đăng ký** (kèm danh sách chờ), đóng form trước 2 ngày và gửi tin nhắn xác nhận tham dự để đảm bảo hội trường luôn lấp đầy đúng 150 sinh viên thực tế.
