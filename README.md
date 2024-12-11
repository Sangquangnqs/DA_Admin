# DA_Admin
# API Documentation

## Tổng quan
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


