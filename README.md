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

4. **Sửa lỗi giao diện thuộc front-end cho Blog, Solution Explorer -> Views -> Blog -> Index.cshtml**
   - Trong `Index.cshtml`:
   - Code lỗi từ template: dòng 72 → 95
   - Code đã sửa: dòng 96 → 133
   - Thử sửa text "Thumbnail" ngay tại Database (`dbo.BLOG [Data]`) nhưng không thành công, web bị sập, chưa rõ cách khắc phục

5. **Test Chức năng Tracking**
   - Hoạt động cơ bản: sau khi điền "Shipping Info" trong phần **Check Out**, hệ thống tự tạo ra Mã ID để nhập vào phần "Order ID" trong **Tracking**

6. **Chức năng thưởng coin**
   - Là chức năng của Template, không rõ cách dùng
   - Không thể xóa chức năng thưởng coin vì làm hỏng code của trang **Check Out** nên giữ nguyên

# 7. Dựa vào Template tìm hiểu cách dựng Web trong Visual Studio

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

#Thư mục VIEWS dựa trên cấu trúc cây thưc mục của MODELS và CONTROLLERS cho 
- VIEWS/SanPham (SHOPPING)
Index.cshtml – Danh sách sản phẩm
```text
@model IEnumerable<Sanpham>

<h2>Danh sách sản phẩm</h2>

<div class="row">
@foreach (var item in Model)
{
    <div class="col-md-3">
        <div class="card">
            <img src="@item.Hinhanh" class="card-img-top" />
            <div class="card-body">
                <h5>@item.Tensp</h5>
                <p>@item.Gia</p>
                <a href="/SanPham/Details/@item.Masp" class="btn btn-primary">Xem</a>
            </div>
        </div>
    </div>
}
</div>\\
```


