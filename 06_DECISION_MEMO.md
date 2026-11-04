STATUS: G4 **ĐÃ DUYỆT**; kiểm toán RQ4 trên giấy **khép lại** (bộ 00–06, 2026-11-04). Không có thực nghiệm; không sửa đề cương.
SOURCE ANCHORS: v14.3 (sha256 `47d64452…fd46`) H4a/H4b, ĐN5–6, Nội dung 4 (tr.~23); `00`–`05` của bộ này (phụ lục toán và trace chi tiết nằm ở `03` và `05`); quyết định D1–D23 (`01` §E–H).
ASSUMPTIONS: mọi "đề xuất" dưới đây **chưa phê duyệt**; số minh họa (K=5, f=0,2…) không phải giá trị khóa.
OPEN QUESTIONS: §6 (còn các `TBD`).
DECISION: `RQ4_PAPER_AUDIT = KHÉP LẠI (đã duyệt)`; `RQ4_EXPERIMENT = HOLD UNTIL RQ3 DEV + approval`; **NO CHANGE NOW** với đề cương.

---

## 1. Trạng thái các hạng mục

| Item | Trạng thái | Bằng chứng / điều còn thiếu |
|---|---|---|
| H4a | **NO CONCLUSION** (sẽ kết luận riêng (ii) và (iii); "đạt đầy đủ" chỉ khi cả hai — D24) | chưa có thử nghiệm đúng lưới; ASR đã định nghĩa (D1); biên "tốt hơn RONI" chưa chốt (D22) |
| H4b | **NO CONCLUSION** | chưa có đánh giá thích nghi; (iv) chờ δ (cần đặc tả DoP) |
| Mệnh đề 2 | **COUNTEREXAMPLE / ASSUMPTION GAP** (bước `m·s → m·f·s`, cách đọc R1) | chặn quota `min(k, m·s)` PROVED; `(1−β)^m` không suy ra được (union bound `1−mβ` PROVED); 2′ = CANDIDATE. Chứng minh do tôi viết, **cần kiểm độc lập trước khi trích** |
| Chế độ nguồn A/B | đặc tả xong | B: bổ đề không nhận dạng ⇒ `UNCERTAIN` hợp lệ; cờ oan/uncertain/coverage định nghĩa; đo thực = P4b |
| Tấn công (i)–(vi) | đặc tả đủ | (iii)/(v) operational ở mức K1 (D12); (iv) chờ δ; (i),(vi) chỉ điểm gãy |
| Baseline | đặc tả + nguồn VERIFIED | **mã/commit đều MISSING**; B1 = Nelson-protocol adaptation, ART chỉ tham chiếu (D18) |
| Phụ thuộc RQ3 | hợp đồng dữ liệu + máy trạng thái (`05` §1–2) | RQ3 DEV chưa có |
| Tác động lên đề cương | **NO CHANGE NOW** | các phương án ở §4, chưa duyệt |

## 2. Tóm tắt quyết định D15–D17 (để đối chiếu) và D24–D30 (sau G4)

- **D15:** `f` đọc theo **R1** (tỷ lệ lật trên nhãn của nguồn trong ngày). Không dùng R2 để cứu Mệnh đề 2; Mệnh đề 2 như viết = ASSUMPTION GAP/COUNTEREXAMPLE, 2′ = CANDIDATE cần duyệt.
- **D16:** cờ nguồn **bền** đến hết lượt chạy hoặc đến khi xác minh, kèm **giữ hồi tố** dòng trong ngữ cảnh, `V_t`, `D_cal` (PROPOSAL); không-hồi-tố là nhạy cảm; **xung đột** với A4 của Mệnh đề 1 (RQ2), giữ `OPEN` theo T2.
- **D24–D30:** H4a báo riêng từng kiểu (ii)/(iii); trần `B_pass` chung, biến thể B1 giảm lặp phải có tên riêng; HELD nhánh chính = loại, không cấp nhãn ngoài `b`; F1 sạch = macro-F1 tại `λ_t`; HB2-R tách khỏi `LOCK_HB2` với kiểm thử bit-parity; thống kê chính Wilcoxon/HL theo ngày gốc (mô hình hỗn hợp = độ nhạy); "tổn hại đủ lớn" ở P4a = CI qua ngày gốc loại trừ 0 **và** trung vị ≥ 2×SD giữa seed.
- **D17:** nhãn dành riêng η·b theo cách **bảo thủ**: đi tới cùng nguồn, đối thủ không phân biệt, nguồn bị chiếm lật cùng tỷ lệ; rủi ro lạm phát `λ_t` qua `D_cal` là mối đe dọa có tên trong probe (đo ở P4a).

## 3. Rủi ro chính (hồ sơ §7)

1. **Mệnh đề 2 chỉ cho quota `m·s`.** `m·f·s` chỉ khi thêm giả định thiết kế (cap ở tầng eligible, audit lô, retention không giảm bảo đảm, cờ bền+xóa) — đổi nghĩa H4b và thiết kế nạp. Nguồn `UNCERTAIN` (U1, D8) quay về `s` mỗi nguồn.
2. **HB2 × (ii)/(iv).** HB2 chọn benign có điểm tấn công cao ⇒ dòng độc đã qua lọc có thể được ưu tiên giữ (DC-2). Cần telemetry hàng của HB2 (chưa có) và yêu cầu HB2 gỡ dòng hồi tố.
3. **Không nhận dạng ở chế độ B.** Nguồn rời vùng đặc trưng không phân biệt được trung thực/độc nếu thiếu chồng lấn; `UNCERTAIN` không phải FPR=0.
4. **Mục tiêu 50% có thể vô nghĩa** nếu `ASR_excess` không phòng thủ nhỏ (nền tảng tham chiếu Biloš: −3 đến −4 pp, tình huống khác) ⇒ P4a trước.
5. **Lạm phát `λ_t` và nhiễm `V_t`:** hai kênh nằm ngoài `C_t`; lọc ngữ cảnh hoàn hảo vẫn có thể thua.
6. **Chi phí `I_i` trên TabFM** ≈ 1,3 GPU-giờ/ngày ở `b=1000` (ngoại suy của NCS, chưa đo) và B1 ≈ 13 — cần trần chung `B_pass` để so sánh công bằng.
7. **Novelty:** DoP NO-GO về novelty; lý thuyết (2′) novelty thấp; `GAP_NARROW` giữ. Điểm tựa khả dĩ = tổ hợp nguồn→dòng + đối thủ biết phòng thủ + đo trên NIDS với nhãn trễ, nhưng *đúng toán học*, *có tác dụng trên DEV*, *mới*, *đủ chuẩn đóng góp* là bốn việc tách biệt và chưa việc nào xác lập cho RQ4.

## 4. Phương án sửa đề cương (CHƯA PHÊ DUYỆT; không áp dụng)

| ID | Phương án | Được | Mất / rủi ro |
|---|---|---|---|
| O1 | Phát biểu lại Mệnh đề 2 thành chặn quota `m·s`; đưa 2′ thành bổ đề có điều kiện | đúng và kiểm được | bỏ hệ số `f`, yếu hơn về ý nghĩa |
| O2 | Nêu rõ ASR trong H4a là `ASR_excess` (FNR tương đối so với sạch) | hết mâu thuẫn chiều | thay đổi diễn giải "recall họ mục tiêu" |
| O3 | Thêm trạng thái `UNCERTAIN` vào luồng nạp và cách xử lý (U1/U2) | khớp ĐN5 | thêm một tham số quyết định |
| O4 | Ghi surrogate K1 như làm rõ ĐN5 (đối thủ không truy vấn mô hình triển khai) | operational | thu hẹp đối thủ |
| O5 | Nêu "kiểu ii–iii: cả hai" và biên "tốt hơn RONI" sau P4a | kiểm định được | lưới nặng hơn |
| O6 | Nhắc HB2 thay ThreeBin tại H2/Nội dung (liên quan RQ2) | nhất quán | v14.3 đang khóa; ngoài phạm vi RQ4 |
| O7 | Đổi kiểm định chính của Nội dung 1 từ mô hình hỗn hợp sang Wilcoxon/HL theo ngày gốc (D29) | thống nhất với RQ2/RQ3, hợp với đơn vị độc lập | cần ≥6 ngày gốc cho p<0,05 chính xác; NCS cho biết đã có trong mục sửa đề cương — tôi không thấy mục đó nên chưa xác minh |
Lưu ý N-6: RQ2 DECISION_LOG ghi v14.3 không sửa được trong 6 tháng; quyết định có xem xét O1–O6 hay không thuộc NCS.

## 5. Bước tiếp theo (không chạy trong nhiệm vụ này)

1. NCS duyệt `00`–`06`; kiểm độc lập chứng minh ở `03`.
2. Cung cấp: đặc tả DoP, RQ3 DEV trace, danh sách root DEV, trần `B_pass`.
3. Chạy lần lượt P4a → (chốt biên "tốt hơn RONI") → P4b → P4c/P4d, mỗi probe một lượt khắc phục; sau đó tiền đăng ký cuối.

## 6. Còn mở

Q-G4-1…5 đã giải quyết (D24–D28). Còn: giá trị `B_pass` (sau P4d); biên "tốt hơn RONI" (sau P4a); định nghĩa chi tiết "SD giữa các seed" (D30); số ngày gốc DEV (≥6); xung đột A4 (RQ2) cho hồi tố; các `TBD` ở `05` §6. **Hệ quả D26 cần lưu ý:** cờ bền + HELD bị loại ⇒ một nguồn trung thực bị cờ oan mất toàn bộ dữ liệu đến hết lượt chạy; tỷ lệ dòng sạch bị giữ oan phải là chỉ số báo cáo chính.

**Giới hạn:** không có kết quả thực nghiệm; mọi số chi phí là ngoại suy; tìm kiếm tài liệu chỉ qua web chuẩn; 32+ công trình prior art vẫn `SECONDARY`.
