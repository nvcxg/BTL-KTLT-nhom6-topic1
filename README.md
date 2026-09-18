# BÀI TẬP LỚN KỸ THUẬT LẬP TRÌNH (C++) - NHÓM 6

> **Học viện Công nghệ Bưu chính Viễn thông - Khoa Viễn thông 1**  
> **Học phần:** Kỹ thuật lập trình  
> **Chủ đề 1:** Quản lý thuê bao di động  
> **Ngôn ngữ & Phương pháp:** C++ | Lập trình hướng đối tượng (OOP) | Lưu trữ File (`<fstream>`)

---

## MỤC LỤC
1. [Giới thiệu đề tài](#1-giới-thiệu-đề-tài)
2. [Phân công nhiệm vụ thành viên](#2-phân-công-nhiệm-vụ-thành-viên)
3. [Mô hình dữ liệu & Quan hệ giữa các đối tượng](#3-mô-hình-dữ-liệu--quan-hệ-giữa-các-đối-tượng)
4. [Yêu cầu chi tiết đối với từng Use Case (CRUD)](#4-yêu-cầu-chi-tiết-đối-với-từng-use-case-crud)
5. [Yêu cầu kỹ thuật & Kiến trúc mã nguồn](#5-yêu-cầu-kỹ-thuật--kiến-trúc-mã-nguồn)
6. [Quy chuẩn lưu trữ file dữ liệu](#6-quy-chuẩn-lưu-trữ-file-dữ-liệu)
7. [Cấu trúc thư mục dự án](#7-cấu-trúc-thư-mục-dự-án)
8. [Kế hoạch thực hiện & Sản phẩm phải nộp](#8-kế-hoạch-thực-hiện--sản-phẩm-phải-nộp)
9. [Tiêu chí đánh giá & Quy định chung](#9-tiêu-chí-đánh-giá--quy-định-chung)

---

## 1. GIỚI THIỆU ĐỀ TÀI
Chương trình **Quản lý thuê bao di động** là một ứng dụng console duy nhất được xây dựng bằng C++ hướng đối tượng, phục vụ công tác quản lý nghiệp vụ viễn thông di động: thông tin khách hàng, số thuê bao (SIM), gói cước, hợp đồng, thiết bị đầu cuối, cước phí, nạp tiền, lưu lượng cuộc gọi và xử lý khiếu nại.

* **Môi trường chạy:** Console/Terminal (Menu điều khiển dạng số).
* **Nguyên tắc dữ liệu:** Không sử dụng hệ quản trị CSDL (như MySQL/SQLite). Toàn bộ dữ liệu được nạp vào bộ nhớ khi khởi động và ghi bền vững vào các tệp tin trong thư mục `data/` sau mỗi thao tác thêm/sửa/xóa.

---

## 2. PHÂN CÔNG NHIỆM VỤ THÀNH VIÊN
Nhóm gồm 5 thành viên phụ trách đủ **10 use case** (mỗi thành viên 2 đối tượng: 1 đối tượng danh mục/cơ bản và 1 đối tượng nghiệp vụ/liên kết):

| STT | Họ và tên | Mã sinh viên | Use case 1 (Đối tượng 1) | Use case 2 (Đối tượng 2) | File dữ liệu (`data/`) |
| :---: | :--- | :---: | :--- | :--- | :--- |
| **1** | **Bùi Gia Khánh** | B24DCVT191 | **Gói cước** | **Khách hàng** | `packages.txt`<br>`customers.txt` |
| **2** | **Nguyễn Văn Cường** | B24DCVT053 | **Loại hình thuê bao** *(Trả trước / Trả sau)* | **Phiếu khiếu nại** | `sub_types.txt`<br>`complaints.txt` |
| **3** | **Kiều Đức Hiệp** | *(Bổ sung)* | **Thiết bị đầu cuối (IMEI)** | **Bản ghi cuộc gọi (CDR)** | `devices.txt`<br>`cdrs.txt` |
| **4** | **Vũ Quang Anh** | *(Bổ sung)* | **SIM số thuê bao** | **Hóa đơn cước** | `sims.txt`<br>`invoices.txt` |
| **5** | **Lương Đức Anh** | B24DCVT006 | **Phiếu nạp tiền** | **Hợp đồng đăng ký** | `topups.txt`<br>`contracts.txt` |

---

## 3. MÔ HÌNH DỮ LIỆU & QUAN HỆ GIỮA CÁC ĐỐI TƯỢNG

Mỗi đối tượng phải có tối thiểu **3 thuộc tính**, trong đó có **1 mã định danh duy nhất (ID/Khóa chính)** không được phép trùng lặp và không được phép sửa.

### Đặc tả sơ bộ các thực thể:
1. **Gói cước (`Package`)** *(Bùi Gia Khánh)*
   * Thuộc tính: `maGoi` (PK), `tenGoi`, `giaCuoc`, `chuKyNgay`, `dungLuongDataMB`, `soPhutNoiMang`, `soPhutNgoaiMang`.
2. **Khách hàng (`Customer`)** *(Bùi Gia Khánh)*
   * Thuộc tính: `maKH` (PK), `hoTen`, `soCCCD`, `ngaySinh`, `diaChi`, `soDienThoaiLienHe`.
3. **Loại hình thuê bao (`SubscriptionType`)** *(Nguyễn Văn Cường)*
   * Thuộc tính: `maLoaiHinh` (PK, ví dụ: `TT` - Trả trước, `TS` - Trả sau), `tenLoaiHinh`, `hanMucCuocMacDinh`, `moTa`.
4. **Phiếu khiếu nại (`Complaint`)** *(Nguyễn Văn Cường)*
   * Thuộc tính: `maPhieuKN` (PK), `soThueBao` (FK tham chiếu SIM), `ngayTiepNhan`, `noiDung`, `mucDoUuTien` (Thấp/Trung bình/Khẩn), `trangThai` (Chờ xử lý/Đang xử lý/Đã giải quyết).
5. **Thiết bị đầu cuối IMEI (`Device`)** *(Kiều Đức Hiệp)*
   * Thuộc tính: `soIMEI` (PK, 15 chữ số), `tenThietBi`, `hangSanXuat`, `loaiThietBi` (Smartphone, Tablet, IoT), `namSanXuat`.
6. **Bản ghi cuộc gọi CDR (`CallDetailRecord`)** *(Kiều Đức Hiệp)*
   * Thuộc tính: `maCDR` (PK), `soGoi` (FK tham chiếu SIM), `soNhan`, `thoiDiemBatDau` (Date/Time), `thoiLuongGiay`, `loaiCuocGoi` (NoiMang / NgoaiMang / QuocTe), `cuocPhatSinh`.
7. **SIM số thuê bao (`SIMCard`)** *(Vũ Quang Anh)*
   * Thuộc tính: `soThueBao` (PK, ví dụ 09xxxxxxxx), `soSeriSIM` (duy nhất), `maLoaiHinh` (FK tham chiếu Loại hình), `trangThai` (Chưa kích hoạt / Hoạt động / Khóa 1 chiều / Khóa 2 chiều), `ngayKichHoat`.
8. **Hóa đơn cước (`BillingInvoice`)** *(Vũ Quang Anh)*
   * Thuộc tính: `maHoaDon` (PK), `soThueBao` (FK), `thangNam` (MM/YYYY), `cuocThueBao`, `cuocPhatSinh`, `tongTien`, `trangThaiThanhToan` (Chưa trả / Đã thanh toán), `ngayThanhToan`.
9. **Phiếu nạp tiền (`TopupTransaction`)** *(Lương Đức Anh)*
   * Thuộc tính: `maGiaoDich` (PK), `soThueBao` (FK), `menhGia`, `hinhThucNap` (TheCao / ChuyenKhoan / ViDienTu), `thoiGianNap`, `trangThai` (Thành công / Thất bại).
10. **Hợp đồng đăng ký (`Contract`)** *(Lương Đức Anh)*
    * Thuộc tính: `maHopDong` (PK), `maKH` (FK tham chiếu Khách hàng), `soThueBao` (FK tham chiếu SIM), `maGoi` (FK tham chiếu Gói cước), `soIMEI` (FK tham chiếu Thiết bị, tùy chọn), `ngayDangKy`, `thoiHanCamKetThang`.

### Quy tắc toàn vẹn dữ liệu (Ràng buộc khóa ngoại):
* **Khi Thêm mới (Create):** Khóa ngoại tham chiếu phải tồn tại trong danh sách tương ứng (ví dụ: tạo Hợp đồng thì `maKH`, `soThueBao`, `maGoi` phải tồn tại).
* **Khi Xóa (Delete):** Không được xóa một bản ghi nếu bản ghi đó đang được tham chiếu bởi bản ghi khác (ví dụ: không được xóa Gói cước nếu đang có Hợp đồng sử dụng gói đó; không được xóa Khách hàng khi họ còn SIM/Hợp đồng đang hoạt động).

---

## 4. YÊU CẦU CHI TIẾT ĐỐI VỚI TỪNG USE CASE (CRUD)
Mỗi sinh viên chịu trách nhiệm cài đặt đầy đủ các thao tác sau cho 2 đối tượng của mình:

| Thao tác | Chức năng | Yêu cầu nghiệp vụ & Kỹ thuật chi tiết |
| :---: | :--- | :--- |
| **C** | **Thêm mới (Create)** | - Nhập dữ liệu từng trường từ bàn phím.<br>- Kiểm tra tính hợp lệ dữ liệu (không rỗng, đúng kiểu số, ngày tháng hợp lệ).<br>- **Kiểm tra trùng mã định danh (PK):** Báo lỗi và yêu cầu nhập lại nếu đã tồn tại.<br>- Kiểm tra sự tồn tại của khóa ngoại (FK).<br>- Lưu ngay bản ghi mới vào file tương ứng trong `data/`. |
| **R** | **Xem & Tìm kiếm (Read)** | - Hiển thị danh sách toàn bộ dạng bảng căn cột đẹp mắt (`<iomanip>`).<br>- Tìm kiếm chính xác theo mã định danh.<br>- Tìm kiếm gần đúng theo tên / từ khóa (không phân biệt chữ hoa/thường).<br>- Sắp xếp danh sách (theo tên A-Z, theo mã, theo giá trị/ngày tháng). |
| **U** | **Cập nhật (Update)** | - Nhập mã định danh để tìm kiếm bản ghi cần sửa.<br>- Hiển thị dữ liệu hiện tại của bản ghi.<br>- Cho phép nhập thông tin mới cho từng trường; **nếu để trống (nhấn Enter) thì giữ nguyên giá trị cũ**.<br>- **Tuyệt đối không cho phép sửa mã định danh (PK)**.<br>- Kiểm tra hợp lệ dữ liệu mới và cập nhật lại file. |
| **D** | **Xóa (Delete)** | - Nhập mã định danh cần xóa.<br>- Hiển thị chi tiết bản ghi và yêu cầu xác nhận: `Ban co chac chan muon xoa? (y/n): `.<br>- **Kiểm tra ràng buộc tham chiếu:** Nếu bản ghi đang được thực thể khác sử dụng thì từ chối xóa và hiển thị lý do.<br>- Cập nhật lại file sau khi xóa. |

---

## 5. YÊU CẦU KỸ THUẬT & KIẾN TRÚC MÃ NGUỒN

### 5.1. Áp dụng kỹ thuật Lập trình hướng đối tượng (OOP)
* **Đóng gói (Encapsulation - Bắt buộc):** Các thuộc tính phải là `private` hoặc `protected`. Truy cập qua các hàm `getter` và `setter` (setter phải kiểm tra tính hợp lệ của dữ liệu).
* **Kế thừa & Trừu tượng (Inheritance & Abstraction - Bắt buộc):**
  * Xây dựng lớp trừu tượng cơ sở chung: `Entity` (hoặc `BaseModel`).
  * `Entity` chứa mã định danh chung và các phương thức thuần ảo (`pure virtual`):
    ```cpp
    virtual void input() = 0;
    virtual void display() const = 0;
    virtual std::string serialize() const = 0;
    virtual void deserialize(const std::string& line) = 0;
    virtual std::string getId() const = 0;
    ```
  * Các lớp đối tượng nghiệp vụ (`Customer`, `SIMCard`, `Package`,...) kế thừa từ `Entity`.
* **Đa hình (Polymorphism - Khuyến khích/Bắt buộc theo rubric):** Sử dụng con trỏ/tham chiếu lớp cơ sở gọi các phương thức ảo được ghi đè (`override`).
* **Xử lý ngoại lệ (Exception Handling - Bắt buộc):** Dùng `try / catch / throw` cho lỗi nhập liệu (sai kiểu, chuỗi rỗng, ngày tháng vô lý) và lỗi đọc/ghi file. Xây dựng lớp ngoại lệ tùy biến (Custom Exception).
* **Khuôn mẫu (Templates - Khuyến khích):**
  * Xây dựng lớp mẫu `Repository<T>` dùng chung cho toàn bộ nhóm để quản lý danh sách: `add()`, `update()`, `remove()`, `findById()`, `getAll()`, `loadFile()`, `saveFile()`.
* **Sử dụng STL:**
  * Dùng `std::vector`, `std::map`, thuật toán `<algorithm>` (`std::sort`, `std::find_if`, `std::remove_if`), biểu thức lambda.
* **Quản lý bộ nhớ:** Giải phóng con trỏ đầy đủ hoặc sử dụng con trỏ thông minh (`std::unique_ptr`, `std::shared_ptr`).

### 5.2. Giao diện Console
* Điều khiển thông qua menu đánh số nhiều cấp (Menu chính $\to$ Menu con của từng use case $\to$ Quay lại / Thoát).
* Dùng `<iomanip>` (`setw`, `left`, `right`) để hiển thị bảng dữ liệu đều, đẹp, dễ nhìn.
* **Xử lý trôi lệnh và nhập sai kiểu:** Dùng hàm tiện ích (ví dụ `InputHelper::getInt()`, `InputHelper::getString()`) kiểm tra `std::cin.fail()`, xóa cờ lỗi `cin.clear()` và loại bỏ ký tự thừa `cin.ignore()` để chương trình không bị lặp vô hạn hay dừng đột ngột.

---

## 6. QUY CHUẨN LƯU TRỮ FILE DỮ LIỆU
* Thống nhất lưu trữ bằng **file văn bản (`.txt`)** đặt trong thư mục `data/`.
* Mỗi dòng là một bản ghi.
* Các trường phân tách nhau bằng ký tự gạch đứng `|`.
* Xử lý tốt khoảng trắng trong chuỗi ký tự.
* Dòng đầu tiên có thể là tiêu đề (header) bắt đầu bằng dấu `#` để chú thích.
* **Dữ liệu mẫu nộp bài:** Mỗi file phải có **tối thiểu 10 bản ghi hợp lệ** và dữ liệu tham chiếu giữa các file phải khớp nhau (ví dụ: các `maKH` trong `contracts.txt` đều phải có trong `customers.txt`).

**Ví dụ cấu trúc `data/packages.txt`:**
```text
#MaGoi|TenGoi|GiaCuoc|ChuKyNgay|DataMB|PhutNoiMang|PhutNgoaiMang
V90|Goi Cuoc V90|90000|30|61440|1000|50
ST120K|Goi Cuoc ST120K|120000|30|92160|0|0
SD135|Goi Cuoc SD135|135000|30|153600|0|0
```

---

## 7. CẤU TRÚC THƯ MỤC DỰ ÁN
```text
BTL-KTLT-nhom6-topic1/
├── CMakeLists.txt                # Cấu hình biên dịch (hoặc Makefile)
├── README.md                     # Tài liệu tổng quan & phân công nhiệm vụ
├── De_bai.md                     # Đề bài chi tiết từ giảng viên
├── data/                         # Thư mục chứa 10 file dữ liệu (.txt)
│   ├── packages.txt
│   ├── customers.txt
│   ├── sub_types.txt
│   ├── complaints.txt
│   ├── devices.txt
│   ├── cdrs.txt
│   ├── sims.txt
│   ├── invoices.txt
│   ├── topups.txt
│   └── contracts.txt
├── include/                      # Header files (.h / .hpp)
│   ├── core/                     # Lớp cơ sở Entity, Repository<T>, Exception
│   │   ├── Entity.h
│   │   ├── Repository.h
│   │   └── Exceptions.h
│   ├── models/                   # 10 lớp thực thể của các thành viên
│   │   ├── Package.h
│   │   ├── Customer.h
│   │   ├── SubscriptionType.h
│   │   ├── Complaint.h
│   │   ├── Device.h
│   │   ├── CallDetailRecord.h
│   │   ├── SIMCard.h
│   │   ├── BillingInvoice.h
│   │   ├── TopupTransaction.h
│   │   └── Contract.h
│   ├── utils/                    # Các hàm tiện ích dùng chung
│   │   ├── Date.h
│   │   ├── InputHelper.h
│   │   └── Validator.h
│   └── views/                    # Menu điều khiển console
│       ├── MainMenu.h
│       └── ... (các Menu con)
└── src/                          # File cài đặt (.cpp)
    ├── main.cpp
    ├── models/
    ├── utils/
    └── views/
```

> **Quy định đóng góp code:** Ở đầu mỗi file mã nguồn (.h, .cpp), ghi rõ họ tên và MSSV của thành viên phụ trách để phục vụ chấm điểm cá nhân:
```cpp
/**
 * Tác giả: Nguyễn Văn Cường
 * MSSV: B24DCVT053
 * Đối tượng: SubscriptionType & Complaint
 */
```

---

## 8. KẾ HOẠCH THỰC HIỆN & SẢN PHẨM PHẢI NỘP

### 8.1. Các mốc thời gian (Milestones)
* **Giai đoạn 1 (Tuần 1 - 3):** Phân tích thiết kế, thống nhất cấu trúc dữ liệu, viết lớp cơ sở `Entity`, template `Repository<T>`, và các thư viện tiện ích dùng chung (`Date`, `InputHelper`).
* **Giai đoạn 2 (Tuần 4 - 8):** Mỗi thành viên độc lập cài đặt 2 thực thể và 2 use case CRUD của mình; chuẩn bị file dữ liệu mẫu 10 bản ghi trong `data/`.
* **Giai đoạn 3 (Tuần 9 - 10):** Tích hợp vào chương trình chính (`main.cpp`, `MainMenu`), kiểm tra ràng buộc khóa ngoại liên thông giữa các phân hệ, sửa lỗi.
* **Giai đoạn 4 (Tuần 11 - 12):** Viết báo cáo BTL (20-30 trang), làm slide thuyết trình (10-15 slide), quay video demo (nếu yêu cầu) và bảo vệ trước giảng viên.

### 8.2. Danh mục sản phẩm nộp
1. **Mã nguồn hoàn chỉnh:** Biên dịch thành công, không có cảnh báo/lỗi, chạy ổn định trên terminal.
2. **Thư mục `data/`:** Đủ 10 file dữ liệu, mỗi file $\ge$ 10 bản ghi chuẩn, liên kết logic chặt chẽ.
3. **Báo cáo (Word / PDF, 20 - 30 trang):**
   * *Bìa, mục lục*
   * *Chương 1:* Giới thiệu và phân tích yêu cầu bài toán quản lý thuê bao di động.
   * *Chương 2 (PI 3.1):* Môi trường phát triển, công cụ lập trình (IDE, Git/GitHub, Debugger), danh sách thư viện chuẩn C++ đã dùng.
   * *Chương 3 (PI 3.2):* Thiết kế giải pháp kiến trúc lớp, quản lý rủi ro (lỗi file, xóa tham chiếu) và tiềm năng mở rộng hệ thống.
   * *Chương 4 (PI 3.3):* Kiến thức mới tự nghiên cứu và áp dụng (Cấu trúc CDR, kỹ thuật bắt ngoại lệ, Template Repository...).
   * *Kết luận.*
4. **Slide thuyết trình:** 10 - 15 trang.
5. **Gói nộp bài:** Nén toàn bộ thành file `.zip` theo định dạng: `Nhom06_QuanLyThueBaoDiDong.zip`.

---

## 9. TIÊU CHÍ ĐÁNH GIÁ & QUY ĐỊNH CHUNG

* **Thang điểm 10:**
  * **Điểm cá nhân (5.5 điểm):**
    * Use case 1 (CRUD đầy đủ, đúng nghiệp vụ): **1.5đ**
    * Use case 2 (CRUD đầy đủ, đúng nghiệp vụ): **1.5đ**
    * Áp dụng đúng OOP (Đóng gói, kế thừa, đa hình, toán tử...): **1.0đ**
    * Kiểm tra dữ liệu & Xử lý ngoại lệ (Validation, try/catch, không crash): **1.0đ**
    * Chất lượng mã nguồn (Đặt tên, phân tách file, chú thích tác giả, không trùng lặp mã): **0.5đ**
  * **Điểm nhóm (4.5 điểm):**
    * Báo cáo đầy đủ theo chuẩn PI 3.1, 3.2, 3.3: **4.0đ**
    * Thuyết trình, trả lời câu hỏi và demo chương trình: **0.5đ**
* **Các trường hợp bị trừ điểm nghiêm trọng:**
  * Không biên dịch được: 0 điểm phần chức năng.
  * Không lưu được vào file: mất điểm lưu trữ (tối đa 50% điểm use case).
  * Không giải thích được mã nguồn do mình viết: trừ hoặc hủy điểm cá nhân.
  * Đạo nhái mã nguồn giữa các nhóm: 0 điểm toàn bài.