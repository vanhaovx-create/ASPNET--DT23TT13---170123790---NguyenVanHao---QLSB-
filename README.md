# ASPNET--DT23TT13---170123790---NguyenVanHao---QLSB-
Website quản lý sân bóng đá mini sử dụng ASP.NET Core MVC.
# THÔNG TIN TÀI KHOẢN QUẢN TRỊ VIÊN

## 🔑 Tài khoản Admin mặc định:
- **Email**: admin@footballmanager.vn
- **Mật khẩu**: Admin123!
- **Vai trò**: Administrator

## 📋 Quyền truy cập Admin:
- Dashboard với thống kê tổng quan
- Quản lý sân bóng
- Quản lý booking (xác nhận, hủy, hoàn thành)
- Quản lý khách hàng
- Xem báo cáo doanh thu

## 🌐 Truy cập trang Admin:
1. Đăng nhập bằng tài khoản admin
2. Truy cập: `/Admin` hoặc click "Quản trị" trên menu
3. URL trực tiếp: https://localhost:5001/Admin

## 📊 Tính năng Admin Dashboard:
- Thống kê: Tổng sân bóng, booking, khách hàng
- Booking chờ xác nhận
- Booking hôm nay
- Tổng doanh thu
- Thao tác nhanh quản lý

## 🛠️ Chức năng quản lý:
- **Sân bóng**: Xem danh sách, thông tin chi tiết
- **Booking**: Xác nhận/Hủy/Hoàn thành/Xóa booking
- **Khách hàng**: Xem danh sách, thông tin liên hệ
- QuanLySanBongMini
| Field     | Type          | Mô tả                  |
| --------- | ------------- | ---------------------- |
| SanId     | int (PK)      | Mã sân                 |
| TenSan    | nvarchar(100) | Tên sân                |
| LoaiSan   | nvarchar(50)  | 5 người, 7 người       |
| GiaSan    | decimal       | Giá thuê/giờ           |
| TrangThai | bit           | 1 = còn trống, 0 = bận |

| Field       | Type          |
| ----------- | ------------- |
| KhachHangId | int (PK)      |
| HoTen       | nvarchar(100) |
| SDT         | varchar(12)   |
| Email       | varchar(100)  |
| Field       | Type                 | Mô tả |
| ----------- | -------------------- | ----- |
| DatSanId    | int (PK)             |       |
| SanId       | int (FK → SanBong)   |       |
| KhachHangId | int (FK → KhachHang) |       |
| NgayDa      | date                 |       |
| GioBatDau   | time                 |       |
| GioKetThuc  | time                 |       |
| TongTien    | decimal              |       |
public class SanBong
{
    public int SanId { get; set; }
    public string TenSan { get; set; }
    public string LoaiSan { get; set; }
    public decimal GiaSan { get; set; }
    public bool TrangThai { get; set; }
}
public class KhachHang
{
    public int KhachHangId { get; set; }
    public string HoTen { get; set; }
    public string SDT { get; set; }
    public string Email { get; set; }
}
public class DatSan
{
    public int DatSanId { get; set; }
    public int SanId { get; set; }
    public int KhachHangId { get; set; }
    public DateTime NgayDa { get; set; }
    public TimeSpan GioBatDau { get; set; }
    public TimeSpan GioKetThuc { get; set; }
    public decimal TongTien { get; set; }

    public SanBong San { get; set; }
    public KhachHang KhachHang { get; set; }
}
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) {}

    public DbSet<SanBong> SanBongs { get; set; }
    public DbSet<KhachHang> KhachHangs { get; set; }
    public DbSet<DatSan> DatSans { get; set; }
}
"ConnectionStrings": {
  "DefaultConnection": "Server=.;Database=QuanLySanBong;Trusted_Connection=True;"
}
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
MVC Controller with views, using Entity Framework
