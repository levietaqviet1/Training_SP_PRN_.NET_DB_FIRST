# Training_SP_PRN_.NET

# :notebook_with_decorative_cover: Table of Contents
- [Lý Thuyết](#Lý-Thuyết)
- [Môi Trường Code](#Môi-Trường-Code)
- [Câu lệch tiện ích](#Câu-lệch-tiện-ích)
- [Tiện ích riêng cho ADO.NET](#tiện-ích-riêng-cho-adonet)
## Lý Thuyết
- [Đến Menu](#notebook_with_decorative_cover-Table-of-Contents)

## Môi Trường Code
- [Đến Menu](#notebook_with_decorative_cover-Table-of-Contents)

 1.Microsoft Visual Studio
 
 ![moiTruongCode NET](https://user-images.githubusercontent.com/85175337/211482817-51ba2afb-8885-443d-ba1b-938d406c182f.png)
 
 2.Microsoft SQL Server
 
 3.Kiểm tra môi trường dotnet
 
 Mở Command Prompt
 
 ![compa](https://user-images.githubusercontent.com/85175337/211512804-849a33f3-3943-441e-88e9-6a9e44ea8194.png)
 
Code kiểm tra môi trường : 

```C#
dotnet tool list --global
```
Code gỡ bàn thừa nếu version dưới 6. : 

```C#
dotnet tool uninstall  --global dotnet-ef
```
Bản cần cài đặt:

```C#
dotnet tool install --global dotnet-ef --version 6.0.1
 ```
 
 `Note`: tùy thời điểm tuy nhiên hiện tại 2022 là từ 6.
 
 4.Packages cần cài để code
 - Với Windows Forms App 
 
 Trường hợp bạn khai thác dữ liệu từ database bằng `EF (Entity Framework)`
 
 ```C# Microsoft.entityframeworkcore.design(6.0.1) ```
 
 ```C# Microsoft.entityframeworkcore.sqlserver(6.0.1) ```
 
 ```C# Microsoft.extensions.configuration.json(6.0.0) ```
 
 Trường hợp bạn khai thác dữ liệu từ database bằng `ADO.NET` 
 
 ```C# Microsoft.extensions.configuration.json(6.0.0) ```
 
 ```C# System.data.sqlclient(4.8.3) ```
 
- Với ASP.NET Core Web App ( Nó có sẵn thư viện json rồi ) :

   Trường hợp bạn khai thác dữ liệu từ database bằng `EF (Entity Framework)`
   
 ```C# Microsoft.entityframeworkcore.design(6.0.1) ```
 
 ```C# Microsoft.entityframeworkcore.sqlserver(6.0.1) ``` 
 
  Trường hợp bạn khai thác dữ liệu từ database bằng `ADO.NET` 
  
 ```C# System.data.sqlclient(4.8.3) ```
 
## Câu lệch tiện ích

- [Đến Menu](#notebook_with_decorative_cover-Table-of-Contents)

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
                var config = new ConfigurationBuilder().AddJsonFile("appsettings.json").Build();
                optionsBuilder.UseSqlServer(config.GetConnectionString("MyCnn"));
```

![onConfi](https://user-images.githubusercontent.com/85175337/211485282-ff519b5a-2b36-4206-b1e1-afe66c9fd940.png)

## Tiện ích riêng cho ADO.NET

- [Đến Menu](#notebook_with_decorative_cover-Table-of-Contents)

Class hỗ trợ khai thác dữ liệu mẫu: 

```c#
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Text;
using System.Configuration;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace DictionaryAppADONET
{
    /// <summary>
    /// Chứa những hàm xử lý database chung cho tất cả
    /// kết nối
    /// executequẻy
    /// </summary>
    public class DataProvider
    {
        //Khai bao cac thanh phan ket noi va xu ly DB
        SqlConnection cnn; //Ket noi DB
        SqlDataAdapter da; //Xu ly cac cau lenh SQL: Select
        SqlCommand cmd; //Thuc thi cau lenh insert,update,delete

        public DataProvider()
        {
            connect();
        }
        private string getConnectionString()
        {
            string connectionString;
            IConfiguration config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json", true, true)
                .Build();
            connectionString = config["ConnectionStrings:MyCnn"];
            return connectionString;
        }
        private void connect()
        {
            try
            {
                string strCnn = getConnectionString();
                cnn = new SqlConnection(strCnn);
                if (cnn.State == ConnectionState.Open)
                {
                    cnn.Close();
                }
                cnn.Open();
            //  MessageBox.Show("Connect success!");
            }
            catch (Exception ex)
            {
                MessageBox.Show("Loi ket noi:" + ex.Message);
            }
        }

        //Hàm execute 1 câu lệnh select
        public DataTable executeQuery(string strSelect)
        {
            DataTable dt = new DataTable();
            try
            {
                da = new SqlDataAdapter(strSelect, cnn);
                da.Fill(dt);
            }
            catch (Exception ex)
            {
                MessageBox.Show("Execute fail:" + ex.Message);
            }
            return dt;
        }

        //Hàm execute câu lệnh insert,update,delete
        public bool executeNonQuery(string strSQL)
        {
            try
            {
                cmd = cnn.CreateCommand();
                cmd.CommandType = CommandType.Text;
                cmd.CommandText = strSQL;
                cmd.ExecuteNonQuery();
            }
            catch (Exception ex)
            {
                MessageBox.Show("Insert/Update/Delete error:" + ex.Message);
                return false;
            }
            return true;
        }

        public IDataReader executeQuery2(string strSelectr, params SqlParameter[] para)
        {
            IDataReader dr = null;
            try
            {
                cmd = cnn.CreateCommand();
                cmd.CommandType = CommandType.Text;
                cmd.CommandText = strSelectr;
                if(para != null)
                {
                    foreach (SqlParameter item in para)
                    {
                        cmd.Parameters.Add(item);
                    }
                }
                dr = cmd.ExecuteReader();
            }
            catch (Exception ex)
            {
                MessageBox.Show("executeQuery2 error:" + ex.Message);

            }
            return dr;
        }
    }
}
```

Ví dụ lấy dữ liệu 

- Lấy tất cả WordType:

```C#
 DataProvider dataProvider = new DataProvider();
 string strSelect = ("select * from WordType");
 cbType.DataSource = dataProvider.executeQuery(strSelect);
 cbType.DisplayMember = "TypeName";
 cbType.ValueMember = "ID";
```

`cbType` là 1 ComboBox có tên là cbType

hoặc: 

```C#
  dgDictionry.DataSource = dataProvider.executeQuery(" SELECT d.WordID , d.Word, d.Meaning , d.EditDate, w.TypeName\n  FROM Dictionary d JOIN WordType w ON d.ID 
```

`dgDictionry` là 1 DataGirdView có tên là dgDictionry







