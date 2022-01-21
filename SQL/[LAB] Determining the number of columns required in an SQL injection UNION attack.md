# Lab: SQL injection UNION attack, determining the number of columns returned by the query

### Link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns
![Require of Lab](/Images/sql3.1.png)

Yêu cầu của bài lab là xác định số cột được trả về bằng câu truy vấn
Truy cập vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Accessories`, `Corporate gifts`, `Food & Drink`, `Gifts` và `Lifestyle`.
![Website](/Images/sql3.2.png)

Để giải quyết bài lab, ta làm theo hai bước như sau. Đầu tiên là xác định số cột được trả về bởi truy vấn, tiếp theo là sử dụng phương pháp gửi một loạt các chuỗi UNION SELECT chỉ định một số giá trị rỗng khác nhau để submit kết quả.

Ta lựa chọn danh mục `Gifts`
![Product of Gifts](/Images/sql3.3.png)


Sau đó, để xác dịnh số cột được trả về trong câu truy vấn, ta sửa đổi URL như sau
> https://ac9e1f731f8f8b24c06917e500930028.web-security-academy.net/filter?category=Gifts'+ORDER+BY+1--

Đổi thành câu truy vấn như sau: 
> SELECT [...] FROM category WHERE category = 'Gifts' ORDER BY 1

Nghĩa là lấy [...] sản phẩm của danh mục Gifts và được sắp xếp tăng dần theo cột đầu tiên được trả về
![Product of Gifts order by 1](/Images/sql3.4.png)

Tương tự như vậy, ta tăng dần vị trí tương đối lên 2,3 và 4 thì nhận được thông báo lỗi như sau
![Product of Gifts order by 4](/Images/sql3.5.png)

Từ đó có thể xác định rằng số cột trả về của câu truy vấn là 3.

Để submit kết quả, ta sử dụng phương pháp UNION SELECT, thay đổi URL thành như sau:
> https://ac9e1f731f8f8b24c06917e500930028.web-security-academy.net/filter?category=Gifts'+UNION+SELECT+NULL,NULL,NULL--

![Submit the result](/Images/sql3.6.png)


<div align="right"> <i> Ngày 20 tháng 01 năm 2021 </i> </div>

