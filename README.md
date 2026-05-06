## Prerequisites
- .NET

## Website có các chức năng
- Hiển thị sản phẩm giày dép, sales, voucher, checkout
- Đọc Blog, theo dõi đơn hàng, sản phẩm yêu thích
- Quản lý số lượng giày dép
- Quản lý đơn hàng và xem báo cáo
- Quản lý Blog, banner

## Quá trình
1. **Chuẩn bị dữ liệu**
   - Download folder chứa template và file `.bacpac`
   - Mở SQL Server → Import Data-tier Application → Import from local disk → Browse đến file `.bacpac` để có database và dữ liệu

2. **Kết nối database trong Visual Studio**
   - Copy dòng lệnh từ tab **[Databases]** trong Connection String
   - Paste vào dòng 62 trong `ShoesDbContext.cs` và dòng 9 trong `appsetting.json`

3. **Chạy và sửa giao diện web**
   - Mở giao diện web, tìm và sửa lỗi, thay đổi nội dung trong khả năng

4. **Sửa lỗi giao diện Blog**
   - Trong `Index.cshtml`:
     - Code lỗi từ template: dòng 72 → 95
     - Code đã sửa: dòng 96 → 133
   - Thử sửa text "Thumbnail" ngay tại Database (`dbo.BLOG [Data]`) nhưng không thành công, web bị sập, chưa rõ cách khắc phục

5. **Chức năng Tracking**
   - Hoạt động cơ bản: sau khi điền "Shipping Info" trong phần **Check Out**, hệ thống tự tạo ra Mã ID để nhập vào phần "Order ID" trong **Tracking**

6. **Chức năng thưởng coin**
   - Là chức năng của Template, không rõ cách dùng
   - Không thể xóa đi vì làm hỏng code của trang **Check Out** nên giữ nguyên