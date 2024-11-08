# Business Logic

### 1. **Authentication-Authorization Service**
   - **Chức năng chính**:
     - **Login**: Xác thực thông tin đăng nhập của người dùng và cấp phát token nếu thành công
     - **Token Generation**: Tạo token JWT (JSON Web Token) cho phiên làm việc của người dùng. Token này chứa các thông tin như user ID, quyền hạn (nếu có), và thời gian hết hạn
     - **Token Validation**: Mỗi khi nhận được yêu cầu từ người dùng (vd: truy xuất thông tin đơn hàng), service này sẽ xác thực token có hợp lệ không
   - **Công nghệ và cách triển khai**:
     - **JWT cho Token**: Sử dụng thư viện như `jsonwebtoken` trong Node.js hoặc `pyjwt` trong Python để tạo và xác thực JWT
     - **TTL (Time To Live)**: Cài đặt thời gian hết hạn cho token, ví dụ 15 phút hoặc 1 giờ. Người dùng cần refresh token nếu cần kéo dài phiên làm việc
     - **Middleware cho Validation**: Với mỗi API của các dịch vụ khác, bạn có thể thêm middleware để xác minh JWT token trước khi xử lý yêu cầu
   - **Triển khai**: Thêm Authentication-Authorization Service vào file `docker-compose.yml` cùng với các biến môi trường quan trọng (như secret key) để đảm bảo bảo mật

### 2. **Relationship between user-service and auth-service**
1. **Đăng ký**: Người dùng gọi API của User Service để tạo tài khoản mới
2. **Login**: Sau khi đăng ký, người dùng đăng nhập qua Authentication-Authorization Service và nhận được token
3. **Sử dụng token**: Khi gọi API từ các dịch vụ khác (vd: Order Service để lấy danh sách đơn hàng), token sẽ được gửi kèm trong request headers. Các service sẽ dùng Authentication-Authorization Service để xác thực token và quyết định cấp quyền truy cập

### 3. **Ưu điểm của thiết kế này**
- **Scalability**: User Service và Authentication-Authorization Service tách biệt giúp hệ thống mở rộng dễ dàng khi số lượng người dùng tăng
- **Security**: Mật khẩu được lưu trữ an toàn bằng cách mã hóa, và token có thời hạn cụ thể
- **Flexibility**: Khi cần, bạn có thể thêm third-party auth (như Google OAuth) hoặc mở rộng JWT với các quyền chi tiết hơn
