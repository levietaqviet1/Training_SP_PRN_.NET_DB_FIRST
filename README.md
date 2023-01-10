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
- Câu lệnh `gen` folder `Modes` sử dụng cho `Entity Framework`:
```C#
dotnet ef dbcontext scaffold "server =localhost; database = Project3_SE1631;uid=sa;pwd=123;" Microsoft.EntityFrameworkCore.SqlServer --output-dir Models
```
hoặc dùng câu lệch này :
```C#
dotnet ef dbcontext scaffold "server =(local); database = PE_PRN211_22FallB5;uid=sa;pwd=123;TrustServerCertificate=true" Microsoft.EntityFrameworkCore.SqlServer --output-dir Models
```

- Nếu Database có sự thay đổi mà muốn `update` lại models đã sinh thì dùng lệch: 
```C#
dotnet ef dbcontext scaffold "server =localhost; database = MyDB2;uid=sa;pwd=123;TrustServerCertificate=true" Microsoft.EntityFrameworkCore.SqlServer -o Models -f  
```

- Trong 1 vài trường hợp bạn chỉ muốn lấy 1 vài class trong database thì dùng lệch: 
```C#
dotnet ef dbcontext scaffold "server =localhost; database = Northwind;uid=sa;pwd=123;TrustServerCertificate=true" Microsoft.EntityFrameworkCore.SqlServer -o Models -t class1
```
`-t` rồi nhập tên class đó

- Đoạn code đọc file json trong hàm onconfig: ( với Entity Framework nhớ thay vào hàm đc dạy trong class có đuôi Context.cs )
```C#
var builder = new ConfigurationBuilder()
                              .SetBasePath(Directory.GetCurrentDirectory())
                              .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true);
            IConfigurationRoot configuration = builder.Build();
            optionsBuilder.UseSqlServer(configuration.GetConnectionString("MyCnn"));
```
![onConfi](https://user-images.githubusercontent.com/85175337/211485282-ff519b5a-2b36-4206-b1e1-afe66c9fd940.png)




