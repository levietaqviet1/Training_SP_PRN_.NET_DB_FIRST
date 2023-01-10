# Training_SP_PRN_.NET

# :notebook_with_decorative_cover: Table of Contents
- [Lý Thuyết](#Lý-Thuyết)
- [Môi Trường Code](#Môi-Trường-Code)
- [Câu lệch tiện ích](#Câu-lệch-tiện-ích)
## Lý Thuyết
## Môi Trường Code
 1.Microsoft Visual Studio
 ![moiTruongCode NET](https://user-images.githubusercontent.com/85175337/211482817-51ba2afb-8885-443d-ba1b-938d406c182f.png)
 2.Microsoft SQL Server
## Câu lệch tiện ích
Câu lệnh gen folder `Modes` sử dụng cho `Entity Framework`:
```C#
dotnet ef dbcontext scaffold "server =localhost; database = Project3_SE1631;uid=sa;pwd=123;" Microsoft.EntityFrameworkCore.SqlServer --output-dir Models
```
hoặc dùng câu lệch này:
```C#
dotnet ef dbcontext scaffold "server =(local); database = PE_PRN211_22FallB5;uid=sa;pwd=123;TrustServerCertificate=true" Microsoft.EntityFrameworkCore.SqlServer --output-dir Models
```

