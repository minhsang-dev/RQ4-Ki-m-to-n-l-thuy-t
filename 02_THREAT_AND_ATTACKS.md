STATUS: G1 DRAFT — chờ NCS duyệt. Chỉ đặc tả trên giấy; không code, không chạy.
SOURCE ANCHORS: v14.3 Định nghĩa 4–6, Nội dung 3–4, Mệnh đề 2 (qua `00_SOURCE_MANIFEST.md` S2/S3); quyết định D1–D7 trong `01_REQUIREMENT_TRACE.md` §E; RQ2_DECISION_LOG §8 (HB2); RQ4_GAP (các mối đe dọa 1–12, `SECONDARY`).
ASSUMPTIONS: A1 K chưa được v14.3 ấn định (CXM) — dùng ký hiệu K. A2 mỗi nguồn = một chuyên viên, b chia cho K nguồn (D4). A3 đặc tả DoP MISSING nên chỉ nêu *thông tin DoP được phép quan sát*, không nêu thống kê của DoP. A4 mọi "ĐX" là đề xuất chưa duyệt.
OPEN QUESTIONS: xem §9.
DECISION: G1 = **GO có điều kiện** sang G2 (các điểm ở §9 không chặn G2, nhưng Q-G1-3 và Q-G1-5 ảnh hưởng đến cách đọc kết quả G2).

Nhãn: `ĐC` đề cương quy định · `SL` suy luận · `GT` giả thuyết · `CXM` chưa xác minh · `ĐX` đề xuất sửa (chưa duyệt).

---

## 1. Mô hình hình thức

**Thực thể.** `K` nguồn nhãn, tập bị chiếm `A ⊂ [K]`, `m = |A| ≤ m_max < K/2` (ĐC). Ngày `t`. Mỗi dòng `i` có đặc trưng `x_i`, nhãn thật `y_i`, họ `fam_i` (chỉ dùng đánh giá), nguồn `j(i)`, thời điểm `sel_i` (RQ3 chọn), `arr_i = sel_i + τ` (nhãn đến), nhãn trả về `ỹ_i`.

**Số nhãn mỗi nguồn mỗi ngày** (D4, ĐC từ ĐN4 + η = 0,2): `n_{j,t} ≈ (1−η)·b / K`. Ví dụ minh họa (K chưa chốt, SL): `b = 100`, `K = 5` ⇒ ≈16 nhãn/nguồn/ngày; `b = 1000`, `K = 5` ⇒ ≈160. Hệ quả: ở `b = 100` mẫu mỗi nguồn mỗi ngày rất nhỏ nên **công suất của kiểm toán ngang hàng theo ngày là vấn đề thật** (chuyển G2 để liên hệ f*, β; ở đây chỉ ghi nhận).

**Hành động của đối thủ (ĐC, ĐN5).** Chỉ thay `ỹ_i` bằng giá trị khác trên các dòng được giao cho nguồn trong `A`. Không đổi `x_i`, không truy cập trọng số. Ghi rõ **không** được phép (vì ĐN5 không nêu): từ chối trả nhãn (withhold), trì hoãn nhãn (đổi τ), thêm dòng mới, đổi `source_id`, đổi `sel_i`. Nếu NCS muốn mở rộng, đó là ĐX, không phải cấu hình mặc định.

**Phạm vi tiếp cận (SL, hệ quả trực tiếp của D4).** Đối thủ chỉ nhìn thấy dòng được **giao cho nguồn của nó**; nó không chọn dòng nào được giao.
- Chế độ A (hoán đổi): giao ngẫu nhiên ⇒ mỗi dòng của họ mục tiêu rơi vào `A` với xác suất `m/K`. Do đó **tỷ lệ nhãn họ mục tiêu có thể bị lật tối đa là ≈ m/K < ½** (trước khi tính chọn lọc của RQ3). Đây là trần của "độc lực" thực tế, và là cơ sở của rủi ro `P4a: tổn hại baseline nhỏ` (hồ sơ §7.4).
- Chế độ B (mạng con): nếu họ mục tiêu tập trung ở subnet không thuộc `A`, tấn công (ii) **không có dòng để lật**; nếu tập trung ở subnet của `A`, tấn công (ii) lật 100% nhãn họ đó mà **không có đồng nghiệp trung thực nào quan sát cùng vùng đặc trưng** (xem §4 về nhận dạng).

## 2. Phân tách: thông tin triển khai được vs chỉ dùng đánh giá

| Đại lượng | Phòng thủ (deployment) | Đối thủ | Đánh giá (oracle) |
|---|---|---|---|
| `x_i`, `ỹ_i`, `source_id`, `sel_i`, `arr_i` của dòng **đã đến** | có | chỉ dòng của nguồn mình | có |
| Nhãn thật `y_i` | **không** (trừ nhãn chuyên viên trong V_t / D_cal — xem §8 Q-G1-3) | biết nhãn thật của dòng được giao (một nguồn bị chiếm biết điều nó "nên" trả) — **GT** của mô hình, không phải dữ liệu mặc định ngoài | có |
| Tập `A`, kiểu tấn công, cờ "dòng độc" | **không** | biết `A` của mình, đồng phạm (kiểu vi) | có |
| `fam_i` | **không** (ĐN5 mục tiêu (ii) là khái niệm của đối thủ; phòng thủ không dùng họ làm tín hiệu) | biết họ mục tiêu mình chọn; không biết họ của dòng ngoài tập mình | có |
| Trọng số mô hình, ngữ cảnh `C_t`, `V_t` | không phải trọng số; `C_t`, `V_t` của chính mình | **không** | có |
| Nhãn Test, ngày tương lai | không | **không** (cấm, hồ sơ §2.2) | chỉ để tính chỉ số |
| `m_max`, `K`, `s`, `f` danh nghĩa | có (tham số thiết kế công khai) | kiểu (iv)–(vi) được biết cơ chế ⇒ Kerckhoffs: biết tham số danh nghĩa, **không** biết giá trị thống kê thực hiện hằng ngày | có |

Quy tắc kiểm: bất kỳ cột "phòng thủ" nào dùng `y_i`, `fam_i`, `A` hay cờ độc ⇒ `ORACLE_LEAKAGE`, kết quả đó không được dùng cho H4.

## 3. Sáu kiểu tấn công — bảng đặc tả

Quy ước cột: **Obs** = thông tin quan sát được; **Budget**; **Action**; **Timing**; **Knowledge tier**; **GT-eval** = chỉ dùng đánh giá. Tầng tri thức đối thủ (đề xuất đặt tên, SL):
- **K0**: chỉ dòng của nguồn mình (đặc trưng, nhãn thật của dòng đó).
- **K1**: K0 + dữ liệu công khai/lịch sử trước triển khai để dựng mô hình thay thế (surrogate). Không có trọng số, không truy vấn mô hình triển khai.
- **K2**: K1 + truy vấn hộp đen dự đoán của mô hình triển khai. **Không có trong v14.3** ⇒ ĐX, không dùng mặc định.

Loại lưới (ĐC, Nội dung 4): chính = (ii)–(v); điểm gãy = (i), (vi).

### (i) Lật ngẫu nhiên — đường cong điểm gãy
- **Obs** K0. **Budget** tỷ lệ lật `φ` trên số dòng của nguồn bị chiếm; mẫu số = số nhãn đã đến của nguồn đó trong ngày (SL, chốt trong prereg). **Action** `ỹ ← 1−ỹ` (nhị phân) hoặc nhãn khác đều (đa lớp) trên dòng chọn đều ngẫu nhiên; seed ghi lại. **Timing** mỗi ngày độc lập. **GT-eval** cờ lật. Không cần tri thức về phòng thủ.

### (ii) Lật họ mục tiêu — H4a, lưới chính
- **Obs** K0: đặc trưng và nhãn thật của dòng được giao. Đối thủ **không** cần `fam_i` toàn cục — chỉ cần nhận ra dòng thuộc họ mình định lật; chọn bằng quy tắc thực thi được trên K0 (ví dụ nhãn thật = attack và cờ họ nằm trong dữ liệu của nó). Trong mô phỏng, họ được cấp qua `family_id` ⇒ **`family_id` dùng như quyền biết của đối thủ trên dòng của chính nó**, và là oracle đối với phòng thủ (xem §2).
- **Budget** lật `ψ` phần các dòng họ mục tiêu mà nó được giao (tối đa 100% các dòng đó). **Action** attack→benign. **Timing** mọi ngày nhận được. **Knowledge** không biết phòng thủ. **Điểm cần kiểm toán**: chọn *họ nào* (họ lớn/hiếm/tái xuất — ảnh hưởng ASR); quy tắc chọn họ phải khóa trước DEV.
- **Trần** (SL, từ §1): chế độ A ⇒ tỷ lệ nhãn họ mục tiêu bị lật ≤ ≈ `m/K`.

### (iii) Lật dòng có ảnh hưởng lớn nhất — H4a, lưới chính
- **Vấn đề cốt lõi (hồ sơ §7.3).** "Ảnh hưởng" của dòng trên mô hình triển khai đòi hỏi mô hình (hoặc `V_t`) — mà ĐN5 cấm trọng số. **Dạng không operational: dùng ảnh hưởng chính xác của mô hình triển khai (oracle influence) — KHÔNG được dùng cho H4.**
- **Dạng operational đề xuất (ĐX):** ảnh hưởng ước lượng bằng mô hình thay thế ở tầng **K1**: đối thủ huấn luyện surrogate (LightGBM hoặc kNN) trên dữ liệu K1, tính `Î_i` loại-một-dòng trên một tập kiểm riêng của nó, lật các dòng có `Î_i` cao nhất trong số dòng nó được giao, tới hết budget. Hai dạng thay thế rẻ hơn, cũng ở K0/K1: (a) khoảng cách tới biên của surrogate; (b) hub-poison (dòng gần mọi dòng khác nhất; có bằng chứng `SECONDARY` rằng hub-poison gây giảm nhỏ 3,3–3,7 điểm % trên TabFM, Bilos et al. 2026 — mở bài gốc ở G3).
- **Budget** như (ii). **Timing** theo ngày; surrogate được cập nhật chỉ bằng dữ liệu đã đến. **GT-eval** *độ tương quan hạng* giữa `Î_i` (surrogate) và `I_i` thật (2-7) — chỉ báo cáo, không đưa vào quyết định của đối thủ.
- **Hệ quả cho diễn giải (SL):** kết quả H4a cho (iii) là điều kiện theo độ trung thực của surrogate; phải báo cáo kèm tương quan hạng. Nếu NCS không chấp nhận surrogate ⇒ (iii) **chưa operational** (`ATTACK_NOT_OPERATIONAL`).

### (iv) Đối thủ thích nghi với tầng nguồn — H4b, lưới chính
- **Knowledge** (ĐC): biết cơ chế lọc và ngưỡng phát hiện danh nghĩa `f` của tầng nguồn (Kerckhoffs). Không biết giá trị thống kê thực hiện hằng ngày của kiểm định ngang hàng, không biết thống kê của đồng nghiệp trung thực.
- **Action** mỗi ngày mỗi nguồn bị chiếm lật `⌊f·n_{j,t}⌋ − δ` dòng (δ ≥ 0 là biên an toàn; **giá trị δ không có trong v14.3** ⇒ ĐX, khóa trước DEV), rải đều qua các ngày. **Chọn dòng nào** không được v14.3 nêu — hai cách đọc: (a) kết hợp (ii), lật họ mục tiêu; (b) lật ngẫu nhiên. Cần NCS chốt (Q-G1-4). **Timing** nhân quả; không nhìn trước.
- **Phản hồi:** F0 = vòng hở (không biết dòng/nguồn mình bị cờ). F1 = biết cờ của chính nguồn mình. Mặc định F0; F1 là nhạy cảm (ĐX).
- **Điểm kiểm toán:** ngưỡng "vừa dưới" không phải ngưỡng cứng đã biết khi mẫu nhỏ (n≈16) — đối thủ có thể bị bắt ngẫu nhiên dù dưới f. Do đó cần định nghĩa chiến lược "dưới ngưỡng" theo xác suất bị phát hiện (ví dụ phát hiện ≤ ω), không theo số cứng (OPEN, G2/DoP).

### (v) Né tầng dòng — H4b, lưới chính
- **Knowledge**: biết tầng dòng dùng `I_i` (2-7) trên `V_t`; **không** biết nội dung `V_t` (V_t là nhãn chuyên viên, không thuộc nguồn bị chiếm — nếu N-5 đúng) và không có trọng số.
- **Action**: lật các dòng có `Î_i` **thấp** (ước lượng như (iii) bằng surrogate K1). **Budget** như (ii). Kết hợp được với (iv) (vừa dưới ngưỡng nguồn, vừa dưới ngưỡng dòng) — v14.3 không nói; đề xuất báo cáo (iv), (v), (iv+v) riêng (ĐX).
- **Nhận xét thiết kế (SL, cần G2/G4 kiểm):** dòng có `I_i` thấp theo `V_t` là dòng có ảnh hưởng nhỏ lên mất mát trên `V_t`. Nếu `V_t` đại diện họ mục tiêu thì dòng đó cũng ít gây hại cho recall họ mục tiêu; đối thủ chỉ "né" được bằng cách gây hại ở nơi `V_t` **không phủ** (mù). Kết quả H4b với (v) do đó phụ thuộc **độ phủ họ mục tiêu của V_t**; V_t được lấy phân tầng/lấy dư vùng điểm cao (ĐC, ĐN6) nhưng không bảo đảm phủ họ mới.

### (vi) Phối hợp làm lệch so sánh ngang hàng — điểm gãy
- **Knowledge**: đồng phạm chia sẻ nhãn của mọi nguồn trong `A`; không biết nhãn của nguồn trung thực. **Budget**: `m` chạy 0..K−1 (vượt K/2 là có chủ đích). **Action**: các nguồn trong `A` trả nhãn nhất quán với nhau (ví dụ cùng lật theo cùng quy tắc) để kéo tham chiếu ngang hàng về phía mình, làm nguồn trung thực "trông bất thường".
- **Quan sát (SL):** khi `m < K/2` và tham chiếu là thống kê bền (trung vị), colluder không dịch được tham chiếu ra ngoài khoảng của đa số trung thực; khi tham chiếu là trung bình hoặc khi nguồn trung thực không đồng nhất (chế độ B) thì có thể. Cụ thể thống kê của DoP **MISSING** ⇒ không khẳng định.
- **Không dùng** bảo đảm `f*`, `β` ở kiểu này (ĐC: "có thể xấu đi").

## 4. Hai chế độ nguồn: nhận dạng, UNCERTAIN, cờ oan

### Chế độ A — hoán đổi được
- **Điều kiện** (ĐC + SL): sau khi RQ3 chọn dòng, việc giao dòng cho nguồn độc lập với `(x, y)`. Khi đó, với mọi tập con nguồn trung thực, phân phối `(x, ỹ)` các dòng đến cùng ngày **bằng nhau** (cùng phân phối thực tế bằng khác nhau chỉ do lấy mẫu). Chọn lọc của RQ3 (theo bất định, v.v.) **không phá** điều kiện này vì giao nguồn xảy ra *sau* chọn dòng (D3, D4).
- **Cái phải kiểm trên mô phỏng** (SL): (a) giao ngẫu nhiên thật sự độc lập với `(x,y)`; (b) nhiễu nhãn ngẫu nhiên của nguồn trung thực (ví dụ 10% như RQ2) có cùng phân phối giữa các nguồn trung thực — **mô hình nhiễu trung thực chưa được v14.3 nêu cho RQ4** (Q-G1-2); (c) cỡ mẫu theo ngày đủ cho kiểm định.
- **Trôi dạt chung** (đặc trưng, tỷ lệ lớp, khái niệm, họ mới) tác động cùng lúc lên mọi nguồn ⇒ phép so sánh ngang hàng cùng ngày **trung hòa** (đây là lý do v14.3 chọn so ngang hàng). Ngoại lệ: họ mới mà phân tích viên *chưa* nhận ra có thể bị mọi nguồn gán sai giống nhau — peer audit không thấy, lỗi này là lỗi mô hình/chuyên gia, không phải đầu độc.

### Chế độ B — theo mạng con
- **Cấu trúc** (ĐC): nguồn trung thực có phân phối đặc trưng khác nhau. Mục đích đo: tỷ lệ gắn cờ oan.
- **Bổ đề không nhận dạng (SL, phác thảo chứng minh sơ cấp).** Gọi `P_j(x, ỹ)` là phân phối quan sát của nguồn `j`. Giả sử vùng đặc trưng `S_j` của nguồn `j` không giao với vùng của mọi nguồn khác (hỗ trợ rời nhau). Hai thế giới: `W0`: nguồn `j` trung thực và `P(y|x)` trên `S_j` là `g`. `W1`: nguồn `j` bị chiếm lật nhãn theo một ánh xạ `h`, và `P(y|x)` trên `S_j` là `h^{-1}∘g`. Hai thế giới cho **cùng** `P_j(x,ỹ)` và cùng quan sát của mọi nguồn khác (vì họ không có dòng trên `S_j`). Do đó nếu chỉ dùng so sánh ngang hàng không có giả định cầu nối giữa các vùng (khái niệm chung + mô hình trơn), không có phép kiểm định nào phân biệt được `W0` và `W1`. **Hệ quả:** trạng thái đúng là `UNCERTAIN`, và đặt nó là `FLAGGED` hay `CLEAR` đều là cược. Điều kiện đủ để nhận dạng một phần: **chồng lấn** ($S_j \cap S_{j'}$ có xác suất dương) hoặc giả định khái niệm chung + mô hình tham số/trơn (giả định cầu nối; mạnh, phải khai báo).
- **Hệ quả cho đối thủ (SL):** nếu họ mục tiêu chỉ có ở subnet của nguồn bị chiếm, (ii) *không bị* tầng nguồn phát hiện theo nghĩa nhận dạng; bảo vệ phải đến từ tầng dòng (I_i) hoặc không có.
- **UNCERTAIN ≠ FLAGGED (ĐC).** Ba trạng thái nguồn: `FLAGGED`, `UNCERTAIN`, `CLEAR`. Không gọi `UNCERTAIN` là "FPR = 0".
- **Chỉ số bắt buộc ở chế độ B (SL):** tỷ lệ nguồn trung thực `FLAGGED` (FPR nguồn), tỷ lệ `UNCERTAIN`, độ phủ (phần dòng trung thực vẫn được ACCEPT), và — vì không phân biệt được — tỷ lệ nguồn bị chiếm rơi vào `UNCERTAIN` thay vì `FLAGGED`.
- **Chính sách cho dòng của nguồn `UNCERTAIN`: v14.3 không quy định** (ĐC chỉ nói không gắn cờ). Hai phương án để báo cáo song song (Q-G1-1): U1 = nạp bình thường nhưng vẫn qua tầng dòng và hạn mức s; U2 = giữ chờ xác minh. Không chọn ở đây.

### Trôi dạt thật vs đầu độc — ma trận nguồn gây cờ oan (SL)
| Hiện tượng | Chế độ A | Chế độ B | Tầng dòng (I_i) |
|---|---|---|---|
| Trôi dạt chung (đặc trưng/lớp/khái niệm) | trung hòa bằng so ngang hàng cùng ngày | trung hòa nếu mọi subnet cùng bị | `V_t` cũ ⇒ dòng đúng nhưng "lạ" có `I_i` > 0 (loss trên V_t tăng) ⇒ bị chặn oan |
| Họ mới xuất hiện (chỉ ở vài subnet) | không | **nguồn trung thực bị coi bất thường** | `V_t` chưa có họ ⇒ `I_i` ≈ 0 hoặc dương ⇒ khó phân biệt |
| Họ tái xuất hiện | không | như trên | `V_t` có thể đã quên họ |
| Nhiễu nhãn trung thực khác nhau giữa nguồn | tạo biến thiên, có thể bị cờ | như trên | — |
| Nguồn trung thực có tay nghề kém hơn | như trên | như trên | — |

Đây là các trường hợp bắt buộc có trong P4b/P4c (chỉ thiết kế, không chạy).

## 5. Ràng buộc thiết kế đã chấp nhận (D3) và rủi ro HB2 × (ii)

**DC-1 (D3).** Thứ tự cho mỗi dòng: chọn (RQ3, nhãn chưa có) → nhãn đến ở `arr` → RQ4 (nguồn rồi dòng) → {ACCEPT, HOLD, UNCERTAIN-route} → HB2 chỉ thấy dòng ACCEPT. RQ4 không dùng nhãn chưa đến, không dùng quyết định RQ3 tương lai.

**DC-2 (khuếch đại chọn lọc của HB2, SL).** HB2 giữ bin `H` (≤ 200 dòng) gồm dòng **nhãn benign mà mô hình chấm điểm tấn công cao** (RQ2 DECISION_LOG §8: top-400 theo điểm tấn công, hardness top-k). Tấn công (ii) tạo dòng đúng loại này (tấn công thật gắn nhãn benign). Với dòng đã ACCEPT (không bị tầng nguồn/tầng dòng chặn) nhưng độc, **xác suất được HB2 chọn cao hơn dòng benign thường** vì được chọn *bằng* điểm tấn công. Hệ quả:
1. Tỷ lệ độc *trong bin H* có thể ≫ tỷ lệ độc trong luồng ACCEPT.
2. Giới hạn của Mệnh đề 2 là "`#dòng nguồn bị chiếm ≤ m·s`" chứ không phải "tỷ lệ độc nhỏ" — tức quota bảo vệ số lượng nhưng **không** bảo vệ thành phần khi chính sách chọn có thiên lệch theo điểm tấn công. G2 phải phát biểu ρ **có điều kiện trên chính sách nạp**.
3. G4 phải xác định cách HB2 xếp hạng hạn chế (ví dụ áp quota theo nguồn *trong H*, hoặc đo tỷ lệ độc theo bin).
Đây là rủi ro đã **chấp nhận làm ràng buộc**, không phải kết luận về kết quả.

**DC-3 (HB2 vs ThreeBin, ghi chú).** v14.3 H2 vẫn nói ThreeBin; RQ2 khóa HB2. Không sửa văn bản. Mọi chỗ trong tài liệu G1–G4 nói "chính sách nạp RQ2" = HB2 (D3, S9).

## 6. Định nghĩa chỉ số ASR (D1)

Ký hiệu: `F` = họ mục tiêu; Test/đánh giá = mẫu cố định phân tầng theo lớp và họ (ĐC, Nội dung 1), dòng tương lai, nhãn thật. `λ_t` = ngưỡng cảnh báo hiệu chỉnh theo Định nghĩa 6 (`α_t = A_FP / N̂_t`) trên `D_cal` của **chính nhánh đó**. `FNR_F(arm) = 1 − recall_F(arm; λ_t(arm))`.

- `ASR_raw(arm) = FNR_F(arm)` (tại λ_t của nhánh).
- `ASR_excess(att, arm) = FNR_F(att, arm) − FNR_F(clean, none)` — trong đó `FNR_F(clean, none)` là không tấn công, không phòng thủ, cùng trace RQ3, cùng ngân sách, cùng mô hình (D1).
- Giảm tương đối của H4a (cách đọc D1, **ĐX chưa duyệt**): `RR = 1 − ASR_excess(att, def) / ASR_excess(att, none)`; H4a yêu cầu `RR ≥ 0,5` kèm tốt hơn RONI và `ΔF1_clean ≤ ε₄`.
- **Ghi chú định nghĩa (SL):**
  1. `ASR_excess(att, def)` trừ baseline *không* phòng thủ nên **gộp chi phí của phòng thủ vào tử số** (phòng thủ tự làm tăng FNR sạch). Báo cáo thêm `ASR_excess_matched = FNR_F(att, def) − FNR_F(clean, def)` làm chẩn đoán; chỉ số chính theo D1. (ĐX nhỏ.)
  2. `ASR_excess(att, none)` ≤ 0 hoặc quá nhỏ so với nhiễu seed ⇒ `RR` không xác định ⇒ `HOLD_ASSUMPTION` (đúng với P4a).
  3. `λ_t` phụ thuộc `D_cal`; nếu `D_cal` lấy từ các nguồn bị chiếm thì λ_t cũng bị đầu độc. Giả định A-DCAL: phần η·b dành cho `V_t`, `D_cal` **không** nằm trong tầm lật của đối thủ (nhãn chuyên viên). **Đây là cùng vấn đề N-5 (V_t) — giữ mở cho G2.**
  4. H4b không có ngưỡng 50% (ĐC) — chỉ "giảm". Không bổ sung ngưỡng nếu không xin duyệt (ĐX).

## 7. Lưới đánh giá phân loại (ĐC) và các lựa chọn mở

| Kiểu | Vai trò | Chế độ chạy | Ghi chú |
|---|---|---|---|
| (ii), (iii) | H4a, lưới chính | A (chính); B: v14.3 không nói có chạy tấn công trong B hay chỉ đo cờ oan (Q-G1-6) | (iii) cần surrogate K1 |
| (iv), (v) | H4b, lưới chính | như trên | (iv) cần δ, F0/F1; (v) cần surrogate |
| (i), (vi) | điểm gãy, `m = 0..K−1` | A, B | không đưa vào H4a/H4b |
| clean | đối chứng ASR_excess và F1 sạch | A, B | cùng trace |

## 8. Đầu ra G1 → G2 (chỉ chuyển giao, chưa chứng minh)

1. Định nghĩa **sự kiện kiểm toán**: ở ngày `t`, nguồn `j` có mẫu `n_{j,t}` nhãn đến; phát hiện đo trên *những dòng này*, không nhất thiết là dòng được nạp (SL từ phản ví dụ §3.3 hồ sơ).
2. DC-2: ρ phụ thuộc chính sách chọn trong HB2 ⇒ Mệnh đề 2 nên phát biểu bound có điều kiện (SL).
3. Phụ thuộc vòng: `V_t` ← tầng nguồn (D6).
4. Chế độ B: bổ đề không nhận dạng ⇒ `f*`, `β` không có nghĩa nếu không có chồng lấn.
5. Cỡ mẫu nhỏ ở `b = 100` ⇒ power(`f*`) có thể thấp (cần DoP để định lượng).

## 9. Câu hỏi mở (chỉ những điểm không suy ra được; không chặn G2)

- **Q-G1-1** Nạp dòng của nguồn `UNCERTAIN`: U1 hay U2 hay báo cả hai?
- **Q-G1-2** Mô hình nhiễu nhãn của nguồn **trung thực** trong RQ4 (không có/10% như RQ2/khác nhau theo nguồn)?
- **Q-G1-3** Tập `V_t`/`D_cal`: nhãn chuyên viên độc lập K nguồn (không thể bị lật), hay lấy từ K nguồn? (giữ cho G2, D6.)
- **Q-G1-4** (iv) lật dòng nào: ngẫu nhiên hay họ mục tiêu? δ, ω cụ thể (khóa trước DEV)?
- **Q-G1-5** Chấp nhận surrogate K1 làm định nghĩa operational của (iii), (v)? Nếu không ⇒ hai kiểu này `ATTACK_NOT_OPERATIONAL`.
- **Q-G1-6** Tấn công có chạy trong chế độ B hay chỉ đo cờ oan?
- **Q-G1-7** Giá trị `K`, `m` trong lưới chính (v14.3 không ấn định; không tự đặt).

## 10. Tóm tắt trạng thái

| Hạng mục | Trạng thái |
|---|---|
| Mô hình hình thức, tách oracle/deployment | xong (DRAFT) |
| Bảng 6 tấn công × (Obs/Budget/Action/Timing/Knowledge/GT-eval) | xong; (iii)/(v) cần Q-G1-5; (iv) cần Q-G1-4 |
| Chế độ A/B, UNCERTAIN, cờ oan, nhận dạng | xong; bổ đề không nhận dạng là SL có phác thảo chứng minh |
| Ràng buộc HB2 × (ii) | ghi DC-1/DC-2, chuyển G2/G4 |
| H4a / H4b | NO CONCLUSION |
| Mệnh đề 2 | PROOF NOT VERIFIED |
| Đề cương | NO CHANGE NOW |
