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
