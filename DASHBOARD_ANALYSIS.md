# Phân tích Dashboard: HG Media · Only Me RPM Dashboard

> **Mục tiêu tổng thể của dashboard:**
> Theo dõi và đánh giá hiệu suất khai thác kênh YouTube trên 3 chiều chính — **RPM (hiệu quả kiếm tiền)**, **View (lưu lượng)**, và **Doanh thu (kết quả tài chính)** — phân tầng từ cấp hệ thống xuống đến cấp kênh, bộ phận, để hỗ trợ ra quyết định đầu tư nội dung và quản lý nhân sự.

---

## Kiến trúc tổng quan

```
┌─────────────────────────────────────────────────────────┐
│                      FILTER BAR                         │
│  Khoảng thời gian · Granularity · Dự án · Network       │
│  Bộ phận · Kênh                                         │
└─────────────────────────────────────────────────────────┘
         │               │                │
    ┌────▼───┐      ┌─────▼──┐      ┌─────▼────┐
    │ Tab RPM │      │Tab VIEW│      │Tab DOANH │
    │         │      │        │      │   THU    │
    └─────────┘      └────────┘      └──────────┘
```

Mỗi tab được chia thành các **vùng dọc** (section), mỗi vùng trả lời 1 câu hỏi kinh doanh cụ thể. Ba tab dùng chung filter bar — khi thay đổi filter, toàn bộ chart và bảng trên cả 3 tab đều re-render đồng thời.

---

## Nguồn dữ liệu (`dashboard_data.json`)

| Key | Nội dung | Dùng ở đâu |
|---|---|---|
| `project_daily` | Doanh thu + view theo ngày, từng dự án | RPM/View/Revenue — vùng Dự án |
| `project_monthly` | Tổng hợp theo tháng, từng dự án | So sánh kỳ tháng |
| `network_daily` | Doanh thu + view theo ngày, từng network | Tab RPM — vùng Network |
| `network_monthly` | Tổng hợp tháng, từng network | So sánh kỳ tháng |
| `team_daily` | Doanh thu + view theo ngày, từng bộ phận | Vùng Bộ phận ở cả 3 tab |
| `team_monthly` | Tổng hợp tháng, từng bộ phận | So sánh kỳ tháng |
| `channel_daily` | Chi tiết ngày, top 50 kênh | Bảng ranking + sparkline |
| `channel_monthly` | Tổng hợp tháng, từng kênh | Bảng kênh tab RPM |
| `top_channels` | Metadata top 50 kênh (name, project, team) | Tab View — ranking table |
| `channels` | Metadata 605 kênh (name, project, network, country, team) | Dropdown filter |
| `days` | Danh sách 372 ngày có dữ liệu | Trục thời gian tất cả chart |
| `months` | Danh sách 13 tháng | Dropdown chọn kỳ |
| `projects` | Danh sách 18 dự án | Dropdown filter |
| `networks` | Danh sách 59 networks | Dropdown filter |
| `teams` / `teams_meta` | 10 bộ phận + số kênh, tỷ lệ monetize | Vùng bộ phận |

> **Lưu ý:** Dữ liệu `project_daily` và `network_daily` là 2 key riêng biệt, không có khóa ngoại nối với nhau. Khi filter đồng thời cả Dự án và Network, kết quả có thể chưa chính xác ở giao điểm.

---

## FILTER BAR (Dùng chung cho cả 3 tab)

| Bộ lọc | Loại | Ý nghĩa |
|---|---|---|
| Từ / Đến | Dropdown tháng | Chọn khoảng thời gian phân tích (mặc định 90 ngày gần nhất) |
| Granularity | Toggle 3 nút | Chuyển đổi trục thời gian: **Ngày** (chi tiết), **Tuần** (trend ngắn hạn), **Tháng** (trend dài hạn) |
| Dự án | Dropdown | Lọc theo 1 trong 18 dự án âm nhạc |
| Network | Dropdown | Lọc theo đối tác phân phối (top 30 theo doanh thu) |
| Bộ phận | Dropdown | Lọc theo 1 trong 10 team nhân sự |
| Kênh | Dropdown | Drill-down vào 1 kênh cụ thể (605 kênh) |

**Tại sao cần granularity?**
- Granularity Ngày: phát hiện spike bất thường, sự cố kênh
- Granularity Tuần: thấy trend mà không bị nhiễu bởi dao động cuối tuần
- Granularity Tháng: so sánh MoM (month-over-month) cho báo cáo quản lý

---

## TAB 1: RPM

> **Câu hỏi trung tâm:** Kênh/Dự án/Network nào đang khai thác quảng cáo **hiệu quả nhất** (doanh thu cao trên mỗi 1000 lượt xem)?

RPM = Revenue Per Mille = Doanh thu / 1000 views. Chỉ số này phản ánh chất lượng traffic và mức độ phù hợp của nội dung với nhà quảng cáo, cao hơn view thuần.

---

### Vùng 1 — KPI Row (hàng số tổng hợp)

Các thẻ số lớn hiển thị ngay đầu trang: RPM trung bình, tổng doanh thu, tổng view, số kênh active trong kỳ được chọn.

**Mục tiêu:** Nhìn nhanh "sức khỏe" tổng thể của toàn hệ thống trong 1 giây — không cần đọc bảng hay chart.

---

### Vùng 2 — RPM theo Dự án

#### Biểu đồ đường (Line Chart) — "Biến động RPM theo thời gian"

```
Trục X: Thời gian (ngày/tuần/tháng)
Trục Y: RPM ($)
Series: Mỗi dự án 1 màu đường riêng
```

**Tại sao dùng Line Chart?**
Line chart là lựa chọn tốt nhất để hiển thị **xu hướng liên tục theo thời gian**. Khi có nhiều dự án trên cùng 1 chart, người dùng có thể:
- So sánh dự án nào tăng/giảm RPM theo thời gian
- Phát hiện dự án nào có RPM bất thường tăng vọt (Q4 Giáng Sinh thường tăng mạnh)
- Phát hiện dự án nào đang suy giảm dần để có action kịp thời

#### Bảng tổng quan RPM & mức thay đổi

| Cột | Ý nghĩa |
|---|---|
| # | Hạng theo doanh thu |
| Dự án | Tên dự án âm nhạc |
| Doanh thu | Tổng $ kỳ hiện tại |
| % Δ DT | So sánh với kỳ trước cùng độ dài (▲ xanh / ▼ đỏ) |
| View | Tổng lượt xem |
| % Δ View | Tăng/giảm view so kỳ trước |
| RPM | Doanh thu / 1000 view |
| % Δ RPM | Tăng/giảm RPM so kỳ trước |

**Tại sao cần bảng song song với chart?**
Chart cho thấy trend, bảng cho thấy **con số chính xác** và **xếp hạng**. Người dùng có thể sort theo bất kỳ cột nào để phát hiện outlier (VD: view tăng nhưng RPM giảm = traffic kém chất lượng hơn).

#### Scatter Chart — "Phân bổ RPM và Doanh thu theo Dự án"

```
Trục X: Doanh thu ($) — quy mô tài chính
Trục Y: RPM ($)       — hiệu quả khai thác
Size:   Số View       — lưu lượng
Màu:    Mỗi dự án 1 màu
● hiện tại   ○ kỳ trước
```

**Tại sao dùng Scatter/Bubble Chart?**
Scatter cho phép **phân loại dự án vào 4 góc phần tư** với 1 cái nhìn duy nhất:

| Góc phần tư | Ý nghĩa | Hành động |
|---|---|---|
| X cao + Y cao | Doanh thu cao, RPM cao | **Ưu tiên đầu tư**, tăng kênh trong dự án này |
| X thấp + Y cao | Doanh thu thấp, RPM cao | **Có tiềm năng** — cần tăng view để scale |
| X cao + Y thấp | Doanh thu cao, RPM thấp | **Cần tối ưu** — traffic nhiều nhưng giá trị thấp |
| X thấp + Y thấp | Cả hai đều thấp | **Xem xét lại** — nên giảm đầu tư |

So sánh điểm ● (hiện tại) và ○ (kỳ trước) cho thấy dự án đang di chuyển về hướng nào.

---

### Vùng 3 — RPM theo Network

Cấu trúc giống Vùng 2 (Line + Bảng + Scatter), áp dụng cho 59 networks. Top 8 network theo doanh thu được hiển thị trên chart.

**Tại sao cần vùng Network riêng?**
Network là **đối tác phân phối** (distributor) quyết định kênh được monetize qua CMS nào. RPM của cùng 1 kênh có thể khác nhau theo network vì:
- Network khác nhau có thỏa thuận quảng cáo khác nhau với YouTube
- Một số network chuyên thị trường Mỹ (RPM cao hơn) vs thị trường Đông Nam Á

**Câu hỏi vùng này trả lời:** *"Nên join kênh vào network nào để tối đa hóa RPM?"*

---

### Vùng 4 — RPM theo Kênh

#### Bar Chart — "Top 10 kênh RPM cao nhất"

```
Trục X: Tên kênh
Trục Y: RPM ($)
```

**Tại sao dùng Bar Chart ngang/dọc thay vì Line?**
Bar chart phù hợp để **so sánh ranking tại 1 thời điểm** (không phải trend). Top 10 kênh RPM cao giúp xác định "kênh ngôi sao" — những kênh này cần được ưu tiên nội dung chất lượng cao và bảo vệ khỏi vi phạm bản quyền.

#### Bảng RPM theo kênh

| Cột | Ý nghĩa |
|---|---|
| Kênh | Tên kênh YouTube |
| Dự án | Kênh thuộc dự án nào |
| Doanh thu | Tổng $ kỳ hiện tại |
| View | Tổng lượt xem |
| RPM | $/1000 view |
| % Δ | So sánh RPM kỳ trước |

**Lưu ý:** Chỉ top 50 kênh có `channel_daily` — 555 kênh còn lại chỉ có metadata, không có trend.

---

### Vùng 5 — RPM theo Bộ phận

#### Line Chart — "Biến động RPM theo thời gian (theo bộ phận)"

Tương tự line chart Dự án nhưng mỗi series là 1 team nhân sự. Cho thấy team nào đang phát triển được kênh có RPM tốt theo thời gian.

#### Bảng tổng quan RPM các Bộ phận

Columns: Bộ phận | Doanh thu | % Δ | View | % Δ | RPM | % Δ

**Mục tiêu:** Đánh giá KPI team theo doanh thu và RPM — không chỉ xem team nào view nhiều mà team nào khai thác **hiệu quả**.

#### Scatter (Bubble) — "So sánh Doanh thu & View theo Bộ phận"

```
Trục X: Doanh thu ($)
Trục Y: RPM ($)
Size:   Số kênh team đang quản lý
```

**Tại sao size = số kênh?**
Để chuẩn hóa hiệu suất: 1 team có doanh thu $10K với 5 kênh tốt hơn team có $10K với 50 kênh. Bubble lớn + vị trí góc phải-trên = team vừa có nhiều kênh vừa RPM cao.

#### Stacked Bar — "Tỷ lệ kênh đã Monetize theo Bộ phận"

```
Màu xanh (≥40%): Team tốt
Màu vàng (25-39%): Team trung bình  
Màu đỏ (<25%): Team cần cải thiện
```

**Dữ liệu thực từ CSV** (không phải giả lập):

| Team | Tỷ lệ Monetize |
|---|---|
| MKT3 / Tuấn 2K | 49% |
| Bùi Thể | 45% |
| Vĩnh Kinh | 43% |
| MKT1 | 35% |
| MKT2 | 26% |
| Hải Nam | 28% |
| Team Tuyên | 20% |
| Only Me | 4% |
| Quang Duy | 0% |

**Tại sao cần biểu đồ này?**
Monetize rate đo **năng lực phát triển kênh** của team — team join được kênh nhưng chưa monetize không tạo ra doanh thu. Đây là chỉ số leading (dự báo) cho doanh thu tương lai.

---

## TAB 2: VIEW

> **Câu hỏi trung tâm:** Kênh nào đang **kéo traffic** về hệ thống? Traffic đó có **chất lượng** không (người xem ở lại hay thoát sớm)?

---

### Vùng 1 — Biến động lượt xem theo kênh

#### Grouped Bar Chart — Top 5/10/20 kênh

```
Trục X: Ngày/Tuần/Tháng
Trục Y: Số View
Group:  Mỗi kênh 1 màu thanh
Toggle: Chọn Top 5 / Top 10 / Top 20
```

**Tại sao dùng Grouped Bar thay vì Stacked?**
Grouped bar cho thấy **sự thay đổi của từng kênh riêng lẻ** qua thời gian. Stacked bar chỉ thấy tổng. Khi muốn biết "kênh A hôm qua giảm view trong khi kênh B tăng" thì Grouped Bar là lựa chọn đúng.

---

### Vùng 2 — Tổng View theo Dự án

#### Stacked Area Chart — "Tổng View theo Dự án"

```
Trục X: Thời gian
Trục Y: Tổng View (tích lũy theo ngày)
Fill:   Mỗi dự án 1 lớp màu xếp chồng
```

**Tại sao dùng Stacked Area?**
Stacked area vừa cho thấy **tổng tăng/giảm** vừa cho thấy **tỷ trọng từng dự án** thay đổi như thế nào. Nếu tổng view tăng nhưng 1 dự án chiếm ngày càng nhiều phần — đó là signal cần cân bằng lại danh mục.

#### Donut Chart — "Tỷ trọng View theo Dự án"

```
Kỳ được chọn (1 snapshot)
Mỗi phần = 1 dự án, label % và tên
```

**Tại sao dùng Donut thay vì Pie?**
Donut (Pie có lỗ giữa) dễ đọc hơn khi có nhiều phần tử nhỏ. Phần giữa trống có thể hiển thị tổng. Nó trả lời câu hỏi "dự án nào đang chiếm nhiều traffic nhất trong kỳ này?" trong 1 cái nhìn.

---

### Vùng 3 — Chất lượng View

#### Scatter — "View vs Watch Time trung bình"

```
Trục X: Tổng View
Trục Y: Watch Time trung bình (giây)
Size:   Doanh thu
Mỗi điểm: 1 kênh
```

**Đây là chart quan trọng nhất trong Tab View.**

**Tại sao?** View cao không có nghĩa là nội dung tốt. Watch Time (thời gian xem trung bình) phản ánh người dùng thực sự xem đến bao lâu. Chart này giúp phân loại:

| Góc phần tư | Ý nghĩa |
|---|---|
| View cao + Watch Time cao | Kênh vàng — nội dung hấp dẫn, giữ chân người xem |
| View cao + Watch Time thấp | Cảnh báo — click-bait, thuật toán có thể penalize |
| View thấp + Watch Time cao | Tiềm năng — nội dung tốt nhưng chưa được đề xuất nhiều |
| View thấp + Watch Time thấp | Cần xem xét lại nội dung |

#### Bar Chart — "Top 10 kênh theo View 48h"

Chỉ số tức thời — 48h gần nhất. Khác với tổng kỳ, chỉ số này phản ánh **momentum hiện tại** của kênh, hữu ích để phát hiện kênh đang viral hoặc kênh vừa bị vấn đề.

---

### Vùng 4 — Bảng xếp hạng kênh

| Cột | Ý nghĩa |
|---|---|
| Kênh | Tên kênh |
| Dự án | Thuộc dự án nào |
| View | Tổng view kỳ hiện tại |
| Watch Time TB | Thời gian xem trung bình (giây) |
| Doanh thu | $ kỳ hiện tại |
| RPM | $/1000 view |
| % Δ View | Tăng/giảm so kỳ trước |
| Trend (ngày) | **Sparkline** mini-chart 30 ngày gần nhất |

**Toggle metric:** Bảng có thể sort và hiển thị theo View / Watch Time / Doanh thu.

**Tại sao có Sparkline?**
Sparkline (mini line chart trong ô bảng) cho thấy **trend** mà không cần thêm 1 chart riêng. Người dùng thấy ngay kênh đang tăng dần hay giảm dần mà không cần click vào đâu thêm — tăng mật độ thông tin mà không làm UI phức tạp hơn.

---

### Vùng 5 — Pattern theo thứ và so sánh kỳ

#### Bar Chart — "View theo ngày trong tuần"

```
Trục X: Thứ 2 → Chủ nhật
Trục Y: TB view theo thứ
```

**Tại sao cần biểu đồ này?**
YouTube traffic không phân bổ đều theo tuần. Âm nhạc thư giãn thường peak vào cuối tuần (Thứ 6–Chủ nhật) khi người dùng có thời gian nghỉ ngơi. Biết pattern này giúp:
- Lên lịch đăng video vào ngày có traffic tự nhiên cao
- Phân bổ ngân sách quảng cáo tập trung vào ngày cao điểm
- Giải thích tại sao view cuối tuần cao hơn đầu tuần (không phải do nội dung tốt hơn)

#### Grouped Bar — "So sánh View kỳ hiện tại vs kỳ trước"

```
Màu xanh: Kỳ hiện tại
Màu xám: Kỳ trước (cùng độ dài)
Group: Từng dự án
```

**Mục tiêu:** Đo tăng trưởng view theo dự án. Nếu kỳ trước xanh cao hơn xám ở mọi dự án — hệ thống đang suy giảm toàn diện, cần action ngay.

---

### Vùng 6 — View theo Bộ phận

| Component | Ý nghĩa |
|---|---|
| Line Chart | Biến động view theo thời gian từng team |
| Donut Chart | Tỷ trọng view team kỳ được chọn |
| Grouped Bar | So sánh view kỳ hiện tại vs kỳ trước theo team |

**Mục tiêu:** Xác định team nào đang tăng traffic và team nào đang mất momentum.

---

## TAB 3: DOANH THU

> **Câu hỏi trung tâm:** Tiền đến từ đâu? Xu hướng doanh thu đang tăng hay giảm? Mục tiêu tháng/quý đang ở đâu so với tiến độ thực tế?

**2 loại doanh thu trong dashboard:**
- **DT Ana (Analytics):** Tổng doanh thu toàn cầu từ YouTube Analytics — nguồn chính xác
- **DT US:** Doanh thu thị trường Mỹ ≈ 68% DT Ana *(hiện đang giả lập — cần YouTube Analytics API thật để tách riêng)*

---

### Vùng 1 — Biến động Doanh thu theo thời gian

#### Stacked Area Chart — "Doanh thu Ana theo Dự án"

Tương tự tab View nhưng trục Y là $. Cho thấy dự án nào đang đóng góp nhiều nhất vào tổng doanh thu và xu hướng thay đổi.

#### Donut Chart — "Tỷ trọng Doanh thu"

Phân bổ DT Ana vs DT US theo dự án trong kỳ. Cho thấy phụ thuộc vào thị trường Mỹ ở mức nào — nếu DT US chiếm quá cao, rủi ro khi thuật toán YouTube Mỹ thay đổi sẽ ảnh hưởng lớn.

---

### Vùng 2 — Phân tích nguồn doanh thu

#### Grouped Bar — "DT Ana vs DT US theo Dự án"

```
Thanh xanh: DT Ana (toàn cầu)
Thanh cam: DT US (thị trường Mỹ)
```

**Tại sao so sánh Ana vs US?**
Nếu DT US / DT Ana < 50%: kênh đang thu hút traffic từ thị trường có RPM thấp hơn Mỹ (Đông Nam Á, Ấn Độ...). Cần tối ưu nội dung để thu hút audience Mỹ hơn. Tỷ lệ benchmark hiện tại ≈ 68%.

#### Bar Chart — "Top 10 Network theo Doanh thu"

So sánh 10 network kiếm nhiều tiền nhất trong kỳ, kèm so sánh kỳ trước. Giúp quyết định network nào nên ưu tiên join kênh mới.

---

### Vùng 3 — Phân tích chất lượng Doanh thu

#### Scatter — "RPM vs Doanh thu theo Dự án"

```
Trục X: Doanh thu ($)
Trục Y: RPM ($)
Size:   Số View
```

Tương tự scatter ở Tab RPM nhưng đặt trong context Tab Doanh thu — nhấn mạnh vào phân loại dự án để **ra quyết định đầu tư**: dự án nào nên tăng kênh (top-right), dự án nào cần tối ưu nội dung (bottom-right).

#### Bar Chart — "Doanh thu theo ngày trong tuần"

Tương tự View DoW nhưng trục Y là $. Pattern doanh thu theo thứ thường khác với pattern view vì:
- Nhà quảng cáo chi nhiều tiền hơn vào **đầu tuần** (Thứ 2, Thứ 3) khi chạy campaign mới
- Cuối tuần (Thứ 6–Chủ nhật) có view cao nhưng RPM thấp hơn vì ngân sách quảng cáo đã cạn
- Hiểu pattern này giúp dự báo doanh thu tuần tốt hơn

---

### Vùng 4 — Tracking tiến độ

#### Line Chart — "Doanh thu tích lũy theo thời gian"

```
Trục X: Thời gian
Trục Y: Tổng $ tích lũy (cumulative sum)
```

**Tại sao cần Cumulative Chart?**
Chart thông thường cho thấy doanh thu từng ngày (có thể dao động mạnh). Cumulative chart cho thấy **tiến độ tổng thể** — đường dốc đều là tốt, đường phẳng đột ngột là có vấn đề. Dễ dàng vẽ thêm đường mục tiêu (target line) để so sánh.

#### Bar Chart — "Tăng trưởng Doanh thu theo tháng"

```
Trục X: Tháng
Trục Y: % thay đổi so tháng trước (MoM growth)
Màu xanh: Tháng tăng
Màu đỏ: Tháng giảm
```

**Tại sao dùng % thay đổi thay vì số tuyệt đối?**
% thay đổi loại bỏ hiệu ứng mùa vụ — tháng 12 luôn cao hơn tháng 1 do Giáng Sinh, nhưng nếu % thay đổi tháng 12 so tháng 11 là -5% thì đang thấp hơn kỳ vọng mùa Giáng Sinh.

---

### Vùng 5 — Bảng xếp hạng Doanh thu theo kênh

| Cột | Ý nghĩa |
|---|---|
| Kênh | Tên kênh |
| Dự án | Thuộc dự án nào |
| DT Ana | Tổng doanh thu toàn cầu kỳ này |
| DT US | Doanh thu thị trường Mỹ |
| RPM | $/1000 view |
| View | Tổng lượt xem |
| % Δ DT | Tăng/giảm doanh thu so kỳ trước |
| Trend DT | Sparkline mini-chart xu hướng doanh thu |

**Sắp xếp mặc định:** DT Ana giảm dần — kênh kiếm nhiều nhất hiển thị đầu tiên.

---

### Vùng 6 — Doanh thu theo Bộ phận

| Component | Chart type | Ý nghĩa |
|---|---|---|
| Biến động DT theo thời gian | Stacked Area | Xem team nào đang tăng DT, đóng góp tích lũy |
| Tỷ trọng DT | Donut | Phân bổ DT giữa các team trong kỳ |
| DT & RPM theo Bộ phận | Scatter/Bubble | So sánh hiệu quả team (DT vs RPM, size = số kênh) |
| DT / Kênh theo Bộ phận | Bar | Doanh thu trung bình mỗi kênh theo team |

**Biểu đồ "DT/Kênh" là gì và tại sao quan trọng?**
Đây là chỉ số **hiệu suất chuẩn hóa**: 1 team có $50K doanh thu với 100 kênh kém hơn team có $30K với 20 kênh. DT/kênh = $500 vs $1500 — team nhỏ hơn nhưng mỗi kênh đang làm việc hiệu quả hơn gấp 3 lần.

---

## Tổng kết — Logic phân tầng của Dashboard

```
CẤP HỆ THỐNG (KPI Row)
    └── CẤP DỰ ÁN (18 dự án âm nhạc)
            ├── CẤP NETWORK (59 đối tác phân phối)
            ├── CẤP KÊNH (605 kênh, top 50 có daily data)
            └── CẤP BỘ PHẬN (10 teams nhân sự)
```

### Từng tab trả lời 1 câu hỏi khác nhau

| Tab | Câu hỏi chính | Người dùng chính |
|---|---|---|
| **RPM** | Nội dung nào có giá trị cao nhất với nhà quảng cáo? | Content strategist, Product manager |
| **View** | Traffic đến từ đâu và có chất lượng không? | Content team, Creator |
| **Doanh thu** | Đang kiếm được bao nhiêu và từ đâu? | Management, Finance |

### Khi nào dùng từng loại biểu đồ

| Loại Chart | Dùng khi | Ví dụ trong dashboard |
|---|---|---|
| **Line** | Theo dõi xu hướng liên tục theo thời gian | RPM theo dự án/team qua các ngày |
| **Stacked Area** | Xu hướng + tỷ trọng cùng lúc | Tổng view/DT theo dự án |
| **Grouped Bar** | So sánh 2 kỳ hoặc 2 metric song song | DT Ana vs DT US, kỳ này vs kỳ trước |
| **Donut/Pie** | Phân bổ tỷ trọng tại 1 thời điểm | Tỷ trọng view/DT theo dự án |
| **Scatter/Bubble** | Phân loại đối tượng theo 2-3 chiều | RPM vs DT (phân 4 góc phần tư) |
| **Bar ngang** | Ranking top N đối tượng | Top 10 kênh RPM cao nhất |
| **Stacked Bar 100%** | So sánh tỷ lệ giữa các nhóm | Tỷ lệ kênh Monetize theo team |
| **Sparkline** | Trend nhỏ gọn trong bảng | Trend 30 ngày trong bảng kênh |
| **Bar (DoW)** | Pattern theo chu kỳ tuần | View/DT theo ngày trong tuần |
| **Cumulative Line** | Tracking tiến độ mục tiêu | Doanh thu tích lũy toàn kỳ |

---

## Các hạn chế hiện tại cần lưu ý

| Vấn đề | Chi tiết | Giải pháp |
|---|---|---|
| Dữ liệu giả lập | `project_daily` views và revenue là demo (giả lập theo công thức) | Kết nối YouTube Analytics API thật |
| DT US chưa thật | Đang nhân 68% từ DT Ana, không tách được theo quốc gia thật | Cần YouTube Analytics API tách revenue by country |
| Coverage kênh | Chỉ 50/605 kênh có `channel_daily` (có trend/sparkline) | Mở rộng daily data cho tất cả kênh active |
| Cross-filter hạn chế | Filter Network + Dự án cùng lúc chưa chính xác vì 2 key riêng | Cần thiết kế lại data model có khóa ngoại |
| Benchmark RPM giả | RPM hiện tại tính từ data giả lập, chưa phản ánh thực tế | Cần data thật từ YouTube Studio / CMS |

---

*Tài liệu này mô tả dashboard tại thời điểm commit `83c81c5` (rename RedOne → Only Me). Cập nhật lần cuối: 2026-05-08.*
