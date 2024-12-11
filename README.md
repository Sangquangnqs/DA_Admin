# DA_Admin
# API Documentation

## I. Tổng quan
Hệ thống API này hỗ trợ xác thực, quản lý người dùng và thanh toán.

---

### Authentication API

#### 1. Đăng ký tài khoản người dùng
- **Mô tả**: Cho phép người dùng đăng ký bằng tên người dùng, mật khẩu, email và số điện thoại (tùy chọn).
- **Method**: POST
- **Endpoint**: `/api/v1/auth/register`
- **Request Body**:
```json
{
  "email": "user@example.com",
  "password": "hashed_password_here",
  "profile": {
    "avatar_url": "https://example.com/avatar.png",
    "bio": "Software developer based in NY",
    "date_of_birth": "1995-05-15",
    "full_name": "John Doe",
    "phone_number": "+84911123456"
  },
  "username": "johndoe"
}
```
- **Responses**:
  - **201**: Đăng ký tài khoản thành công
  - **400**: Request body không hợp lệ
  - **409**: Email, username, phone number đã tồn tại
  - **500**: Lỗi server

#### 2. Đăng nhập
- **Mô tả**: Xác thực người dùng vào hệ thống.
- **Method**: POST
- **Endpoint**: `/api/v1/auth/login`
- **Request Body**:
```json
{
  "username": "string",
  "password": "string"
}
```
- **Responses**:
  - **200**: Login thành công, trả về token
  - **400**: Request body không hợp lệ
  - **401**: Mật khẩu hoặc username không chính xác
  - **403**: Tài khoản bị cấm
  - **500**: Lỗi server

#### 3. Đăng nhập qua Google
- **Mô tả**: Xác thực người dùng bằng tài khoản Google.
- **Method**: POST
- **Endpoint**: `/api/v1/auth/google-login`
- **Parameters**: `id_token`
- **Responses**:
  - **200**: Login thành công, trả về token
  - **401**: `id_token` không hợp lệ
  - **403**: Tài khoản bị cấm
  - **500**: Lỗi server

#### 4. Quên mật khẩu
- **Mô tả**: Gửi mã OTP đến email người dùng để đặt lại mật khẩu.
- **Method**: POST
- **Endpoint**: `/api/v1/auth/forgot-password`
- **Request Body**:
```json
{
  "email": "string"
}
```
- **Responses**:
  - **200**: Mã OTP đã được gửi
  - **400**: Email không đúng định dạng
  - **404**: Không tìm thấy user với email cung cấp
  - **500**: Lỗi server

#### 5. Đặt lại mật khẩu
- **Mô tả**: Cho phép người dùng đặt lại mật khẩu bằng OTP.
- **Method**: POST
- **Endpoint**: `/api/v1/auth/reset-password`
- **Request Body**:
```json
{
  "otp": "string",
  "new_password": "string"
}
```
- **Responses**:
  - **200**: Đặt lại mật khẩu thành công
  - **400**: Mật khẩu không đúng định dạng
  - **401**: OTP không hợp lệ
  - **500**: Lỗi server

#### 6. Refresh Token
- **Mô tả**: Tạo token mới bằng token cũ hợp lệ.
- **Method**: POST
- **Endpoint**: `/api/v1/auth/refresh-token`
- **Authorization**: `token_string`
- **Responses**:
  - **200**: Token mới đã được cấp
  - **401**: Token không hợp lệ
  - **500**: Lỗi server

#### 7. Đăng xuất
- **Mô tả**: Vô hiệu hóa token.
- **Method**: POST
- **Endpoint**: `/api/v1/auth/logout`
- **Authorization**: `token_string`
- **Responses**:
  - **200**: Token đã được vô hiệu hóa
  - **400**: Token không được cung cấp

---

## Admin Management API

#### 1. Lấy thông tin tổng quát về tất cả user
- **Mô tả**: Quản trị viên xem danh sách tất cả user.
- **Method**: GET
- **Endpoint**: `/api/v1/admin/users`
- **Authorization**: `token_string`
- **Responses**:
  - **200**: Trả về danh sách user
  - **403**: Token không hợp lệ
  - **500**: Lỗi server

#### 2. Cập nhật thông tin user
- **Mô tả**: Quản trị viên cập nhật thông tin của một user cụ thể.
- **Method**: PUT
- **Endpoint**: `/api/v1/admin/users/{user_id}`
- **Authorization**: `token_string`
- **Request Body**:
```json
{
  "email": "string",
  "username": "string",
  "profile": {
    "full_name": "string",
    "phone_number": "string",
    "bio": "string",
    "avatar_url": "string"
  },
  "role": "string"
}
```
- **Responses**:
  - **200**: Cập nhật thành công
  - **400**: Request body không hợp lệ
  - **403**: Token không hợp lệ
  - **404**: User không tồn tại
  - **500**: Lỗi server

#### 3. Xóa user
- **Mô tả**: Quản trị viên xóa một user khỏi hệ thống.
- **Method**: DELETE
- **Endpoint**: `/api/v1/admin/users/{user_id}`
- **Authorization**: `token_string`
- **Responses**:
  - **200**: Xóa user thành công
  - **403**: Token không hợp lệ
  - **404**: User không tồn tại
  - **500**: Lỗi server


## II. Đánh giá dự án

### Những cái làm được:

- Xây dựng các API cho người dùng sử dụng hệ thống cơ bản (register, login, xem/chỉnh sửa profile, xem lịch sử nâng cấp VIP,...).
- Sử dụng JWT token để phân quyền người dùng.
- Sử dụng API test của MoMo để phục vụ quá trình thanh toán.
- Gửi email OTP reset password (có thời hạn ngắn: 5 phút) cho người dùng thông qua MAILJET.
- Có các API cho việc quản lý người dùng.
- Hash những thông tin nhạy cảm trước khi lưu vào database (password, OTP,...).
- Ràng buộc các đầu vào như trường email, username, password, phone number,...
- Có hỗ trợ login dùng `id_token` của Google.
- Tách các hàm hỗ trợ ra một package riêng.
- Sử dụng interface để kết nối với cơ sở dữ liệu nhằm hỗ trợ viết unit test dễ dàng hơn và tăng tính linh hoạt khi cần thay đổi hệ quản trị cơ sở dữ liệu (DBMS).
- Có tích hợp Swagger trong việc xây dựng API docs.
- Tạo một hàm `AuthMiddleware` để kiểm tra phân quyền người dùng mà không cần phải kiểm tra quyền nhiều lần tại các endpoint.
- Khi đăng xuất, hủy token hiện tại để vô hiệu hóa phiên làm việc của người dùng.
- Có triển khai các unit test.
- In ra các trường hợp gây lỗi nếu có.
- Các secret key được đưa vào file `.env`.
- Tự động nếu giao dịch bị hết hạn, đơn hàng sẽ được cập nhật về “failed”.
- Có hỗ trợ kiểm tra trạng thái giao dịch để cập nhật VIP cho người dùng.

### Những cái chưa làm được:

- Chưa giới hạn lại số lần đăng nhập thất bại (security).
- Chưa hỗ trợ phân trang và lọc dữ liệu khi lấy danh sách thông tin người dùng hoặc tất cả giao dịch.
- Khi admin xóa tài khoản người dùng, chưa lưu lại hành động xóa để kiểm tra tài khoản khi cần thiết.
- MoMo khi test chưa có redirect lại trang web của hệ thống.
- MoMo API chưa thể sử dụng trong tài khoản thực tế của doanh nghiệp, chỉ dừng lại ở mức test với các dữ liệu ảo trong giai đoạn phát triển (development).
- Chưa thể xác thực số điện thoại có phải người dùng thật không bằng cách gửi OTP SMS về số điện thoại.
- Tuy đã áp dụng interface nhưng việc sử dụng interface vẫn chưa thực sự hiệu quả do chưa có nhiều kinh nghiệm.
- Thiếu một số API khác (như khôi phục dữ liệu hệ thống, tính doanh thu theo thời gian, chưa thể thống kê hệ thống (statistics) dành cho admin).
- Unit test chưa hoàn thiện và chuẩn xác, bao quát hết 100% ở một số endpoint.
- Phần code hiện tại vẫn còn nhiều điểm chưa tối ưu, như cấu trúc chưa thực sự chặt chẽ, xử lý lỗi chưa đồng bộ,...


