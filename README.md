# Quản Lý Cửa Hàng Máy Tính (ComputerStore) — C# WinForms + SQL Server

Ứng dụng **Quản Lý Cửa Hàng Máy Tính** được xây dựng bằng **C# WinForms** theo mô hình tách lớp (DAL/BLL/Models/Forms), kết nối **SQL Server** thông qua **ADO.NET**.  
Mục tiêu của dự án là hỗ trợ chủ cửa hàng quản lý **sản phẩm – tồn kho – đơn hàng/hóa đơn – doanh thu** một cách trực quan và dễ sử dụng.

---

## Mục lục
- [Giới thiệu](#giới-thiệu)
- [Tính năng chính](#tính-năng-chính)
- [Công nghệ sử dụng](#công-nghệ-sử-dụng)
- [Cấu trúc thư mục](#cấu-trúc-thư-mục)
- [Yêu cầu môi trường](#yêu-cầu-môi-trường)
- [Cài đặt & chạy chương trình](#cài-đặt--chạy-chương-trình)
- [Cấu hình kết nối CSDL](#cấu-hình-kết-nối-csdl)
- [Gợi ý sử dụng](#gợi-ý-sử-dụng)
- [Troubleshooting (lỗi thường gặp)](#troubleshooting-lỗi-thường-gặp)
- [Định hướng phát triển](#định-hướng-phát-triển)
- [Tác giả](#tác-giả)

---

## Giới thiệu
Dự án mô phỏng quy trình quản lý tại một cửa hàng máy tính:
- Quản lý danh mục/nhóm hàng và các sản phẩm (PC, linh kiện, phụ kiện…)
- Theo dõi số lượng tồn kho để hạn chế bán vượt tồn
- Tạo đơn hàng/hóa đơn và tự động tính tổng tiền
- Thống kê doanh thu theo thời gian/loại mặt hàng

---

## Tính năng chính
- **Quản lý sản phẩm**
  - Thêm / sửa / xóa sản phẩm
  - Tìm kiếm và lọc theo danh mục

- **Phân loại mặt hàng**
  - Quản lý danh mục, sắp xếp theo nhiều loại

- **Quản lý đơn hàng / hóa đơn**
  - Tạo hóa đơn đặt mua hàng
  - Tự động tính tổng tiền thanh toán

- **Kiểm tra tồn kho**
  - Kiểm tra số lượng tồn trước khi xác nhận mua hàng
  - Hạn chế lỗi đặt hàng khi không đủ số lượng

- **Thống kê doanh thu**
  - Báo cáo theo ngày / tháng (tùy theo chức năng bạn triển khai trong form thống kê)

---

## Công nghệ sử dụng
- **Ngôn ngữ:** C#
- **UI:** Windows Forms (WinForms)
- **Framework:** `.NET 8.0 (net8.0-windows)`
- **CSDL:** Microsoft SQL Server
- **Truy cập dữ liệu:** ADO.NET (`System.Data.SqlClient`)

---

## Cấu trúc thư mục
Repo hiện có cấu trúc chính như sau:

- `ComputerStore/`
  - `ComputerStore.sln` — Solution chính
  - `ComputerStore/` (Project WinForms)
    - `Forms/` — Giao diện WinForms (màn hình quản lý, thống kê, hóa đơn, ...)
    - `Models/` — Các lớp mô hình dữ liệu (entity/model)
    - `DAL/` — Data Access Layer: thao tác DB (query, connection, CRUD…)
    - `BLL/` — Business Logic Layer: xử lý nghiệp vụ
    - `Program.cs` — Điểm chạy chương trình (hiện `Application.Run(new Form1());`)
- `Database/` — Thư mục liên quan database (script/backup… nếu có)

> Gợi ý: nên bổ sung script `.sql` trong `Database/` để người khác dễ dựng lại DB.

---

## Yêu cầu môi trường
Để chạy dự án, bạn cần:
- **Windows 10/11**
- **.NET SDK 8.0+**
- **Visual Studio 2022** (khuyến nghị) với workload:
  - `.NET desktop development`
- **SQL Server** (Express/Developer/Standard đều được)
- (Khuyến nghị) **SQL Server Management Studio (SSMS)** để import DB và chạy script

---

## Cài đặt & chạy chương trình

### 1) Clone repository
```bash
git clone https://github.com/loanhviet/BaiTapLonCSharp.git
```

### 2) Mở solution bằng Visual Studio
Mở file:
- `ComputerStore/ComputerStore.sln`

### 3) Restore packages
Visual Studio thường tự restore. Nếu cần, bạn có thể:
- `Right click Solution` → **Restore NuGet Packages**

### 4) Build & Run
- Chọn project startup là `ComputerStore`
- Nhấn **F5** để chạy

---

## Cấu hình kết nối CSDL
Ứng dụng WinForms + ADO.NET thường có 1 chuỗi kết nối (connection string) nằm trong:
- `App.config` / `Settings.settings` (nếu có)
- hoặc hard-code trong lớp ở `DAL/`

Bạn hãy tìm từ khóa sau trong project để sửa đúng cấu hình máy bạn:
- `connectionString`
- `SqlConnection`
- `Data Source=`
- `Initial Catalog=`

Ví dụ connection string (tham khảo):
```txt
Data Source=YOUR_SERVER_NAME;
Initial Catalog=YOUR_DATABASE_NAME;
Integrated Security=True;
TrustServerCertificate=True;
```

> Nếu bạn dùng SQL Login:
```txt
Data Source=YOUR_SERVER_NAME;
Initial Catalog=YOUR_DATABASE_NAME;
User ID=sa;
Password=YOUR_PASSWORD;
TrustServerCertificate=True;
```

---

## Gợi ý sử dụng
Luồng sử dụng cơ bản (tham khảo):
1. Mở ứng dụng → đăng nhập (nếu có màn hình login)
2. Vào **Quản lý sản phẩm**:
   - tạo danh mục / nhập sản phẩm
3. Vào **Đơn hàng/Hóa đơn**:
   - chọn sản phẩm, nhập số lượng
   - hệ thống kiểm tra tồn kho
   - tạo hóa đơn và tính tổng tiền
4. Vào **Thống kê**:
   - xem doanh thu theo ngày/tháng/loại hàng

---

## Troubleshooting (lỗi thường gặp)

### Lỗi không kết nối được SQL Server
- Kiểm tra SQL Server service đã chạy chưa
- Kiểm tra tên server đúng chưa (ví dụ: `.\SQLEXPRESS`)
- Kiểm tra DB đã được tạo và có bảng dữ liệu
- Nếu dùng `Integrated Security=True` cần đảm bảo Windows account có quyền truy cập DB

### Lỗi thiếu bảng / sai cấu trúc dữ liệu
- Bạn cần import đúng script/backup trong thư mục `Database/` (nếu có)
- Đảm bảo tên bảng/cột đúng như code DAL đang query

### Chạy lên nhưng không hiện đúng form
Hiện tại `Program.cs` chạy:
```csharp
Application.Run(new Form1());
```
Nếu `Form1` không phải màn hình chính bạn mong muốn, hãy đổi sang form chính (ví dụ `MainForm`, `LoginForm`, ...).

---

## Định hướng phát triển
Một số hướng nâng cấp (tùy nhu cầu):
- Thêm **đăng nhập + phân quyền** (Admin/Nhân viên)
- Thêm **xuất hóa đơn PDF / in hóa đơn**
- Thêm **import/export Excel**
- Thống kê nâng cao (biểu đồ doanh thu, top sản phẩm bán chạy)
- Chuẩn hóa cấu hình bằng `appsettings` (nếu chuyển sang .NET Generic Host) hoặc gom cấu hình vào `App.config`

---

## Tác giả
- GitHub: https://github.com/loanhviet
- Repo: https://github.com/loanhviet/BaiTapLonCSharp
