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
   - Trong `Index.cshtml`:
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

# SanPham (SHOPPING)
- Index.cshtml → hiển thị danh sách sản phẩm  
- Details.cshtml → hiển thị chi tiết sản phẩm  
- Create.cshtml → thêm sản phẩm mới  
- Edit.cshtml → chỉnh sửa sản phẩm  
- Delete.cshtml → xóa sản phẩm  

# Blog (BLOG)
- Index.cshtml → danh sách bài viết  
- Details.cshtml → chi tiết bài viết  
- Create.cshtml → thêm bài viết  
- Edit.cshtml → chỉnh sửa bài viết  
- Delete.cshtml → xóa bài viết  

# TrackingOrder (TRACKING)
- Index.cshtml → danh sách đơn hàng  
- Details.cshtml → chi tiết đơn hàng  
- Create.cshtml → tạo đơn hàng  
- Edit.cshtml → chỉnh sửa đơn hàng  
- Delete.cshtml → xóa đơn hàng  

# Account (ACCOUNT)
- Login.cshtml → đăng nhập  
- Register.cshtml → đăng ký  
- Profile.cshtml → thông tin tài khoản (tùy chọn)  

# ShoppingCart (Giỏ hàng)
- Index.cshtml → hiển thị giỏ hàng  
- Checkout.cshtml → thanh toán (tùy chọn)  

# Shared (Dùng chung)
- _Layout.cshtml → layout chính (navbar, footer)  
- _ViewImports.cshtml → khai báo TagHelper  
- _ViewStart.cshtml → cấu hình layout mặc định 

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
Details.cshtml – Chi tiết sản phẩm
```text
@model Sanpham

<h2>@Model.Tensp</h2>

<img src="@Model.Hinhanh" width="300" />
<p>Giá: @Model.Gia</p>
<p>@Model.Mota</p>

<a href="/ShoppingCart/AddToCart/@Model.Masp" class="btn btn-success">
    Thêm vào giỏ
</a>
```
- VIEWS/Blog (BLOG)

Index.cshtml
```text
@model IEnumerable<Blog>

<h2>Bài viết</h2>

@foreach (var item in Model)
{
    <div>
        <h3>
            <a href="/Blog/Details/@item.Id">@item.Tieude</a>
        </h3>
        <p>@item.Noidung.Substring(0, 100)...</p>
    </div>
}
```
Details.cshtml
```text
@model Blog

<h2>@Model.Tieude</h2>

<p>@Model.Noidung</p>
```
- VIEWS/TrackingOrder (TRACKING)

Index.cshtml
```text
@model IEnumerable<Donhang>
<h2>Đơn hàng của bạn</h2>

<table class="table">
    <tr>
        <th>Mã đơn</th>
        <th>Ngày</th>
        <th>Trạng thái</th>
        <th></th>
    </tr>

@foreach (var item in Model)
{
    <tr>
        <td>@item.Madonhang</td>
        <td>@item.Ngaydat</td>
        <td>@item.Trangthai</td>
        <td>
            <a href="/TrackingOrder/Details/@item.Madonhang">Xem</a>
        </td>
    </tr>
}
</table>
```

Details.cshtml
```text
@model IEnumerable<Chitietphieumua>

<h2>Chi tiết đơn hàng</h2>

<table class="table">
    <tr>
        <th>Sản phẩm</th>
        <th>Số lượng</th>
        <th>Giá</th>
    </tr>

@foreach (var item in Model)
{
    <tr>
        <td>@item.Masp</td>
        <td>@item.Soluong</td>
        <td>@item.Gia</td>
    </tr>
}
</table>
```

- VIEWS/Account (ACCOUNT)

Login.cshtml
```text
@model Taikhoan

<h2>Đăng nhập</h2>

<form method="post">
    <label>Tài khoản</label>
    <input asp-for="Username" class="form-control" />

    <label>Mật khẩu</label>
    <input asp-for="Password" type="password" class="form-control" />

    <button type="submit" class="btn btn-primary">Đăng nhập</button>
</form>

```

Register.cshtml
```text
@model Taikhoan

<h2>Đăng ký</h2>

<form method="post">
    <input asp-for="Username" placeholder="Username" />
    <input asp-for="Password" type="password" placeholder="Password" />
    <input asp-for="Email" placeholder="Email" />

    <button type="submit">Đăng ký</button>
</form>
```

- VIEWS/ShoppingCart

Index.cshtml
```text
@model List<Sanpham>

<h2>Giỏ hàng</h2>

<table class="table">
@foreach (var item in Model)
{
    <tr>
        <td>@item.Tensp</td>
        <td>@item.Gia</td>
    </tr>
}
</table>

<a href="/TrackingOrder/Create" class="btn btn-success">Thanh toán</a>
```

_ViewImports.cshtml
```text
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

