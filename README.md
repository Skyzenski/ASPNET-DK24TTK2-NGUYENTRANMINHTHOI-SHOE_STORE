## Prerequisites:
- .Net 
## Website có các chức năng:
+ Hiển thị sản phẩm giày dép, sales, voucher, checkout
+ Chỉnh sửa thông tin cá nhân, sổ địa chỉ, comment (customer)
+ Đọc Blog , tích xu, theo dõi đơn hàng, sản phẩm yêu thích
+ Quản lý số lượng giày dép, các đợt khuyến mãi, nhân viên, voucher
+ Quản lý đơn hàng và xem báo cáo
+ Quản lý Blog, banner
## Quá trình:
+ Down folder chứa template và file bacpac -> Mở SQL Server -> Import Data-tier Application... -> Import from local disk -> Browse đến file bacpac để có database và dữ liệu
+ Thao tác trong Visual Studio -> Copy dòng lệnh từ tabs [Databases] trong Connection String past vào dòng 62 trong ShoesDbContext.cs và dòng 9 trong appsetting.json
+ Mở giao diện web lên tìm và sửa lỗi và thay đổi nội dung trong khả năng
+ Giao diện BLOG có phần bị lỗi, tiến hành sửa trong [Index.cshtml*] -> Code lỗi từ template lưu lại từ dòng 72 -> 95; code đã sửa từ dòng 96 -> 133

