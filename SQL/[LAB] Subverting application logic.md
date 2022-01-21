# Lab: SQL injection vulnerability allowing login bypass

### Link: https://portswigger.net/web-security/sql-injection/lab-login-bypass

![Require of Lab](/Images/sql2.1.png)

Yêu cầu của bài lab là ta phải log in vào website với username là administrator. Truy cập vào bài lab, ta thấy đây là 1 website bán hàng có đăng nhập người dùng.
![Website](/Images/sql2.2.png)

Bấm vào `My account`, ta thấy yêu cầu phải điền username và password vào form đăng nhập.
![Login](/Images/sql2.3.png)

Điền thử username là administrator và password là 123 để kiểm tra thử phản ứng của website. 
![Test Login](/Images/sql2.4.png)

Thì thấy rằng username hoặc password không chính xác nên không thể đăng nhập vào được.
![Test Login 2](/Images/sql2.5.png)

Sau khi nghịch nghịch mấy cái URL ta có thể nhận ra rằng khác với bài lab đầu tiên là ta không thể chỉnh sửa URL để có thể xâm nhập vô dtb của website. Đọc kỹ lại phần hướng dẫn của bài lab, ở đó có nói rằng nếu giả sử như có một user đăng nhập vào website với username là `wiener` và password là `bluecheese`, thì ứng dụng sẽ kiểm tra username có ở trong dtb và password  có đúng hay không dựa vào câu truy vấn như sau
 > SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'

Nếu kết quả trả về là thông tin của 1 user thì đăng nhập thành công và ngược lại, đăng nhập thất bại

Ở yêu cầu bài lab là đăng nhập vào website với username là `administrator`, do đó ta có thể chắc chắn rằng có một username là `administrator` trong dtb của website. 
Dựa vào kiến thức từ bài lab trước, biết rằng dấu -- trong SQL được nhận biết là dấu hiệu của comment, do đó ta có thể lợi dụng lỗ hổng đó như sau

Ở phần đăng nhập, ta điền username là `administrator'--` và password là 1 số ký tự bất kỳ như `123`. 
![Login](/Images/sql2.6.png)

Khi đó website sẽ kiểm tra thông tin của user ta vừa nhận vào với câu truy vấn như sau
> SELECT * FROM users WHERE username = 'administrator'--' AND password = '123'

Mà sau dấu -- là phần comment nên câu truy vấn trở thành như sau
> SELECT * FROM users WHERE username = 'administrator'

Và tất nhiên sẽ có thông tin của user trả về, nên ta đã đăng nhập thành công vào website
![Login](/Images/sql2.7.png)


<div align="right"> <i> Ngày 18 tháng 01 năm 2021 </i> </div>