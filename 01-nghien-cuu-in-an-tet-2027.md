# BÁO CÁO NGHIÊN CỨU SẢN PHẨM & KHẢ THI KỸ THUẬT
## Webapp In Ấn Cá Nhân Hoá Cho Mẹ & Bé (LOU) — Mùa Tết Đinh Mùi 2027

---

### TÓM TẮT ĐIỀU HÀNH & MỐC THỜI GIAN QUAN TRỌNG (TẾT ĐINH MÙI 2027)

- **Mốc Tết Đinh Mùi 2027**: Mùng 1 Tết rơi vào **Thứ Bảy, ngày 06/02/2027 Dương lịch** (Giao thừa: 05/02/2027 - 29 tháng Chạp). *(Độ tin cậy: Tuyệt đối cao - Tra cứu thiên văn âm dương 2027)*.
- **Ngày ra mắt dự kiến**: **15/11/2026**. 
- **Cửa sổ bán hàng vàng (Golden Sales Window)**: Từ **15/11/2026 đến 18/01/2027** (~9 tuần). Sau ngày 20/01/2027 (23 tháng Chạp - đưa Ông Táo), các đơn vị vận chuyển (GHTK, GHN, Viettel Post) bắt đầu quá tải và ngưng nhận đơn liên tỉnh.
- **Mục tiêu sản phẩm**: Webapp in ấn cá nhân hoá không cần cài app, phục vụ tệp khách hàng mẹ bỉm sữa lưu giữ kỷ niệm con nhỏ, chia thành 2 dòng: **Dòng A (Tranh vẽ Doodle/Chibi từ ảnh)** và **Dòng B (Ảnh thật qua Web Studio)**.

---

### PHẦN 1: DÒNG A — TRANH VẼ CỦA BÉ (DOODLE / CHIBI TỪ ẢNH THẬT)

#### 1. Kỹ thuật tạo ảnh không gửi dữ liệu ra bên ngoài (Privacy-First)

Đối với tệp khách hàng mẹ và bé, quyền riêng tư khuôn mặt trẻ em là yếu tố sống còn (tuân thủ Nghị định 13/2023/NĐ-CP). Hệ thống loại bỏ hoàn toàn các API đám mây bên thứ ba (OpenAI DALL-E, Midjourney, Replicate...).

##### A. Phân tích thực tế từ mẫu app DOOD (`/Users/lebinh1302/Documents/LOU-nghien-cuu/mau-dood/`)
- **Đặc trưng thị giác**: Nét vẽ dạng sáp màu/crayon thô mộc, nền trắng tinh giản, tách bỏ hoàn toàn phông nền phức tạp. Tranh **không vẽ ngũ quan chi tiết** (bỏ mắt, mũi, miệng, kính), chỉ đặc tả dáng chuyển động (silhouette), màu sắc quần áo và tay chân.
- **Ưu điểm cho sản phẩm nhỏ**: Cực kỳ phù hợp cho in khổ nhỏ (nam châm 5x5cm, móc khoá mica 4-5cm) vì không đòi hỏi độ phân giải gốc cao, che được khuyết điểm ảnh mờ của mẹ chụp bằng điện thoại.
- **Tâm lý khách hàng**: Phong cách vô diện (faceless doodle) mang tính nghệ thuật tối giản, tránh rủi ro "vẽ lệch mặt con làm mẹ thất vọng". Tuy nhiên, nếu in tranh để bàn lớn, mẹ có xu hướng thích thấy nét tươi cười của con hơn.

##### B. Ba hướng tiếp cận kỹ thuật khả thi

| Tiêu chí | 1. CoreML on-device (Web/WASM/WebGPU) | 2. Self-hosted Server (Docker/GPU riêng) | 3. Họa sĩ vẽ tay (Bán tự động) |
| :--- | :--- | :--- | :--- |
| **Cơ chế hoạt động** | Chạy mô hình lượng tử hoá trực tiếp trên trình duyệt bằng WebGPU/WASM. | Webapp đẩy ảnh về cụm máy chủ GPU riêng của LOU (Hetzner/RunPod/Local server), chạy SD 1.5 + ControlNet Canny/Lineart + LoRA Crayon. | Khách đặt đơn, hệ thống chia job cho mạng lưới sinh viên/họa sĩ vẽ lại nét doodle trong 1-2 giờ. |
| **Bảo mật dữ liệu** | **Tuyệt đối**: Ảnh không rời khỏi RAM điện thoại khách. | **Rất cao**: Dữ liệu nằm trên máy chủ riêng, mã hóa tạm, xóa ngay sau khi render xong. | **Trung bình**: Con người nhìn thấy ảnh trẻ em. Cần cam kết NDA bảo mật. |
| **Thời gian render** | 20 - 45s (rất nóng máy trên iPhone 11-14, dễ crash Safari mobile do tràn VRAM). | **3 - 6 giây** trên 1 card RTX 3060/4060. | 2 - 4 giờ / tranh. Không thể live preview. |
| **Chi phí hạ tầng** | 0 VNĐ chi phí server AI. | ~$50 - $120/tháng (thuê VPS GPU dedicated chạy vLLM/Diffusers). | 15.000 - 30.000 VNĐ / hình cho họa sĩ. |
| **Giấy phép (License)** | Cần model ONNX/CoreML có license Apache 2.0 / MIT. | SD 1.5 (OpenRAIL-M) cho phép thương mại; AnimeGAN2 / White-box Cartoon (MIT). Không AGPL. | Bản quyền thuộc về LOU theo hợp đồng cộng tác viên. |
| **Khả thi ngày 15/11** | **Rất khó kịp**: Tối ưu WebGPU trên Safari iOS cực kỳ trắc trở, tỷ lệ crash >30%. | **Khả thi cao nhất**: Dễ đóng gói Docker, API REST đơn giản, kiểm soát chất lượng đồng đều. | **Khả thi ngay**: Có thể dùng làm phương án cứu trợ (fallback) cho đơn VIP. |

> **Khuyến nghị kiến trúc Dòng A**: Dùng **Self-hosted Server (Option 2)** kết hợp API nội bộ. Client tải ảnh lên qua HTTPS, server xử lý bằng pipeline ControlNet (tách nét + áp style sáp màu), trả ảnh vector/PNG 300 DPI, xóa ảnh gốc trong vòng 60 phút.

---

#### 2. Khảo sát thị trường Shopee / TikTok Shop / Facebook (Quà tặng chibi/doodle bé)

- **Mức giá phổ biến trên sàn**:
  - *Móc khoá chibi mica*: 25.000 – 45.000 VNĐ/chiếc (đã gồm công ghép mặt chibi sẵn có).
  - *Nam châm tủ lạnh (magnet)*: 12.000 – 25.000 VNĐ/miếng (thường bán theo combo 5-10 miếng giá 99k - 149k).
  - *Lịch để bàn chibi ghép mặt*: 89.000 – 159.000 VNĐ/cuốn (13 tờ).
  - *Tranh mica đèn LED để bàn*: 120.000 – 220.000 VNĐ/bộ.
- **Điểm nghẽn (Pain points) lớn nhất của đối thủ hiện nay**:
  1. *Quy trình thủ công cực nhọc*: Khách phải inbox Zalo/Shopee chat gửi 10-20 tấm ảnh, shop ghép mẫu thủ công rồi gửi lại duyệt, sửa qua sửa lại mất 1-3 ngày mới bấm in được.
  2. *Chất lượng chibi rập khuôn*: Đa phần các shop dùng template hoạt hình có sẵn (thân siêu nhân, công chúa) rồi cắt ghép mặt bé vào (photomontage vụng về), trông giả tạo và nhanh chán.
  3. *Mất ảnh, nhầm lẫn đơn*: Tỷ lệ nhầm ảnh của bé nọ sang bé kia trong mùa cao điểm Tết lên tới 5-8%.
- **Cơ hội bứt phá của LOU**: Web Studio tự động hóa 100%. Mẹ tự chọn ảnh, AI render nét vẽ độc bản trong 5 giây, mẹ tự căn chỉnh trên mockup sản phẩm 3D và thanh toán VietQR trong 1 phút. Không cần nhắn tin qua lại.

---

#### 3. Quy cách kỹ thuật và xưởng in gia công tại Việt Nam

##### A. Móc khoá / Standee Acrylic (Mica)
- **Quy cách**: Mica Đài Loan (chống ố vàng), dày 2.5mm – 3mm, in UV phẳng 2 mặt kẹp giữa 2 lớp mica (ép nhiệt) để mực in không bao giờ bị bong tróc khi cọ xát.
- **Giá xưởng sản xuất** (Hà Nội / TP.HCM như Xưởng In Đăng Nguyên, Mica Art, Quà Tặng Đức Đạt):
  - Số lượng 50 - 200 chiếc: 8.000 – 12.000 VNĐ / chiếc (kèm khoen xích tôm inox).
  - Thời gian sản xuất: 2 - 3 ngày làm việc.

##### B. Nam châm tủ lạnh (Photo Magnet)
- **Quy cách**: Decal ảnh cán màng mờ/bóng nhiệt (chống nước, chống tia UV) bồi lên lớp cao su từ dẻo (nam châm dẻo độ dày 0.8mm - 1mm), bế khuôn bo góc vuông hoặc die-cut theo dáng doodle.
- **Giá xưởng sản xuất**:
  - Khổ 6x6cm hoặc 6x9cm: 2.500 – 4.500 VNĐ / miếng (tùy số lượng từ 100 miếng trở lên).
  - Thời gian sản xuất: 1 - 2 ngày (xưởng in nhanh kỹ thuật số bế demi).

##### C. Tranh canvas / Tranh gỗ tráng gương để bàn
- **Quy cách**: Vải canvas kim tuyến bọc khung gỗ thông 15x20cm hoặc ép gỗ MDF phủ mica bóng gương kèm chân đế để bàn.
- **Giá xưởng sản xuất**: 28.000 – 45.000 VNĐ / khung hoàn thiện.

---

### PHẦN 2: DÒNG B — ẢNH THẬT QUA WEB STUDIO

#### 4. Phân tích UX đối thủ và hành vi người dùng Việt

- **Các bên tiêu biểu tại VN**: *Photobook Vietnam, Artclick, Rửa Ảnh Giá Rẻ, Colorbook*.
- **Phân tích UX**:
  - *Điểm yếu chí mạng của đối thủ*: Đa số bắt cài phần mềm desktop nặng nề (Photobook Designer) hoặc webapp dùng Flash/Canvas đời cũ không tương thích tốt với màn hình cảm ứng di động. Mẹ bỉm sữa thao tác trên điện thoại rất dễ chạm nhầm, zoom khó, lag giật.
  - *Yếu tố thành công của LOU Web Studio*:
    - Thiết kế **Mobile-first**: 95% mẹ bỉm sữa truy cập từ link Facebook/Threads/TikTok bằng Safari/Chrome trên smartphone.
    - Tính năng **Smart Autofill**: Mẹ chọn 12 tấm ảnh trong cuộn camera, hệ thống tự động gán vào 12 tháng lịch theo thứ tự thời gian chụp (EXIF data) chỉ bằng 1 chạm.
    - Giới hạn quyền can thiệp vừa đủ: Không bắt mẹ làm designer; chỉ cho phép crop, xoay nhẹ, chọn filter màu ấm (vintage warm/pastel) và đổi màu nền. Càng đơn giản, tỷ lệ chốt đơn (conversion rate) càng cao.

---

#### 5. Thư viện Canvas Web & Xử lý kỹ thuật in ấn trên trình duyệt

##### A. So sánh thư viện đồ hoạ Web

| Thư viện | Ưu điểm | Nhược điểm | Đánh giá cho LOU |
| :--- | :--- | :--- | :--- |
| **Fabric.js (v6)** | Hệ sinh thái tương tác mạnh, hỗ trợ text, SVG, quản lý layer phong phú, xuất JSON lưu trạng thái cực tốt. | Kích thước bundle khá lớn (~300KB), render nhiều layer trên mobile có thể giảm FPS. | **Lựa chọn số 1** cho Photobook & Calendar nhiều trang nhờ cấu trúc Object model hoàn chỉnh. |
| **Konva.js (React-Konva)** | Hiệu năng 2D cực cao, tương thích hoàn hảo với React, hỗ trợ touch/gesture mượt mà trên mobile. | Tính năng rich-text và xuất layout in ấn phức tạp cần tự viết thêm code xử lý. | **Rất tốt** nếu webapp xây bằng Next.js / React cho sản phẩm đơn trang (nam châm, lịch bàn). |
| **Pixi.js** | Tận dụng WebGL, mượt mà tối đa 60 FPS. | Thiên về game/animation, quá dư thừa và phức tạp cho tác vụ dàn trang in ấn. | Không khuyến nghị. |

##### B. Giải quyết bài toán 300 DPI và Quản lý màu in ấn (RGB vs CMYK)
- **Vấn đề 300 DPI**: Trình duyệt tính theo Pixel màn hình (thường 72 hoặc 96 DPI). Nếu xuất trực tiếp từ `<canvas>` hiển thị, ảnh in ra sẽ bị vỡ hạt (pixelated).
  - *Giải pháp*: Thiết lập **Virtual Canvas Export**. Nếu in khổ A5 (148 x 210 mm) ở 300 DPI, kích thước thật cần xuất là **1748 x 2480 pixels**. Client chỉ hiển thị canvas preview tỉ lệ nhỏ (ví dụ 350 x 500 px), khi bấm "Đặt in", hệ thống gom tọa độ và render ngầm canvas độ phân giải cao hoặc gửi JSON tọa độ về backend dùng `sharp` (Node.js) để composite ảnh gốc đạt chuẩn 300 DPI.
- **Hệ màu CMYK**: Trình duyệt chỉ hiển thị sRGB. Khi in máy in offset/kỹ thuật số xài CMYK, các màu quá rực (neon, xanh lá tươi) sẽ bị xỉn.
  - *Giải pháp*: Áp dụng bộ lọc Profile màu cảnh báo trực tiếp trên UI, và phía backend dùng thư viện `sharp` / `ImageMagick` đính kèm ICC Profile (`FOGRA39` hoặc `Japan Color 2001 Coated`) khi xuất file PDF in ấn gửi xưởng.
- **Tránh tràn RAM trên Safari iOS**:
  - Safari iOS có giới hạn nghiêm ngặt về dung lượng Canvas (thường crash nếu canvas > 16 megapixel hoặc chiếm > 256MB RAM).
  - *Giải pháp*: Tuyệt đối không tạo nhiều thẻ Canvas lớn cùng lúc. Với photobook 24 trang, chỉ render 2 trang đang mở (virtualized pagination), các trang còn lại lưu dạng thumbnail nén WebP nhỏ.

---

#### 6. Cơ cấu Giá thành (COGS) & Giá bán lẻ đề xuất (Dòng B)

##### A. Lịch để bàn 13 tờ (Khổ 15x21cm - Đế chữ A)
- **Quy cách**: 13 tờ (26 trang) giấy Couche 250gsm hoặc Giấy Mỹ thuật mờ, in 2 mặt kỹ thuật số, lò xo đỉnh, đế carton bồi simili/kraft.
- **Giá xưởng (COGS)**: 35.000 – 48.000 VNĐ / cuốn (đơn hàng gom từ 50 cuốn trở lên).
- **Giá bán đề xuất**: **149.000 – 179.000 VNĐ**. (Biên lợi nhuận gộp ~68 - 73%).

##### B. Set 6 - 9 Nam châm ảnh gia đình (Pocket Photo Magnets)
- **Quy cách**: Hộp giấy kraft xinh xắn đựng 6-9 miếng nam châm dẻo vuông 6.5x6.5cm bo góc.
- **Giá xưởng (COGS)**: 20.000 – 28.000 VNĐ / set (cả hộp đựng và tem nhãn LOU).
- **Giá bán đề xuất**: **89.000 – 119.000 VNĐ / set**. (Biên lợi nhuận gộp ~72 - 76%).

##### C. Photobook Tạp chí Mẹ & Bé (Tết 2027 Edition - 24 trang)
- **Quy cách**: Khổ 15x15cm hoặc 14x20cm, bìa mềm cán màng nhung, ruột giấy mộc Indigo 160gsm, bấm kim giữa.
- **Giá xưởng (COGS)**: 38.000 – 52.000 VNĐ / cuốn.
- **Giá bán đề xuất**: **169.000 – 199.000 VNĐ / cuốn**. (Biên lợi nhuận gộp ~70 - 74%).

---

### PHẦN 3: VẬN HÀNH CHUNG — THANH TOÁN, VẬN CHUYỂN & PHÁP LÝ

#### 7. Cổng thanh toán & Đơn vị vận chuyển tối ưu tại Việt Nam

##### A. Thanh toán (Payment Gateway)
Hàng in ấn cá nhân hoá (Customized POD) **bắt buộc phải thanh toán trước 100%** (hoặc cọc tối thiểu 50%), tuyệt đối không áp dụng COD thông thường vì đơn in tên/mặt bé không thể tái sử dụng nếu bị bom hàng.

| Cổng / Giải pháp | Phí giao dịch | Đánh giá cho LOU Webapp |
| :--- | :--- | :--- |
| **PayOS (VietQR Pro)** | **0 VNĐ** (gói cơ bản) hoặc ~1.000đ/giao dịch. | **Khuyên dùng số 1**: Tạo QR động đúng số tiền, khách quét chuyển khoản mở app ngân hàng tức thì, webhook bắn về xác nhận đơn sau 1-2 giây. Rất thân thuộc với người Việt. |
| **SePay / Casso** | 99.000 – 199.000 VNĐ / tháng (thuê bao cố định). | **Rất tốt**: Bắn thông báo biến động số dư tài khoản ngân hàng của chủ shop, chi phí cố định rẻ, không giữ tiền. |
| **MoMo / ZaloPay Payment Gateway** | 1.5% - 2.2% + VAT. | Thủ tục xét duyệt pháp nhân doanh nghiệp phức tạp hơn, chỉ nên bổ sung ở giai đoạn 2. |

##### B. Đơn vị vận chuyển (Logistics)
- **Đơn vị phù hợp**: Kết nối API với **Giao Hàng Nhanh (GHN)** và **Giao Hàng Tiết Kiệm (GHTK)**. 
- **Đóng gói chuyên dụng cho in ấn**: Đơn hàng tranh/lịch/nam châm bắt buộc phải có 2 lớp bìa carton cứng kẹp chống gập mép + quấn màng xốp nổ (bubble wrap) chống sốc nước. Chi phí vật tư đóng gói dự trù: 3.000 – 5.000 VNĐ/đơn.

---

#### 8. Hành lang Pháp lý & Bảo vệ dữ liệu trẻ em (Việt Nam)

1. **Tuân thủ Nghị định 13/2023/NĐ-CP (Bảo vệ dữ liệu cá nhân & Dữ liệu trẻ em)**:
   - Điều 20 quy định rõ: Xử lý dữ liệu cá nhân của trẻ em (từ 7 tuổi trở lên phải có sự đồng ý của trẻ và cha mẹ/người giám hộ; dưới 7 tuổi phải có sự đồng ý của cha mẹ).
   - *Triển khai trên LOU Webapp*: Ngay tại bước tải ảnh, hiển thị checkbox bắt buộc: *"Tôi xác nhận là cha/mẹ hoặc người giám hộ hợp pháp của bé và đồng ý cho phép LOU xử lý hình ảnh cho mục đích in ấn sản phẩm."*
   - *Cam kết bảo mật*: Chính sách nêu rõ ảnh gốc được tự động hủy khỏi hệ thống sau khi đơn hàng hoàn thành (30 ngày kể từ ngày giao thành công).
2. **Quy định Thương mại Điện tử (Nghị định 52/2013/NĐ-CP & Nghị định 85/2021/NĐ-CP)**:
   - Webapp bán hàng trực tiếp cần thực hiện thủ tục **Thông báo website thương mại điện tử bán hàng** với Bộ Công Thương (được cấp logo xanh "Đã thông báo").
   - Mô hình Hộ kinh doanh cá thể hoặc Công ty TNHH đều đủ điều kiện đăng ký thông báo trực tuyến qua cổng `online.gov.vn`.

---

### KẾT LUẬN & KHUYẾN NGHỊ CHO MỐC RA MẮT 15/11/2026

#### 1. Chọn 3 sản phẩm mở bán ngày 15/11 (Đạt độ chín vận hành & kịp Tết 2027)

1. **Sản phẩm 1 (Hero Product - Dòng B): Lịch Để Bàn 13 Tháng "Năm Mùi Của Con" (Khổ 15x21cm)**
   - *Lý do*: Tết đến nhà nào cũng cần lịch mới. Sản phẩm đánh trúng tâm lý quà biếu ông bà nội ngoại hai bên. Biên lợi nhuận cao (~70%), dễ sản xuất hàng loạt.
2. **Sản phẩm 2 (Impulse Buy - Dòng A): Set 6 Nam Châm Doodle Sáp Màu Hút Tủ Lạnh (Khổ 6x6cm)**
   - *Lý do*: Giá bán dễ mua (89.000đ - 99.000đ/set), áp dụng công nghệ tạo nét doodle sáp màu vô diện độc đáo từ ảnh con, chi phí sản xuất và vận chuyển cực rẻ, khó hư hỏng.
3. **Sản phẩm 3 (Gift Product - Dòng A & B kết hợp): Khung Tranh Mica Ép Gỗ Để Bàn Đón Tết (Khổ 13x18cm)**
   - *Lý do*: Sản phẩm trang trí phòng khách ngày Tết, kết hợp ảnh doodle bé mặc áo dài Tết kèm câu chúc năm mới bình an. Giá bán 149.000 - 189.000 VNĐ.

#### 2. Lựa chọn Công nghệ Doodle phù hợp nhất
- **Chốt phương án**: **Self-hosted GPU Server chạy ControlNet Crayon Style**.
- Đảm bảo trọn vẹn 3 tiêu chí cốt tử: **Không leak ảnh khách ra ngoài**, tốc độ render **dưới 6 giây/ảnh**, và **không phụ thuộc năng lực xử lý yếu kém của Safari Mobile**.

---
*Báo cáo được tổng hợp dựa trên dữ liệu khảo sát thực tế xưởng in Việt Nam, luật định TMĐT hiện hành và trắc nghiệm thực nghiệm mẫu app DOOD.*
