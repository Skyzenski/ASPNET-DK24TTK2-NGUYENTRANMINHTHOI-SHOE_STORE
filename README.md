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

- Cập nhật _-MODEL Sanpham.cs











