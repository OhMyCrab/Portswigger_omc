### 1.Modifying serialized objects

- Mô tả: Bài lab sử dụng cơ chế phiên dựa trên tuần tự hóa và do đó dễ bị tấn công leo thang đặc quyền. Để giải bài lab cần chỉnh sửa đối tượng được tuần tự hóa trong phiên cookie để khai thác lỗ hổng này và giành quyền quản trị. Sau đó, xóa user carlos.

- POC:

1. Sau khi đăng nhập bằng tài khoản wiener/peter và quan sát cookie

<img width="743" height="324" alt="image" src="https://github.com/user-attachments/assets/6d18e968-6706-48b4-8f45-9d8d225eefaf" />

2. Decode cookie

<img width="327" height="443" alt="image" src="https://github.com/user-attachments/assets/51fa6f3c-0b20-4fd3-a787-26a8c0679dd8" />

3. Thay giá trị b=0 thành b=1

<img width="335" height="419" alt="image" src="https://github.com/user-attachments/assets/85f48dcf-fb25-4ee5-b4d5-c304b748cc4a" />

4. Truy cập admin panel và xóa user carlos

<img width="1266" height="206" alt="image" src="https://github.com/user-attachments/assets/d7b4d7e1-71a5-455e-99b4-0bef283ac110" />

- Phân tích

`O:4:"User":2:{
  s:8:"username";s:6:"wiener";
  s:5:"admin";b:1;
}`

PHP serialize:

O:4: - "User" Object thuộc class User

:2: -	Có 2 property

s:8: - "username";s:6:"wiener";	$this->username = "wiener"

s:5: - "admin";b:1;	$this->admin = true

Trong PHP:

b:0 → false

b:1 → true

-> Server tin dữ liệu deserialize từ client và không kiểm tra lại quyền admin trong database

### 2.Modifying serialized data types

- Mô tả:

- POC:

1. Đăng nhập vào hệ thống với tài khoản thông thường wiener:peter. Sử dụng Burp Suite để chặn (intercept) yêu cầu HTTP và quan sát cookie session.

  Giá trị cookie: Được mã hóa base64.

<img width="805" height="431" alt="image" src="https://github.com/user-attachments/assets/1e86c6ee-d2b5-4931-bd0a-ad940b4b9414" />

2. Bôi đen giá trị cookie và quan sát tab Decoder

  Dữ liệu gốc: `O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"e59n0ymtrqsx0lfslf2wv8z0suqg06vg";}`

  Đối tượng User có hai thuộc tính là username và access_token.

<img width="1872" height="432" alt="image" src="https://github.com/user-attachments/assets/189425d3-5015-4f7c-8799-66684f792e2a" />

3. Chỉnh sửa nội dung đối tượng đã tuần tự hóa để thực hiện leo thang đặc quyền:

  Thay đổi Username: Đổi s:6:"wiener" thành s:13:"administrator" để giả mạo quyền quản trị viên.

  Bypass Access Token: Thay đổi giá trị access_token từ dạng chuỗi (s:32:"...") sang số nguyên bằng 0 (i:0).

  Do PHP sử dụng so sánh lỏng lẻo (==), khi so sánh một chuỗi băm bắt đầu bằng chữ cái với số nguyên 0, kết quả sẽ trả về true.

  Payload sau khi chỉnh sửa: `O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}`

<img width="1526" height="321" alt="image" src="https://github.com/user-attachments/assets/31d68e6a-77da-42b0-b842-39d9dce56d65" />

4. Thực thi Payload và hoàn thành mục tiêu

  Mã hóa lại payload sang base64 và thay thế vào cookie session.

  Thay `/my-account?id=wiener` thành `/admin/delete?username=carlos`.

<img width="783" height="349" alt="image" src="https://github.com/user-attachments/assets/fd5b78cb-8247-461c-9ba5-f7162ad68fda" />

5. Hệ thống xác thực thành công tài khoản quản trị viên thông qua lỗi deserialization, cho phép thực hiện hành động xóa người dùng carlos thành công.

<img width="1271" height="264" alt="image" src="https://github.com/user-attachments/assets/4e6bea70-59b6-42e8-a61b-0bbed3dd69e2" />
