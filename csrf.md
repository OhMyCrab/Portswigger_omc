Ứng dụng cung cấp chức năng cho người dùng thay đổi email qua endpoint:

POST /my-account/change-email


Khi người dùng gửi request thay đổi email, server không triển khai bất kỳ biện pháp bảo vệ CSRF nào, cụ thể:

Không có CSRF token

Không kiểm tra Origin hoặc Referer header

Cookie session của user được gửi ở chế độ mặc định SameSite=None/Lax, cho phép gửi qua cross-site request

Server chấp nhận mọi Content-Type, bao gồm cả text/plain (dễ khai thác)

Điều này cho phép attacker tạo một trang độc hại chứa form tự submit để ép trình duyệt nạn nhân gửi yêu cầu đổi email mà không cần thao tác.

📌 Luồng tấn công

Kẻ tấn công:

Tạo một trang HTML độc hại (host trên Exploit Server)

Gửi link này cho nạn nhân

Khi nạn nhân truy cập, trình duyệt của nạn nhân tự động submit form, gửi request đổi email đến server

Server xử lý request như thể nạn nhân tự gửi → email bị thay đổi

Do server không kiểm tra CSRF token hay Origin → tấn công thành công 100%.

📌 PoC (Proof of Concept)
1. Mở lab và đăng nhập tài khoản nạn nhân (bot)

Khai thác từ tài khoản attacker trong Exploit Server.

2. Truy cập Exploit Server và tạo payload

Body HTML độc hại:

<form id="autosubmit" action="https://TARGET.web-security-academy.net/my-account/change-email" method="POST" enctype="text/plain">
  <input name="email" type="hidden" value="attacker@example.com" />
</form>

<script>
  document.getElementById("autosubmit").submit();
</script>

3. Lưu exploit (Store)
4. Gửi exploit đến nạn nhân

Nhấn Deliver exploit to victim
→ Bot (nạn nhân) sẽ truy cập trang
→ Form auto-submit gửi request đổi email.

5. Kết quả

Email của tài khoản nạn nhân bị đổi sang attacker@example.com.

Server phản hồi trạng thái 200 OK, xác nhận thay đổi thành công.

<img width="798" height="399" alt="image" src="https://github.com/user-attachments/assets/ed933cce-8b9f-48c6-9fd3-9a5420b074de" />

<img width="751" height="357" alt="image" src="https://github.com/user-attachments/assets/fb0e73f9-adc6-4fed-8dfc-70a6fe06e3c4" />

<img width="984" height="321" alt="image" src="https://github.com/user-attachments/assets/2778a7a7-0c63-4dda-918c-1b962342d99d" />
