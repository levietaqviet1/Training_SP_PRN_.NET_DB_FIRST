# Training_SP_PRN_.NET

# :notebook_with_decorative_cover: Table of Contents
- [Lý Thuyết](#Lý-Thuyết)
- [Môi Trường Code](#Môi-Trường-Code) 
- [Packages cần cài để code](#Packages-cần-cài-để-code)
- [Câu lệch tiện ích cài đặt EF](#Câu-lệch-tiện-ích-cài-đặt-EF)
- [Tiện ích riêng cho ADO.NET](#tiện-ích-riêng-cho-adonet)
- [Tiện ích riêng cho EF](#tiện-ích-riêng-cho-ef)
- [Sự khác, giống nhau giữa các collection trong C#](#sự-khác-giống-nhau-giữa-các-collection-trong-c)
## Lý Thuyết
- [Đến Menu](#notebook_with_decorative_cover-Table-of-Contents)

.NET là một nền tảng phát triển đa ngôn ngữ được tạo bởi Microsoft. Nó cho phép lập trình viên sử dụng các ngôn ngữ lập trình như C#, F#, và Visual Basic để xây dựng các ứng dụng cho Windows, the web, và các thiết bị di động.

.NET Framework là phiên bản chính của .NET và bao gồm các thư viện và các công cụ để xây dựng và chạy các ứng dụng Windows. .NET Core là một phiên bản của .NET được tạo ra để hỗ trợ các nền tảng khác như Linux và macOS.

...

## Môi Trường Code
- [Đến Menu](#notebook_with_decorative_cover-Table-of-Contents)

 1.Microsoft Visual Studio
 
 ![moiTruongCode NET](https://user-images.githubusercontent.com/85175337/211482817-51ba2afb-8885-443d-ba1b-938d406c182f.png)
 
 2.Microsoft SQL Server
 
 ![image](https://user-images.githubusercontent.com/85175337/216105122-35994c7d-2226-4743-a1d6-415e9fc952fc.png)

 
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
 
## Packages cần cài để code
 - Với Windows Forms App 
 
 Trường hợp bạn khai thác dữ liệu từ database bằng `EF (Entity Framework)`
 
 ```C# 
 Microsoft.entityframeworkcore.design(6.0.1) 
 ```
 
 ```C# 
 Microsoft.entityframeworkcore.sqlserver(6.0.1)
 ```
 
 ```C#
 Microsoft.extensions.configuration.json(6.0.0) 
 ```
 
 Trường hợp bạn khai thác dữ liệu từ database bằng `ADO.NET` 
 
 ```C# 
 Microsoft.extensions.configuration.json(6.0.0) 
 ```
 
 ```C# 
 System.data.sqlclient(4.8.3) 
 ```
 
- Với ASP.NET Core Web App ( Nó có sẵn thư viện json rồi ) :

   Trường hợp bạn khai thác dữ liệu từ database bằng `EF (Entity Framework)`
   
 ```C# 
 Microsoft.entityframeworkcore.design(6.0.1)
 ```
 
 ```C# 
 Microsoft.entityframeworkcore.sqlserver(6.0.1)
 ``` 
 
  Trường hợp bạn khai thác dữ liệu từ database bằng `ADO.NET` 
  
 ```C#
 System.data.sqlclient(4.8.3)
 ```
 
## Câu lệch tiện ích cài đặt EF

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

Text trong appsettings.json :
```C#
{
  "ConnectionStrings": {
    "MyCnn": "server=localhost;database=MyDB2;uid=sa;pwd=123"
  }
}
```

Với ADO nên sài như này

```C#
{
  "ConnectionStrings": {
    "MyCnn": "server=localhost;database=PRN211DB;uid=sa;pwd=123;MultipleActiveResultSets=True"
  }
}
```

Với câu Q2 PE nên dùng 

```C#
{
  "ConnectionStrings": {
    "MyCnn": "server=(local);database=MySaleDB;uid=sa;pwd=123;TrustServerCertificate=true"
  }
}
```

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

- Lấy dữ liệu vào trong thẻ trong  Với Windows Forms App 

```C#
      DataProvider dataProvider = new DataProvider();
      DataTable dt = dataProvider.executeQuery("
      SELECT d.WordID , d.Word, d.Meaning , d.EditDate, w.ID  \r\nFROM Dictionary d JOIN WordType w ON d.ID = w.ID\r\n  WHERE d.WordID = " + code);
                if(dt.Rows.Count > 0)
                {
                    txtWord.Text = dt.Rows[0].ItemArray[1].ToString();
                    txtMeaning.Text = dt.Rows[0].ItemArray[2].ToString();
                    cbType.SelectedIndex = int.Parse(dt.Rows[0].ItemArray[4].ToString())-1; 
                } 
```

Trong code trên, đoạn code đầu tiên tạo ra một đối tượng `DataProvider` và gọi hàm executeQuery trên đối tượng đó để thực thi một truy vấn SQL. Truy vấn này trả về một `DataTable` với kết quả từ các bảng `Dictionary` và `WordType`, với một JOIN giữa chúng trên trường ID. Câu truy vấn còn có một điều kiện WHERE để chỉ lấy các dòng có WordID bằng giá trị của biến code.

Tiếp theo, trong khối if dưới đó, nó kiểm tra xem DataTable có hơn 0 dòng. Nếu có, nó sẽ lấy giá trị đầu tiên trong bảng (vì dùng Rows[0]) và gán giá trị đó cho các `textbox` và `combobox` tương ứng.

## Tiện ích riêng cho EF

- [Đến Menu](#notebook_with_decorative_cover-Table-of-Contents)

- Lấy danh sách Course :

```C#
 List<Course> course = _context.Courses.Include(x => x.Subject).Include(x => x.Instructor).Include(x => x.Subject)
                .Include(x => x.Term).Include(x => x.Campus).ToList();
 course = course.Where(x => x.CourseCode.Equals(search)).ToList();
```

Trong đoạn code này, `_context` là một đối tượng context của EF, nó chứa các tham chiếu đến các bảng trong cơ sở dữ liệu. `Courses` là một thuộc tính trong `_context` chứa tham chiếu đến bảng `"Courses"`.

`Include` là một phương thức mà `EF` cung cấp để tải các thuộc tính liên kết từ các bảng khác. Ví dụ `x => x.Subject`, dùng để tải thuộc tính `Subject` từ bảng khác và gán cho từng khóa học.

Phương thức `Where` là một phương thức mà LINQ cung cấp để lọc các phần tử trong một danh sách theo một điều kiện. Trong đoạn code này, điều kiện lọc là `x => x.CourseCode.Equals(search)`. Điều kiện này sẽ kiểm tra xem mã khóa học của mỗi khóa học trong danh sách có bằng với giá trị của biến `search` hay không. Nếu bằng thì nó sẽ được giữ lại trong danh sách, ngược lại sẽ bị loại bỏ.

Cụ thể, đoạn code trên sẽ truy vấn cơ sở dữ liệu và lấy tất cả các khóa học trong bảng "`Courses`", và các thuộc tính liên kết với nó như `Subject, Instructor, Term, Campus` được tải theo và gán cho biến course.

- Các LINQ thường dùng
 
`Include` là một phương thức mà `EF` cung cấp để tải các thuộc tính liên kết từ các bảng khác.
 
`Where` là một phương thức mà LINQ cung cấp để lọc các phần tử trong một danh sách theo một điều kiện.

`Skip` là một phương thức của LINQ, nó cho phép bỏ qua một số phần tử đầu tiên trong một danh sách.

```C#
List<int> numbers = Enumerable.Range(1, 100).ToList();
var result = numbers.Skip(10).ToList();
```

`Take` là một phương thức của LINQ, nó cho phép lấy một số phần tử đầu tiên trong một danh sách.

```C#
List<int> numbers = Enumerable.Range(1, 100).ToList();
var result = numbers.Take(10).ToList();
```

`Select`: Chọn các thuộc tính của một đối tượng trong một danh sách.

```C#
var selectedStudents = students.Select(s => new { s.Name, s.Major }).ToList();
```

`OrderBy`: Sắp xếp các phần tử trong một danh sách theo thứ tự tăng dần hoặc giảm dần.

```C#
var orderedStudents = students.OrderBy(s => s.Name).ToList();
```

`GroupBy`: Nhóm các phần tử trong một danh sách theo một thuộc tính.

```C#
var groupedStudents = students.GroupBy(s => s.Major);
```

`First`, `FirstOrDefault`: Lấy phần tử đầu tiên trong một danh sách.

```C#
var oldestStudent = students.OrderByDescending(s => s.Age).First();
```

`Last`, `LastOrDefault`: Lấy phần tử cuối cùng trong một danh sách.

`Count`, `LongCount` : Đếm số lượng phần tử trong một danh sách.

```C#
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
int evenCount = numbers.Count(n => n % 2 == 0);
```

`Any` : Kiểm tra xem một danh sách có bất kỳ phần tử nào không.

```C#
var isAnyStudentOver25 = students.Any(s => s.Age > 25);
```

`All` : Kiểm tra xem tất cả các phần tử trong một danh sách có thỏa mãn một điều kiện hay không.

```C#
var isAllStudentsOver20 = students.All(s => s.Age > 20);
```

`Sum`, `Min`, `Max` : Tính

## Sự khác, giống nhau giữa các collection trong C#

- [Đến Menu](#notebook_with_decorative_cover-Table-of-Contents)

- Khác: 
ArrayList: Lưu trữ các phần tử có kiểu dữ liệu object và không yêu cầu kiểu dữ liệu cho các phần tử. ArrayList cũng có thể chứa các giá trị null.

SortedList: Lưu trữ các phần tử theo cặp <key, value> và tự động sắp xếp các phần tử theo key. SortedList chỉ cho phép sử dụng các kiểu dữ liệu làm key mà có thể so sánh.

Dictionary: Lưu trữ các phần tử theo cặp <key, value> nhưng không tự động sắp xếp các phần tử. Dictionary sử dụng một cách hiệu quả các thuật toán băm để tra cứu các phần tử theo key.

Hashtable: Tương tự như Dictionary, nhưng sử dụng các thuật toán băm để sắp xếp các phần tử theo key. Do đó, tốc độ tra cứu các phần tử trong Hashtable là nhanh hơn so với SortedList.

Về các yếu tố khác: 

Yêu cầu về kiểu dữ liệu: Nếu bạn muốn lưu trữ các phần tử có kiểu dữ liệu khác nhau, ArrayList là lựa chọn tốt nhất. Nếu bạn muốn sắp xếp các phần tử theo key, bạn có thể chọn SortedList hoặc Hashtable.

Yêu cầu về tốc độ: Nếu bạn cần tra cứu các phần tử một cách nhanh chóng, bạn nên chọn Dictionary hoặc Hashtable. Tuy nhiên, Hashtable có tốc độ tra cứu nhanh hơn Dictionary vì nó sử dụng các thuật toán băm để sắp xếp các phần tử theo key.

Yêu cầu về bộ nhớ: ArrayList và SortedList có thể chiếm nhiều bộ nhớ hơn Dictionary và Hashtable do việc sắp xếp các phần tử.

Yêu cầu về tính linh hoạt: ArrayList và Dictionary cho phép bạn thêm, xóa hoặc sửa đổi các phần tử một cách linh hoạt. SortedList và Hashtable cũng có thể cho phép bạn thực hiện các thao tác tương tự, nhưng có thể tốn thời gian hơn do việc cần sắp xếp lại các phần tử sau mỗi lần thao
