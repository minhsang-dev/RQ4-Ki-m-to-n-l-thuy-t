STATUS: G0 DRAFT — chờ NCS duyệt.
SOURCE ANCHORS: v14.3 docx (sha256 47d64452…fd46) qua S2/S3 trong `00_SOURCE_MANIFEST.md`; hồ sơ 2026-11-04 (S1). Trang = bản dựng LibreOffice (36 tr.), chưa khớp Word: H4a/H4b tr.8; ĐN5 tr.8 & 18; ε₄ tr.20; Nội dung 4, (2-7), (2-8), Mệnh đề 2 tr.23; KT4 tr.16–17.
ASSUMPTIONS: chỉ trích nguyên văn v14.3; mọi diễn giải gắn nhãn.
OPEN QUESTIONS: ASR_A/ASR_B; đặc tả DoP; N-3…N-6 (xem manifest §3, §5).
DECISION: H4a = NO CONCLUSION; H4b = NO CONCLUSION; Mệnh đề 2 = PROOF NOT VERIFIED; NO CHANGE NOW với đề cương.

Nhãn: `ĐC` = ĐỀ CƯƠNG QUY ĐỊNH · `BC` = BẰNG CHỨNG ĐÃ KIỂM · `SL` = SUY LUẬN · `GT` = GIẢ THUYẾT · `CXM` = CHƯA XÁC MINH · `ĐX` = ĐỀ XUẤT SỬA (CHƯA DUYỆT).

---

## A. Nguyên văn then chốt (rút gọn, đã khớp S2)

**RQ4.** "Khi một phần nguồn gán nhãn bị chiếm và lật nhãn có chủ đích, làm thế nào ngăn nhãn độc đi vào ngữ cảnh mà không loại nhầm dữ liệu trôi dạt thật và nguồn trung thực có phân phối khác?"

**H4a.** "Với số nguồn bị chiếm không vượt m_max < K/2 …, bộ lọc hai tầng giảm tỷ lệ thành công của tấn công lật nhãn có chủ đích (kiểu ii–iii, Định nghĩa 5; kiểu i chỉ xuất hiện trên đường cong điểm gãy) ít nhất 50% (tương đối) so với không phòng thủ và tốt hơn RONI; F1 trên dữ liệu sạch giảm không quá ε₄."

**H4b.** "Với đối thủ biết cơ chế phòng thủ (kiểu iv–v; kiểu vi chỉ xuất hiện trên đường cong điểm gãy), bộ lọc hai tầng vẫn giảm tỷ lệ thành công tấn công so với không phòng thủ và so với RONI. H4a và H4b được kiểm định và báo cáo riêng…"

**ĐN5 / Nội dung 4 / Mệnh đề 2 / (2-7), (2-8):** xem S3, giữ nguyên, không chép lại ở đây để tránh sai lệch.

## B. Bảng truy vết yêu cầu

| ID | Yêu cầu / khẳng định | Nhãn | Nguồn (v14.3 hoặc hồ sơ) | Trạng thái G0 |
|---|---|---|---|---|
| R1 | H4a: giảm ASR ≥50% tương đối vs không phòng thủ, kiểu (ii)–(iii) | ĐC | H4a | NO CONCLUSION; ASR chưa có chiều (N-2) |
| R2 | H4a: tốt hơn RONI | ĐC | H4a | "tốt hơn" chưa định nghĩa (khác biệt tối thiểu? kiểm định?) → `OPEN`, chuyển G4 |
| R3 | H4a: F1 sạch giảm ≤ ε₄ | ĐC | H4a; ε₄ = 0,01 F1, lấy giá trị lớn hơn của quy đổi vận hành và 2×SD seed, khóa khi đăng ký | ε₄ "dự kiến" (không phải cuối cùng) — giữ nguyên |
| R4 | H4b: vẫn giảm ASR vs không phòng thủ và RONI, kiểu (iv)–(v) | ĐC | H4b | NO CONCLUSION; **không có ngưỡng 50%** — khác H4a. Ghi nhận, không "bổ sung" |
| R5 | H4a, H4b báo cáo riêng, độc lập kết luận | ĐC | H4b | giữ |
| R6 | Lưới chính = (ii)–(v); (i), (vi) chỉ trên đường cong điểm gãy, m từ 0 đến K−1 | ĐC | Nội dung 4 | giữ. Lưu ý: "H4a nêu (ii–iii)" và "lưới chính (ii)–(v)" nhất quán |
| R7 | ĐN5: K nguồn mô phỏng, m_max < K/2, hai chế độ A/B, đối thủ chỉ sửa nhãn, không sửa đặc trưng, không truy cập trọng số | ĐC | ĐN5 | → G1 |
| R8 | Chế độ B: dùng đo tỷ lệ gắn cờ oan; nguồn khác phân phối xếp "chưa xác định" thay vì bị gắn cờ | ĐC | ĐN5, Nội dung 4 | → G1 (identifiability) |
| R9 | Tầng nguồn = DoP, so sánh ngang hàng cùng ngày; cần đa số trung thực + hoán đổi được | ĐC | Nội dung 4 | đặc tả DoP MISSING |
| R10 | Tầng dòng = I_i (2-7) trên V_t, chỉ cho dòng mới nạp, theo lô; dòng từ nguồn gắn cờ hoặc I_i vượt ngưỡng bị giữ chờ xác minh | ĐC | Nội dung 4 | "ngưỡng" I_i chưa quy định → `OPEN` (không bịa ngưỡng); N-5 |
| R11 | Chính sách nạp: mỗi nguồn ≤ s dòng trong ngữ cảnh | ĐC | Nội dung 4 | giả định của Mệnh đề 2; chưa nói cơ chế cưỡng chế (cắt dòng thừa nào?) → G2 |
| R12 | Mệnh đề 2: xác suất ≥ (1−β)^{m_max}; số dòng độc ≤ m_max·f*·s; ρ ≤ m_max f* s / k; với s = k/K, ρ < f*/2 | ĐC | Mệnh đề 2, (2-8) | **PROOF NOT VERIFIED**; hồ sơ nêu nghi vấn m·s vs m·f·s, độc lập, mẫu số |
| R13 | "Chặn này đúng cho mọi mô hình vì chỉ phụ thuộc chính sách nạp" | ĐC | Mệnh đề 2 | `CXM` — phụ thuộc cách hiểu f* (G2) |
| R14 | kNN: dự đoán không đổi nếu m_max·f*·s < ½ chênh lệch phiếu, theo [49] | ĐC | Mệnh đề 2 | [49] = Jia et al. AAAI'22 (BC: S4); đối chiếu định lý ở G2/G3. Quy tắc vote/tie ở [49] xem S5 G2-8 (`SECONDARY`) |
| R15 | TFM không có bảo đảm tương tự; kiểm bằng thực nghiệm | ĐC | Mệnh đề 2 | giữ |
| R16 | "Chặn trên … vẫn đúng khi phối hợp vì giới hạn s áp cho từng nguồn; khi phối hợp f*, β có thể xấu đi" | ĐC | Mệnh đề 2 | `SL` của chính v14.3; quota ≠ f*·s → G2 |
| R17 | Đối chứng: không phòng thủ, RONI [33], confident learning [32], kNN cleaning; data valuation tùy chọn | ĐC | Nội dung 4 | → G3 |
| R18 | Chỉ số: ASR, F1 sạch, tỷ lệ dòng sạch bị chặn, tỷ lệ nguồn trung thực bị gắn cờ oan (chế độ B) | ĐC | Nội dung 4 | giữ; "ASR = recall họ mục tiêu" = N-2 |
| R19 | RQ3 và RQ4 đánh giá khi ba cơ chế chạy chung; thứ tự "lọc → chọn → nạp" | ĐC | đoạn sau H4b; (2-2) | N-3 |
| R20 | Mục tiêu chứng minh Chuyên đề 2: liên hệ f*, β với power của audit theo số dòng/nguồn/ngày, độc lập và phối hợp | ĐC | cuối Nội dung 4 | đây chính là cái cần DoP; MISSING |
| R21 | Giữ v14.3; không sửa RQ/H/ε/code/data; không chạy; không dùng DoP-N1 làm xác nhận H4 | ĐC (hồ sơ §1) | S1 §1 | tuân thủ |
| R22 | DoP bị đánh giá NO-GO về novelty (2026-10-24/25); novelty RQ4 không dựa tầng nguồn | NCS-STATED | tin nhắn bổ sung | ghi nhận; G3 phải tìm novelty ở tầng dòng, ở cap+bound, hoặc ở tổ hợp; GAP_NARROW (S6) đã nêu |
| R23 | RQ2 khóa = HB2 (CBRS_FAMILY + hard-benign bin); ThreeBin CLOSED_AT_DEV | NCS-STATED + BC(S9) | S9 | v14.3 H2 vẫn ghi ThreeBin; **không đổi**; interface RQ4→RQ2 viết theo HB2 và chú thích |

## C. Phân loại các khẳng định trong hồ sơ S1 §3 (chưa phải kết quả của G0, chỉ ghi nhận trạng thái)

| Khẳng định | Nhãn | Ghi chú |
|---|---|---|
| `#dòng nguồn bị chiếm trong C_t ≤ min(k, m·s)` | `SL` (hồ sơ nêu "chứng minh ngay") | Hiển nhiên khi quota đúng trên toàn context; sẽ viết lại hình thức ở G2, kèm điều kiện cưỡng chế quota |
| Không suy ra được chặn `m·f·s` chỉ từ quota + power của DoP | `SL` (hồ sơ) | Cần phản ví dụ hình thức ở G2 (cố gắng độc lập, không chép) |
| Phản ví dụ 1000 nhãn/ngày, 10 lật, s = 10 | `GT` tính toán số học | Số học hợp lý, nhưng mẫu số "tỷ lệ lật" và quy tắc chọn/nạp chưa nằm trong v14.3 → G2 |
| Union bound ≥ 1 − mβ thay vì (1−β)^m | `SL` | G2 |
| Bằng chứng DoP-N1: 30/30, 60/60 | `CXM` | không có báo cáo gốc; không trích |
| Hub-poison giảm 3.7/3.3 pp trên TabFM (từ S6) | `SECONDARY` | Bilos et al. 2026 — cần mở ở G3 |

## D. Checklist hồ sơ §10, tiến độ G0

- [x] Đối chiếu nguyên văn H4a/H4b, ĐN5, (2-7), (2-8) — **xong**
- [x] Danh mục thiếu — manifest §4
- [x] Sơ đồ triển khai — manifest §7
- [ ] Còn lại thuộc G1–G4

**Dừng tại đây chờ duyệt G0.**

---

## E. Quyết định của NCS sau G0 (2026-11-04) — ghi nhận, áp dụng từ G1

| ID | Quyết định | Nhãn |
|---|---|---|
| D1 | `ASR_raw` = FNR = 1 − recall của họ mục tiêu tại ngưỡng vận hành λ_t của Định nghĩa 6. `ASR_excess` = FNR_attacked − FNR_clean (cùng trace, không phòng thủ). Báo cáo cả hai. "Giảm ≥50% tương đối" của H4a đọc trên `ASR_excess`. | Cách đọc này là **ĐỀ XUẤT SỬA (CHƯA DUYỆT)**, chốt trong preregistration; không đổi văn bản v14.3 |
| D2 | Chưa có tệp DoP ⇒ G2 làm phần độc lập DoP; `f*`, `β` ở trạng thái HOLD | HOLD_PROOF_SOURCE |
| D3 | Thứ tự: RQ3 chọn dòng chưa nhãn → nhãn đến sau τ → lọc RQ4 → HB2 nạp. "Lọc → chọn → nạp" đọc theo chức năng (ngữ cảnh RQ3 dùng cho g(x) chỉ chứa dòng đã lọc). RQ4 **phải lọc trước khi HB2 nạp** (ràng buộc thiết kế; rủi ro HB2×(ii) phân tích ở G1/G4) | Giải quyết N-3 theo cách đọc chức năng |
| D4 | b nhãn/ngày chia cho K nguồn mô phỏng, mỗi nguồn = một chuyên viên | Giải quyết N-4 |
| D5 | Không sửa đề cương; chỉ chuẩn bị phương án | N-6 |
| D6 | Giữ mở cho G2: V_t phụ thuộc tầng nguồn (vòng lặp nếu tầng nguồn hỏng) | N-5 còn mở |
| D7 | Kiểm năm/nơi đăng bài Koh et al. ở G3 | — |

## F. Quyết định của NCS sau G1 (2026-11-04)

| ID | Quyết định | Nhãn |
|---|---|---|
| D8 (Q1) | Dòng của nguồn `UNCERTAIN`: **U1 là chính** (không tự động giữ; đi qua tầng dòng như thường); U2 báo cáo như nhạy cảm. Cơ sở: v14.3 "chưa xác định thay vì bị gắn cờ". | quyết định phân tích |
| D9 (Q2) | Nhiễu nhãn nguồn trung thực: chính = 0; một mức nhiễu ngẫu nhiên đối xứng làm nhạy cảm, giá trị chốt trong prereg (lấy từ prereg DoP nếu có — **prereg DoP đang MISSING**). | PROPOSAL, chưa duyệt |
| D10 (Q3) | `V_t`, `D_cal` lấy từ nhãn η·b dành riêng của K nguồn, **chỉ nguồn không bị gắn cờ** (Nội dung 3); không có tập chuyên viên độc lập riêng. Vòng lặp phụ thuộc giữ mở cho G2. | ĐC (Nội dung 3) + giữ mở |
| D11 (Q4) | Kiểu (iv): lật dòng họ mục tiêu; tỷ lệ lật hằng ngày = ngưỡng phát hiện tầng nguồn trừ δ; δ HOLD đến khi có đặc tả DoP, khóa trong prereg. | quyết định phân tích |
| D12 (Q5) | Surrogate cho (iii)/(v) chấp nhận ở mức K1 (đối thủ tự huấn luyện trên dữ liệu nó được thấy hợp lệ; không truy vấn, không trọng số của mô hình triển khai). Ghi là **làm rõ** Định nghĩa 5, không phải sửa đổi. | làm rõ |
| D13 (Q6) | Lưới chính H4a/H4b ở chế độ A. Chế độ B: đo cờ oan chỉ-trung-thực + tấn công (ii) như phân tích phụ. | quyết định phân tích |
| D14 (Q7) | **PROPOSAL**: K ∈ {5, 10}; m ∈ {1, 2} khi K=5 và {1, 2, 4} khi K=10; thêm đường cong điểm gãy m = 0..K−1. Giá trị cuối chốt sau P4b (số dòng/nguồn/ngày = b/K quyết định công suất DoP). | PROPOSAL, chưa duyệt |

Ghi chú: D8 làm Mệnh đề 2 yếu đi ở chế độ B (xem `03_PROPOSITION_2_AUDIT.md` §8, Hệ quả 2′-b).

## G. Quyết định của NCS sau G2 (2026-11-04)

| ID | Quyết định | Nhãn |
|---|---|---|
| D15 | Cách đọc `f` = **R1** (tỷ lệ lật trên nhãn của nguồn trong ngày — cái audit quan sát). Không dùng R2 để cứu Mệnh đề 2. Trạng thái: Mệnh đề 2 như viết = `ASSUMPTION GAP / COUNTEREXAMPLE`; 2′ = `CANDIDATE`, cần phê duyệt đề xuất. | quyết định phân tích |
| D16 | Cờ nguồn: **PROPOSAL** chính = bền đến hết lượt chạy hoặc đến khi được xác minh, kèm **giữ hồi tố** các dòng của nguồn đó đang trong ngữ cảnh, `V_t`, `D_cal`; không-hồi-tố là nhạy cảm. Xung đột với giả định A4 của Mệnh đề 1 (RQ2) — loại bỏ hồi tố — T2 đã đánh dấu `OPEN`. | PROPOSAL, chưa duyệt; xung đột RQ2 còn mở |
| D17 | Nhãn η·b dành riêng: bảo thủ. Dòng dành riêng đi tới cùng các nguồn, đối thủ không phân biệt được, nên nguồn bị chiếm lật với cùng tỷ lệ. Giữ nguy cơ đầu độc `D_cal` (lạm phát `λ_t`) như mối đe dọa có tên trong các probe G4. | quyết định phân tích |

## H. Quyết định của NCS sau G3 (2026-11-04)

| ID | Quyết định | Nhãn |
|---|---|---|
| D18 | B1 = "RONI (Nelson-protocol adaptation)"; ART chỉ tham chiếu. Dùng ART nguyên trạng thì gọi "ART-RONI" và nêu đủ 5 khác biệt. | quyết định phân tích |
| D19 | Biến thể "V_t sạch oracle": chỉ trong báo cáo phụ; **không** dùng trong bất kỳ quyết định H4a/H4b. | ràng buộc |
| D20 | Tuning tối đa 12 cấu hình trên DEV, **bằng nhau cho mọi nhánh** kể cả S2, S3. | ràng buộc công bằng |
| D21 | `I_i` nhánh chính trên TabFM. LightGBM: xấp xỉ refit lá (giữ cấu trúc cây), chỉ nhánh phụ, ghi rõ là xấp xỉ. | quyết định phân tích |
| D22 | Biên "tốt hơn RONI" của H4a chưa chốt số; chốt sau P4a, trước tiền đăng ký, kèm lý do. | HOLD |
| D23 | Thư mục tham khảo: `koh2022`→`koh2021` (Machine Learning 2021), thêm 2 mục; mục patent ghi rõ là patent. **Đã áp dụng** vào `RQ4_REFS (2).bib`. | đã làm |
