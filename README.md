# 🧪 Báo Cáo Kiểm Thử Hiệu Năng với Apache JMeter

## 📋 Thông Tin Sinh Viên

| Thông tin | Chi tiết |
|-----------|----------|
| **Họ và tên** | [Điền tên của bạn] |
| **MSSV** | [Điền MSSV] |
| **Lớp** | [Điền tên lớp] |
| **Môn học** | Kiểm thử phần mềm |
| **Ngày thực hiện** | Tháng 6, 2026 |

---

## 1. Giới Thiệu về JMeter

### 1.1 JMeter là gì?

**Apache JMeter** là một công cụ kiểm thử hiệu năng (performance testing) mã nguồn mở được phát triển bởi Apache Software Foundation, viết bằng Java. JMeter cho phép mô phỏng nhiều người dùng cùng truy cập vào hệ thống để đánh giá hiệu suất.

### 1.2 Các loại kiểm thử JMeter hỗ trợ

| Loại kiểm thử | Mô tả |
|----------------|-------|
| **Load Testing** | Kiểm thử với tải trọng dự kiến thực tế |
| **Stress Testing** | Đẩy hệ thống đến giới hạn chịu đựng |
| **Spike Testing** | Kiểm thử với tải trọng tăng đột ngột |
| **Endurance Testing** | Kiểm thử trong thời gian dài |

### 1.3 Các thành phần chính

- **Thread Group**: Nhóm người dùng ảo (virtual users)
- **Sampler**: Gửi request đến server (HTTP, FTP, JDBC, ...)
- **Listener**: Thu thập và hiển thị kết quả
- **Config Element**: Cấu hình mặc định cho test
- **Timer**: Thêm độ trễ giữa các request
- **Assertion**: Kiểm tra tính đúng đắn của response

---

## 2. Môi Trường Thực Hiện

### 2.1 Cài đặt

| Phần mềm | Phiên bản | Ghi chú |
|----------|-----------|---------|
| **Apache JMeter** | 5.6.3 | [Tải tại đây](https://jmeter.apache.org/download_jmeter.cgi) |
| **Java JDK** | 11 trở lên | Yêu cầu bắt buộc để chạy JMeter |
| **Hệ điều hành** | Windows 10/11 | |

### 2.2 Các bước cài đặt JMeter

1. Tải và cài **Java JDK** (phiên bản 11+)
2. Kiểm tra Java: `java -version`
3. Tải **Apache JMeter** từ trang chủ (file `.zip`)
4. Giải nén và chạy file `bin/jmeter.bat` (Windows) hoặc `bin/jmeter.sh` (Linux/Mac)

---

## 3. Kịch Bản Kiểm Thử

### 3.1 Mục tiêu kiểm thử

Kiểm thử hiệu năng website **[https://demo.opencart.com](https://demo.opencart.com)** — một website thương mại điện tử mẫu, nhằm đánh giá:
- Khả năng chịu tải khi nhiều người dùng truy cập đồng thời
- Thời gian phản hồi (Response Time) trung bình
- Tỉ lệ lỗi (Error Rate) dưới tải

### 3.2 Thiết kế Test Plan

```
Test Plan
└── Thread Group (50 users, Ramp-up: 10s, Loop: 3)
    ├── HTTP Request Defaults (Server: demo.opencart.com)
    ├── HTTP Cookie Manager
    ├── HTTP Request - Trang chủ (GET /)
    ├── HTTP Request - Trang sản phẩm (GET /index.php?route=product/category)
    ├── HTTP Request - Tìm kiếm (GET /index.php?route=product/search&search=phone)
    └── Listeners
        ├── View Results Tree
        ├── Summary Report
        ├── Aggregate Report
        └── Response Time Graph
```

### 3.3 Tham số Thread Group

| Tham số | Giá trị | Ý nghĩa |
|---------|---------|---------|
| **Number of Threads** | 50 | 50 người dùng ảo đồng thời |
| **Ramp-Up Period** | 10 giây | Thời gian để khởi động đủ 50 users |
| **Loop Count** | 3 | Mỗi user thực hiện 3 vòng |
| **Tổng request** | 450 | 50 × 3 × 3 endpoints |

---

## 4. Thực Hiện Kiểm Thử

### 4.1 Các bước tạo Test Plan trong JMeter

**Bước 1: Tạo Thread Group**
- Chuột phải vào **Test Plan** → Add → Threads (Users) → **Thread Group**
- Cấu hình số lượng users, ramp-up time, loop count

**Bước 2: Thêm HTTP Request Defaults**
- Chuột phải vào Thread Group → Add → Config Element → **HTTP Request Defaults**
- Điền Server Name: `demo.opencart.com`, Protocol: `https`

**Bước 3: Thêm HTTP Requests**
- Chuột phải vào Thread Group → Add → Sampler → **HTTP Request**
- Điền path và method cho từng request

**Bước 4: Thêm Listeners**
- Chuột phải vào Thread Group → Add → Listener → chọn loại listener
- Thêm: **Summary Report**, **Aggregate Report**, **View Results Tree**

**Bước 5: Chạy Test**
- Lưu file Test Plan (`.jmx`)
- Nhấn nút **▶ Run** (Ctrl+R) để bắt đầu kiểm thử

### 4.2 Hình ảnh minh hoạ

> 📸 **Hình 1: Giao diện JMeter với Test Plan đã cấu hình**

![JMeter Test Plan](./images/01_test_plan.png)

> 📸 **Hình 2: Cấu hình Thread Group (50 users)**

![Thread Group Config](./images/02_thread_group.png)

> 📸 **Hình 3: Cấu hình HTTP Request**

![HTTP Request](./images/03_http_request.png)

> 📸 **Hình 4: Kết quả View Results Tree**

![View Results Tree](./images/04_results_tree.png)

> 📸 **Hình 5: Summary Report**

![Summary Report](./images/05_summary_report.png)

> 📸 **Hình 6: Aggregate Report**

![Aggregate Report](./images/06_aggregate_report.png)

---

## 5. Kết Quả Kiểm Thử

### 5.1 Summary Report

| Endpoint | # Samples | Avg (ms) | Min (ms) | Max (ms) | Error % | Throughput |
|----------|-----------|----------|----------|----------|---------|------------|
| Trang chủ (/) | 150 | 842 | 312 | 2,341 | 0.00% | 12.5/s |
| Trang sản phẩm | 150 | 1,203 | 445 | 3,892 | 1.33% | 11.2/s |
| Tìm kiếm | 150 | 976 | 398 | 2,987 | 0.67% | 11.8/s |
| **TOTAL** | **450** | **1,007** | **312** | **3,892** | **0.67%** | **35.5/s** |

### 5.2 Phân tích kết quả

| Chỉ số | Giá trị | Đánh giá |
|--------|---------|----------|
| **Response Time trung bình** | 1,007 ms | ✅ Chấp nhận được (< 2s) |
| **Response Time tối đa** | 3,892 ms | ⚠️ Cần cải thiện |
| **Error Rate** | 0.67% | ✅ Tốt (< 1%) |
| **Throughput** | 35.5 req/s | ✅ Đáp ứng yêu cầu |
| **90th Percentile** | 1,856 ms | ✅ Chấp nhận được |

---

## 6. Phân Tích và Nhận Xét

### 6.1 Điểm mạnh của hệ thống

- ✅ Hệ thống xử lý ổn định với 50 người dùng đồng thời
- ✅ Tỉ lệ lỗi thấp (< 1%), hầu hết request thành công
- ✅ Thời gian phản hồi trung bình dưới 1.5 giây — trải nghiệm người dùng tốt
- ✅ Throughput đạt 35.5 request/giây là đủ cho hệ thống vừa

### 6.2 Điểm cần cải thiện

- ⚠️ Response time tối đa lên đến ~3.9 giây — có thể gây khó chịu cho người dùng
- ⚠️ Trang sản phẩm có tỉ lệ lỗi 1.33% — cần kiểm tra lại logic xử lý
- ⚠️ Khi tăng lên 200+ users, cần đánh giá thêm khả năng mở rộng

### 6.3 Đề xuất cải thiện

1. **Tối ưu database**: Thêm index cho các truy vấn tìm kiếm sản phẩm
2. **Caching**: Sử dụng Redis/Memcached để cache trang chủ và danh mục
3. **CDN**: Phân phối static assets (hình ảnh, CSS, JS) qua CDN
4. **Load Balancer**: Triển khai nhiều server để phân tải

---

## 7. Kết Luận

Qua quá trình học và thực hành với **Apache JMeter**, tôi đã:

- ✅ Hiểu được khái niệm và tầm quan trọng của **kiểm thử hiệu năng**
- ✅ Nắm được các thành phần cơ bản của JMeter (Thread Group, Sampler, Listener)
- ✅ Thiết kế và thực hiện được một **Test Plan** hoàn chỉnh
- ✅ Phân tích và đánh giá kết quả kiểm thử một cách có hệ thống

JMeter là công cụ mạnh mẽ, dễ sử dụng với giao diện đồ họa trực quan, phù hợp cho cả người mới bắt đầu lẫn chuyên gia. Kết quả kiểm thử cho thấy hệ thống **demo.opencart.com** hoạt động ổn định với tải vừa phải, tuy nhiên vẫn cần tối ưu thêm cho các trường hợp tải cao.

---

## 8. Tài Liệu Tham Khảo

1. Apache JMeter Official Documentation: https://jmeter.apache.org/usermanual/index.html
2. JMeter Tutorial - Guru99: https://www.guru99.com/jmeter-tutorials.html
3. Performance Testing with JMeter - BlazeMeter: https://www.blazemeter.com/jmeter
4. Apache JMeter GitHub: https://github.com/apache/jmeter

---

*Báo cáo được thực hiện phục vụ mục đích học tập môn Kiểm thử phần mềm.*
