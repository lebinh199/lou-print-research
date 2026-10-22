# CHƯƠNG 6: BẢN ĐẶC TẢ KỸ THUẬT & KHẢO SÁT 6 DÒNG SẢN PHẨM THỰC TẾ

## 1. Bảng Khảo Sát Kỹ Thuật 6 Dòng Sản Phẩm Thực Tế Tại Trung Quốc & Việt Nam

Toàn bộ thông số kích thước, chất liệu gia công và giá bán niêm yết nguyên bản từ các xưởng in hàng đầu Taobao/Tmall:

| STT | Tên sản phẩm | Kích thước thực tế | Quy cách & Chất liệu gia công | Giá niêm yết Taobao (NDT) | Giá quy đổi VNĐ |
| :---: | :--- | :--- | :--- | :---: | :---: |
| **1** | **Sách ảnh bìa cứng 24 trang**<br>*(宝宝成长纪念册)* | Khổ 12 inch dọc: $210 \times 285\text{ mm}$ (chuẩn A4) | Bìa cứng carton dày bồi giấy mỹ thuật cán màng mờ, ruột Couche 200gsm, góc bo tròn chống cào tay trẻ em. Chứa 13 - 44 ảnh. | 32.5 – 50 NDT | 115.000 – 175.000 đ |
| **2** | **Lịch để bàn 13 tờ**<br>*(宝宝成长台历)* | Khổ 8 inch ngang: $210 \times 145\text{ mm}$ | Giấy mỹ thuật vân sần 250gsm, in 2 mặt (1 bìa + 12 tháng), đóng gáy lò xo kép kim loại, chân đế chữ A hoặc đế gỗ rời xẻ rãnh. | 9.8 – 22 NDT | 35.000 – 78.000 đ |
| **3** | **Set 9 Nam châm dẻo tủ lạnh**<br>*(磁性冰箱贴 9枚装)* | Khổ vuông: $60 \times 60\text{ mm}$ (dày 0.8mm) | Decal ảnh in sắc nét phủ màng bóng chống nước, mặt sau bồi lớp cao su từ tính dẻo 0.8mm hít tủ lạnh cực chắc. Đựng trong hộp quà kraft. | 18 – 28 NDT | 65.000 – 100.000 đ |
| **4** | **Set 9 Nam châm Acrylic bóng**<br>*(亚克力拍立得冰箱贴)* | Dáng Polaroid: $65 \times 90\text{ mm}$ | Mặt trước là lớp mica trong suốt 2mm in UV ngược, đế từ tính vĩnh cửu. Mặt dưới có khoảng trắng in tên con hoặc ngày tháng. | 35 – 55 NDT | 125.000 – 195.000 đ |
| **5** | **Bộ 12 Thẻ mốc tháng chân gỗ**<br>*(月龄里程碑卡片)* | Khổ A6: $100 \times 140\text{ mm}$ | Bìa cứng mỹ thuật 350gsm in 2 mặt, ép kim số tháng (1 to 12 Months) kèm 1 chân đế gỗ dẻ gai xẻ rãnh để bàn. | 25 – 45 NDT | 90.000 – 160.000 đ |
| **6** | **Khung tranh 3D kỷ vật đầu đời**<br>*(胎毛手足印立体相框)* | $200 \times 250\text{ mm}$ (hộp sâu 30mm) | Khung gỗ mặt mica trong, chia 3 ngăn: 1 ảnh bé $10\times 15\text{ cm}$, 1 ống thủy tinh đựng tóc máu/cuống rốn, 1 khuôn đất sét in dấu tay chân. | 45 – 88 NDT | 160.000 – 310.000 đ |

---

## 2. Giải Pháp Công Nghệ Doodle AI Bảo Mật Dữ Liệu Trẻ Em (Nghị định 13)
- **Học hỏi từ thực nghiệm mẫu app DOOD**: 
  - Phong cách sáp màu vô diện (lược bỏ hoàn toàn ngũ quan mắt, mũi, miệng, kính; chỉ giữ lại dáng vóc chuyển động và màu sắc trang phục).
  - Ưu điểm: Rất hợp đồ nhỏ (nam châm, móc khóa mica) vì không đòi hỏi ảnh gốc quá nét, che được khuyết điểm mờ nhoè và tránh rủi ro "vẽ lệch mặt con làm mẹ thất vọng".
- **Kiến trúc Server GPU riêng (Self-hosted)**:
  - Để tuân thủ **Nghị định 13/2023/NĐ-CP** về bảo vệ dữ liệu trẻ em, tuyệt đối không gửi ảnh người dùng lên các API đám mây bên thứ ba.
  - LOU triển khai mô hình Stable Diffusion 1.5 + ControlNet Crayon trên VPS GPU riêng của LOU, ảnh được lưu tạm trong RAM, xử lý xong trong 3-5 giây và tự động xóa vĩnh viễn trong vòng 60 phút.

---

## 3. Kỹ Thuật Web Studio Canvas 300 DPI Chống Tràn RAM Safari iOS
- **Thư viện đồ họa**: Sử dụng **Fabric.js v6** kết hợp kỹ thuật **Virtual Canvas Export**.
- **Giải quyết bài toán 300 DPI**: Trình duyệt di động chỉ hiển thị 72 DPI (350x500px). Khi bấm đặt in, client gửi tọa độ crop và ID ảnh gốc về backend; server dùng thư viện `sharp` kết xuất file in thực tế đạt chuẩn 300 DPI (1748x2480px cho khổ A5) kèm ICC Profile CMYK (`FOGRA39`).
- **Tránh crash trên Safari iOS**: Áp dụng cơ chế phân trang ảo (Virtual Pagination), mỗi thời điểm chỉ render 2 trang đang mở, các trang khác lưu dạng thumbnail nén WebP nhỏ giúp app mượt mà và không bao giờ bị văng.
