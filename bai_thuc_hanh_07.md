# Yêu cầu đầu ra:

## Bộ promt:

### 1/ Prompt 0. Khóa ràng buộc và vai trò

```
CONTEXT
Bạn là chuyên gia tổ chức sự kiện sinh viên. Câu lạc bộ công nghệ tổ chức "AI Innovation Day" cho sinh viên trong trường.
Mục tiêu: giúp sinh viên hiểu và trải nghiệm ứng dụng AI trong học tập và công việc.
Ràng buộc cứng: đúng 150 sinh viên; 1 ngày; đúng 10 nhân sự; ngân sách tối đa 15.000.000 đồng (tổng mọi chi phí).
Đề chưa nêu: địa điểm, ngày tổ chức, nguồn thu. Giả định đề xuất: [ĐỊA ĐIỂM: ...], [NGUỒN THU: không tính, toàn bộ chi phí nằm trong 15 triệu].

REQUEST
Ghi nhớ ngữ cảnh và ràng buộc trên để cùng tôi lập kế hoạch qua nhiều bước liên tiếp.

EXPLANATION
Các bước sau dùng kết quả của bước trước làm đầu vào, nên mọi ràng buộc cứng phải được giữ nguyên ở mọi bước.

ACTION
1/ Đọc ngữ cảnh.
2/ Nếu giả định chưa hợp lý, nói rõ và hỏi tôi, không tự quyết.
3/ Chỉ trả lời "Đã nhận ngữ cảnh" kèm danh sách 4 ràng buộc cứng.

TONE
Chuyên nghiệp, ngắn gọn, thân thiện với sinh viên.

EXTRAS
- Mọi số liệu, mốc thời gian, đơn giá tự đưa ra phải ghi "(đề xuất)".
- Không thêm hạng mục ngoài: khả thi, lịch trình, ngân sách, phân công, truyền thông, kiểm tra.
- Trình bày gọn: mỗi ý một câu, ưu tiên bảng và danh sách đánh số.
```

---

### 2/ Prompt 1. Phân tích tính khả thi (Chain-of-Thought)

```
CONTEXT
Dựa trên ngữ cảnh và 4 ràng buộc cứng ở Prompt 0.

REQUEST
Phân tích tính khả thi của sự kiện.

EXPLANATION
Cần suy luận từng bước và hiển thị phép tính để kiểm tra được từng kết luận nhỏ, tránh kết luận cảm tính.

ACTION
Làm lần lượt:
Bước 1. Quy mô: 150 SV cần loại không gian nào, bao nhiêu khu vực, có chia nhóm không?
Bước 2. Nhân sự: 10 người có đủ vận hành không? Tỷ lệ người tổ chức/SV?
Bước 3. Ngân sách sơ bộ: 15 triệu / 150 SV = bao nhiêu đồng/người? Liệt kê nhóm chi phí bắt buộc, ước lượng sơ bộ (đề xuất), tổng có vượt 15 triệu không?
Bước 4. Thời gian: một ngày chứa tối đa bao nhiêu giờ nội dung sau khi trừ đón tiếp, nghỉ, ăn nhẹ, dọn dẹp?
Bước 5. Mục tiêu: có đạt "hiểu và trải nghiệm AI" không? Cần tối thiểu loại hoạt động nào?
Bước 6. Rủi ro: tối đa 5 rủi ro lớn nhất (mô tả, mức độ Cao/TB/Thấp, cách giảm thiểu).
Kết luận: Khả thi / Khả thi có điều kiện / Không khả thi, kèm lý do và điều kiện.

TONE
Khách quan, thẳng thắn, không tô hồng.

EXTRAS
- Dùng bảng cho Bước 3 và Bước 6.
- Không đề xuất đổi 150 SV, 10 nhân sự hay 15 triệu.
- Cuối cùng thêm mục "Thông tin cần tôi xác nhận".
```

---

### 3/ Prompt 2. Lịch trình theo khung giờ (Prompt Chaining)

```
CONTEXT
Kết quả phân tích khả thi ở bước trước:
{{KẾT QUẢ Prompt 1}}

REQUEST
Xây dựng lịch trình sự kiện trong 1 ngày theo khung giờ.

EXPLANATION
Lịch trình là xương sống: ngân sách, phân công, truyền thông đều bám theo nó. Phải khớp số giờ khả dụng, cách chia nhóm và rủi ro đã nêu ở Prompt 1.

ACTION
1/ Lập bảng: Khung giờ | Hoạt động | Hình thức (nghe/thực hành/giao lưu) | Khu vực | Vật dụng cần có.
2/ Bắt buộc có: check-in, khai mạc, AI trong học tập, AI trong công việc, thực hành trải nghiệm, nghỉ/ăn nhẹ, tổng kết.
3/ Khung giờ liền mạch, không chồng lấn, có đệm chuyển hoạt động.
4/ Tự kiểm tra: tổng thời lượng khớp giờ khả dụng Prompt 1? Thực hành đủ cho 150 SV theo cách chia nhóm Prompt 1? Mỗi hoạt động phục vụ mục tiêu?
5/ Rút ra danh sách "Vật dụng/dịch vụ cần chi tiền".

TONE
Rõ ràng, thực tế, như một bản chương trình dùng được ngay.

EXTRAS
- Mọi khung giờ và thời lượng ghi "(đề xuất)".
- Chỉ dùng hoạt động phù hợp ngân sách và rủi ro ở Prompt 1.
- Danh sách cuối làm đầu vào cho Prompt 3.
```

---

### 4/ Prompt 3. Dự toán ngân sách (Chain-of-Thought + Prompt Chaining)

```
CONTEXT
- Khả thi: {{KẾT QUẢ Prompt 1}}
- Lịch trình: {{KẾT QUẢ Prompt 2}}

REQUEST
Lập bảng dự toán ngân sách cho sự kiện.

EXPLANATION
Mỗi khoản chi phải truy ngược được về một hoạt động trong lịch trình, và tổng phải nằm trong 15 triệu. Tính từng bước để phát hiện sai số ngay.

ACTION
Bước 1. Từ lịch trình, liệt kê hạng mục cần chi, gắn với khung giờ.
Bước 2. Ước lượng đơn giá, số lượng (đề xuất, theo mặt bằng giá sinh viên tại Việt Nam).
Bước 3. Tính thành tiền từng dòng, tổng từng nhóm, hiển thị phép tính.
Bước 4. Cộng tổng chi phí chính thức.
Bước 5. Trích quỹ dự phòng (đề xuất 5–10%).
Bước 6. Kiểm tra (Tổng + dự phòng) ≤ 15.000.000 đ. Nếu vượt, cắt giảm hoặc thay hạng mục (không đổi 150 SV, 10 nhân sự), rồi tính lại từ Bước 3.

TONE
Chính xác, minh bạch, ưu tiên con số.

EXTRAS
Đầu ra gồm:
1/ Bảng: STT | Nhóm | Hạng mục | Khung giờ (Prompt 2) | Đơn giá | SL | Thành tiền.
2/ Bảng tổng hợp theo nhóm và % trên 15 triệu.
3/ Dòng kết luận: Tổng, Dự phòng, Còn lại, Đạt/Không đạt.
4/ Chi phí bình quân/SV.
5/ Dòng riêng: ngân sách truyền thông (dùng ở Prompt 5).
```

---

### 5/ Prompt 4. Phân công 10 nhân sự (Prompt Chaining)

```
CONTEXT
- Lịch trình: {{KẾT QUẢ Prompt 2}}
- Dự toán: {{KẾT QUẢ Prompt 3}}

REQUEST
Phân công đúng 10 nhân sự, đặt mã NS1 đến NS10.

EXPLANATION
Mỗi khung giờ phải có người phụ trách và mỗi khoản chi phải có người chịu trách nhiệm, nếu không kế hoạch sẽ có khoảng trống khi vận hành.

ACTION
1/ Lập bảng: Mã | Vai trò | Nhiệm vụ trước sự kiện | Nhiệm vụ trong sự kiện (khung giờ Prompt 2) | Hạng mục chi phụ trách (Prompt 3).
2/ Có người phụ trách: điều phối, nội dung/diễn giả, thực hành/kỹ thuật, check-in, hậu cần, truyền thông, tài chính.
3/ Mỗi khung giờ có ít nhất 1 người; mỗi hạng mục chi có đúng 1 người.
4/ Không giao một người hai việc ở hai khu vực cùng lúc.
5/ Tự kiểm tra: đúng 10 người? Còn khung giờ/hạng mục trống? Có chồng việc?

TONE
Rõ trách nhiệm, công bằng, dễ giao việc.

EXTRAS
Sau bảng thêm 1–2 dòng trả lời trực tiếp: "Ai chịu trách nhiệm gì chính?"
```

---

### 6/ Prompt 5. Kế hoạch truyền thông trước sự kiện (Prompt Chaining)

```
CONTEXT
- Khả thi: {{KẾT QUẢ Prompt 1}}
- Lịch trình: {{KẾT QUẢ Prompt 2}}
- Dự toán (lấy riêng ngân sách truyền thông): {{KẾT QUẢ Prompt 3}}
- Phân công: {{KẾT QUẢ Prompt 4}}

REQUEST
Xây dựng kế hoạch truyền thông TRƯỚC sự kiện để đủ 150 sinh viên đăng ký.

EXPLANATION
Thông điệp phải khớp nội dung thật của lịch trình, người làm phải đúng vai trò ở Prompt 4, chi phí phải nằm trong dòng truyền thông ở Prompt 3, để các hạng mục liên kết với nhau.

ACTION
1/ Đối tượng: sinh viên trong trường. Mục tiêu: đủ 150 đăng ký (có thể nêu dự phòng vắng mặt, không đổi quy mô).
2/ Lập bảng lộ trình: Mốc (T-n) | Kênh | Nội dung | Người phụ trách (NS) | Chi phí.
3/ Nêu cách đo hiệu quả (ví dụ số đăng ký theo mốc).
4/ Tự kiểm tra: người phụ trách đúng vai trò Prompt 4? Tổng chi phí ≤ dòng truyền thông Prompt 3? Nội dung quảng bá khớp lịch trình Prompt 2?

TONE
Trẻ trung, hấp dẫn, phù hợp sinh viên, nhưng thông tin chính xác.

EXTRAS
Các mốc T-n, kênh, chỉ tiêu đo ghi "(đề xuất)".
```

---

### 7/ Prompt 6. Tóm tắt ngữ cảnh dự án (Context Management)

```
CONTEXT
Toàn bộ kết quả Prompt 1 đến Prompt 5 trong hội thoại này.

REQUEST
Nén thành MỘT bản tóm tắt để dán sang hội thoại mới mà không mất thông tin.

EXPLANATION
Hội thoại mới không nhớ gì, nên bản tóm tắt phải đủ con số và ràng buộc để kiểm tra lại kế hoạch.

ACTION
Viết tối đa khoảng 500 từ, dạng khối văn bản copy được, đúng cấu trúc:
1/ Dự án và mục tiêu
2/ Ràng buộc cứng
3/ Giả định (đánh dấu cái chưa xác nhận)
4/ Kết luận khả thi và điều kiện
5/ Lịch trình rút gọn
6/ Ngân sách rút gọn: theo nhóm, tổng, dự phòng, còn lại
7/ Phân công rút gọn: NS1–NS10 kèm vai trò
8/ Truyền thông rút gọn: mốc, kênh, ngân sách, người phụ trách
9/ Rủi ro chính
10/ Việc còn mở

TONE
Súc tích, trung lập, dạng bàn giao.

EXTRAS
Giữ nguyên mọi con số quan trọng. Không thêm thông tin mới, không bỏ ràng buộc.
```

---

### 8/ Prompt 7. Kiểm tra và hiệu chỉnh (hội thoại mới; Chain-of-Thought + Context Management)

```
CONTEXT
Bản tóm tắt dự án "AI Innovation Day", dùng làm cơ sở duy nhất:
{{BẢN TÓM TẮT Prompt 6}}

REQUEST
Kiểm tra tính thống nhất của kế hoạch, phát hiện vấn đề và hiệu chỉnh.

EXPLANATION
Các phần được làm ở nhiều prompt nên dễ lệch nhau (tổng tiền, khung giờ, người phụ trách). Cần đối chiếu chéo từng bước để tìm chỗ lệch và sửa mà không phá ràng buộc cứng.

ACTION
Làm lần lượt, ghi rõ phép đối chiếu:
Bước 1. Ràng buộc: đối chiếu 150 SV, 10 người, ≤ 15 triệu, 1 ngày.
Bước 2. Ngân sách: cộng lại các khoản, tổng và dự phòng có khớp con số đã nêu?
Bước 3. Lịch trình: chồng lấn, hở, quá tải? Hoạt động nào cần tiền mà không có trong ngân sách?
Bước 4. Nhân sự: đúng 10 người? Hạng mục thiếu người hoặc người bị chồng việc?
Bước 5. Truyền thông: kênh, người phụ trách, chi phí khớp phân công và ngân sách?
Bước 6. Rủi ro: với mỗi rủi ro, nêu khả năng, tác động, kế hoạch đã xử lý chưa, đề xuất bổ sung.
Bước 7. Hiệu chỉnh: cách sửa nhỏ nhất cho mỗi vấn đề, rồi kiểm tra lại các bước liên quan.

TONE
Phản biện nghiêm túc, cụ thể, không nể nang nhưng mang tính xây dựng.

EXTRAS
Đầu ra gồm:
1/ Bảng vấn đề: STT | Phần ảnh hưởng | Mô tả | Mức độ | Cách sửa.
2/ Bảng rủi ro.
3/ Phần kế hoạch sau hiệu chỉnh: chỉ ghi mục đã đổi (trước → sau).
4/ Kết luận Đạt/Chưa đạt cho từng ràng buộc cứng.
5/ Nếu thiếu dữ liệu để kiểm tra, ghi "chưa đủ dữ liệu" và hỏi tôi, không tự bịa.
```
## Bản phân tích tính khả thi (Prompt 1)

| Bước | Lập luận và phép tính | Kết quả |
| :--- | :--- | :--- |
| **1. Quy mô** | 150 SV cần 1 hội trường $\ge 150$ chỗ, bàn ghế di động. Chia $30 \text{ bàn} \times 5 \text{ SV}$ | Cần 1 không gian, 30 nhóm |
| **2. Nhân sự** | $10 / 150 = 1 : 15$. Lúc thực hành có 6 người hướng dẫn, $150 / 6 = 25 \text{ SV/người}$ | Đủ, nhưng cần phiếu hướng dẫn |
| **3. Ngân sách** | $15.000.000 / 150 = 100.000\text{ đ/SV}$ | Xem bảng dưới |
| **4. Thời gian** | 07:45–17:30 = 585 phút. Trừ check-in 45, nghỉ trưa 90, giải lao 30, dọn dẹp 45 | 375 phút (6,25 giờ) nội dung |
| **5. Mục tiêu** | Nghe 120 phút + thực hành 180 phút, thực hành chiếm $180 / 300 = 60\%$ nội dung chính | Đạt “hiểu và trải nghiệm” |

* **Ngân sách sơ bộ (triệu đồng):** Thiết bị và mạng `2,5`; nội dung và giải thưởng `4,0`; ăn uống `4,5`; ấn phẩm `1,5`; truyền thông `1,0`; dự phòng `1,0`. **Tổng:** $14,5 \le 15$.

### Rủi ro (Bước 6)

| Rủi ro | Mức | Giảm thiểu |
| :--- | :--- | :--- |
| Mạng yếu khi 150 SV dùng AI cùng lúc | **Cao** | Thuê 2 router 4G, có phiếu hướng dẫn |
| SV đăng ký nhưng vắng | **TB** | Danh sách chờ 20, nhắc lịch T-3 và T-1 |
| SV thiếu thiết bị | **TB** | Ghép nhóm 5 SV, yêu cầu mang laptop hoặc điện thoại |
| Vượt ngân sách do giá thực tế | **TB** | Quỹ dự phòng, chốt báo giá sớm |
| Diễn giả hủy hoặc trễ | **Thấp** | Có 1 diễn giả dự phòng |

* **Kết luận:** Khả thi có điều kiện. Cần mượn được hội trường miễn phí, SV tự mang thiết bị, và mạng ổn định.
* **Cần xác nhận:** Địa điểm và phí thuê, ngày tổ chức, việc SV tự túc cơm trưa.

---

##  Lịch trình sự kiện theo khung giờ (Prompt 2)

| Khung giờ | Hoạt động | Hình thức | Khu vực | Vật dụng |
| :--- | :--- | :--- | :--- | :--- |
| **07:45–08:30** | Check-in | Giao lưu | Cửa hội trường | Thẻ tên, QR |
| **08:30–08:45** | Khai mạc | Nghe | Hội trường | Micro, backdrop |
| **08:45–09:45** | Talk 1: AI trong học tập | Nghe | Hội trường | Micro, máy chiếu |
| **09:45–10:00** | Giải lao | Giao lưu | Sảnh | Bánh, nước |
| **10:00–11:30** | Workshop 1: Thực hành AI trong học tập | Thực hành | 30 bàn nhóm | Phiếu hướng dẫn, wifi |
| **11:30–13:00** | Nghỉ trưa | Nghỉ | Tự do | Cơm cho BTC và diễn giả |
| **13:00–14:00** | Talk 2: AI trong công việc | Nghe | Hội trường | Micro, máy chiếu |
| **14:00–15:30** | Workshop 2: Thực hành AI trong công việc | Thực hành | 30 bàn nhóm | Phiếu hướng dẫn, wifi |
| **15:30–15:45** | Giải lao | Giao lưu | Sảnh | Bánh, nước |
| **15:45–16:30** | Demo nhóm và trao giải | Giao lưu | Hội trường | Micro, giải thưởng |
| **16:30–16:45** | Tổng kết, bế mạc, khảo sát | Nghe | Hội trường | QR khảo sát |
| **16:45–17:30** | Dọn dẹp (BTC) | – | Hội trường | – |

* **Tự kiểm tra:** Tổng thời lượng nội dung 375 phút, khớp Prompt 1. Thực hành đủ 150 SV ($30 \text{ bàn} \times 5$).
* **Danh sách cần chi tiền:** Micro và loa, ổ cắm, router 4G, thù lao diễn giả, giải thưởng, bánh nước, cơm BTC, backdrop, thẻ tên, phiếu hướng dẫn, truyền thông.

---

## Bảng dự toán ngân sách (Prompt 3)

| STT | Nhóm | Hạng mục | Khung giờ | Đơn giá | SL | Thành tiền |
| :---: | :---: | :--- | :--- | :---: | :---: | :---: |
| 1 | A | Micro + loa | Khai mạc, Talk, Demo | 1.200.000 | 1 gói | 1.200.000 |
| 2 | A | Ổ cắm kéo dài | Workshop 1–2 | 60.000 | 15 | 900.000 |
| 3 | A | Router 4G thuê | Workshop 1–2 | 300.000 | 2 | 600.000 |
| 4 | B | Thù lao diễn giả | Talk 1, Talk 2 | 1.000.000 | 2 | 2.000.000 |
| 5 | B | Giải nhất | Demo | 1.000.000 | 1 | 1.000.000 |
| 6 | B | Giải nhì | Demo | 600.000 | 1 | 600.000 |
| 7 | B | Giải ba | Demo | 400.000 | 1 | 400.000 |
| 8 | C | Bánh + nước giải lao | 09:45, 15:30 | 12.000 | 320 | 3.840.000 |
| 9 | C | Cơm trưa BTC + diễn giả | Nghỉ trưa | 35.000 | 12 | 420.000 |
| 10 | D | Backdrop | Khai mạc | 600.000 | 1 | 600.000 |
| 11 | D | Thẻ tên + dây | Check-in | 5.000 | 160 | 800.000 |
| 12 | D | Phiếu hướng dẫn | Workshop 1–2 | 10.000 | 30 | 300.000 |
| 13 | E | Poster A3 | T-14 | 30.000 | 10 | 300.000 |
| 14 | E | Tờ rơi A5 | T-10 | 1.000 | 300 | 300.000 |
| 15 | E | Quảng cáo Facebook | T-12 → T-5 | 600.000 | 1 | 600.000 |

### Tổng hợp theo nhóm

| Nhóm | Thành tiền | % trên 15 triệu |
| :--- | :---: | :---: |
| **A. Thiết bị, mạng** | 2.700.000 | 18,0% |
| **B. Nội dung, giải thưởng** | 4.000.000 | 26,7% |
| **C. Ăn uống** | 4.260.000 | 28,4% |
| **D. Ấn phẩm** | 1.700.000 | 11,3% |
| **E. Truyền thông** | 1.200.000 | 8,0% |
| **Dự phòng** | 1.000.000 | 6,7% |
| **Còn lại** | 140.000 | 0,9% |

* **Kết luận:** Chính thức = `13.860.000 đ`. Dự phòng = `1.000.000 đ`. Tổng = `14.860.000 đ`. Còn lại = `140.000 đ`. **Đạt** ($\le 15.000.000\text{ đ}$).
* **Bình quân/SV:** $13.860.000 / 150 = 92.400\text{ đ}$ (chưa gồm dự phòng).
* **Ngân sách truyền thông:** `1.200.000 đ`.

---

## Bảng phân công 10 nhân sự (Prompt 4)

| Mã | Vai trò | Trước sự kiện | Trong sự kiện | Hạng mục chi |
| :---: | :--- | :--- | :--- | :---: |
| **NS1** | Điều phối chung | Duyệt kế hoạch, họp BTC T-7 và T-1 | Điều phối mọi khung giờ, khai mạc, bế mạc | – |
| **NS2** | Nội dung, diễn giả | Mời diễn giả, chốt nội dung, tiêu chí chấm | Đón diễn giả, Q&A, mentor Workshop, giám khảo Demo | #4 |
| **NS3** | Trưởng kỹ thuật thực hành | Thiết kế 2 bài workshop | Mentor Workshop, giám khảo Demo | #3, #12 |
| **NS4** | Âm thanh, thiết bị | Thuê và test micro, loa, ổ cắm | Âm thanh các phiên, mentor Workshop, trả thiết bị 16:45 | #1, #2 |
| **NS5** | Check-in, danh sách | Quản lý form đăng ký, nhắc lịch | Check-in 07:45, mentor Workshop | #11 |
| **NS6** | Đón tiếp, hỗ trợ khu vực | Làm biển chỉ dẫn | Xếp chỗ, mentor Workshop, hỗ trợ giải lao | – |
| **NS7** | Hậu cần | Đặt bánh nước, cơm trưa | Giải lao 09:45 và 15:30, cơm trưa, mentor Workshop, dọn dẹp | #8, #9 |
| **NS8** | Tài chính, giải thưởng | Lập quỹ, mua giải thưởng | Trao giải 15:45, đối soát chi 16:45 | #5, #6, #7 |
| **NS9** | Truyền thông | Thực hiện kế hoạch truyền thông | Chụp ảnh, đăng tin, QR khảo sát | #13, #14, #15 |
| **NS10** | MC, ấn phẩm | Viết kịch bản MC, đặt backdrop | MC khai mạc, Talk, Demo, bế mạc | #10 |

* **Tự kiểm tra:** Đúng 10 người; 15 hạng mục chi đều có đúng 1 người phụ trách; mỗi khung giờ có ít nhất 1 người phụ trách.
* **Ai làm gì chính:** NS1 điều phối, NS2 nội dung, NS3–NS4 kỹ thuật, NS5–NS6 đón tiếp, NS7 hậu cần, NS8 tài chính, NS9 truyền thông, NS10 MC. Mentor Workshop là NS2–NS7, mỗi người phụ trách 5 bàn.

---

## Kế hoạch truyền thông trước sự kiện (Prompt 5)

* **Mục tiêu:** Đủ 150 đăng ký, kèm danh sách chờ 20 (không đổi quy mô 150).

| Mốc | Kênh | Nội dung | Phụ trách | Chi phí |
| :---: | :--- | :--- | :---: | :---: |
| **T-21** | Nội bộ | Chốt poster và thông điệp | NS9 | 0 |
| **T-21** | Google Form | Tạo form đăng ký (150 chỗ + chờ 20) | NS5 | 0 |
| **T-18** | Fanpage CLB | Teaser “AI giúp học và làm việc thế nào?” | NS9 | 0 |
| **T-14** | Fanpage, group khoa/lớp | Mở đăng ký, công bố lịch trình | NS9 | 0 |
| **T-14** | Tại trường | Dán 10 poster A3 | NS9 | 300.000 |
| **T-12 → T-5** | Facebook Ads | Quảng cáo cho SV trường | NS9 | 600.000 |
| **T-10** | Căng tin, thư viện | Phát 300 tờ rơi A5 | NS9 | 300.000 |
| **T-7** | Fanpage, group | Giới thiệu diễn giả và workshop | NS9 | 0 |
| **T-5** | Form | Đóng đăng ký khi đủ 150, chốt danh sách chờ | NS5 | 0 |
| **T-3** | Email, Zalo | Nhắc lịch, mang laptop/điện thoại, sạc | NS5 | 0 |
| **T-1** | Email, Zalo | Gửi sơ đồ, QR check-in, xác nhận đến | NS5 | 0 |

* **Tổng chi phí:** `1.200.000 đ`, khớp dòng truyền thông ở Prompt 3.
* **Đo hiệu quả:** Đăng ký tích lũy $T-14 \ge 30$; $T-10 \ge 70$; $T-7 \ge 110$; $T-5$ đủ 150. Tỷ lệ đến thực tế $\ge 85\%$ ($\ge 128 \text{ SV}$).

---

## Bản tóm tắt ngữ cảnh dự án (Prompt 6)

* **DỰ ÁN:** AI Innovation Day cho SV trong trường. Mục tiêu: giúp SV hiểu và trải nghiệm AI trong học tập và công việc.
* **RÀNG BUỘC CỨNG:** 150 SV; 1 ngày; 10 nhân sự (NS1–NS10); ngân sách $\le 15.000.000\text{ đ}$ (tổng mọi chi phí).
* **GIẢ ĐỊNH (chưa xác nhận):** Hội trường trường mượn miễn phí, có bàn ghế di động; không có nguồn thu; SV tự túc cơm trưa và tự mang laptop/điện thoại; chưa chốt ngày.
* **KHẢ THI:** Khả thi có điều kiện: (1) có hội trường miễn phí, (2) SV tự mang thiết bị, (3) mạng ổn định. Rủi ro: mạng yếu (Cao), vắng mặt, thiếu thiết bị, vượt giá, diễn giả hủy.
* **LỊCH TRÌNH:** 07:45 check-in; 08:30 khai mạc; 08:45 Talk 1 (AI học tập); 09:45 giải lao; 10:00 Workshop 1 (90'); 11:30 nghỉ trưa; 13:00 Talk 2 (AI công việc); 14:00 Workshop 2 (90'); 15:30 giải lao; 15:45 demo + trao giải; 16:30 bế mạc; 16:45–17:30 dọn dẹp. Workshop: $30 \text{ bàn} \times 5 \text{ SV}$.
* **NGÂN SÁCH:** A thiết bị/mạng `2.700.000`; B nội dung/giải thưởng `4.000.000`; C ăn uống `4.260.000` (320 suất giải lao = $160 \text{ người} \times 2 \text{ đợt}$; 12 suất cơm trưa); D ấn phẩm `1.700.000` (160 thẻ tên); E truyền thông `1.200.000`. Chính thức `13.860.000`; dự phòng `1.000.000`; tổng `14.860.000`; còn lại `140.000`.
* **PHÂN CÔNG:** NS1 điều phối; NS2 nội dung/diễn giả; NS3 kỹ thuật thực hành; NS4 âm thanh/thiết bị; NS5 check-in/danh sách; NS6 đón tiếp; NS7 hậu cần (giải lao, cơm trưa); NS8 tài chính/giải thưởng; NS9 truyền thông; NS10 MC/ấn phẩm. Mentor Workshop: NS2–NS7, mỗi người 5 bàn.
* **TRUYỀN THÔNG:** T-21 → T-1; Fanpage, group khoa/lớp, poster, tờ rơi, Facebook Ads, email/Zalo; ngân sách `1.200.000`; NS9 phụ trách chính, NS5 form và nhắc lịch; đóng đăng ký T-5, danh sách chờ 20.
* **VIỆC CÒN MỞ:** Xác nhận địa điểm và ngày; cơm trưa tự túc; thử tải mạng; kiểm tra lại số suất và số người.

---

## Báo cáo kiểm tra và hiệu chỉnh kế hoạch (Prompt 7, hội thoại mới)

### Bảng vấn đề

| STT | Phần | Mô tả | Mức | Cách sửa |
| :---: | :--- | :--- | :---: | :--- |
| 1 | Ngân sách | Giải lao tính 160 người, thiếu 2 diễn giả (4 suất = 48.000). Thẻ tên thiếu 2 thẻ (10.000) | **Thấp** | Chuyển 58.000 từ dự phòng sang chi phí chính thức |
| 2 | Địa điểm | Giả định hội trường miễn phí chưa xác nhận | **Cao** | Chốt trước T-21. Nếu có phí, bù từ dự phòng + còn lại (tối đa 1.082.000). Vượt thì giảm giải thưởng (2.000.000) |
| 3 | Nhân sự | NS7 mentor đến 15:30 nhưng phải phát giải lao lúc 15:30 | **TB** | NS7 rời bàn lúc 15:15, NS1 nhận 5 bàn trong 15:15–15:30 |
| 4 | Lịch trình | Nghỉ trưa SV tự túc, Talk 2 dễ bị trễ | **TB** | Giữ 13:00, nhắc ở T-3 và T-1 kèm danh sách quán gần trường (0 đ) |
| 5 | Kỹ thuật | Chưa có bước thử tải mạng trước sự kiện | **Cao** | Thêm T-3: NS3 thử tải với 20 thiết bị cùng lúc |

### Bảng rủi ro

| Rủi ro | Khả năng | Tác động | Kế hoạch đã xử lý chưa | Bổ sung |
| :--- | :---: | :---: | :--- | :--- |
| Mạng yếu | TB | Cao | Có 2 router, thiếu thử tải | Thử tải T-3 |
| Vắng mặt | TB | TB | Có danh sách chờ 20 và nhắc lịch | Không cần |
| Địa điểm có phí | TB | Cao | Chưa | Chốt T-21, có phương án cắt giải thưởng |

### Kế hoạch sau hiệu chỉnh (trước → sau)

* **Giải lao:** $320 \rightarrow 324$ suất (`3.840.000` $\rightarrow$ `3.888.000`).
* **Thẻ tên:** $160 \rightarrow 162$ (`800.000` $\rightarrow$ `810.000`).
* **Ngân sách:** Chính thức `13.860.000` $\rightarrow$ `13.918.000`. Dự phòng `1.000.000` $\rightarrow$ `942.000`. Tổng `14.860.000`, còn lại `140.000` (không đổi).
* **Nhân sự (NS7):** Mentor đến 15:30 $\rightarrow$ rời 15:15, NS1 nhận 5 bàn.
* **Truyền thông và kỹ thuật:** Thêm mốc T-3 thử tải mạng (NS3).

### Kết luận kiểm tra lại

| Ràng buộc | Kết quả |
| :--- | :---: |
| 150 SV (danh sách chờ chỉ vào khi có người vắng) | **Đạt** |
| 10 nhân sự | **Đạt** |
| 1 ngày (07:45–17:30) | **Đạt** |
| Ngân sách $14.860.000 \le 15.000.000$ | **Đạt** |