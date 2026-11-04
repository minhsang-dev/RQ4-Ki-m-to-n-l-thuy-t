STATUS: G4 **ĐÃ DUYỆT** (2026-11-04; D24–D30 đã áp dụng). **Không chạy gì.** Toàn bộ là đặc tả và tiền đăng ký NHÁP. Mọi probe: `HOLD UNTIL RQ3 DEV + approval`.
SOURCE ANCHORS: hồ sơ §2.3, §6, §7; v14.3 Định nghĩa 4–6, Nội dung 1, 3, 4; D1–D23 trong `01_REQUIREMENT_TRACE.md`; `02`, `03`, `04` của bộ này; RQ2_DECISION_LOG §8–10 (HB2).
ASSUMPTIONS: A-G4-1 chưa có RQ3 DEV nên giao diện là *hợp đồng dữ liệu đề xuất*, không phải mô tả hệ thống có sẵn. A-G4-2 Số liệu chi phí GPU lấy từ ngoại suy do NCS cung cấp (hệ số Bài 1, **chưa đo**); tôi chỉ suy ra các con số phụ thuộc. A-G4-3 Mọi ngưỡng số mới đánh dấu `TBD` — tôi không đặt; các số cố định duy nhất: 50% và `ε₄ = 0,01` (v14.3).
OPEN QUESTIONS: §9 (Q-G4-1…5 đã giải quyết; còn các `TBD` và xung đột A4).
DECISION: G4 = soạn xong thiết kế; **không** kích hoạt probe. Điều kiện kích hoạt ở §7. Đề cương: NO CHANGE NOW.

Nhãn: `ĐC` đề cương · `SL` suy luận · `ĐX` đề xuất chưa duyệt · `TBD` chưa có căn cứ để chốt · `CXM` chưa xác minh.

---

## 1. Hợp đồng dữ liệu RQ3 DEV → RQ4

Ba bảng; RQ4 chỉ đọc bảng (1) và (2) khi ra quyết định, bảng (3) chỉ dùng để chấm điểm.

**(1) `T_sel` — nhật ký chọn (RQ3 ghi).** `row_id`, `day`, `selection_time`, `chosen_by_policy`, `policy_id`, `seed`, `b`, `τ`, `label_request_time`, `source_id` *(gán lúc yêu cầu nhãn, D4; chế độ A ngẫu nhiên độc lập với `(x, y)`; chế độ B theo subnet)*, `reserved` *(thuộc phần η·b cho V_t/D_cal)*.

**(2) `T_lab` — nhãn đến (nguồn mô phỏng ghi).** `row_id`, `label_arrival_time = selection_time + τ`, `ỹ` (nhãn trả về), `label_status ∈ {PENDING, ARRIVED}`. Đặc trưng `x` đi kèm `row_id` ở bảng dữ liệu luồng.

**(3) `T_eval` — chỉ đánh giá (niêm phong).** `y` (nhãn gốc), `family_id`, `poison_flag`, `A` (tập nguồn bị chiếm), `attack_type`. Truy cập từ code phòng thủ bị chặn (kiểm tra ở prereg bằng "ledger" khẳng định không có cột nào của bảng (3) vào đặc trưng, ngưỡng, hay tham số).

**Quy tắc cứng.**
- R-T1 Mọi truy vấn của RQ4 tại thời điểm `t` chỉ thấy dòng có `label_arrival_time ≤ t`.
- R-T2 `reserved` **không hiển thị cho nguồn** (đối thủ không phân biệt dòng dành riêng, D17); phòng thủ biết `reserved` để dựng `V_t`, `D_cal`.
- R-T3 Chọn của RQ3 trên ngày `t'>t` không được dùng để quyết định ở ngày `t`.
- R-T4 Tập phát lại, thứ tự, seed, `b/τ` giống hệt giữa các nhánh S0–S3, B1–B3 (so cặp trên cùng trace).

**Cái RQ3 phải cung cấp (điều kiện kích hoạt, hồ sơ §6):** trace DEV với đủ các cột trên, một chính sách chọn và quy tắc thay thế đã chốt. Không yêu cầu RQ3 đạt H3.

## 2. Dòng thời gian và máy trạng thái (RQ3 → nhãn trễ → RQ4 → RQ2)

```
 t-τ            t                                    t (cuối ngày)           t+1
  |-- RQ3 chọn dòng chưa nhãn, gán nguồn j --|
  .          [nhãn đến, arr = sel+τ ]
  .             |
  .             v   (1) kiểm toán nguồn: nhãn đến CÙNG NGÀY t của K nguồn
  .             v   (2) cập nhật cờ + hành động hồi tố (D16)
  .             v   (3) dựng lại V_t, D_cal (nguồn không cờ); hiệu chỉnh λ_t (ĐN6)
  .             v   (4) tầng dòng: I_i cho dòng mới đến (nếu nguồn CLEAR/UNCERTAIN)
  .             v   (5) {ACCEPT | HOLD} -> HB2 chỉ thấy ACCEPT -> C_t
  .             v   (6) RQ3 chọn mẫu cho ngày t+1 dùng g(x) trên C_t  (ngữ cảnh chỉ chứa dòng đã lọc)
  .             v   (7) đánh giá trên mẫu cố định với oracle (T_eval) - không quay lại ảnh hưởng 1-6
```

**Trạng thái của một dòng.**

```
SELECTED -> PENDING --(arr<=t)--> ARRIVED
ARRIVED --nguồn FLAGGED--------------------------> HELD_SOURCE
ARRIVED --nguồn CLEAR/UNCERTAIN--> SCORED
SCORED --I_i <= ngưỡng-----------> ACCEPT --HB2--> IN_CONTEXT / EVICTED
SCORED --I_i  > ngưỡng-----------> HELD_ROW
HELD_* --nhánh chính: không xác minh trong lượt chạy = LOẠI (D26)
HELD_* --biến thể 'verify-b': xác minh tiêu nhãn từ chính b--> ACCEPT | DISCARD
nguồn bị cờ sau này: IN_CONTEXT/ACCEPT cũ -> HELD (hồi tố, PROPOSAL D16)
```

**Ghi chú thiết kế.**
- *Trục thời gian thứ hai:* phân bố dòng mà RQ3 hỏi nhãn quyết định tập nhãn RQ4 có thể kiểm toán (ví dụ RQ3 chọn theo bất định ⇒ lô nghiêng về vùng gần biên; audit ngang hàng vẫn hoán đổi được vì gán nguồn xảy ra *sau* chọn, D4). Nhưng RQ4 không dùng quyết định tương lai của RQ3 (R-T3).
- `τ = 7`: audit ngày `t` xử lý dòng chọn ở `t−7`; số nhãn/nguồn/ngày mà kiểm toán thấy vẫn ≈ `b/K` ở trạng thái ổn định (E1).
- *Dòng HELD (D26):* nhánh chính coi HELD là bị loại cho tới hết lượt chạy; báo cáo **tỷ lệ dòng sạch bị giữ oan** (kể cả toàn bộ dòng sạch của nguồn trung thực bị cờ oan, vì cờ bền — D16). Biến thể 'verify-b': mỗi lần xác minh trừ vào `b` (chia sẻ với chọn mẫu RQ3), nên so với S0 phải báo cả hai thiết kế ghép cặp (S0 giữ nguyên `b` cho chọn mẫu / S0 nhận phần nhãn được giải phóng). **Không có nhãn ngoài `b`.**
- *Hồi tố (D16):* khi nguồn `j` bị cờ ở ngày `t`: chuyển dòng của `j` trong `C_{t−1}` (và trong `V_t`, `D_cal`) sang HELD; tính lại `V_t`, `D_cal`, `λ_t`. **Xung đột** với giả định A4 của Mệnh đề 1 (RQ2, loại bỏ hồi tố): giữ `OPEN` theo T2; không đổi RQ2. HB2 chỉ nhận dòng ACCEPT nên phải hỗ trợ "gỡ dòng khỏi ngữ cảnh" — cho phép qua biến thể **HB2-R** (D28): hook gỡ dòng + telemetry từng hàng, tách khỏi `LOCK_HB2`; kiểm thử hồi quy bắt buộc (không lệnh gỡ ⇒ đầu ra giống HB2 từng bit; telemetry không được tiêu thụ RNG hay đổi thứ tự thao tác, nếu không bit-parity vỡ — SL). Xung đột A4 (RQ2) vẫn `OPEN`.
- *Chọn của RQ3 và `g(x)`:* theo D3, ngữ cảnh RQ3 dùng chỉ chứa dòng đã lọc. Bước (6) dùng `C_t` đã qua (5).
- *RQ2 = HB2 (không ThreeBin):* mọi chỗ "chính sách nạp RQ2" = HB2 (S9). v14.3 H2 giữ nguyên chữ.

## 3. Mối đe dọa có tên bổ sung (D17): lạm phát `λ_t` qua `D_cal`

Đối thủ lật attack→benign trên dòng dành riêng cùng tỷ lệ (R-T2, D17) ⇒ `D_cal` (benign) lẫn dòng thật sự là attack ⇒ đuôi điểm benign nặng hơn ⇒ `λ_t` (từ `α_t = A_FP/N̂_t`) tăng ⇒ `FNR` toàn họ tăng, **độc lập** với ngữ cảnh. Kênh này làm `ASR_raw` tăng ngay cả khi lọc ngữ cảnh hoàn hảo.
- **Đo trong P4a** bằng phân rã kênh: (a) kênh ngữ cảnh = `ASR` với `λ_t` tính từ `D_cal` sạch oracle; (b) kênh `λ_t` = hiệu còn lại. Biến thể `D_cal` oracle **chỉ báo cáo phụ** (như D19), không vào quyết định.
- Phòng thủ tầng nguồn phải bảo vệ `D_cal` (không chỉ `C_t`); hồi tố (D16) áp dụng cho `D_cal`.

## 4. Chi phí `I_i` trên TabFM — phân tích và xấp xỉ đề xuất

**Số liệu gốc (A-G4-2).** NCS: ở `b = 1000`, khoảng 800 lần bỏ-một-dòng/ngày; `|V_t| ≈ 300`; ≈ 1,3 GPU-giờ/ngày mô phỏng, vượt ngân sách 1 GPU-giờ/ngày đề xuất cho RQ3.

**Suy ra (SL, từ số trên, chưa đo).** Hệ số ngầm ≈ `1,3·3600/800 ≈ 5,9 s` mỗi lượt suy luận (ngữ cảnh k=2000, `|V|≈300`). Với hệ số đó:

| `b` | dòng chấm/ngày `N=(1−η)b` | GPU-giờ/ngày, chấm đủ `I_i` | ghi chú |
|---|---|---|---|
| 100 | 80 | ≈0,13 | |
| 300 | 240 | ≈0,39 | |
| 1000 | 800 | ≈1,3 | vượt 1 GPU-giờ của RQ3 |

**Nhân với số nhánh (SL):** baseline B1 theo Nelson-protocol cần ≈10 lượt/ứng viên ⇒ ở `b=1000`: 8.000 lượt ≈ **13 GPU-giờ/ngày** theo cùng hệ số. Ngân sách tuning bằng nhau (D20) chưa đủ để công bằng nếu chi phí vận hành lệch 10 lần ⇒ Nguyên tắc đã duyệt (D25): **trần `B_pass` chung** cho mọi nhánh; giá trị đặt sau P4d (`TBD`). Nếu B1 phải giảm số lần lặp để vào trần thì biến thể đó có **tên riêng** (ví dụ "B1-R: RONI Nelson-protocol, reduced repetitions") và không được gọi là B1; báo cáo chi phí thực (lượt/ngày, GPU-giờ) của từng nhánh.

**Xấp xỉ đề xuất cho `I_i` (đều là ĐX; mọi xấp xỉ phải báo sai lệch so với cách chính xác trên mẫu con, theo tinh thần Nội dung 3 v14.3).**

| ID | Cách | Chi phí lượt/ngày | Rủi ro / giới hạn |
|---|---|---|---|
| X0 | chính xác từng dòng (tham chiếu) | `N` | vượt ngân sách ở `b=1000` |
| X1 | **loại theo nhóm** (nhóm `g` dòng cùng nguồn-ngày): ΔL của nhóm; nhóm vượt ngưỡng thì chia nhỏ | ≈ `N/g + N·(1−(1−π)^g)` (π = tỷ lệ độc trong lô, chưa biết) | gần vô ích khi π lớn; tác động cộng dồn giả định; tương tác dòng-dòng bị bỏ qua |
| X2 | **chỉ chấm dòng của nguồn không cờ** (CLEAR/UNCERTAIN) | `N·(1−tỷ lệ dòng nguồn cờ)` | tiết kiệm ≈ `m/K` chỉ khi tầng nguồn bắt được; không giúp với (iv) |
| X3 | **sàng bằng surrogate kNN**: tính LOO của kNN rất rẻ (đóng, `O(κ)`), chọn top-q% rồi chấm chính xác bằng TabFM | `q·N` | độ phủ của surrogate; phải đo recall của dòng độc trong top-q% so với X0 |
| X4 | **trần ngân sách**: chấm theo thứ tự ưu tiên đến hết `B_pass`; phần còn lại HOLD tới hôm sau | `B_pass` | tăng trễ, giảm coverage; quy tắc phải khóa trước |
| X5 | LightGBM nhánh phụ: **refit lá** (giữ cấu trúc cây) (D21) | rất rẻ | xấp xỉ; ghi rõ; báo sai lệch trên mẫu con |

Tái dùng bộ nhớ đệm KV của TabFM để rẻ hóa việc bỏ một dòng: **CXM** — tôi chưa kiểm cơ chế của TabFM/TabICL; không khẳng định tính khả thi.
Giảm `|V_t|` ít giúp (SL): chi phí chủ yếu ở mã hóa ngữ cảnh 2000 dòng, không ở số hàng truy vấn — **CXM** cho tới khi P4d đo.
**Khuyến nghị (ĐX):** X2 làm mặc định, X3 + X4 để vào ngân sách, X1 chỉ khi π nhỏ; chọn bằng P4d.

## 5. Các probe tương lai (chỉ thiết kế; không chạy)

Đơn vị độc lập: **ngày gốc (root day)**; mọi seed/kịch bản sinh từ cùng ngày gốc không độc lập (Nội dung 1). Khai báo ledger phơi nhiễm DEV; RQ2 DECISION_LOG §3 cảnh báo mọi lần chạy lại trên root đã dùng là `DEV_REUSE` — danh sách root của RQ4 **TBD**.

### P4a — tổn hại nền và tính hợp lệ của ASR
- **Câu hỏi:** tấn công (ii) (rồi (iii) với surrogate K1) có làm hại baseline S0 đủ để một "giảm 50%" có nghĩa không?
- **Thiết kế:** cùng trace RQ3; clean | (i) ngẫu nhiên | (ii) | (iii); không phòng thủ; chế độ A; `K=5`, `m ∈ {1,2}` (D14, PROPOSAL); thêm phân rã kênh ngữ cảnh vs `λ_t` (§3).
- **Chỉ số:** `ASR_raw`, `ASR_excess` (D1), macro-F1, Recall/FNR họ mục tiêu, tương quan hạng `Î_i` vs `I_i` (chỉ đánh giá).
- **Điều kiện dừng/kết luận (D30):** "tổn hại đủ lớn" khi thỏa **cả hai**: (a) **cận dưới một phía mức `1−α` của khoảng tin cậy Hodges–Lehmann** của `ASR_excess(att, none)` qua ngày gốc **> 0** (NCS xác nhận 2026-11-04; không cần CI hai phía); (b) trung vị `ASR_excess` qua ngày gốc ≥ 2×SD giữa các seed. SD giữa các seed (D33, đã chốt): SD của `ASR_excess` qua seed trong từng ngày gốc, rồi gộp qua ngày gốc bằng căn bậc hai của trung bình bình phương. Điều kiện (a) dùng cùng quy tắc một phía theo hướng đăng ký (tổn hại > 0): đọc là cận dưới một phía của HL > 0 — **nhất quán với D34, nhờ NCS xác nhận cách đọc**. Không thỏa ⇒ `HOLD_ASSUMPTION`: chưa dùng "50%" làm kết quả có nghĩa. Seed cùng ngày gốc không độc lập nên không được dùng để tăng cỡ mẫu cho (a). Chốt biên "tốt hơn RONI" (D22) **sau P4a, trước tiền đăng ký**.
- **Nhãn kết luận:** `ASSUMPTION FALSIFIED` / `INCONCLUSIVE` / `PROVISIONALLY SUPPORTED` (đều chỉ là DEV).

### P4b — DoP có phân biệt được nguồn bị chiếm (chế độ A và B)?
- **Câu hỏi:** TPR nguồn độc, FPR nguồn trung thực, tỷ lệ `UNCERTAIN`, độ phủ; công suất theo cỡ lô.
- **Lưới cỡ lô** — **[ĐÍNH CHÍNH E1]** kiểm toán thấy `n=b/K` (gồm dòng dành riêng, D14/D17): `b=100`: 20 (K=5), 10 (K=10); `b=300`: 60, 30; `b=1000`: 200, 100. Phần ứng viên vào ngữ cảnh `(1−η)b/K`: 16/8, 48/24, 160/80 (η=0,2) — bảng cũ. Cần NCS xác nhận kiểm toán gồm dòng dành riêng.
- **Quyết định phụ:** chốt `K`, `m` cuối (D14); liên hệ `f*`, `β` với công suất (M2 của 2′) — cần **đặc tả DoP (MISSING)**; không có ⇒ P4b chỉ chạy được dạng "hộp đen".
- **Điều kiện dừng:** nếu chế độ B không nhận dạng ⇒ `UNCERTAIN` là kết quả hợp lệ; **không** ép gắn cờ (hồ sơ §6). Báo cáo U1 (chính) và U2 (nhạy cảm) (D8).

### P4c — `I_i` phân biệt dòng lật với dòng sạch khi có trôi dạt?
- **Thiết kế:** clean/poison × drift/no-drift × chế độ A/B × nhãn đã đến; `V_t` nhiễm theo D17 (chính); biến thể `V_t` sạch oracle (phụ, D19).
- **Chỉ số:** phân bố điểm; AUROC/AUPRC poison vs clean; tỷ lệ dòng sạch bị chặn; không dùng oracle để tính điểm.
- **Đo thêm (DC-2):** tỷ lệ độc trong bin `H` của HB2 so với trong luồng ACCEPT. Cần **telemetry** hàng-theo-hàng của HB2 (RQ2 §10: BASE/H row id chưa từng được ghi) ⇒ điều kiện trước.

### P4d — chi phí và độ trung thực của xấp xỉ `I_i` (mới)
- **Câu hỏi:** đo thực thời gian một lượt TabFM (k=2000, `|V|≈300`) trên GPU thật; so X0 với X1–X5 trên mẫu con.
- **Chỉ số:** giây/lượt, lượt/ngày, GPU-giờ/ngày; Spearman hạng và recall dòng độc trong top-q% so với X0; sai lệch X5 (LightGBM refit lá) so với tính chính xác.
- **Điều kiện dừng:** nếu không xấp xỉ nào vào `B_pass` với recall ≥ `TBD` ở `b=1000` ⇒ `HOLD` S2/S3 TabFM ở `b` đó (hoặc hạ `b`), phương án chuẩn bị cho NCS, không tự đổi lưới.
- *Phối hợp:* NCS nhắc "P3b/P4"; tôi không có tài liệu P3b của RQ3 nên không đối chiếu.

## 6. Tiền đăng ký NHÁP (khung; chỗ `TBD` phải điền trước khi quan sát DEV)

| Mục | Nội dung nháp | Căn cứ |
|---|---|---|
| Tập DEV | root, ledger phơi nhiễm, đơn vị độc lập = ngày gốc | TBD; Nội dung 1 |
| Cấu hình | `K ∈ {5,10}`; `m` theo D14; `s`, tỷ lệ lật, `b ∈ {100,300,1000}`, `τ ∈ {0,1,7}` | D14 PROPOSAL; ĐC |
| Tấn công | lưới chính (ii)–(v), chế độ A; điểm gãy (i),(vi); chế độ B: cờ oan + (ii) phụ | ĐC; D13 |
| Đối thủ | K0/K1 (surrogate), không K2; (iv): lật họ mục tiêu, tỷ lệ = ngưỡng tầng nguồn − δ; δ `TBD` sau khi có đặc tả DoP | D11, D12 |
| ASR | `ASR_raw`, `ASR_excess` (D1); `RR = 1 − ASR_excess(att,def)/ASR_excess(att,none)`; chẩn đoán `ASR_excess_matched` | D1 (ĐX) |
| Estimand H4a — **PROPOSAL (D37)** | Xem §6c. Mỗi ô × mỗi kiểu (ii)/(iii), không gộp; đạt chỉ khi **cả ba** kiểm định T1–T3 (Wilcoxon một phía, đơn vị = ngày gốc) cùng đạt. F1 = **macro-F1 tại `λ_t`** (D27) | ĐC (50%, ε₄ = 0,01); biên T2: D22 |
| Estimand H4b — **PROPOSAL (D37)** | Xem §6c, T4. Mỗi ô × mỗi kiểu (iv)/(v), không gộp; **không** có mục tiêu 50% | ĐC |
| Báo cáo riêng | H4a, H4b độc lập kết luận | ĐC |
| Kiểu (ii)–(iii) | **Kết luận H4a riêng cho từng kiểu** (D24): "H4a(ii) đạt/không đạt", "H4a(iii) đạt/không đạt", mỗi kiểu có RR, so RONI, ΔF1. Chỉ được viết "H4a đạt đầy đủ" khi cả hai đạt; nếu chỉ một đạt, viết đúng kiểu đó | D24 |
| Nhánh | S0–S3, B1 (Nelson-protocol adaptation), B2, B3; B4 tùy chọn (D18) | ĐC; D18 |
| Tuning | tối đa 12 cấu hình DEV, **bằng nhau mọi nhánh** kể cả S2, S3 (D20); thêm trần `B_pass` chung | D20; ĐX |
| `I_i` | TabFM chính; LightGBM refit lá phụ (D21) | D21 |
| Thống kê chính | **Wilcoxon signed-rank một phía (hướng đăng ký trước) + ước lượng Hodges–Lehmann theo ngày gốc** (D29, D32): gộp seed/kịch bản về **một giá trị mỗi ngày gốc** (trung vị) cho mỗi hiệu cặp; **quyết định theo từng ô, trung vị ΔF1 qua ngày gốc, không gộp các ô**; CI từ HL. **Khai báo độ lệch:** khác chữ v14.3 Nội dung 1 (mô hình hỗn hợp là kiểm định chính); nguồn của quyết định: mục 4 §5 `THESIS_AUDIT_LOG_RQ3_RQ4.md` (chưa có trong repo, D31). Mô hình hỗn hợp (ngày gốc ngẫu nhiên, kịch bản lồng, bộ dữ liệu cố định) + bootstrap phân cấp chỉ **độ nhạy**. Công suất mô phỏng trước đăng ký; <0,8 ⇒ thêm ngày gốc, không nới biên. *Cỡ mẫu (D34):* kiểm định một phía ⇒ p chính xác nhỏ nhất = 1/2ⁿ; cần `1/2ⁿ < α/m` (`m` = số kiểm định trong họ khai báo; bảng ở §6b) | D29, D32, D34 |
| Chi phí | thời gian/CPU LightGBM; GPU-giờ TabFM mỗi nhánh; báo cáo tách chi phí nhãn và độ trễ | hồ sơ §6 |
| `V_t`, `D_cal` | nhiễm theo D17; biến thể sạch oracle chỉ phụ (D19) | D17, D19 |
| Kiểm soát | split-before-poisoning; Test nguyên vẹn; ngưỡng chỉ trên Validation/DEV; so cặp trên cùng trace; không suy luận độc lập từ seed cùng root | hồ sơ §6 |
| Quy tắc quyết | `GO / HOLD / NO-GO` tính từ effect size cùng CI; ngưỡng số mới `TBD`, xin duyệt trước DEV | hồ sơ §6 |

## 6b. Số ngày gốc tối thiểu (D34)

Kiểm định Wilcoxon một phía chính xác: `p_min = 1/2ⁿ`. Điều kiện `1/2ⁿ < α/m`. Minh họa với `α = 0,05` (**α và `m` chưa được v14.3 hay hồ sơ ấn định cho RQ4**; số liệu dưới chỉ là phép tính):

| m (số kiểm định trong họ) | n ngày gốc tối thiểu | p_min = 1/2ⁿ |
|---|---|---|
| 1 | 5 | 0.0312 |
| 2 | 6 | 0.0156 |
| 4 | 7 | 0.0078 |
| 8 | 8 | 0.0039 |
| 12 | 8 | 0.0039 |
| 16 | 9 | 0.0020 |

Hệ quả: nếu H4a xét (ii) và (iii) qua nhiều ô thì `m` lớn; ví dụ `m = 4` cần ≥7 ngày gốc (khớp lời NCS). Họ `m` phải khai báo trước cùng với danh sách ô; **Phạm vi (đính chính theo NCS và `THESIS_AUDIT_LOG` §2 cuối):** quy tắc `1/2ⁿ < α/m` chỉ ràng buộc **họ kiểm định xác nhận** đã khai báo; thiếu ngày gốc ở đó ⇒ `HOLD` cho nghiên cứu xác nhận (không đào thêm vòng DEV). Các probe DEV P4a–P4d dùng **p chưa hiệu chỉnh** làm tiêu chí và chỉ báo p-Holm để mô tả (như RQ3 G4); thiếu ngày gốc ở DEV **không** tự dẫn tới HOLD. Họ xác nhận: Holm trên một họ chính nhỏ khai báo trước, hoặc đủ ngày gốc (RQ3 chọn phương án họ chính `m ≤ 3` ô, sau P3a). Cùng quy tắc với tiền đăng ký RQ3.

## 6c. Quy tắc quyết định H4a/H4b — **PROPOSAL** (D37, NCS 2026-11-04; thay thế đề xuất theo mẫu Q15 ở phiên bản trước)

Áp dụng cho **từng ô và từng kiểu tấn công**, không gộp. Đơn vị = **ngày gốc `r`** (mỗi đại lượng là trung vị qua seed/kịch bản trong ngày gốc đó). Mọi kiểm định là Wilcoxon signed-rank **một phía** theo hướng đăng ký trước, mức `α`. Ký hiệu `A(·)` = `ASR_excess` (D1) của nhánh; `none` = S0; `def` = S3; B1 theo D18 (biến thể giảm lặp phải có tên riêng, D25).

| ID | Hỏi | Đại lượng theo ngày gốc `r` | H1 (một phía) |
|---|---|---|---|
| T1 | giảm ≥ 50% | `d_r = 0,5·A_r(none) − A_r(def)` | `d_r > 0` |
| T2 | tốt hơn RONI | `e_r = A_r(B1) − A_r(def) − biên` | `e_r > 0`; `biên` chốt sau P4a (D22), `TBD` |
| T3 | F1 sạch không kém (non-inferiority) | `g_r = ε₄ − (F1_r(none) − F1_r(def))` | `g_r > 0`, `ε₄ = 0,01`, F1 = macro-F1 tại `λ_t` |
| T4 | H4b: giảm so với không phòng thủ | `h_r = A_r(none) − A_r(def)` | `h_r > 0` |
| T5 | H4b: tốt hơn RONI | `k_r = A_r(B1) − A_r(def)` | `k_r > 0` (không có biên: v14.3 và NCS không nêu; thêm biên là ĐX riêng) |

**Quy tắc đạt.** Ô × kiểu (ii) hoặc (iii) **đạt H4a** khi T1, T2, T3 **cùng** đạt. Ô × kiểu (iv) hoặc (v) **đạt H4b** khi T4 và T5 cùng đạt (H4b không có điều kiện F1 theo chữ v14.3; F1 sạch chỉ báo cáo — D38). Phát biểu kết luận ghi đúng ô và kiểu đạt; "H4a đạt đầy đủ" chỉ khi cả (ii) và (iii) đạt (D24). Trung vị `RR_r` chỉ báo cáo làm **ước lượng điểm**, không dùng để kiểm định; HL cho hiệu mỗi kiểm định để báo cáo kích thước hiệu ứng.

**Lý do (NCS):** Wilcoxon trên hiệu `A(none) − A(def)` chỉ cho biết có giảm, không kiểm được ≥50%; `RR_r` không ổn định khi mẫu số nhỏ và vô định khi `A_r(none) ≤ 0`; `d_r` luôn xác định.

**Sáu ghi chú — NCS xác nhận 2026-11-04 (D38); ghi vào tiền đăng ký:**
1. *Ngày gốc có `A_r(none) ≤ 0`:* **giữ nguyên quy tắc.** Không có tổn hại thì không có gì để giảm; ngày đó kéo T1 xuống là đúng bản chất. Đây là lý do P4a chạy trước. Báo số ngày gốc thuộc loại này.
2. *Hiệu bằng 0:* dùng **Wilcoxon chính xác, bỏ các hiệu bằng 0**; quy tắc `1/2ⁿ < α/m` áp lên **`n` hiệu dụng sau khi bỏ** (khai báo trong tiền đăng ký; số ngày gốc kế hoạch phải dự phòng ngày hiệu bằng 0, ví dụ ngày chạm trần).
3. *T3:* báo cáo ước lượng **HL**; thêm **sign test** làm phân tích độ nhạy (không cần giả định đối xứng). Sign test cũng có `p_min = 1/2ⁿ` trên `n` hiệu dụng.
4. *Đếm `m`:* theo **ô × kiểu tấn công**. Một ô chỉ đạt khi **mọi kiểm định trong ô đều đạt ở mức `α`** (giao–hợp, intersection–union), nên không hiệu chỉnh thêm cho số kiểm định trong ô. **H4a và H4b là hai họ kiểm định riêng** (`m` đếm riêng).
5. *T2 với B1 giảm lặp:* biến thể phải có tên riêng (D25); T2 báo theo tên nhánh thực dùng.
6. *H4b:* chỉ gồm **T4 và T5**, đúng chữ v14.3. F1 sạch **vẫn báo cáo nhưng không kiểm định**.
7. *Phạm vi (D36):* ở DEV (P4a–P4d) p chưa hiệu chỉnh làm tiêu chí, p-Holm mô tả; ở xác nhận, họ khai báo trước.

**Quy tắc GO/HOLD/NO-GO (hồ sơ §6, giữ nguyên nghĩa).**
- GO: giả định được hỗ trợ sơ bộ và đo được trong nguồn lực; chỉ GO thiết kế tiếp, sau phê duyệt.
- HOLD: bất định hoặc thiếu trace/nguồn/cỡ mẫu; một lần khắc phục, không đào nhiều vòng DEV.
- NO-GO: giả định bị phản ví dụ logic hoặc không đo được bằng tín hiệu cho phép ⇒ memo phương án sửa, không tự sửa.

## 7. Điều kiện kích hoạt và STOP

**Kích hoạt P4a–d** chỉ khi đồng thời: (1) NCS phê duyệt bằng văn bản; (2) có trace RQ3 DEV với đủ cột §1, chính sách chọn và quy tắc thay thế đã chốt; (3) tiền đăng ký đã điền hết `TBD` quan trọng và được khóa; (4) với P4c: telemetry HB2 sẵn sàng; (5) với P4b: đặc tả DoP có thật (hoặc chấp nhận chạy hộp đen và bỏ phần `f*`, `β`).
**STOP ngay** nếu thiếu RQ3 DEV, hoặc phát hiện oracle rò rỉ vào phòng thủ, hoặc cần sửa mã RQ2/RQ3/DoP đã khóa.

## 8. Việc phụ thuộc bên ngoài RQ4

| Phụ thuộc | Trạng thái |
|---|---|
| RQ3 DEV trace + chính sách chọn | chưa có |
| Đặc tả DoP (`f*`, `β`, cờ bền) | MISSING |
| HB2-R: gỡ dòng hồi tố; telemetry hàng; kiểm thử hồi quy bit-parity với HB2 | cho phép (D28), chưa có; xung đột A4 RQ2 mở |
| Đo thực chi phí TabFM (P4d) | chưa đo |
| Danh sách root DEV + ledger phơi nhiễm RQ4 | TBD |

## 9. Câu hỏi mở cho NCS

Đã giải quyết (`01` §I): Q-G4-1 → D24; Q-G4-2 → D25; Q-G4-3 → D26; Q-G4-4 → D27; Q-G4-5 → D28. Còn mở:
- giá trị `B_pass` (sau P4d); biên "tốt hơn RONI" (D22); các `TBD` ở §6;
- định nghĩa chi tiết "SD giữa các seed" cho D30 (ĐX ở §5 P4a);
- xung đột A4 (RQ2) cho hồi tố;
- danh sách root DEV (số ngày gốc theo §6b: `n > log₂(m/α)`, tức ≥5 khi `m=1`, ≥7 khi `m=4` với α=0,05).
