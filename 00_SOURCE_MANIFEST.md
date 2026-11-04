STATUS: G0 DRAFT — chờ NCS duyệt trước G1. Kiểm toán trên giấy, không chạy code/thực nghiệm.
SOURCE ANCHORS: hồ sơ RQ4 2026-11-04 (§5 G0, §9); v14.3 docx sha256 47d64452…fd46; các tệp repo ở §2.
ASSUMPTIONS: (a) nội dung do NCS nêu trong tin nhắn bổ sung (DoP NO-GO, HB2, công thức) được ghi là `NCS-STATED` đến khi có tệp gốc; (b) số trang là của bản dựng LibreOffice, chưa đối chiếu trang Word.
OPEN QUESTIONS: xem §5.
DECISION: G0 = **GO có điều kiện** cho G1 (chỉ phần threat model). **HOLD_PROOF_SOURCE một phần** cho G2 (thiếu đặc tả DoP; công thức (2-7)/(2-8) đã có).

---

## 1. Tài liệu đã thật sự mở (đọc trong phiên này)

| ID | Tệp | Cách kiểm | Tình trạng |
|---|---|---|---|
| S1 | `RQ4_CLAUDE_CLOUD_HOSO_VA_HUONGDAN_2026-11-04.md` | đọc toàn văn | VERIFIED READ |
| S2 | `De_cuong_NCS_NguyenMinhSang_v14.3-2710_50ref.docx` (sha256 `47d6445299f578acb756bd16869e8dd01c63c4edd0a8037ee33a6993ed60fd46`) | pandoc → text (780 dòng) + LibreOffice → PDF 36 trang + đọc OMML trong `document.xml` | VERIFIED READ (các mục RQ4, ĐN5–6, Nội dung 4, ĐN/biên ε) |
| S3 | `outline_v14_3_RQ3_RQ4_verbatim.txt` | đọc; **mọi đoạn dài (>60 ký tự) khớp nguyên văn với S2** (script so khớp, 0 lệch) | VERIFIED = S2 |
| S4 | `outline_v14_3_references.txt` ([1]–[50]) | đọc; đối chiếu [33] Nelson, [34] Vejling, [35] Xu, [49] Jia et al. | VERIFIED READ |
| S5 | `RQ4_PRIOR_ART (2).md` (sha `2b002123…`) | đọc toàn văn | READ; nội dung = **kết quả tìm kiếm của agent trước, chưa mở lại URL** → `SECONDARY` |
| S6 | `RQ4_GAP (2).md` (sha `115d4a9d…`) — kết luận `GAP_NARROW` | đọc toàn văn | READ; `SECONDARY` |
| S7 | `RQ4_BASELINE_SPECS (2).md` (sha `6e42660633…`) | đọc toàn văn | READ; `SECONDARY` (9 mục `[VERIFY]`) |
| S8 | `RQ4_REFS (2).bib` (sha `68177467…`; 43 mục = 40 công trình + 3 phần mềm) | đọc danh sách khóa + 3 mục mẫu | READ |
| S9 | `RQ2_DECISION_LOG (7).md` (sha `a4095066…`) | đọc toàn văn | READ; ghi nhận kết quả RQ2 là bằng chứng đã quan sát của NCS |

Quy ước: `SECONDARY` = báo cáo của agent trước; chưa được tôi kiểm lại với bài gốc. Sẽ chỉ nâng lên `VERIFIED` khi mở bài gốc ở G3.

## 2. Hai công thức — kết quả truy xuất (yêu cầu cốt lõi của G0)

| Công thức | Truy xuất | Nguyên văn | Nhận xét |
|---|---|---|---|
| (2-7) | S2/S3, văn bản thường | `I_i = L_V(C) − L_V(C∖{(x_i,y_i)})` | `VERIFIED`. Dấu: I_i > 0 ⇔ bỏ dòng làm **giảm** loss ⇔ dòng có hại. Ngược chiều dấu với g(x) ở (2-6) (`L(C) − L(C∪{·})` > 0 ⇔ có lợi). Ghi nhận `SIGN_CONVENTION_NOTE`. |
| (2-8) | S2 dạng OMML (pandoc không dựng được, đã đọc trực tiếp XML) | `ρ ≤ m_max · f* · s / k` | `VERIFIED` ở cả S2 và S3. |
| Trích dẫn phụ trong cùng đoạn | S2 trang ~23 | `ρ ≤ (m_max/K)·f < f*/2` khi `s = k/K` | Cùng đoạn có `f` và `f*` (xem §3, N-1). |

Kết luận: `HOLD_PROOF_SOURCE` **không còn áp dụng cho công thức**; **vẫn còn áp dụng cho định nghĩa của `f*`, `β`**, vì chúng gắn với "công suất của kiểm toán ngang hàng" (DoP) mà v14.3 chỉ nói là "báo cáo kỹ thuật nội bộ, chưa công bố" (S2, danh mục viết tắt và mục 3.1).

## 3. Vấn đề ký hiệu / nhất quán phát hiện ở G0 (chưa sửa gì)

| Mã | Vấn đề | Chỗ thấy |
|---|---|---|
| N-1 `NOTATION_ISSUE` | Phát biểu Mệnh đề 2 dùng `f` ("tỷ lệ lật vượt f"), công thức (2-8) và đoạn sau dùng `f*`. Chưa có định nghĩa `f*` ≠ `f` hay `f* = f`. Hồ sơ §3.1 viết `f` (NCS đã ghi nhận). | S2/S3 Nội dung 4 |
| N-2 `METRIC_DIRECTION_ISSUE` | Nội dung 4: "tỷ lệ thành công tấn công (**recall trên họ mục tiêu**)". Tấn công (ii) lật attack→benign thành công khi recall họ mục tiêu **giảm**. Hai khả năng: (A) ASR = 1 − recall_target (hay FNR họ mục tiêu), "giảm ≥50% tương đối" nghĩa là FNR giảm một nửa; (B) ASR = recall_target thô, thì "giảm ASR" là giảm recall, trái mục đích. Cần chốt trước P4a; ở G1 tôi chỉ dùng ký hiệu `ASR_A` / `ASR_B` và không chọn. | S2/S3 Nội dung 4; hồ sơ §2.1 |
| N-3 `INTERFACE_CONFLICT` (trục thời gian) | (2-2) gọi N_t là "dòng vừa có nhãn **đã qua bộ lọc**", còn đoạn thứ tự ghi "lọc → chọn → nạp"; nhưng RQ3 *chọn dòng chưa nhãn* trước, nhãn đến sau τ, nên "lọc trước chọn" không thể là lọc trên nhãn chưa có. Cần timeline ở G4; ở đây chỉ ghi mâu thuẫn. | S2 §2 sau H4b; Định nghĩa 4 |
| N-4 `INTERFACE_CONFLICT` (nguồn nhãn) | Định nghĩa 4: "chuyên viên" gán nhãn theo ngân sách b. Định nghĩa 5: nhãn đến từ **K nguồn mô phỏng**. Chưa nói K nguồn là cách chia b nhãn của chuyên viên hay một cơ chế khác. | ĐN4 vs ĐN5 |
| N-5 `CIRCULARITY_RISK` | V_t "gồm các dòng có nhãn chuyên viên, **từ nguồn không bị gắn cờ**" (Nội dung 3). I_i (2-7) tính trên V_t, nên tầng dòng phụ thuộc kết quả tầng nguồn. Nếu nguồn bị chiếm lọt vào V_t, dấu hiệu RONI sai; nếu V_t là nhãn chuyên viên sạch thì không cần "không bị gắn cờ". Chưa rõ giả định. | Nội dung 3, Nội dung 4 |
| N-6 `VERSION_CONFLICT` | Hồ sơ §10 cho phép "cân nhắc sửa đề cương sau RQ3 DEV"; RQ2 Decision Log §2 ghi v14.3 "cannot be revised for 6 months". Cần NCS xác nhận quy tắc nào chi phối memo G4. | S1 vs S9 |
| N-7 | Kiểu tấn công trong bảng hồ sơ §2.2 khớp ĐN5; "hai chế độ A/B" ở v14.3 là (a) ngẫu nhiên hoán đổi được, (b) theo mạng con. Khớp. | ĐN5 |

## 4. Kiểm kê hiện vật theo hồ sơ §4/§9

Trạng thái: `VERIFIED` (đã mở và đọc) / `MENTIONED_ONLY` (được nêu tên, chưa có tệp) / `MISSING`.

| Hiện vật | Tình trạng | Hệ quả |
|---|---|---|
| De_cuong v14.3 docx | VERIFIED | — |
| `BAN_GIAO_NCS_CHATGPT_KHAC_2026-11-01.md` | **MISSING** (hồ sơ liệt kê "bắt buộc", không có trong upload/repo) | Quy tắc phân biệt bằng chứng lấy từ hồ sơ S1 thay thế |
| `RQ3_RQ4_ONE_PASS_ARTIFACT_INVENTORY_DECISION_2026-11-01_2.md` | **MISSING** | Không kiểm được "proof gap" gốc; chỉ dùng phản ví dụ mục 3.3 của S1 (đã nêu trong hồ sơ, chưa kiểm độc lập) |
| `RQ4_GATE2_PROOF_NOVELTY_AUDIT_2026-11-01.md` | **MISSING** | G2 làm từ đầu, không dựa audit cũ |
| `dop_confirm_report.md`, `dop_confirm_results.json` | **MISSING** | Không trích số DoP-N1 (30/30, 60/60) vào bài; chỉ nhắc như lời hồ sơ |
| `preregistration_dop_confirmatory (2).md`, `…appendix_A (1).md`, `dop_dafar_decisions.md` | **MISSING** | — |
| Đặc tả/mã DoP (`f*`, `β`, ngưỡng, thống kê kiểm định) | **MISSING** (v14.3: "báo cáo kỹ thuật nội bộ, chưa công bố") | **G2 không thể liên hệ f*, β với power** nếu không có; G2 chỉ làm phần độc lập DoP (quota bound, union bound, kNN) |
| Mã RONI-like / `I_i` của NCS | **MISSING** | S2 baseline = chỉ đặc tả, không "VERIFIED" |
| Mã/commit/hash của B1–B3 (ART `RONIDefense`, cleanlab, kNN cleaning) | **MENTIONED_ONLY** (S7 có URL, phiên bản, `[VERIFY]`; không có commit/hash đã kiểm) | G3 phải mở nguồn |
| PDF các baseline (Nelson, Northcutt, Paudice, Vejling, Jia…) | **MISSING** (chỉ có URL trong S5/S7) | Prior art cũ giữ `SECONDARY` |
| RQ3 DEV interface/trace | **MISSING** (đúng dự kiến: chưa có) | P4a–c = HOLD; G4 chỉ đặc tả |
| Bằng chứng DoP **NO-GO về novelty** (2026-10-24/25) | **MENTIONED_ONLY** (NCS-STATED trong tin nhắn) | Ghi vào requirement trace; novelty RQ4 không dựa tầng nguồn |
| RQ2 policy khóa = **HB2** (CBRS_FAMILY + hard-benign bin); ThreeBin = `CLOSED_AT_DEV` | VERIFIED qua S9 §1, §8–9 và NCS-STATED | Hồ sơ S1 và v14.3 vẫn nói "ThreeBin". Không đổi H2 trong v14.3. Interface RQ4→RQ2 phải nói theo HB2 |
| `THESIS_AUDIT_LOG_RQ3_RQ4.md` (tệp upload `…_1.md`, sha256 `49fd799f…`) | **VERIFIED READ** (2026-11-04). Chưa nằm trong repo; NCS sẽ đưa vào `sources/` | D31, D32 VERIFIED (D32 có ghi chú: Q15 ở §4, nhãn PROPOSAL) |
| Prior art cũ (S5–S8) | READ, `SECONDARY` | Mở rộng, không làm lại (theo chỉ dẫn NCS) |

## 5. Danh mục thiếu / câu hỏi cho NCS (chỉ những điểm không suy ra được từ hồ sơ)

1. **Đặc tả DoP** (mã hoặc báo cáo kỹ thuật): định nghĩa phát biểu "phát hiện nguồn có tỷ lệ lật > f với xác suất ≥ 1−β" — mẫu số (theo ngày/theo dòng nạp?), số dòng mỗi nguồn mỗi ngày, giả định độc lập/hoán đổi. Thiếu ⇒ G2 phần liên hệ f*, β = `HOLD_PROOF_SOURCE`.
2. **Chốt ASR** (N-2): `ASR_A` hay `ASR_B`?
3. **Tên đúng của các tệp MISSING** ở §4 có upload được không, hay coi như không tồn tại trong phạm vi RQ4?
4. N-3/N-4/N-5/N-6 ở §3: chỉ cần NCS xác nhận "để G1/G4 xử lý như `INTERFACE_CONFLICT` mở", không phải quyết ngay.

## 6. Rủi ro tương tác mới thấy ở G0 (`SUY LUẬN`, chưa kiểm — chuyển sang G1)

**R-HB2.** RQ2 hiện khóa bin hard-benign: giữ dòng **nhãn benign mà mô hình chấm điểm tấn công cao** (S9 §8). Tấn công (ii) lật attack→benign tạo đúng loại dòng này. Nếu hard-benign bin nằm *sau* RQ4 trong luồng thì chỉ nhận dòng đã ACCEPT; nếu RQ2 nạp bất kỳ dòng nào có điểm cao, nó **ưu tiên giữ dòng độc**. Điều này (a) làm quota `s` của Mệnh đề 2 chưa đủ (dòng độc có xác suất được chọn cao hơn ngẫu nhiên, ngược với giả định "ngẫu nhiên" của phản ví dụ nhẹ), và (b) là lý do bắt buộc phải định nghĩa thứ tự RQ4→RQ2. Sẽ phân tích ở G1/G4; không kết luận ở đây.

---

## 7. Sơ đồ triển khai (G0 → G4)

```
G0 SOURCE   [xong, chờ duyệt] -> 00_SOURCE_MANIFEST, 01_REQUIREMENT_TRACE
   | NCS duyệt
G1 THREAT   -> 02_THREAT_AND_ATTACKS      (bảng obs/budget/action/timing/knowledge × (i)-(vi) × mode A/B)
   | NCS duyệt
G2 PROOF    -> 03_PROPOSITION_2_AUDIT     (m·s vs m·f·s; mẫu số; union bound; collusion; kNN)
   |           phần f*,β ↔ power: HOLD nếu chưa có đặc tả DoP
G3 BASELINE -> 04_BASELINE_AND_NOVELTY    (mở bài gốc S5-S8; mở rộng, không làm lại)
G4 FUTURE   -> 05_RQ3_INTERFACE_AND_FUTURE_GATES, 06_DECISION_MEMO
               (timeline RQ3->nhãn trễ->RQ4->RQ2(HB2); P4a-c; prereg DRAFT; KHÔNG chạy)
```

Không có bước nào chạy code, GPU/CPU thực nghiệm, hay sửa v14.3.
