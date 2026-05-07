## Prerequisites
- .NET

## Website có các chức năng
- Hiển thị sản phẩm giày dép, checkout
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

4. **Sửa lỗi giao diện thuộc front-end cho Blog, Solution Explorer -> Views -> Blog -> Index.cshtml**
   - Trong `Index.cshtml`
   - Code lỗi từ template: dòng 72 → 95
   - Code đã sửa: dòng 96 → 133
   - Thử sửa text "Thumbnail" ngay tại Database (`dbo.BLOG [Data]`) nhưng không thành công, web bị sập, chưa rõ cách khắc phục

5. **Test Chức năng Tracking**
   - Hoạt động cơ bản: sau khi điền "Shipping Info" trong phần **Check Out**, hệ thống tự tạo ra Mã ID để nhập vào phần "Order ID" trong **Tracking**

6. **Chức năng thưởng coin**
   - Là chức năng của Template, không rõ cách dùng
   - Không thể xóa chức năng thưởng coin vì làm hỏng code của trang **Check Out** nên giữ nguyên

## Dựa vào Template tìm hiểu cách dựng Web trong Visual Studio

#Cấu trúc cơ bản của một trang ASP.NET MVC/Core
- Controller → xử lý logic  
- Model → dữ liệu  
- View → giao diện  

#Navigation Bar gồm 4 trang chính
- Shopping
- Blog
- Tracking
- Account

#Thư mục MODELS trong Solution Explorer
- Sanpham.cs → sản phẩm (SHOPPING)  
- Blog.cs → bài viết (BLOG)  
- Donhang.cs / Chitietphieumua.cs → đơn hàng (TRACKING)  
- Taikhoan.cs → tài khoản (ACCOUNT)  
- ShoesDbContext.cs → kết nối database  

#Thư mục CONTROLLERS trong Solution Explorer
- SanPhamController → SHOPPING  
- BlogController → BLOG  
- TrackingOrderController → TRACKING  
- AccountController → ACCOUNT  
- ShoppingCartController → giỏ hàng  

#Thư mục VIEWS trong Solution Explorer

#SanPham (SHOPPING)
- Index.cshtml → hiển thị danh sách sản phẩm  
- Details.cshtml → hiển thị chi tiết sản phẩm  
- Create.cshtml → thêm sản phẩm mới  
- Edit.cshtml → chỉnh sửa sản phẩm  
- Delete.cshtml → xóa sản phẩm  

#Blog (BLOG)
- Index.cshtml → danh sách bài viết  
- Details.cshtml → chi tiết bài viết  
- Create.cshtml → thêm bài viết  
- Edit.cshtml → chỉnh sửa bài viết  
- Delete.cshtml → xóa bài viết  

#TrackingOrder (TRACKING)
- Index.cshtml → danh sách đơn hàng  
- Details.cshtml → chi tiết đơn hàng  
- Create.cshtml → tạo đơn hàng  
- Edit.cshtml → chỉnh sửa đơn hàng  
- Delete.cshtml → xóa đơn hàng  

#Account (ACCOUNT)
- Login.cshtml → đăng nhập  
- Register.cshtml → đăng ký  
- Profile.cshtml → thông tin tài khoản (tùy chọn)  

#ShoppingCart (Giỏ hàng)
- Index.cshtml → hiển thị giỏ hàng  
- Checkout.cshtml → thanh toán (tùy chọn)  

#Shared (Dùng chung)
- _Layout.cshtml → layout chính (navbar, footer)  
- _ViewImports.cshtml → khai báo TagHelper  
- _ViewStart.cshtml → cấu hình layout mặc định 

## Dựa vào cấu trúc cây thư mục của Template viết code cho từng mục trong MODELS; CONTROLLERS; VIEWS tạo ra các mục Shopping; Blog; Tracking; Account

- VIEWS/SanPham (SHOPPING)

#MODELS

- File: Sanpham.cs
using System.ComponentModel.DataAnnotations;

public class Sanpham
{
[Key]
public int Masp { get; set; }
```text
public string Tensp { get; set; }

public decimal Gia { get; set; }

public string Hinhanh { get; set; }

public string Mota { get; set; }
```
- File: Blog.cs
using System.ComponentModel.DataAnnotations;

public class Blog
{
[Key]
public int Id { get; set; }
```text
public string Tieude { get; set; }

public string Noidung { get; set; }

public string Hinhanh { get; set; }

public DateTime Ngaydang { get; set; }
```






















































