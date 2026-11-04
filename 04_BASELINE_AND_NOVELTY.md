STATUS: G3 DRAFT — chờ NCS duyệt. Chỉ đặc tả và kiểm toán nguồn; không chạy code, không chạy baseline.
SOURCE ANCHORS: bài gốc được mở trong phiên này (§1); `RQ4_PRIOR_ART`, `RQ4_GAP`, `RQ4_BASELINE_SPECS`, `RQ4_REFS` (đã mở rộng, không làm lại); v14.3 Nội dung 1, 3, 4; D1–D17 trong `01_REQUIREMENT_TRACE.md`.
ASSUMPTIONS: (a) "VERIFIED" ở đây = tôi đã mở bài gốc/PDF/mã và đọc đúng đoạn nêu; không có nghĩa đã đọc toàn bài; (b) tìm kiếm web chuẩn (3 truy vấn mới + các lượt trước của agent trước) — vắng mặt không phải bằng chứng không tồn tại; (c) chi phí chỉ là ước tính của tôi, không đo.
OPEN QUESTIONS: Q-G3-1…Q-G3-4 (§6).
DECISION: G3 = **GO** sang G4. Kết luận novelty: `GAP_NARROW` **được giữ**; lý thuyết (2′) novelty thấp; novelty khả dĩ nằm ở tổ hợp + đánh giá, chưa phải đóng góp đã xác lập. B1–B3: **không có mã/commit nào được xác minh trong task cloud** (`MISSING`).

Nhãn: `VERIFIED` (đã mở bài gốc, đoạn nêu) · `SECONDARY` (từ tệp agent trước, chưa mở lại) · `MENTIONED_ONLY` · `MISSING` · `SL` suy luận · `ĐX` đề xuất (chưa duyệt).

---

## 1. Nhật ký xác minh nguồn (phiên này, 2026-11-04)

PDF/mã tải về scratchpad; hash 16 ký tự đầu của sha256 để truy vết.

| Công trình | Hiện vật | Điều đã xác minh | Sửa/ghi nhận so với tệp cũ |
|---|---|---|---|
| Jia, Liu, Cao, Gong [49] (arXiv 2012.03765; AAAI 2022) | PDF `2be33460…` | **Định lý 1**: dự đoán `a` không đổi với mọi `D*` có `S(D,D*) ≤ ⌈(s_a−s_b+I(a>b))/2⌉ − 1`. Mô hình tấn công = **sửa/thêm/xóa** mẫu; `S` = số mẫu bị sửa/thêm/xóa tối thiểu. Hòa phiếu: lấy **nhãn có thứ hạng lớn nhất**; hòa láng giềng: thứ hạng tất định (ví dụ hash của đặc trưng và nhãn). Worst case: mỗi mẫu độc đổi một phiếu `a→b`. | Đóng điểm treo ở G2 §8: công thức khớp Bổ đề 3 (G2) với quy ước hòa ngược chiều (G2 dùng chỉ số nhỏ thắng, Jia dùng chỉ số lớn thắng ⇒ `𝟙[a<b]` ↔ `I(a>b)`). Xác nhận [49] trong tham khảo v14.3 chính là bài này. |
| Koh, Steinhardt, Liang (arXiv 1811.00741) | PDF `89cf047a…` | Đầu bài: "Published in *Machine Learning*, 2021"; arXiv v1 2/11/2018, v2 3/12/2021; journal-ref "Machine Learning, 2021". "k-NN defense" = loại điểm **xa** k láng giềng (khoảng cách, *không dùng nhãn*); điểm độc đặt sát nhau né được. | **Sửa**: `RQ4_REFS` ghi `year = {2022}` ⇒ đúng là **2021**. Quan trọng: kết quả né của Koh áp dụng cho kNN *khoảng cách*, không phải kNN *bỏ phiếu nhãn* của Paudice — `RQ4_PRIOR_ART` G3-4 viết chung "kNN". |
| Vejling et al. (arXiv 2507.13835) | PDF `fbebd793…` | v1 18/7/2025, v2 8/10/2026; không có venue. Định lý 4.1–4.5 = hiệu lực, FDR, PRDS. **Công suất chỉ bằng mô phỏng** (Hình A1–A4), không có định lý công suất. Cần calibration và inlier hoán đổi được; tự nêu hạn chế "settings like time series". Thừa nhận nhóm "honest agents with unintentionally biased data" (bị cờ theo thiết kế). Tấn công đối kháng = hướng tương lai. | Xác nhận `RQ4_GAP`: không có kết quả công suất để mượn cho `f*`, `β`. |
| Nelson et al. 2008 (RONI) | trang USENIX | §5.1: `T`=20, `V`=50, 5 lần, loại nếu tác động "significantly negative"; không có ngưỡng số; dictionary attack: 100% phát hiện, non-attack tối đa −4,4 ham-as-ham so với dictionary tối thiểu −6,8; "RONI fails to differentiate focused attack emails from non-attack emails" (lý do: ảnh hưởng của focused không hiện trên tập huấn luyện). | Xác nhận `RQ4_BASELINE_SPECS` §1. |
| ART `RONIDefense` | `roni.py` từ nhánh `main` tại 2026-11-04, sha256 `ab852439…`; pypi: 1.20.1, MIT, 7/7/2025 | Xem §3 B1: **không phải RONI nguyên bản**. | Hash commit của `roni.py` **không lấy được** (API GitHub bị chặn) ⇒ `commit = MISSING`; chỉ có hash tệp. |
| Northcutt et al. (arXiv 1911.00068) | PDF `052ad95f…` | "k = 4 is fixed in the experiments using cross-validation"; `P̂` là xác suất ngoài mẫu. cleanlab 2.9.0 (13/1/2026, Apache-2.0) xác nhận từ pypi. | Xác nhận `RQ4_BASELINE_SPECS` §2. |
| Paudice et al. (arXiv 1803.00992) | PDF `3c841e31…` | Algorithm 2: nếu `conf(S_ki) ≥ η` thì `y_i' = mod(S_ki)`; `η = 0,5`; `k` chọn bằng tập tin cậy. | Xác nhận §3. |
| Biloš et al. 2026 (arXiv 2605.21288) | PDF `d3a24bec…` | Bảng 3: hub poison = lật nhãn **top 15% hub trong ngữ cảnh**; TabPFNv2 −3,7 [−5,5; −2,1] pp, TabICLv2 −3,3 [−4,8; −1,9], Mitra −2,8 [−4,0; −1,6]; chỉ số = **độ chính xác**, 24 bộ phân loại × 5 seed, độ chính xác sạch ≈0,88. | **Giới hạn đọc**: không phải NIDS, không phải recall họ mục tiêu, lật ngữ cảnh 15%. Không dùng làm bằng chứng cho mức tổn hại ở H4a. |
| Xu et al. 2016 (arXiv 1605.05860) | PDF tải về, chỉ xác nhận tiêu đề/tác giả | — | Chi tiết mô hình giữ `SECONDARY`. |
| Mahloujifar, Mahmoody, Mohammed — "Universal Multi-Party Poisoning Attacks" (arXiv 1809.03474) **(mới)** | trang abs | Mô hình **(k,p)-poisoning**: đối thủ điều khiển `k` trong `m` bên; mỗi bên độc giữ dữ liệu "(1−p)-gần" đúng. Chỉ tấn công. Chưa đọc thân bài. | Mô hình ngân sách theo nguồn đã có tiền lệ ⇒ cấu trúc ĐN5 + Mệnh đề 2 không mới về mô hình đe dọa. |
| Patent US12405930B2 (Dell; ưu tiên 29/6/2023) **(mới)** | trang Google Patents | Chọn phần dữ liệu bị đầu độc bằng tối ưu hóa dựa trên mức bất thường và **đặc tính nguồn** (tư thế bảo mật lịch sử/hiện tại, số nguồn). Không thấy hai tầng nguồn→dòng; không ICL; không đối thủ thích nghi. Mới đọc 100.000/175.509 ký tự. | Tiền lệ phát minh dùng "đặc tính nguồn" trong lọc đầu độc; không phải bài khoa học. Ghi `MENTIONED_ONLY` cho phần chưa đọc. |
| Djilani 2025, Hu–Ghelichi 2026 | kết quả tìm kiếm | Không có lọc nhãn độc cho ngữ cảnh TabPFN. | Xác nhận khoảng trống đánh giá. |

**Các nguồn khác trong `RQ4_PRIOR_ART` (32+ công trình) vẫn `SECONDARY`**, bao gồm WeiDetect, Korycki, Ma–Olshevsky, Fang, FLTrust, PriorPoison, DPA, Rosenfeld, Steinhardt. Cần mở bài gốc nếu luận án trích; tôi không mở lại ở G3 vì phạm vi chỉ là các điểm quyết định.

## 2. Bốn nhánh hệ thống (S0–S3) — đặc tả

Cùng một **trace RQ3 cố định**, ngân sách `b`, `τ`, nguồn mô phỏng, mô hình. Các nhánh chỉ khác bộ lọc. Trạng thái hiện vật: **cả bốn = MISSING** (không có mã trong repo này).

| ID | Thành phần | Đầu vào | Đầu ra | Dữ liệu được phép | Ghi chú (lấy từ D8–D17) |
|---|---|---|---|---|---|
| S0 | Không phòng thủ | dòng đến `(x, ỹ, j, arr)` | ACCEPT tất cả (nạp cho HB2) | — | `V_t`, `D_cal`: bao gồm mọi nguồn (không cờ) |
| S1 | Chỉ tầng nguồn (DoP) | như S0 | trạng thái nguồn ∈ {`FLAGGED`,`UNCERTAIN`,`CLEAR`} | nhãn đến cùng ngày của K nguồn | cờ bền + giữ hồi tố (PROPOSAL, D16); `UNCERTAIN`: U1 chính, U2 nhạy cảm (D8). **Đặc tả DoP MISSING** |
| S2 | Chỉ tầng dòng (`I_i`, (2-7)) | dòng đến, `C_t`, `V_t` | `PASS`/`HOLD` mỗi dòng | `V_t` | **`V_t` không lọc** (không có tầng nguồn) ⇒ bao gồm nguồn bị chiếm. Gọi là "RONI-like" |
| S3 | Hai tầng | như S1 rồi S2 | ACCEPT/HOLD | S1 + `V_t` lọc | `V_t` chỉ từ nguồn không cờ (D10) |

**Quy tắc công bằng thông tin (SL, ĐX):** S3 hưởng lợi vì `V_t` sạch hơn nhờ tầng nguồn, trong khi S2 và B1–B3 không. Đó là hiệu ứng hợp lệ của phương pháp nhưng phải **tách** trong báo cáo bằng (a) so S2 vs S3, (b) biến thể chẩn đoán *chỉ đánh giá* "`V_t` sạch oracle" cho mọi nhánh — để đo mức mất do `V_t` bẩn, tách khỏi mức mất do bộ lọc. Không đưa biến thể oracle vào quyết định H4a/H4b.

## 3. Baseline B1–B4 — đặc tả (đã mở rộng từ `RQ4_BASELINE_SPECS`)

### B1 — RONI (Nelson protocol), đối chiếu ART
- **Nguồn:** Nelson et al. 2008 §5.1 (VERIFIED).
- **ART `RONIDefense` ≠ RONI nguyên bản** (đọc `roni.py`, sha `ab852439…`):
  1. Đánh giá bằng **nhóm huấn luyện = `x_val` tin cậy + `x_i`**, không phải mẫu con `T` rút ngẫu nhiên từ pool.
  2. **Tích lũy tuần tự**: duyệt theo hoán vị ngẫu nhiên; điểm được chấp nhận được **thêm vào tập tin cậy** ⇒ kết quả phụ thuộc thứ tự.
  3. Tập "quiz" `x_quiz` và tập hiệu chỉnh `x_cal` được lấy từ **`x_train` (dữ liệu nghi ngờ)**, không phải `x_val`; nếu `x_train` bị đầu độc thì ngưỡng và quiz cũng bị đầu độc.
  4. Ngưỡng mặc định hiệu chỉnh: `perf_shift < median − 3·std` của các dịch chuyển do điểm hiệu chỉnh gây ra; nếu không hiệu chỉnh: cố định `eps`. Nguyên bản: không có ngưỡng số.
  5. Mặc định `perf_func='accuracy'`, `pp_cal=0.2`, `pp_quiz=0.2`, `eps=0.1`.
- **Hệ quả ĐX:** B1 trong RQ4 = **"RONI (Nelson-protocol adaptation)"** theo các lựa chọn C-R1…C-R5 của `RQ4_BASELINE_SPECS` §1 (V_t cho trusted, impact = Δlog-loss hoặc Δrecall họ mục tiêu, hiệu chỉnh trên dòng đã biết sạch, giữ dòng nhãn chưa đến). ART chỉ làm **tham chiếu tham số/hành vi** (không phải B1). Nếu NCS dùng ART nguyên trạng thì phải gọi là "ART-RONI" và nêu 5 khác biệt trên.
- **Tri thức yêu cầu:** nhãn tin cậy cho `V_t` (C-R1) — trong RQ4, `V_t` chứa nhãn của chính K nguồn (D10, D17) ⇒ **B1 không có "tập tin cậy" đúng nghĩa**; cùng giới hạn cho B2, B3.
- **Điểm yếu đã biết:** focused attack (Nelson, VERIFIED) — đúng loại (ii)/(iv). Dự báo B1 yếu với (ii): `SL`, cần P4a.
- **Chi phí (ước tính):** 2 lượt suy luận (có/không dòng) × 5 lần lặp ⇒ ~10 lượt TabFM mỗi dòng ứng viên; ~(1−η)b dòng/ngày ⇒ khoảng 8.000 lượt/ngày ở `b=1000` (SL, chưa đo).
- **Tuning:** `T`, `V`, số lần, ngưỡng — theo giao thức DEV tối đa 12 cấu hình (xem Q-G3-3).
- **Trạng thái hiện vật:** ART = `MENTIONED_ONLY` (URL/phiên bản/hash tệp xác nhận; **commit MISSING**); B1 thật = `MISSING`.

### B2 — Confident learning
- **Nguồn:** Northcutt et al. (VERIFIED: k=4 folds trong thí nghiệm của bài); cleanlab 2.9.0, Apache-2.0 (VERIFIED từ pypi).
- **Giả định:** nhiễu phụ thuộc lớp (class-conditional); đối thủ (ii)/(iii)/(v) chọn dòng theo đặc trưng ⇒ vi phạm (`SECONDARY`+`SL`).
- **Chống rò rỉ:** `pred_probs` ngoài mẫu: chia fold **theo nguồn** (leave-one-source-out) hoặc theo ngày; ngưỡng `t_j` ước lượng trên ngữ cảnh đã giữ, không trên lô mới (C-C2).
- **Chi phí:** ~một lượt TabFM mỗi fold (ước tính). **Hiện vật:** `MISSING` (chưa có mã); thư viện = `MENTIONED_ONLY`.

### B3 — kNN label cleaning (Paudice Algorithm 2)
- **Nguồn:** Paudice et al. (VERIFIED). `η=0,5`; `k` chọn trên tập tin cậy; chỉ nhị phân trong bài.
- **Không nhầm với kNN khoảng cách của Koh** (VERIFIED): Paudice dùng **bỏ phiếu nhãn**; Koh đánh bại bộ phát hiện bất thường theo khoảng cách. Khả năng né của điểm độc xếp sát nhau áp dụng cho Paudice theo suy luận (C-K3), **không** do Koh trực tiếp chứng minh ⇒ `SL`.
- **Chi phí:** `O(κ·|C|·d)` mỗi lô (ước tính). **Hiện vật:** `MISSING`; Paudice không có mã công bố tìm thấy (`SECONDARY`).

### B4 — Định giá dữ liệu xấp xỉ
- **Tùy chọn (ĐC)**; không đặc tả thêm; không trở thành baseline bắt buộc. Lưu ý: `I_i` (2-7) bản thân là *leave-one-out valuation* trên `V_t` ⇒ B4 (Data Shapley/KNN-Shapley/TracIn) là biến thể mở rộng, không phải đối chứng độc lập về ý tưởng (SL).

### Quan hệ `I_i` (2-7) với RONI — để tránh gọi sai tên
`I_i = L_V(C) − L_V(C∖{i})` khác Nelson ở: (a) loại một dòng khỏi *ngữ cảnh hiện tại* thay vì so hai mô hình huấn luyện trên mẫu con ngẫu nhiên; (b) mất mát (loss) trên `V_t`, không đếm lỗi; (c) không lặp mẫu con; (d) tập tham chiếu là `V_t` có thể bị nhiễm (D17). ⇒ S2 = "row-influence (RONI-like)"; không gọi là RONI.
**Khả thi chi phí (SL, CXM):** TabFM: mỗi `I_i` cần một lượt suy luận trên toàn `V_t` với ngữ cảnh thiếu dòng `i` ⇒ ~(1−η)b lượt/ngày (≈800 ở `b=1000`). LightGBM: loại một dòng ≈ huấn luyện lại ⇒ rất tốn; v14.3 chỉ nêu xấp xỉ refit cho `g(x)` của RQ3 (Nội dung 3), **không** cho `I_i` ⇒ khoảng trống đặc tả (Q-G3-4).

## 4. Fair comparison — các quy tắc đề xuất (ĐX, chưa duyệt)

1. Cùng trace, cùng seed, cùng `V_t`-cấu-trúc (khác nhau chỉ do cờ nguồn), cùng mô hình khóa.
2. Cùng ngân sách chọn tham số: v14.3 cấp "tối đa 12 cấu hình" cho chính sách quản lý dữ liệu (Nội dung 1); áp dụng tương tự cho từng baseline RQ4 trên DEV (mở rộng cách đọc — hỏi NCS).
3. Mọi baseline xuất `PASS/HOLD`; "tốt hơn RONI" của H4a phải định nghĩa trước: ví dụ chênh `ASR_excess` ≥ một biên khóa + khoảng tin cậy (ĐX; ngưỡng số chưa có ⇒ **không bịa**).
4. Ghi riêng: tỷ lệ dòng sạch bị chặn (mọi baseline), kích thước ngữ cảnh thực tế mỗi ngày (bài học RQ2: chính sách để trống ô không được so với k đầy).
5. Hiệu ứng "filtering-induced drift" (AndroVenom, `SECONDARY`): báo cáo LightGBM và TabFM riêng.

## 5. Kiểm toán novelty

Bốn khía cạnh tách biệt, theo hồ sơ §7.6.

| Yếu tố đóng góp (ứng viên) | Đúng toán học | Có tác dụng (DEV) | Mới so với tài liệu | Đủ chuẩn đóng góp luận án |
|---|---|---|---|---|
| N1 Tầng nguồn ngang hàng (DoP) | n/a (DoP MISSING) | **chưa biết** (DoP-N1 là bằng chứng cũ có điều kiện, chưa trích) | **NO-GO về novelty** theo NCS (24–25/10/2026): có tiền lệ kiểm định nguồn có kiểm soát FDR — Vejling (VERIFIED), Xu (`SECONDARY`) | không dựa vào N1 |
| N2 Tầng dòng `I_i` | định nghĩa rõ | chưa biết | **thấp**: RONI-like (Nelson), leave-one-out valuation | không đứng riêng |
| N3 Tổ hợp nguồn→dòng cho ngữ cảnh mô hình bảng | — | chưa biết | **chưa thấy cùng tổ hợp**; gần nhất: Vejling (kiểm định agent rồi chọn dòng bằng BH, để *mua dữ liệu trước khi huấn luyện*, cần dữ liệu cục bộ sạch, hoán đổi được); patent Dell (nguồn+bất thường, không ICL, không 2 tầng, không thích nghi) | **ứng viên chính** nhưng `GAP_NARROW`: khoảng cách hẹp, dựa vào tìm kiếm web có hạn |
| N4 Cap theo nguồn + ngưỡng audit ⇒ chặn tỷ lệ độc (2′) | CANDIDATE (đếm + union bound) | n/a | **thấp**: mô hình ngân sách theo bên có tiền lệ — (k,p)-poisoning (Mahloujifar et al., chỉ tấn công); chứng nhận kNN [49]; DPA | không đủ như định lý chính |
| N5 Đánh giá đối thủ biết phòng thủ cho cặp nguồn+dòng | n/a | chưa biết | Koh et al. (đối thủ biết bộ lọc dòng); Miao/Fang (đối thủ biết kiểm toán nguồn); **không thấy cặp nguồn+dòng** (`RQ4_GAP`) | đóng góp *đánh giá*, giá trị phụ thuộc kết quả |
| N6 Đo bảo vệ ngữ cảnh TabFM cho NIDS với nhãn trễ | n/a | chưa biết | khoảng trống đánh giá: Biloš (tấn công, không phải NIDS, không phòng thủ), Djilani (né tránh lúc kiểm thử), Hu–Ghelichi (nhiễu ngẫu nhiên) | đóng góp *thực nghiệm* nếu P4a xác nhận tổn hại |
| N7 `UNCERTAIN` (abstain) cho nguồn khác phân phối | bổ đề không nhận dạng (G1) | n/a | **không mới**: kiểm định có abstain là chuẩn; Vejling gắn cờ nguồn lệch theo thiết kế (nhóm "honest-biased") | không |

**Kết luận novelty (SL, không phải chứng minh):**
1. Lý thuyết (N4) **không** nên là điểm tựa của đóng góp RQ4: sơ cấp, và Mệnh đề 2 như viết đã bị phản ví dụ.
2. Điểm tựa khả dĩ: N3 + N5 + N6, thành *một phát hiện thực nghiệm có kiểm soát*, nếu P4a cho thấy tổn hại đủ lớn để "giảm 50%" có nghĩa và S3 vượt S2/B1 với khoảng tin cậy.
3. Rủi ro: nếu `ASR_excess` không phòng thủ nhỏ (Biloš: −3 đến −4 pp trong tình huống khác) thì H4a không kiểm định được ⇒ `HOLD_ASSUMPTION` (đã nêu ở G1).
4. Rủi ro HB2 (DC-2): tương tác chưa được tài liệu nào xét, vừa là cơ hội vừa là nguy cơ.

**Nhận dạng (identifiability) — đối chiếu tài liệu:** Vejling cần chồng lấn/hoán đổi được và thừa nhận hạn chế chuỗi thời gian (VERIFIED). Bổ đề không nhận dạng của G1 (SL, chứng minh sơ cấp) nói điều tương đương cho chế độ B; không tìm thấy kết quả ngang hàng không-ground-truth có bảo đảm khi không có chồng lấn ngoài Ma–Olshevsky (cần overlap, `SECONDARY`).

**Giới hạn tìm kiếm:** thêm 3 truy vấn chuẩn (ICL poisoning defence; hai tầng nguồn→dòng trong IDS; chặn tỷ lệ độc theo nguồn). Không quét Google Scholar/DBLP. Một lượt gần đây mới (2026) có thể chưa lập chỉ mục.

## 6. Việc cần NCS quyết trước G4

- **Q-G3-1** B1 = "RONI (Nelson-protocol adaptation)" và ART chỉ tham chiếu — đồng ý?
- **Q-G3-2** Biến thể chẩn đoán `V_t` sạch oracle (chỉ đánh giá) — cho phép đưa vào báo cáo phụ?
- **Q-G3-3** Ngân sách tuning cho baseline RQ4 = tối đa 12 cấu hình trên DEV (tương tự Nội dung 1)?
- **Q-G3-4** `I_i` với LightGBM: chấp nhận xấp xỉ refit (cấu trúc cây giữ nguyên) như Nội dung 3 hay giới hạn S2/S3 ở TabFM?

## 7. Sửa lỗi cho `RQ4_REFS (2).bib` (chưa sửa tệp gốc; để NCS quyết định)

| Khóa | Sửa |
|---|---|
| `koh2022` | `year = {2021}`; nên đổi khóa thành `koh2021`; journal: *Machine Learning* 2021 (VERIFIED). |
| `priorpoison2026`, `lan2025`, … | không đổi trong G3 |
| thêm | khối BibTeX bên dưới |

```bibtex
@misc{mahloujifar2018multiparty,
  author = {Mahloujifar, Saeed and Mahmoody, Mohammad and Mohammed, Ameer},
  title = {Universal Multi-Party Poisoning Attacks},
  howpublished = {arXiv:1809.03474},
  year = {2018},
  url = {https://arxiv.org/abs/1809.03474},
  note = {abstract page opened 2026-11-04; venue not on page [VERIFY]}
}
@misc{ezrielev2025patent,
  author = {Ezrielev, Ofir and Yehuda, Hanna and Walsh, Kristen Jeanne},
  title = {System and method for identifying poisoned data during data curation using data source characteristics},
  howpublished = {US Patent 12,405,930 B2 (Dell Products LP)},
  year = {2025},
  note = {patent, not peer reviewed; first 100k of 175k characters of the page read}
}
```

## 8. Trạng thái

| Mục | Trạng thái |
|---|---|
| S0–S3 đặc tả | xong (DRAFT); hiện vật `MISSING` |
| B1–B3 | đặc tả + nguồn VERIFIED; hiện vật `MISSING`/`MENTIONED_ONLY` |
| B4 | tùy chọn, không bắt buộc |
| Novelty | `GAP_NARROW` giữ; lý thuyết thấp; đóng góp khả dĩ = tổ hợp + đánh giá, chưa xác lập |
| H4a, H4b | NO CONCLUSION |
| Mệnh đề 2 | `ASSUMPTION GAP / COUNTEREXAMPLE`; 2′ `CANDIDATE` |
| Đề cương | NO CHANGE NOW |
