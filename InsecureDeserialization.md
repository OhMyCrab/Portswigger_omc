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
