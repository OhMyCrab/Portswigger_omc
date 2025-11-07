### 1.Username enumeration via different responses

- Mô tả:

Ứng dụng dễ bị tấn công bằng cách liệt kê tên người dùng và tấn công dò mật khẩu, có một tài khoản với tên người dùng và mật khẩu dễ đoán, có thể được tìm thấy trong danh sách từ sau:

Candidate usernames

Candidate passwords

Để giải quyết, hãy liệt kê tên người dùng hợp lệ, dùng phương pháp tấn công brute force, sau đó truy cập vào trang tài khoản của họ.

- POC:

1. Gửi 1 request đăng nhập

<img width="767" height="485" alt="image" src="https://github.com/user-attachments/assets/9f3f0e5e-8c76-4c94-aace-293e48447ea4" />

2. Send to intruder và add 2 tham số của username và password

<img width="1082" height="535" alt="image" src="https://github.com/user-attachments/assets/39a6e618-21b7-4935-b14f-3df42c9d7df0" />

3.  Sau khi chạy phát hiện username ak incorrect password

<img width="954" height="345" alt="image" src="https://github.com/user-attachments/assets/be7a47d9-0c97-4bf3-a689-e56b9e58831d" />

4. Quét password của username ak

<img width="1894" height="523" alt="image" src="https://github.com/user-attachments/assets/1bb31428-59e1-4370-8d61-8df2fae39e32" />

5. Có 1 request không xuất hiện incorrect password

<img width="929" height="619" alt="image" src="https://github.com/user-attachments/assets/827da629-ad9a-4009-b044-ea168f21949e" />

4. Đăng nhập vào tài khoản ak với password daniel

<img width="928" height="570" alt="image" src="https://github.com/user-attachments/assets/215c62c4-aa98-4e8b-80dd-dd1cdfdba294" />
