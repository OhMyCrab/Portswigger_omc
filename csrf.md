### 1.CSRF vulnerability with no defenses

- Mô tả: Ứng dụng cung cấp chức năng cho người dùng thay đổi email qua endpoint: POST /my-account/change-email. Khi người dùng gửi request thay đổi email, server không triển khai bất kỳ biện pháp bảo vệ CSRF nào. Điều này cho phép attacker tạo một trang độc hại chứa form tự submit để ép trình duyệt nạn nhân gửi yêu cầu đổi email mà không cần thao tác. Cần tạo một trang HTML độc hại (host trên Exploit Server), sau đó nhấn Deliver exploit to victim. Khi bot truy cập, trình duyệt tự động submit form, gửi request đổi email đến server

- POC:
1. Mở lab và đăng nhập tài khoản wiener:peter, sau đó thử đổi gmail

<img width="798" height="399" alt="image" src="https://github.com/user-attachments/assets/ed933cce-8b9f-48c6-9fd3-9a5420b074de" />

<img width="751" height="357" alt="image" src="https://github.com/user-attachments/assets/fb0e73f9-adc6-4fed-8dfc-70a6fe06e3c4" />

2. Truy cập Exploit Server và tạo payload

<img width="984" height="321" alt="image" src="https://github.com/user-attachments/assets/2778a7a7-0c63-4dda-918c-1b962342d99d" />

3. Gửi exploit đến nạn nhân bằng cách nhấn Deliver exploit to victim

→ Bot (nạn nhân) sẽ truy cập trang

→ Form auto-submit gửi request đổi email.

4. Kết quả

Email của tài khoản bot bị đổi sang hwllnah@gmail.com và Solved lab

