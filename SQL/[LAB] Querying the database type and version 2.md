# Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft

### Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft
![Require of Lab](/Images/sql7.1.png)

Yêu cầu của bài lab là trả về giá trị phiên bản của dtb

Truy cập vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Corporate gifts`, `Food & Drink`, `Gifts` và `Lifestyle`, `Pets`....
![Website](/Images/sql7.2.png)

Tương tự như các bài lab trước, đầu tiên ta phải xác định số cột được trả về bằng cách thay đổi URL bằng cách thêm vào chuỗi `'+ORDER+BY+1--`

> https://ac751f0a1fd7d53bc0eca05800000027.web-security-academy.net/filter?category=Pets%27+order+by+1--

![Product of Pets order by 1](/Images/sql7.4.png)

Kỳ lạ, phương pháp thường làm lại không hoạt động *bối rối -ing *

Ta nhận thấy đây là hệ quản trị cơ sở dữ liệu MySQL hoặc là Microsoft, thì dấu hiệu comment của Microsoft là dấu -- còn MySQL có thêm dấu #

Vì thế, ta thay URL trên thành như sau
> https://ac751f0a1fd7d53bc0eca05800000027.web-security-academy.net/filter?category=Pets%27+order+by+1#
> 
![Product of Pets order by 1](/Images/sql7.5.png)

Hụ hụ, lại không được, đi xem hướng dẫn thì người ta bảo là sử dụng Burp Suite

******
Nếu bạn chưa biết gì về Burp Suite có thể xem tại link sau: 
> https://portswigger.net/burp/documentation/desktop/getting-started
******

Sau khi mở Burp Suit lên, chọn **Proxy** và **Intercept** > **Open Brower**. Khi đó sẽ xuất hiện một cửa sổ trình duyệt mới, thả đường link bài lab vào trình duyệt đó và bấm vào xem một danh mục bất kỳ
![Product of Pets](/Images/sql7.3.png)

Tiếp theo, tại **HTTP history** lựa chọn request trễ nhất và bấm chuột phải chọn **Send to Repeater**
![HTTP history](/Images/sql7.6.png)

Qua phần **Repeater**, ta chỉnh sửa requets bằng cách thêm vào chuỗi `'+order+by+1#` và bấm **Send**
> GET /filter?category=Pets'+order+by+1# HTTP/1.1

![Repeater](/Images/sql7.7.png)

Tăng dần vị trí tương đối lên 3 thì nhận được thông báo lỗi, do đó xác định được số cột trả về là 2
![Error of query](/Images/sql7.8.png)

Sau khi xác định được số cột trả về, tiếp theo ta xác định các cột trả về có chứa văn bản hay không
> GET /filter?category=Pets'+UNION+SELECT+**'avc'**,**'g13'**# HTTP/1.1
> 
![Check value of column](/Images/sql7.9.png)

Sau khi xác định các cột trả về đều chứa văn bản, ta tiến hành trả về giá trị phiên bản của dtb
> GET /filter?category=Pets'+UNION+SELECT+@@version,NULL# HTTP/1.1

![Result](/Images/sql7.10.png)


<div align="right"> <i> Ngày 21 tháng 01 năm 2021 </i> </div>
