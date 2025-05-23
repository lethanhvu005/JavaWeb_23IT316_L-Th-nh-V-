# JavaWeb_23IT316_LeThanhVu
1. Tên dự án:
Website bán vé xem phim sử dụng Java Sping Boot.

2. Giới thiệu:
Xây dựng website quản lý bán vé xem phim với các tính năng chính: tìm kiếm phim, đặt vé, chọn suất chiếu và thanh toán.

3. Thành viên nhóm:
Lê Thành Vũ MSV: 23IT316
4. Công nghệ:
Database: MySQL 8.0.32
Backend: Restful API
Java 17
Spring Boot 3.0.6
Maven 3.9.1
JWT (io.jsonwebtoken) 0.11.5
Frontend:
HTML
CSS
JS
Khác:
Docker: Cho phép triển khai project nhanh chóng trên các máy tính khác nhau.
Nginx: Dựng server cho các web service trong docker.
Sandbox của VNPay: Tích hợp ứng dụng thanh toán của VNPAY trong việc đặt vé.
5. Cài đặt:
Yêu cầu phải máy tính phải cài đặt sẵn docker và docker-compose
Tại thư mục mẹ, gõ lệnh dưới để chạy project:
docker-compose up --build --no-deps
Dừng project bằng Ctrl + C và gõ lệnh:
docker-compose down
6. Thông tin:
A. Các Website:
Website chính (Front-end): http://localhost:80
Hiển thị nội dung liên quan đến phim và cho phép đặt/thanh toán vé.
Website Admin (Front-end): http://localhost:81
Trang admin cho phép quản lý các nội dung trên website chính và các user. Yêu cầu tài khoản có quyền admin để đăng nhập.
Website cho API (Back-end): http://localhost:9595
Xử lý các request được gửi từ front-end.
Xem chi tiết tại http://localhost:9595/swagger-ui/index.html
Website cho Database (Back-end): http://localhost:32346
Hiển thị trực quan database của web (dùng PHPMyAdmin).
B. Thông tin thanh toán VNPAY:


7. Mô hình hoạt động:
     A. Mô hình Database:
![so_do_databases](https://github.com/user-attachments/assets/ac2ed2c7-b847-4bbc-b298-61467bf8ced1)


     B. Mô hình hoạt động cơ bản:


     C. Mô hình hoạt động của website dành cho User:


     D. Mô hình hoạt động của website dành cho Admin:



8. Chức năng của trang web:
A. Chức năng của User:
Đăng ký
Xác thực tài khoản qua email
Đăng nhập
Đăng xuất
Quên mật khẩu
Xác nhận mật khẩu mới qua email
Tìm kiếm phim theo từ khóa, thể loại
Đặ̣t vé:
Lựa chọn suất chiếu(ngày, giờ, phòng chiếu)
Chọn chỗ ngồi
Thanh toán (VNPAY, QR Code, Thẻ Nội Địa, Thể Quốc Tế) i
Gửi vé điện tử và chi tiết thanh toán qua email khi thanh toán thành công.
B. Chức năng của Admin:
Quản lý user.
Thêm, xóa, sữa dữ liệu liên quan đến các suất chiếu phim như: phim chiếu, lịch chiếu, phòng chiếu, số lượng ghế và lưu lại thông tin thanh toán.

9. Bảo mật:
Sử dụng JWT (JSON Web Token) để phân quyền truy cập và xác thực user.
Cấu trúc của token:
  1. Thuật toán sử dụng: RS256 với key có kích thước 2048 bit
  2. Loại data có trong token bao gồm:
roles : Dùng để phân quyền người dùng, bao gồm SUPER_ADMIN, ADMIN, USER
sub : Chứa username của người dùng.
iat : Lưu thời điểm tạo token.
exp : Lưu thời điểm hết hạn của token (sau 1 giờ kể từ lúc tạo)

10. Demo:
10.1. Website cho User:
     A. Trang Chủ


     B. Đăng nhập/đăng ký User:



     C. Trang chọn phim


     D. Trang chi tiết phim:


     E. Trang chọn giờ xem:


     F. Trang Chọn ghế:


     G. Trang thanh toán:



10.2. Website cho Admin:
     A. Đăng nhập Admin:


     B. Admin quản lý tài khoản:


     C. Admin thêm suất chiếu:



10.3. Website cho API:
     Trang API document:



11. Các Lỗ hổng được thiết kế trong website:
A. Bypass xác thực của JWT Token:
      Web API sử dụng JWT token để xác thực và phân quyền người dùng, thuật toán được sử dụng cho việc cấp và xác thực là RS256. Lỗ hổng này được mô phỏng dựa trên CVE-2015-9235, xảy ra là trang web sẽ dựa vào thuật toán trên phần header của token để thực hiện quy trình xác thực sẽ thuật toán đó. Khi trang web sử dụng thuật toán RS/ES để ký (dùng private key) và xác thực (dùng public key) cho token, nhưng khi kẻ tấn công có được public key, chúng sẽ sử dụng thuật toán HS (dùng public key cho cả việc ký và xác thực) để tạo token, khi đưa cho web kiểm tra, nó sẽ dựa vào thuật toán trong phần header của token để lựa chọn thuật toán xác thực, và vì cả thuật toán RS/ES và HS đều dùng public key để xác thực nên việc bypass sẽ thành công.

      Mức độ ảnh hưởng: Cao.
      Phạm vi ảnh hưởng: Toàn bộ các trang web.
      Hậu quả: Leo thang đặc quyền, có khả năng lấy và chỉnh sửa toàn bộ data có trong database thông qua các API.

B. Broken Access Control:
      Uri GET /api/auth/login không có cơ chế giới hạn lượt truy cập sau nhiều lần đăng nhập không thành công dẫn đến kẻ tấn công có thể thực hiện tấn công brute-force để dò mật khẩu của nạn nhân.

      Mức độ ảnh hưởng: Trung bình.
      Phạm vi ảnh hưởng: Chỉ ảnh hường đến user để mật khẩu yếu và dễ đoán.
      Hậu quả: Đánh cắp thông tin và kiểm soát tài khoản của nạn nhân.

C. Insecure Authentication:
      Trang web có chức năng thay đổi mật khẩu mới khi người dùng quên mật khẩu, khi đó một mã xác thực sẽ được gửi đến mail của người dùng, cho phép họ thay đổi mật khẩu cho mình. Trong mã xác thực này chứ tên của người dùng và thời hạn sử dụng, trang web sẽ dựa trên các thông tin này để cho phép đổi mật khẩu mới hay không, tuy nhiên mã xác thực này rất yếu, kẻ tấn công có khả năng tạo một mã xác thực khác tương tự và có thể thay đổi mật khẩu của bất kỳ user nào trong hệ thống.

      Mức độ ảnh hưởng: Cao.
      Phạm vi ảnh hưởng: Tất cả user tồn tại trong hệ thống.
      Hậu quả: Đánh cắp thông tin và kiểm soát tài khoản của nạn nhân. Leo thang đặc quyền đối các tài khoản admin.

D. Lỗ hổng
      Lỗ hổng

      Mức độ ảnh hưởng:
      Phạm vi ảnh hưởng:
      Hậu quả:

E. Lỗ hổng
      Lỗ hổng

      Mức độ ảnh hưởng:
      Phạm vi ảnh hưởng:
      Hậu quả:

F. Lỗ hổng
      Lỗ hổng

      Mức độ ảnh hưởng:
      Phạm vi ảnh hưởng:
      Hậu quả:

