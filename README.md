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

#Shared (DÙNG CHUNG)
- _Layout.cshtml → layout chính của website  
- _ViewImports.cshtml → import TagHelper và namespace  
- _ViewStart.cshtml → cấu hình layout mặc định cho toàn bộ Views  
- _Error.cshtml → trang hiển thị lỗi hệ thống  

## Dựa vào cấu trúc cây thư mục của Template tìm hiểu cách viết code cho từng mục trong MODELS; CONTROLLERS; VIEWS tạo ra các mục Shopping; Blog; Tracking; Account

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
- File: Taikhoan.cs

using System.ComponentModel.DataAnnotations;

public class Taikhoan
{
[Key]
public int Id { get; set; }
```text
public string Username { get; set; }

public string Password { get; set; }

public string Email { get; set; }

public List<Phieumua> Phieumuas { get; set; }
```
- File: Phieumua.cs

using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

public class Phieumua
{
[Key]
public int Maphieumua { get; set; }

```text
public DateTime Ngaydat { get; set; }

public string Trangthai { get; set; }

public int TaikhoanId { get; set; }

[ForeignKey("TaikhoanId")]
public Taikhoan Taikhoan { get; set; }

public List<Chitietphieumua> Chitietphieumuas { get; set; }
```
- File: Chitietphieumua.cs

using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

public class Chitietphieumua
{
[Key]
public int Id { get; set; }
```text
public int Maphieumua { get; set; }

public int Masp { get; set; }

public int Soluong { get; set; }

public decimal Gia { get; set; }

[ForeignKey("Maphieumua")]
public Phieumua Phieumua { get; set; }

[ForeignKey("Masp")]
public Sanpham Sanpham { get; set; }
```
- File: ShoesDbContext.cs

using Microsoft.EntityFrameworkCore;

public class ShoesDbContext : DbContext
{
public ShoesDbContext(DbContextOptions<ShoesDbContext> options) : base(options) { }
```text
public DbSet<Sanpham> Sanphams { get; set; }
public DbSet<Blog> Blogs { get; set; }
public DbSet<Taikhoan> Taikhoans { get; set; }
public DbSet<Phieumua> Phieumuas { get; set; }
public DbSet<Chitietphieumua> Chitietphieumuas { get; set; }
```

#CONTROLLERS

- SanPhamController.cs

using Microsoft.AspNetCore.Mvc;
using System.Linq;

public class SanPhamController : Controller
{
private readonly ShoesDbContext _context;
```text
public SanPhamController(ShoesDbContext context)
{
    _context = context;
}

public IActionResult Index()
{
    return View(_context.Sanphams.ToList());
}

public IActionResult Details(int id)
{
    var sp = _context.Sanphams.Find(id);
    if (sp == null) return NotFound();
    return View(sp);
}

public IActionResult Create()
{
    return View();
}

[HttpPost]
public IActionResult Create(Sanpham sp)
{
    if (ModelState.IsValid)
    {
        _context.Add(sp);
        _context.SaveChanges();
        return RedirectToAction("Index");
    }
    return View(sp);
}

public IActionResult Edit(int id)
{
    var sp = _context.Sanphams.Find(id);
    return View(sp);
}

[HttpPost]
public IActionResult Edit(Sanpham sp)
{
    _context.Update(sp);
    _context.SaveChanges();
    return RedirectToAction("Index");
}

public IActionResult Delete(int id)
{
    var sp = _context.Sanphams.Find(id);
    return View(sp);
}

[HttpPost, ActionName("Delete")]
public IActionResult DeleteConfirmed(int id)
{
    var sp = _context.Sanphams.Find(id);
    _context.Remove(sp);
    _context.SaveChanges();
    return RedirectToAction("Index");
}
```
- BlogController.cs

using Microsoft.AspNetCore.Mvc;
using System.Linq;

public class BlogController : Controller
{
private readonly ShoesDbContext _context;
```text
public BlogController(ShoesDbContext context)
{
    _context = context;
}

public IActionResult Index()
{
    return View(_context.Blogs.ToList());
}

public IActionResult Details(int id)
{
    return View(_context.Blogs.Find(id));
}

public IActionResult Create()
{
    return View();
}

[HttpPost]
public IActionResult Create(Blog b)
{
    _context.Add(b);
    _context.SaveChanges();
    return RedirectToAction("Index");
}

public IActionResult Edit(int id)
{
    return View(_context.Blogs.Find(id));
}

[HttpPost]
public IActionResult Edit(Blog b)
{
    _context.Update(b);
    _context.SaveChanges();
    return RedirectToAction("Index");
}

public IActionResult Delete(int id)
{
    return View(_context.Blogs.Find(id));
}

[HttpPost, ActionName("Delete")]
public IActionResult DeleteConfirmed(int id)
{
    var b = _context.Blogs.Find(id);
    _context.Remove(b);
    _context.SaveChanges();
    return RedirectToAction("Index");
}
```
- TrackingOrderController.cs

using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using System.Linq;

public class TrackingOrderController : Controller
{
private readonly ShoesDbContext _context;
```text
public TrackingOrderController(ShoesDbContext context)
{
    _context = context;
}

public IActionResult Index()
{
    return View(_context.Phieumuas.ToList());
}

public IActionResult Details(int id)
{
    var list = _context.Chitietphieumuas
        .Include(x => x.Sanpham)
        .Where(x => x.Maphieumua == id)
        .ToList();

    return View(list);
}

public IActionResult Create()
{
    return View();
}

[HttpPost]
public IActionResult Create(Phieumua pm)
{
    _context.Add(pm);
    _context.SaveChanges();
    return RedirectToAction("Index");
}

public IActionResult Edit(int id)
{
    return View(_context.Phieumuas.Find(id));
}

[HttpPost]
public IActionResult Edit(Phieumua pm)
{
    _context.Update(pm);
    _context.SaveChanges();
    return RedirectToAction("Index");
}

public IActionResult Delete(int id)
{
    return View(_context.Phieumuas.Find(id));
}

[HttpPost, ActionName("Delete")]
public IActionResult DeleteConfirmed(int id)
{
    var pm = _context.Phieumuas.Find(id);
    _context.Remove(pm);
    _context.SaveChanges();
    return RedirectToAction("Index");
}
```
- AccountController.cs

using Microsoft.AspNetCore.Mvc;
using System.Linq;

public class AccountController : Controller
{
private readonly ShoesDbContext _context;
```text
public AccountController(ShoesDbContext context)
{
    _context = context;
}

public IActionResult Register()
{
    return View();
}

[HttpPost]
public IActionResult Register(Taikhoan tk)
{
    _context.Add(tk);
    _context.SaveChanges();
    return RedirectToAction("Login");
}

public IActionResult Login()
{
    return View();
}

[HttpPost]
public IActionResult Login(string username, string password)
{
    var user = _context.Taikhoans
        .FirstOrDefault(x => x.Username == username && x.Password == password);

    if (user != null)
    {
        HttpContext.Session.SetString("user", user.Username);
        return RedirectToAction("Index", "SanPham");
    }

    ViewBag.Error = "Sai tài khoản hoặc mật khẩu";
    return View();
}

public IActionResult Logout()
{
    HttpContext.Session.Clear();
    return RedirectToAction("Login");
}
```
- ShoppingCartController.cs

using Microsoft.AspNetCore.Mvc;
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;

public class ShoppingCartController : Controller
{
private readonly ShoesDbContext _context;
```text
public ShoppingCartController(ShoesDbContext context)
{
    _context = context;
}

public IActionResult Index()
{
    return View(GetCart());
}

public IActionResult AddToCart(int id)
{
    var cart = GetCart();
    var sp = _context.Sanphams.Find(id);
    cart.Add(sp);
    SaveCart(cart);
    return RedirectToAction("Index");
}

public IActionResult Remove(int id)
{
    var cart = GetCart();
    var item = cart.FirstOrDefault(x => x.Masp == id);
    if (item != null)
    {
        cart.Remove(item);
        SaveCart(cart);
    }
    return RedirectToAction("Index");
}

private List<Sanpham> GetCart()
{
    var data = HttpContext.Session.GetString("cart");
    if (data == null) return new List<Sanpham>();
    return JsonConvert.DeserializeObject<List<Sanpham>>(data);
}

private void SaveCart(List<Sanpham> cart)
{
    HttpContext.Session.SetString("cart", JsonConvert.SerializeObject(cart));
}
```

#VIEWS

- Shared/_Layout.cshtml
```text
<nav> <a href="/SanPham">Shopping</a> | <a href="/Blog">Blog</a> | <a href="/TrackingOrder">Tracking</a> | <a href="/Account/Login">Account</a> </nav>

@RenderBody()
```
- SanPham/Index.cshtml
```text
@model IEnumerable<Sanpham>

@foreach (var item in Model)
{
<h3>@item.Tensp</h3>
<p>@item.Gia</p>
<a href="/SanPham/Details/@item.Masp">Xem</a>
}
```
- SanPham/Details.cshtml
```text
@model Sanpham

<h2>@Model.Tensp</h2> <p>@Model.Gia</p> <a href="/ShoppingCart/AddToCart/@Model.Masp">Mua</a>
```
- TrackingOrder/Index.cshtml
```text
@model IEnumerable<Phieumua>

@foreach (var item in Model)
{
<p>@item.Maphieumua - @item.Trangthai</p>
<a href="/TrackingOrder/Details/@item.Maphieumua">Chi tiết</a>
}

```
- TrackingOrder/Details.cshtml
```text
@model IEnumerable<Chitietphieumua>

@foreach (var item in Model)
{
<p>@item.Sanpham.Tensp - @item.Soluong</p>
}
```
- Account/Login.cshtml
```text
<form method="post"> <input name="username" /> <input name="password" type="password" /> <button>Login</button> </form>
```
- Cấu trúc tổng thể:

Views
│

├── SanPham

├── Blog

├── TrackingOrder

├── Account

├── ShoppingCart

└── Shared

## Trong SHOPPING của template sổ ra 4 mục All, Basketball, Football, Jogging, tìm hiểu cách code để có hiệu ứng tương tự

- Cập nhật: MODEL -> Sanpham.cs

using System.ComponentModel.DataAnnotations;
public class Sanpham

```text
{
    [Key]
    public int Masp { get; set; }

    public string Tensp { get; set; }

    public decimal Gia { get; set; }

    public string Hinhanh { get; set; }

    public string Mota { get; set; }

    public string Category { get; set; }
}
```
- Cập nhật: SanPhamController.cs

using Microsoft.AspNetCore.Mvc;
using System.Linq;
public class SanPhamController : Controller

```text
{
    private readonly ShoesDbContext _context;

    public SanPhamController(ShoesDbContext context)
    {
        _context = context;
    }

    public IActionResult Index(string category)
    {
        var products = _context.Sanphams.AsQueryable();

        if (!string.IsNullOrEmpty(category))
        {
            products = products.Where(x => x.Category == category);
        }

        return View(products.ToList());
    }

    public IActionResult Details(int id)
    {
        var sp = _context.Sanphams.Find(id);

        if (sp == null)
        {
            return NotFound();
        }

        return View(sp);
    }
}
```
- Cập nhật: VIEWS/Shared/_Layout.cshtml

Thêm dropdown Shopping:

using Microsoft.AspNetCore.Mvc;
using System.Linq;
public class SanPhamController : Controller

```text
{
    private readonly ShoesDbContext _context;

    public SanPhamController(ShoesDbContext context)
    {
        _context = context;
    }

    public IActionResult Index(string category)
    {
        var products = _context.Sanphams.AsQueryable();

        if (!string.IsNullOrEmpty(category))
        {
            products = products.Where(x => x.Category == category);
        }

        return View(products.ToList());
    }

    public IActionResult Details(int id)
    {
        var sp = _context.Sanphams.Find(id);

        if (sp == null)
        {
            return NotFound();
        }

        return View(sp);
    }
}
```
- Cập nhật: VIEWS/SanPham/Index.cshtml

Hiển thị sản phẩm theo category:

```text
@model IEnumerable<Sanpham>

<h2>Products</h2>

<div class="row">

@foreach (var item in Model)
{
    <div class="col-md-3">

        <div class="card">

            <img src="@item.Hinhanh"
                 class="card-img-top">

            <div class="card-body">

                <h5>@item.Tensp</h5>

                <p>@item.Gia VNĐ</p>

                <p>@item.Category</p>

                <a href="/SanPham/Details/@item.Masp"
                   class="btn btn-primary">
                    View
                </a>

            </div>
        </div>

    </div>
}

</div>
```
- CÁCH HOẠT ĐỘNG

/SanPham
→ hiện ALL

/SanPham?category=Basketball
→ chỉ hiện Basketball

/SanPham?category=Football
→ chỉ hiện Football

/SanPham?category=Jogging
→ chỉ hiện Jogging

## Trong SHOPPING -> All của template hiển thị trang All, tìm hiểu cách code để có content trong trang All tương tự

```text
@model IEnumerable<Sanpham>

@{
    ViewData["Title"] = "Shop Category";
}

<section class="banner-area">

    <div class="container">

        <div class="row align-items-center justify-content-between">

            <div class="col-lg-12 banner-content text-center">

                <h1 class="text-white">
                    Shop Category page
                </h1>

                <p class="text-white">
                    Home → Shopping → All
                </p>

            </div>

        </div>

    </div>

</section>

<section class="category-area">

    <div class="container">

        <div class="row">

            <!-- LEFT FILTER -->

            <div class="col-lg-3">

                <div class="filter-bar">

                    <h4>Filter Products</h4>

                    <hr />

                    <h5>COLORS</h5>

                    <ul class="list-unstyled">

                        <li>Black</li>
                        <li>Blue</li>
                        <li>SaddleBrown</li>
                        <li>GhostWhite</li>
                        <li>Red</li>
                        <li>Pink</li>
                        <li>Orange</li>
                        <li>Yellow</li>

                    </ul>

                    <hr />

                    <h5>PRICE</h5>

                    <p>Price from: 0đ - 10.000.000đ</p>

                </div>

            </div>

            <!-- RIGHT PRODUCTS -->

            <div class="col-lg-9">

                <!-- SORT BAR -->

                <div class="sort-bar mb-4">

                    <form method="get">

                        <select name="sortPrice"
                                class="form-select w-25"
                                onchange="this.form.submit()">

                            <option value="">
                                Sort Price
                            </option>

                            <option value="asc">
                                Low To High
                            </option>

                            <option value="desc">
                                High To Low
                            </option>

                        </select>

                    </form>

                </div>

                <!-- PRODUCT LIST -->

                <div class="row">

                    @foreach (var item in Model)
                    {
                        <div class="col-lg-4 col-md-6 mb-5">

                            <div class="product-card">

                                <img src="@item.Hinhanh"
                                     class="img-fluid product-image" />

                                <div class="product-info">

                                    <h5>
                                        @item.Tensp
                                    </h5>

                                    <p>
                                        @item.Category
                                    </p>

                                    <h6>
                                        @item.Gia.ToString("N0") đ
                                    </h6>

                                    <a href="/SanPham/Details/@item.Masp"
                                       class="btn btn-dark mt-2">

                                        View Product

                                    </a>

                                </div>

                            </div>

                        </div>
                    }

                </div>

            </div>

        </div>

    </div>

</section>

<style>

    .banner-area{
        background:#f97316;
        padding:70px 0;
    }

    .banner-content h1{
        font-size:45px;
        font-weight:bold;
    }

    .category-area{
        padding:60px 0;
        background:#f5f5f5;
    }

    .filter-bar{
        background:white;
        padding:25px;
        border-radius:10px;
    }

    .filter-bar h4{
        background:#7f8db0;
        color:white;
        padding:15px;
        margin-bottom:25px;
    }

    .sort-bar{
        background:#7f8db0;
        padding:20px;
        border-radius:10px;
    }

    .product-card{
        background:white;
        padding:20px;
        border-radius:10px;
        transition:0.3s;
    }

    .product-card:hover{
        transform:translateY(-5px);
    }

    .product-image{
        width:100%;
        height:250px;
        object-fit:cover;
    }

    .product-info{
        margin-top:20px;
    }

</style>
```
- Controller tương ứng:

```text
public IActionResult Index(string category, string sortPrice)
{
    var products = _context.Sanphams.AsQueryable();

    // FILTER CATEGORY

    if (!string.IsNullOrEmpty(category))
    {
        products = products.Where(x => x.Category == category);
    }

    // SORT PRICE

    if (sortPrice == "asc")
    {
        products = products.OrderBy(x => x.Gia);
    }
    else if (sortPrice == "desc")
    {
        products = products.OrderByDescending(x => x.Gia);
    }

    return View(products.ToList());
}
```
- Model Sanpham.cs:
```text
public class Sanpham
{
    public int Masp { get; set; }

    public string Tensp { get; set; }

    public decimal Gia { get; set; }

    public string Hinhanh { get; set; }

    public string Mota { get; set; }

    public string Category { get; set; }
}
```
- Dropdown menu trong _Layout.cshtml:

```text
<li class="nav-item dropdown">

    <a class="nav-link dropdown-toggle"
       href="#"
       data-bs-toggle="dropdown">

        SHOPPING

    </a>

    <ul class="dropdown-menu">

        <li>
            <a class="dropdown-item"
               href="/SanPham">

                ALL

            </a>
        </li>

        <li>
            <a class="dropdown-item"
               href="/SanPham?category=Basketball">

                BASKETBALL

            </a>
        </li>

        <li>
            <a class="dropdown-item"
               href="/SanPham?category=Football">

                FOOTBALL

            </a>
        </li>

        <li>
            <a class="dropdown-item"
               href="/SanPham?category=Jogging">

                JOGGING

            </a>
        </li>

    </ul>

</li>
```

## Từ trang All tạo thêm các trang Basketball; Football; Jogging dựa vào template tìm hiểu cách code để có content tương tự.

```text
@model IEnumerable<Sanpham>

@{
    ViewData["Title"] = "Basketball";
}

<section class="banner-area">

    <div class="container">

        <div class="row align-items-center justify-content-between">

            <div class="col-lg-12 banner-content text-center">

                <h1 class="text-white">
                    Basketball Shoes
                </h1>

                <p class="text-white">
                    Home → Shopping → Basketball
                </p>

            </div>

        </div>

    </div>

</section>

<section class="category-area">

    <div class="container">

        <div class="row">

            <!-- FILTER -->

            <div class="col-lg-3">

                <div class="filter-bar">

                    <h4>Filter Products</h4>

                    <h5>Category</h5>

                    <ul class="list-unstyled">

                        <li>
                            <a href="/SanPham">
                                All
                            </a>
                        </li>

                        <li>
                            <a href="/SanPham/Basketball">
                                Basketball
                            </a>
                        </li>

                        <li>
                            <a href="/SanPham/Football">
                                Football
                            </a>
                        </li>

                        <li>
                            <a href="/SanPham/Jogging">
                                Jogging
                            </a>
                        </li>

                    </ul>

                </div>

            </div>

            <!-- PRODUCTS -->

            <div class="col-lg-9">

                <div class="row">

                    @foreach (var item in Model)
                    {
                        <div class="col-lg-4 mb-5">

                            <div class="product-card">

                                <img src="@item.Hinhanh"
                                     class="img-fluid product-image" />

                                <div class="product-info">

                                    <h5>@item.Tensp</h5>

                                    <p>@item.Category</p>

                                    <h6>
                                        @item.Gia.ToString("N0") đ
                                    </h6>

                                    <a href="/SanPham/Details/@item.Masp"
                                       class="btn btn-dark">

                                        View Product

                                    </a>

                                </div>

                            </div>

                        </div>
                    }

                </div>

            </div>

        </div>

    </div>

</section>
```
- Football.cshtml

```text
@model IEnumerable<Sanpham>

@{
    ViewData["Title"] = "Football";
}

<section class="banner-area">

    <div class="container">

        <div class="row">

            <div class="col-lg-12 text-center">

                <h1 class="text-white">
                    Football Shoes
                </h1>

                <p class="text-white">
                    Home → Shopping → Football
                </p>

            </div>

        </div>

    </div>

</section>

<section class="category-area">

    <div class="container">

        <div class="row">

            @foreach (var item in Model)
            {
                <div class="col-lg-4 mb-5">

                    <div class="product-card">

                        <img src="@item.Hinhanh"
                             class="img-fluid product-image" />

                        <h5>@item.Tensp</h5>

                        <p>@item.Category</p>

                        <h6>
                            @item.Gia.ToString("N0") đ
                        </h6>

                        <a href="/SanPham/Details/@item.Masp"
                           class="btn btn-dark">

                            View Product

                        </a>

                    </div>

                </div>
            }

        </div>

    </div>

</section>
```
- Jogging.cshtml

```text
@model IEnumerable<Sanpham>

@{
    ViewData["Title"] = "Jogging";
}

<section class="banner-area">

    <div class="container">

        <div class="row">

            <div class="col-lg-12 text-center">

                <h1 class="text-white">
                    Jogging Shoes
                </h1>

                <p class="text-white">
                    Home → Shopping → Jogging
                </p>

            </div>

        </div>

    </div>

</section>

<section class="category-area">

    <div class="container">

        <div class="row">

            @foreach (var item in Model)
            {
                <div class="col-lg-4 mb-5">

                    <div class="product-card">

                        <img src="@item.Hinhanh"
                             class="img-fluid product-image" />

                        <h5>@item.Tensp</h5>

                        <p>@item.Category</p>

                        <h6>
                            @item.Gia.ToString("N0") đ
                        </h6>

                        <a href="/SanPham/Details/@item.Masp"
                           class="btn btn-dark">

                            View Product

                        </a>

                    </div>

                </div>
            }

        </div>

    </div>

</section>
```
- THÊM ACTION TRONG SanPhamController.cs

```text
public IActionResult Basketball()
{
    var products = _context.Sanphams
        .Where(x => x.Category == "Basketball")
        .ToList();

    return View(products);
}

public IActionResult Football()
{
    var products = _context.Sanphams
        .Where(x => x.Category == "Football")
        .ToList();

    return View(products);
}

public IActionResult Jogging()
{
    var products = _context.Sanphams
        .Where(x => x.Category == "Jogging")
        .ToList();

    return View(products);
}
```
- CẬP NHẬT DROPDOWN MENU

```text
<ul class="dropdown-menu">

    <li>
        <a class="dropdown-item"
           href="/SanPham">
            ALL
        </a>
    </li>

    <li>
        <a class="dropdown-item"
           href="/SanPham/Basketball">
            BASKETBALL
        </a>
    </li>

    <li>
        <a class="dropdown-item"
           href="/SanPham/Football">
            FOOTBALL
        </a>
    </li>

    <li>
        <a class="dropdown-item"
           href="/SanPham/Jogging">
            JOGGING
        </a>
    </li>

</ul>
```

## Trong các trang All, Basketball; Football; Jogging dựa vào template đều có sản phẩm và trang của từng sản phẩm tìm hiểu cách code để có content tương tự cho 1 trang sản phẩm mẫu

- Thêm file mới: Views/SanPham/Details.cshtml
```text
@model Sanpham

@{
    ViewData["Title"] = "Product Details";
}

<section class="banner-area">

    <div class="container">

        <div class="row">

            <div class="col-lg-12 text-center">

                <h1 class="text-white">
                    Product Details Page
                </h1>

                <p class="text-white">
                    Home → Shopping → Product Details
                </p>

            </div>

        </div>

    </div>

</section>

<section class="product-details-area">

    <div class="container">

        <div class="row align-items-center">

            <!-- PRODUCT IMAGE -->

            <div class="col-lg-6">

                <div class="product-image-box">

                    <img src="@Model.Hinhanh"
                         class="img-fluid main-product-image" />

                </div>

            </div>

            <!-- PRODUCT INFO -->

            <div class="col-lg-6">

                <div class="product-info">

                    <h2>
                        @Model.Tensp
                    </h2>

                    <h3 class="price">
                        @Model.Gia.ToString("N0") đ
                    </h3>

                    <p class="mt-4">
                        @Model.Mota
                    </p>

                    <!-- SIZE -->

                    <div class="mt-4">

                        <h5>
                            Table Size
                        </h5>

                        <div class="size-group">

                            <button class="size-btn">38</button>
                            <button class="size-btn">39</button>
                            <button class="size-btn">40</button>
                            <button class="size-btn">41</button>

                        </div>

                    </div>

                    <!-- BUTTON -->

                    <div class="mt-4">

                        <a href="/ShoppingCart/AddToCart/@Model.Masp"
                           class="btn add-cart-btn">

                            ADD TO CART

                        </a>

                    </div>

                </div>

            </div>

        </div>

        <!-- TABS -->

        <div class="product-tabs mt-5">

            <ul class="nav nav-tabs">

                <li class="nav-item">

                    <button class="nav-link active"
                            data-bs-toggle="tab"
                            data-bs-target="#description">

                        Description

                    </button>

                </li>

                <li class="nav-item">

                    <button class="nav-link"
                            data-bs-toggle="tab"
                            data-bs-target="#reviews">

                        Reviews

                    </button>

                </li>

                <li class="nav-item">

                    <button class="nav-link"
                            data-bs-toggle="tab"
                            data-bs-target="#size">

                        Size Table

                    </button>

                </li>

            </ul>

            <div class="tab-content p-4 bg-white">

                <!-- DESCRIPTION -->

                <div class="tab-pane fade show active"
                     id="description">

                    <p>
                        @Model.Mota
                    </p>

                </div>

                <!-- REVIEWS -->

                <div class="tab-pane fade"
                     id="reviews">

                    <h4>
                        Based on 0 Reviews
                    </h4>

                    <p>
                        Add a Review
                    </p>

                </div>

                <!-- SIZE -->

                <div class="tab-pane fade"
                     id="size">

                    <table class="table">

                        <tr>
                            <th>EU</th>
                            <th>US</th>
                        </tr>

                        <tr>
                            <td>38</td>
                            <td>6</td>
                        </tr>

                        <tr>
                            <td>39</td>
                            <td>7</td>
                        </tr>

                        <tr>
                            <td>40</td>
                            <td>8</td>
                        </tr>

                    </table>

                </div>

            </div>

        </div>

        <!-- NEW PRODUCTS -->

        <section class="new-products mt-5">

            <h2 class="text-center mb-5">
                NEW PRODUCTS
            </h2>

            <div class="row">

                <div class="col-lg-3">

                    <div class="new-product-card">

                        <img src="/images/product/p1.jpg"
                             class="img-fluid" />

                        <h6>
                            DURAMO SL 2.0
                        </h6>

                        <p>
                            1,900,000đ
                        </p>

                    </div>

                </div>

                <div class="col-lg-3">

                    <div class="new-product-card">

                        <img src="/images/product/p2.jpg"
                             class="img-fluid" />

                        <h6>
                            SUPER FAST ELITE
                        </h6>

                        <p>
                            2,900,000đ
                        </p>

                    </div>

                </div>

                <div class="col-lg-3">

                    <div class="new-product-card">

                        <img src="/images/product/p3.jpg"
                             class="img-fluid" />

                        <h6>
                            BLACK EDITION
                        </h6>

                        <p>
                            2,800,000đ
                        </p>

                    </div>

                </div>

                <div class="col-lg-3">

                    <div class="new-product-card">

                        <img src="/images/product/p4.jpg"
                             class="img-fluid" />

                        <h6>
                            ELITE FAST
                        </h6>

                        <p>
                            5,900,000đ
                        </p>

                    </div>

                </div>

            </div>

        </section>

    </div>

</section>

<style>

    .banner-area{
        background:#f97316;
        padding:70px 0;
    }

    .banner-area h1{
        font-size:40px;
        font-weight:bold;
    }

    .product-details-area{
        background:#f5f5f5;
        padding:80px 0;
    }

    .product-image-box{
        background:#e9ecef;
        padding:40px;
    }

    .main-product-image{
        width:100%;
    }

    .product-info h2{
        font-size:30px;
        font-weight:bold;
    }

    .price{
        color:#f97316;
        margin-top:10px;
    }

    .size-group{
        display:flex;
        gap:10px;
    }

    .size-btn{
        border:1px solid #ddd;
        background:white;
        width:50px;
        height:40px;
    }

    .add-cart-btn{
        background:#f97316;
        color:white;
        padding:12px 30px;
        border-radius:5px;
    }

    .product-tabs{
        margin-top:80px;
    }

    .new-product-card{
        background:white;
        padding:15px;
        text-align:center;
    }

</style>
```
- Cập nhật: Thêm action trong SanPhamController.cs

```text
public IActionResult Details(int id)
{
    var sp = _context.Sanphams.Find(id);

    if (sp == null)
    {
        return NotFound();
    }

    return View(sp);
}
```

- Cập nhật: Views/SanPham/Index.cshtml, tạo nút View Product để chuyển sang trang chi tiết

```text
<a href="/SanPham/Details/@item.Masp"
   class="btn btn-dark">

    View Product

</a>
```

## Dựa vào trang BLOG của template tìm hiểu cách code để có content tương tự.

```text
@model IEnumerable<Blog>

@{
    ViewData["Title"] = "Blog";
}

<section class="banner-area">

    <div class="container">

        <div class="row">

            <div class="col-lg-12 text-center">

                <h1 class="text-white">
                    Blog Page
                </h1>

                <p class="text-white">
                    Home → Blog
                </p>

            </div>

        </div>

    </div>

</section>

<!-- BLOG CATEGORY -->

<section class="blog-category-area">

    <div class="container">

        <div class="row">

            <div class="col-lg-4">

                <div class="category-card">

                    <img src="/images/blog/social.jpg"
                         class="img-fluid category-image" />

                    <div class="category-overlay">

                        <h4>SOCIAL LIFE</h4>

                        <p>Enjoy your social life together</p>

                    </div>

                </div>

            </div>

            <div class="col-lg-4">

                <div class="category-card">

                    <img src="/images/blog/politics.jpg"
                         class="img-fluid category-image" />

                    <div class="category-overlay">

                        <h4>POLITICS</h4>

                        <p>Be a part of politics</p>

                    </div>

                </div>

            </div>

            <div class="col-lg-4">

                <div class="category-card">

                    <img src="/images/blog/food.jpg"
                         class="img-fluid category-image" />

                    <div class="category-overlay">

                        <h4>FOOD</h4>

                        <p>Let the food be finished</p>

                    </div>

                </div>

            </div>

        </div>

    </div>

</section>

<!-- BLOG LIST -->

<section class="blog-list-area">

    <div class="container">

        <h2 class="text-center mb-5">
            Blog List
        </h2>

        <div class="row">

            @foreach (var item in Model)
            {
                <div class="col-lg-4 col-md-6 mb-5">

                    <div class="blog-card">

                        <img src="@item.Hinhanh"
                             class="img-fluid blog-image" />

                        <div class="blog-content">

                            <h5>
                                @item.Tieude
                            </h5>

                            <p>
                                @item.Noidung
                            </p>

                            <a href="/Blog/Details/@item.Id"
                               class="btn btn-outline-primary">

                                View More

                            </a>

                        </div>

                    </div>

                </div>
            }

        </div>

    </div>

</section>

<style>

    .banner-area{
        background:#f97316;
        padding:70px 0;
    }

    .banner-area h1{
        font-size:45px;
        font-weight:bold;
    }

    .blog-category-area{
        padding:60px 0;
        background:#f5f5f5;
    }

    .category-card{
        position:relative;
        overflow:hidden;
    }

    .category-image{
        width:100%;
        height:180px;
        object-fit:cover;
    }

    .category-overlay{
        position:absolute;
        top:0;
        left:0;
        width:100%;
        height:100%;
        background:rgba(0,0,0,0.5);
        color:white;
        display:flex;
        flex-direction:column;
        justify-content:center;
        align-items:center;
    }

    .blog-list-area{
        padding:70px 0;
        background:#f5f5f5;
    }

    .blog-card{
        background:white;
        border:1px solid #ddd;
        transition:0.3s;
    }

    .blog-card:hover{
        transform:translateY(-5px);
    }

    .blog-image{
        width:100%;
        height:250px;
        object-fit:cover;
    }

    .blog-content{
        padding:20px;
        text-align:center;
    }

</style>
```
- BlogController.cs

```text
using Microsoft.AspNetCore.Mvc;
using System.Linq;

public class BlogController : Controller
{
    private readonly ShoesDbContext _context;

    public BlogController(ShoesDbContext context)
    {
        _context = context;
    }

    public IActionResult Index()
    {
        var blogs = _context.Blogs.ToList();

        return View(blogs);
    }

    public IActionResult Details(int id)
    {
        var blog = _context.Blogs.Find(id);

        if (blog == null)
        {
            return NotFound();
        }

        return View(blog);
    }

    public IActionResult Create()
    {
        return View();
    }

    [HttpPost]
    public IActionResult Create(Blog blog)
    {
        _context.Blogs.Add(blog);
        _context.SaveChanges();

        return RedirectToAction("Index");
    }

    public IActionResult Edit(int id)
    {
        var blog = _context.Blogs.Find(id);

        return View(blog);
    }

    [HttpPost]
    public IActionResult Edit(Blog blog)
    {
        _context.Blogs.Update(blog);
        _context.SaveChanges();

        return RedirectToAction("Index");
    }

    public IActionResult Delete(int id)
    {
        var blog = _context.Blogs.Find(id);

        return View(blog);
    }

    [HttpPost, ActionName("Delete")]
    public IActionResult DeleteConfirmed(int id)
    {
        var blog = _context.Blogs.Find(id);

        _context.Blogs.Remove(blog);

        _context.SaveChanges();

        return RedirectToAction("Index");
    }
}
```
- Blog.cs

```text
using System.ComponentModel.DataAnnotations;

public class Blog
{
    [Key]
    public int Id { get; set; }

    public string Tieude { get; set; }

    public string Noidung { get; set; }

    public string Hinhanh { get; set; }

    public DateTime Ngaydang { get; set; }
}
```
- Blog/Details.cshtml

```text
@model Blog

<section class="container mt-5">

    <img src="@Model.Hinhanh"
         class="img-fluid mb-4" />

    <h1>
        @Model.Tieude
    </h1>

    <p>
        @Model.Ngaydang.ToShortDateString()
    </p>

    <p>
        @Model.Noidung
    </p>

</section>
```

## Dựa vào trang TRACKING của template tìm hiểu cách code để có content tương tự.

```text
@model Donhang

@{
    ViewData["Title"] = "Order Tracking";
}

<section class="banner-area">

    <div class="container">

        <div class="row">

            <div class="col-lg-12 text-end">

                <h1 class="text-white">
                    Order Tracking
                </h1>

                <p class="text-white">
                    Home → Tracking Category
                </p>

            </div>

        </div>

    </div>

</section>

<section class="tracking-area">

    <div class="container">

        <div class="tracking-box">

            <p class="tracking-text">

                To track your order please enter your Order ID
                in the box below and press the "Track" button.
                This was given to you on your receipt and in the
                confirmation email you should have received.

            </p>

            <form asp-action="TrackOrder"
                  method="post">

                <input type="text"
                       name="orderId"
                       class="form-control tracking-input"
                       placeholder="Order ID" />

                <button type="submit"
                        class="btn tracking-btn">

                    TRACK ORDER

                </button>

            </form>

            @if (ViewBag.Message != null)
            {
                <div class="alert alert-danger mt-4">

                    @ViewBag.Message

                </div>
            }

            @if (Model != null)
            {
                <div class="tracking-result mt-5">

                    <h3>
                        Order Information
                    </h3>

                    <hr />

                    <p>
                        <strong>Order ID:</strong>
                        @Model.Madonhang
                    </p>

                    <p>
                        <strong>Customer:</strong>
                        @Model.Tenkhachhang
                    </p>

                    <p>
                        <strong>Date:</strong>
                        @Model.Ngaydathang.ToShortDateString()
                    </p>

                    <p>
                        <strong>Total:</strong>
                        @Model.Tongtien.ToString("N0") đ
                    </p>

                    <p>
                        <strong>Status:</strong>
                        @Model.Trangthai
                    </p>

                </div>
            }

        </div>

    </div>

</section>

<style>

    .banner-area{
        background:#f97316;
        padding:80px 0;
    }

    .banner-area h1{
        font-size:45px;
        font-weight:bold;
    }

    .tracking-area{
        background:#f5f5f5;
        padding:80px 0;
        min-height:600px;
    }

    .tracking-box{
        background:white;
        padding:40px;
        border-radius:10px;
    }

    .tracking-text{
        margin-bottom:30px;
        line-height:30px;
    }

    .tracking-input{
        height:55px;
        margin-bottom:25px;
    }

    .tracking-btn{
        background:#f97316;
        color:white;
        padding:12px 35px;
        border-radius:5px;
    }

    .tracking-result{
        background:#fafafa;
        padding:30px;
        border-radius:10px;
    }

</style>
```
- TrackingOrderController.cs

```text
using Microsoft.AspNetCore.Mvc;
using System.Linq;

public class TrackingOrderController : Controller
{
    private readonly ShoesDbContext _context;

    public TrackingOrderController(ShoesDbContext context)
    {
        _context = context;
    }

    // GET

    public IActionResult Index()
    {
        return View();
    }

    // POST

    [HttpPost]
    public IActionResult TrackOrder(string orderId)
    {
        if (string.IsNullOrEmpty(orderId))
        {
            ViewBag.Message = "Please enter Order ID";

            return View("Index");
        }

        var order = _context.Donhangs
            .FirstOrDefault(x => x.Madonhang.ToString() == orderId);

        if (order == null)
        {
            ViewBag.Message = "Order not found";

            return View("Index");
        }

        return View("Index", order);
    }
}
```
- Donhang.cs

```text
using System.ComponentModel.DataAnnotations;

public class Donhang
{
    [Key]
    public int Madonhang { get; set; }

    public string Tenkhachhang { get; set; }

    public DateTime Ngaydathang { get; set; }

    public decimal Tongtien { get; set; }

    public string Trangthai { get; set; }
}
```
- Thêm menu Tracking trong: Views/Shared/_Layout.cshtml

```text
<li class="nav-item">

    <a class="nav-link"
       href="/TrackingOrder">

        TRACKING

    </a>

</li>
```

## Trong ACCOUNT của template sổ ra 2 mục Login, Register , tìm hiểu cách code để có hiệu ứng tương tự.

- Trong _Layout.cshtml: tìm menu navigation và thêm

```text
<li class="account-menu">
    <a href="#">ACCOUNT</a>

    <ul class="account-dropdown">
        <li>
            <a asp-controller="Account" asp-action="Login">
                LOGIN
            </a>
        </li>

        <li>
            <a asp-controller="Account" asp-action="Register">
                REGISTER
            </a>
        </li>
    </ul>
</li>
```
- Cập nhật _Layout.cshtml: thêm đoạn code dưới vào trong menu

```text
<li class="account-menu">
    <a href="#">ACCOUNT</a>

    <ul class="account-dropdown">
        <li>
            <a asp-controller="Account" asp-action="Login">
                LOGIN
            </a>
        </li>

        <li>
            <a asp-controller="Account" asp-action="Register">
                REGISTER
            </a>
        </li>
    </ul>
</li>
```

- Cập nhật style.css: thêm cuối file

```text
.account-menu{
    position: relative;
    list-style: none;
}
```
- Thêm mới Register.cshtml

```text
Views/Account/Register.cshtml
```

## Dựa vào trang Login của template tìm hiểu cách code để có content tương tự.

- Thêm Views/Account/Login.cshtml

```text
@{
    ViewData["Title"] = "Login/Register";
}

<div class="login-page">

    <!-- Banner -->
    <div class="login-banner">
        <div class="login-banner-content">
            <h1>Login/Register</h1>

            <p>
                Home →
                <span>Login/Register</span>
            </p>
        </div>
    </div>

    <!-- Main -->
    <div class="login-container">

        <!-- Left -->
        <div class="login-left">

            <div class="overlay"></div>

            <div class="login-left-content">
                <h2>New to our website?</h2>

                <p>
                    There are advances being made in science and
                    technology everyday, and a good example of this is the
                </p>

                <a asp-controller="Account"
                   asp-action="Register"
                   class="create-account-btn">
                    CREATE AN ACCOUNT
                </a>
            </div>

        </div>

        <!-- Right -->
        <div class="login-right">

            <h3>LOG IN TO ENTER</h3>

            <form method="post">

                <input type="text"
                       name="username"
                       placeholder="Username" />

                <input type="password"
                       name="password"
                       placeholder="Password" />

                <button type="submit">
                    LOG IN
                </button>

            </form>

        </div>

    </div>

</div>
```

- Cập nhật wwwroot/css/style.css, thêm code vào cuối file

```text
/* LOGIN PAGE */

.login-page{
    background-color: #f5f5f5;
    min-height: 100vh;
}

/* Banner */

.login-banner{
    height: 170px;

    background: linear-gradient(to right, #ff6a00, #f9b233);

    display: flex;
    justify-content: flex-end;
    align-items: center;

    padding-right: 120px;

    color: white;
}

.login-banner-content h1{
    font-size: 40px;
    font-weight: bold;
    margin-bottom: 10px;
}

.login-banner-content p{
    font-size: 14px;
}

/* Main */

.login-container{
    width: 85%;

    margin: 70px auto;

    display: flex;

    background-color: white;
}

/* Left */

.login-left{
    width: 55%;

    position: relative;

    background-image: url('/images/login-bg.jpg');

    background-size: cover;
    background-position: center;

    min-height: 500px;

    display: flex;
    justify-content: center;
    align-items: center;
}

.overlay{
    position: absolute;

    top: 0;
    left: 0;

    width: 100%;
    height: 100%;

    background-color: rgba(0,0,0,0.4);
}

.login-left-content{
    position: relative;

    color: white;

    text-align: center;

    width: 70%;
}

.login-left-content h2{
    font-size: 35px;
    margin-bottom: 20px;
}

.login-left-content p{
    line-height: 1.8;
    margin-bottom: 30px;
}

.create-account-btn{
    background-color: #ff7b00;

    color: white;

    padding: 14px 35px;

    text-decoration: none;

    display: inline-block;
}

/* Right */

.login-right{
    width: 45%;

    display: flex;
    flex-direction: column;
    justify-content: center;

    padding: 60px;
}

.login-right h3{
    text-align: center;

    margin-bottom: 40px;

    letter-spacing: 2px;
}

.login-right form{
    display: flex;
    flex-direction: column;
}

.login-right input{
    height: 50px;

    margin-bottom: 20px;

    border: none;

    background-color: #edf1f7;

    padding-left: 15px;

    font-size: 15px;
}

.login-right button{
    height: 50px;

    border: none;

    background: linear-gradient(to right, #ff6a00, #f9b233);

    color: white;

    font-weight: bold;

    cursor: pointer;
}
```
- Cập nhật Controllers/AccountController.cs
Thêm action Login POST
```text
[HttpPost]
public IActionResult Login(string username, string password)
{
    var user = _context.Taikhoans
        .FirstOrDefault(x =>
            x.Tendangnhap == username &&
            x.Matkhau == password);

    if(user != null)
    {
        HttpContext.Session.SetString("User", user.Tendangnhap);

        return RedirectToAction("Index", "Home");
    }

    ViewBag.Error = "Sai tài khoản hoặc mật khẩu";

    return View();
}
```

- Cập nhật đầu file AccountController.cs

```text
using ShoesStore.Models;
using Microsoft.EntityFrameworkCore;
```

- Cập nhật trong AccountController
Thêm DbContext:

```text
private readonly ShoesDbContext _context;

public AccountController(ShoesDbContext context)
{
    _context = context;
}
```

- Thêm ảnh nền
```text
wwwroot/images/login-bg.jpg
```

## Dựa vào trang Register của template tìm hiểu cách code để có content tương tự.

- Cập nhật AccountController.cs

Thêm action Register POST
```text
[HttpPost]
public IActionResult Register(
    string username,
    string email,
    string password,
    string phone,
    string gender,
    DateTime birthday)
{
    var checkUser = _context.Taikhoans
        .FirstOrDefault(x => x.Tendangnhap == username);

    if(checkUser != null)
    {
        ViewBag.Error = "Username already exists";

        return View();
    }

    Taikhoan tk = new Taikhoan();

    tk.Tendangnhap = username;
    tk.Email = email;
    tk.Matkhau = password;
    tk.Sdt = phone;
    tk.Gioitinh = gender;
    tk.Ngaysinh = birthday;

    _context.Taikhoans.Add(tk);

    _context.SaveChanges();

    return RedirectToAction("Login");
}
```

- Cập nhật đầu file AccountController.cs

```text
using System;
using System.Linq;
```
- Yêu cầu database, Model Taikhoan.cs cần có:
```text
public string Email { get; set; }

public string Sdt { get; set; }

public string Gioitinh { get; set; }

public DateTime? Ngaysinh { get; set; }
```

## Dựa vào trang Shopping Cart của template và Database đã nhập trước đó, tìm hiểu cách code để có content tương tự.

- Thêm mới Views/ShoppingCart/Index.cshtml

```text
@{
    ViewData["Title"] = "Shopping Cart";
}

<div class="cart-page">

    <!-- Banner -->

    <div class="cart-banner">

        <div class="cart-banner-content">

            <h1>Shopping Cart</h1>

            <p>
                Home →
                <span>Cart</span>
            </p>

        </div>

    </div>

    <!-- Cart Content -->

    <div class="cart-container">

        <!-- Header -->

        <div class="cart-header">

            <div class="cart-product">
                Products
            </div>

            <div class="cart-quantity">
                Quantity
            </div>

            <div class="cart-total">
                Total cost
            </div>

        </div>

        <!-- Product -->

        <div class="cart-item">

            <!-- Image -->

            <div class="cart-product-info">

                <img src="/images/duramo.jpg" />

                <div>

                    <h3>Shoes DURAMO SL 2.0</h3>

                    <p>Color: Ghost White</p>

                    <p>Size: 38</p>

                </div>

            </div>

            <!-- Quantity -->

            <div class="cart-quantity-box">

                <button>-</button>

                <input type="text"
                       value="1" />

                <button>+</button>

            </div>

            <!-- Price -->

            <div class="cart-price">

                1,900,000₫

            </div>

            <!-- Delete -->

            <div class="cart-delete">

                <button>
                    🗑
                </button>

            </div>

        </div>

        <!-- Total -->

        <div class="cart-bill">

            <h3>Total Bill</h3>

            <span>1,900,000₫</span>

        </div>

        <!-- Buttons -->

        <div class="cart-buttons">

            <a asp-controller="SanPham"
               asp-action="SanPhamTheoLoai"
               class="continue-btn">

                CONTINUE SHOPPING

            </a>

            <a asp-controller="ShoppingCart"
               asp-action="Checkout"
               class="checkout-btn">

                CHECKOUT

            </a>

        </div>

    </div>

</div>
```

- Cập nhật wwwroot/css/style.css, thêm cuối file:

```text 
/* SHOPPING CART */

.cart-page{
    background-color: #f5f5f5;

    min-height: 100vh;
}

/* Banner */

.cart-banner{
    height: 170px;

    background: linear-gradient(to right,#ff6a00,#f9b233);

    display: flex;
    justify-content: flex-end;
    align-items: center;

    padding-right: 120px;

    color: white;
}

.cart-banner-content h1{
    font-size: 42px;

    margin-bottom: 10px;
}

/* Container */

.cart-container{
    width: 90%;

    margin: 60px auto;

    background-color: white;

    padding: 40px;
}

/* Header */

.cart-header{
    display: flex;

    padding-bottom: 20px;

    border-bottom: 1px solid #ddd;

    font-weight: bold;
}

.cart-product{
    width: 55%;
}

.cart-quantity{
    width: 20%;
}

.cart-total{
    width: 20%;
}

/* Item */

.cart-item{
    display: flex;

    align-items: center;

    padding: 30px 0;

    border-bottom: 1px solid #ddd;
}

/* Product */

.cart-product-info{
    width: 55%;

    display: flex;

    align-items: center;
}

.cart-product-info img{
    width: 120px;

    margin-right: 25px;
}

.cart-product-info h3{
    margin-bottom: 10px;
}

/* Quantity */

.cart-quantity-box{
    width: 20%;

    display: flex;

    align-items: center;
}

.cart-quantity-box button{
    width: 35px;
    height: 35px;

    border: none;

    background-color: white;

    font-size: 20px;

    cursor: pointer;
}

.cart-quantity-box input{
    width: 45px;
    height: 35px;

    text-align: center;

    border: 1px solid #ddd;
}

/* Price */

.cart-price{
    width: 20%;

    font-weight: bold;
}

/* Delete */

.cart-delete button{
    background-color: crimson;

    color: white;

    border: none;

    width: 35px;
    height: 35px;

    cursor: pointer;
}

/* Bill */

.cart-bill{
    display: flex;

    justify-content: flex-end;

    gap: 60px;

    padding: 30px 0;

    font-size: 22px;

    font-weight: bold;
}

/* Buttons */

.cart-buttons{
    display: flex;

    justify-content: flex-end;

    gap: 20px;
}

.continue-btn{
    background-color: #e9ecef;

    padding: 15px 30px;

    text-decoration: none;

    color: black;
}

.checkout-btn{
    background: linear-gradient(to right,#ff6a00,#f9b233);

    padding: 15px 30px;

    text-decoration: none;

    color: white;
}
``` 

- Cập nhật Controllers/ShoppingCartController.cs, thêm action:

```text 
public IActionResult Index()
{
    return View();
}
```
- Thêm ảnh sản phẩm mẫu, dùng ảnh đôi giày (tùy mẫu)

```text 
wwwroot/images/duramo.jpg
```text 