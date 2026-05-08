# Phân Tích Dữ Liệu - Bao Cao.csv

## Tổng Quan

| Thông tin | Giá trị |
|-----------|---------|
| File | `Bao Cao.csv` |
| Tổng số dòng (raw) | 8,696 |
| Số kênh thực tế | **605 kênh** |
| Số cột | **35 cột** |
| Công ty | HG Media |
| Phòng ban | RedOne |
| Khoảng thời gian dữ liệu | 04/2025 – 05/2026 |

---

## Cấu Trúc Cột

### Nhóm 1: Định danh kênh
| # | Cột | Mô tả |
|---|-----|-------|
| 1 | STT | Số thứ tự |
| 2 | Tên kênh | Tên kênh YouTube |
| 3 | Id Kênh | YouTube Channel ID (UCxxx...) |
| 4 | Link kênh | URL đầy đủ của kênh |
| 5 | Tài khoản thương hiệu | Brand account liên kết |

### Nhóm 2: Thông tin hợp đồng / Deal
| # | Cột | Mô tả |
|---|-----|-------|
| 6 | Tên Deal | Tên deal hợp tác (81 deal duy nhất) |
| 7 | Công ty sở hữu | Công ty chủ sở hữu kênh (hiện chỉ có `-`) |
| 8 | Công ty nhận | Công ty nhận kênh (hiện chỉ có `-`) |
| 16 | Phần trăm hợp tác | Tỷ lệ doanh thu chia sẻ (nhiều dạng: %, bậc thang) |

### Nhóm 3: Tổ chức nội bộ
| # | Cột | Mô tả |
|---|-----|-------|
| 9 | Bộ phận phụ trách | Bộ phận quản lý kênh (10 bộ phận) |
| 10 | Công ty | Tất cả đều là **HG Media** |
| 11 | Phòng ban | Tất cả đều là **RedOne** |
| 12 | Team | Tên team (10 team, khớp với bộ phận) |
| 13 | Nhân sự phát triển | Tên + Email người phụ trách kênh (89 nhân sự) |
| 14 | Mã nhân sự phát triển | Mã định danh nhân sự |
| 15 | Email nhận kênh | Email tiếp nhận kênh |

### Nhóm 4: Network / CMS
| # | Cột | Mô tả |
|---|-----|-------|
| 17 | Network | Tên mạng lưới phân phối (58 network, 334/605 có giá trị) |
| 18 | CMS | Content Management System (63 CMS, 334/605 có giá trị) |

### Nhóm 5: Phân loại nội dung
| # | Cột | Mô tả |
|---|-----|-------|
| 19 | Dự án | Thể loại nhạc / dự án (17 loại, 561/605 có giá trị) |
| 20 | Dự án đề xuất | Dự án được đề xuất |

### Nhóm 6: Mốc thời gian
| # | Cột | Mô tả |
|---|-----|-------|
| 21 | Ngày thêm kênh vào hệ thống | Ngày kênh được nhập vào hệ thống |
| 22 | Ngày nét cấp/join | Ngày kênh được cấp quyền hoặc join deal (324/605) |
| 23 | Ngày nhân sự nhận kênh | Ngày nhân sự tiếp nhận kênh (569/605) |
| 24 | Ngày phân kênh cho bộ phận | Ngày kênh được phân về bộ phận (605/605) |

### Nhóm 7: Thống kê kênh YouTube
| # | Cột | Mô tả |
|---|-----|-------|
| 25 | Quốc gia | Quốc gia chính của kênh (29 quốc gia) |
| 26 | Lượt đăng ký | Số subscribers |
| 27 | View | Tổng lượt xem |
| 28 | Số video | Tổng số video trên kênh |
| 29 | Cấp quyền truy cập | Trạng thái cấp quyền (Đã cấp / Chưa cấp) |
| 30 | Trạng thái phân kênh | Trạng thái workflow phân kênh |
| 31 | Trạng thái kiếm tiền | Đã bật / Chưa bật kiếm tiền |
| 32 | View 48h | Lượt xem trong 48 giờ gần nhất |
| 33 | DT Ana 28 ngày | Doanh thu Analytics 28 ngày |
| 34 | DT US 28 ngày | Doanh thu US 28 ngày |
| 35 | Thời lượng xem TB (s) | Average view duration (giây) |

---

## Phân Tích Chi Tiết

### Tổ Chức Nội Bộ

#### Bộ phận phụ trách (10 bộ phận)
| Bộ phận | Số kênh | % |
|---------|---------|---|
| MKT2 | 123 | 20.3% |
| Tuấn 2K | 106 | 17.5% |
| Vĩnh Kinh | 93 | 15.4% |
| RedOne | 80 | 13.2% |
| Bùi Thể | 71 | 11.7% |
| Hải Nam | 47 | 7.8% |
| Team Tuyên | 40 | 6.6% |
| MKT1 | 37 | 6.1% |
| MKT3 | 7 | 1.2% |
| Quang Duy | 1 | 0.2% |

#### Nhân sự phát triển (Top 10)
| Nhân sự | Email | Số kênh |
|---------|-------|---------|
| Hoàng Võ Hữu Phước | phuochvh@redone.vn | 15 |
| Phạm Minh Quân | quanpm@redone.vn | 15 |
| Nguyễn Ngọc Khiêm | khiemnn@redone.vn | 14 |
| Vương Mạnh Chuyền | chuyenvm@redone.vn | 13 |
| Dương Văn Hà | hadv@redone.vn | 12 |
| Võ Vĩnh Kinh | kinhvv@redone.vn | 12 |
| Nguyễn Hữu Đăng | dangnh@redone.vn | 12 |
| Trần Thanh Song | songtt@redone.vn | 12 |
| Hồ Trọng Dương | duonght@redone.vn | 12 |
| Nguyễn Việt Long | longnv@redone.vn | 12 |

---

### Dự Án / Thể Loại Nội Dung (17 loại)

| Dự án | Số kênh | Đã kiếm tiền | Chưa kiếm tiền | Tỷ lệ monetize |
|-------|---------|-------------|----------------|----------------|
| Relax | 186 | 46 | 140 | 24.7% |
| Classical | 117 | 61 | 56 | 52.1% |
| Deep House | 100 | 39 | 61 | 39.0% |
| Sóng âm | 57 | 18 | 39 | 31.6% |
| Jazz | 31 | 6 | 25 | 19.4% |
| Giáng Sinh Cover | 19 | 3 | 16 | 15.8% |
| Lofi | 15 | 6 | 9 | 40.0% |
| Downtempo | 9 | 6 | 3 | 66.7% |
| Reggae Cover | 7 | 0 | 7 | 0% |
| Guitar Cover | 7 | 0 | 7 | 0% |
| R&B | 4 | 1 | 3 | 25.0% |
| Relax Piano Cover | 3 | 1 | 2 | 33.3% |
| Ghibli | 2 | 0 | 2 | 0% |
| Indie/Folk | 1 | 0 | 1 | 0% |
| Nhạc Pop Trung Quốc | 1 | 1 | 0 | 100% |

---

### Trạng Thái Kênh

#### Trạng thái kiếm tiền
| Trạng thái | Số kênh | % |
|------------|---------|---|
| Chưa kiếm tiền | 406 | 67.1% |
| Đã bật kiếm tiền | 199 | 32.9% |

#### Trạng thái phân kênh
| Trạng thái | Số kênh | % |
|------------|---------|---|
| Nhân sự đã nhận kênh | 569 | 94.0% |
| Đã phân kênh về bộ phận | 36 | 6.0% |

#### Cấp quyền truy cập
| Trạng thái | Số kênh | % |
|------------|---------|---|
| Đã cấp quyền | 436 | 72.1% |
| Chưa cấp quyền | 169 | 27.9% |

---

### Monetization theo Team

| Team | Đã bật kiếm tiền | Chưa kiếm tiền | Tỷ lệ |
|------|-----------------|----------------|-------|
| MKT3 | 6 | 1 | 85.7% |
| Tuấn 2K | 52 | 54 | 49.1% |
| Vĩnh Kinh | 40 | 53 | 43.0% |
| Bùi Thể | 32 | 39 | 45.1% |
| MKT1 | 13 | 24 | 35.1% |
| Hải Nam | 13 | 34 | 27.7% |
| Team Tuyên | 8 | 32 | 20.0% |
| MKT2 | 32 | 91 | 26.0% |
| RedOne | 3 | 77 | 3.8% |
| Quang Duy | 0 | 1 | 0% |

---

### Network & CMS

#### Top 10 Network (trong 334 kênh có network)
| Network | Số kênh |
|---------|---------|
| OHENEMEDIA | 50 |
| AGE | 31 |
| FLYINGNUNRECORDS | 22 |
| PT SUARA MAS ABADI | 14 |
| ZEE MEDIA | 13 |
| Special Effects Media | 12 |
| HaHaHa | 11 |
| DASHGO | 11 |
| MUSE NETWORK | 10 |
| METUB | 10 |

> 271/605 kênh (44.8%) chưa có thông tin Network/CMS.

---

### Phân Bố Quốc Gia (Top 10)

| Quốc gia | Số kênh | % |
|----------|---------|---|
| US (Mỹ) | 447 | 75.0% |
| CA (Canada) | 21 | 3.5% |
| GB (Anh) | 17 | 2.9% |
| ES (Tây Ban Nha) | 16 | 2.7% |
| DE (Đức) | 15 | 2.5% |
| RU (Nga) | 10 | 1.7% |
| FR (Pháp) | 8 | 1.3% |
| NO (Na Uy) | 8 | 1.3% |
| BR (Brazil) | 7 | 1.2% |
| CH (Thụy Sĩ) | 6 | 1.0% |

---

### Thống Kê Số Liệu YouTube

> **Lưu ý:** Các giá trị số trong cột metrics (Lượt đăng ký, View, v.v.) có vẻ là giá trị đã được encode/normalize (0–1000 range), không phải số liệu thực tế tuyệt đối.

| Chỉ số | Số kênh có dữ liệu | Min | Max | Trung bình |
|--------|-------------------|-----|-----|------------|
| Lượt đăng ký | 603/605 | 0 | 998 | 69.2 |
| View | 467/605 | 0 | 960 | 140.9 |
| Số video | 605/605 | 0 | 963 | 78.9 |
| View 48h | 605/605 | 0 | 941 | 51.9 |
| DT Ana 28 ngày | 605/605 | 0 | 999 | 40.0 |
| DT US 28 ngày | 605/605 | 0 | 947 | 55.3 |
| Thời lượng xem TB (s) | 605/605 | 0 | 990 | 88.6 |

#### Top 5 kênh theo Lượt đăng ký
1. REDØNE LAB – 998
2. Deep Relax Music – 992
3. Lunexa Sleep Frequency – 982
4. Relaxing Jazz Radio – 973
5. Timeless Christmas / Gentle Xmas Melodies / Xmas Harmony – 972

#### Top 5 kênh theo View
1. Calming Paradise for Pets – 960
2. Helios Classical – 942
3. Спокойная Душа – 925
4. RP Relaxation – 924
5. La La Land – 906

---

### Timeline Tăng Trưởng Kênh

#### Số kênh thêm vào hệ thống theo tháng
| Tháng | Số kênh |
|-------|---------|
| 2025-06 | 17 |
| 2025-07 | 53 |
| 2025-08 | 34 |
| 2025-09 | 48 |
| 2025-10 | 28 |
| 2025-11 | 42 |
| 2025-12 | 25 |
| 2026-01 | 55 |
| 2026-02 | 15 |
| 2026-03 | 37 |
| 2026-04 | 45 |
| 2026-05 | 3 |

> Đỉnh cao nhất: **Tháng 1/2026** (55 kênh), **Tháng 7/2025** (53 kênh).

---

### Deals

- Tổng số kênh có Deal: **335/605 (55.4%)**
- Số deal duy nhất: **81 deal**
- Deal prefix phổ biến: `NET-OHE`, `NET-AGE`, `NET-ZEE`, `NET-FLY`, `NET-BTU`

---

## Insights Chính

1. **Thị trường trọng điểm là Mỹ** – 75% kênh hướng đến khán giả US.
2. **Thể loại Relax chiếm ưu thế** – 186 kênh (30.7%), nhưng tỷ lệ monetize chỉ 24.7%. Classical có tỷ lệ monetize cao nhất trong top dự án lớn (52.1%).
3. **Gần 67% kênh chưa kiếm tiền** – dư địa tăng trưởng doanh thu còn lớn.
4. **MKT2 quản lý nhiều kênh nhất** (123), nhưng MKT3 có tỷ lệ monetize cao nhất (85.7%).
5. **RedOne team có tỷ lệ monetize rất thấp** (3.8%) – 80/80 kênh hầu hết chưa kiếm tiền.
6. **44.8% kênh chưa có Network/CMS** – cần bổ sung thông tin đối tác phân phối.
7. **27.9% kênh chưa được cấp quyền truy cập** – bottleneck trong quy trình onboarding.
8. **Tăng trưởng mạnh đầu 2026** – tháng 1/2026 đạt đỉnh 55 kênh mới.
