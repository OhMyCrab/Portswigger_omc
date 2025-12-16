### 1.Basic SSRF against the local server

- Mô tả: Bài lab có tính năng kiểm tra kho hàng, lấy dữ liệu từ một hệ thống nội bộ. Để hoàn thành cần thay đổi URL kiểm tra kho hàng để truy cập giao diện quản trị tại http://localhost/admin và xóa người dùng carlos.

- POC:

1. Mở lab https://0a51000203ea0bc9829d5795003b0077.web-security-academy.net/

<img width="880" height="962" alt="image" src="https://github.com/user-attachments/assets/fc548d56-0fa4-4658-8e9a-640d2e872195" />

2. Bắt request chức năng check stock

<img width="1488" height="935" alt="image" src="https://github.com/user-attachments/assets/ae1d5275-23ad-4f57-b680-077aee6c7c8a" />

3. Thay toàn bộ url http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D2 thành http://localhost/admin

<img width="578" height="355" alt="image" src="https://github.com/user-attachments/assets/d44776fa-e0bc-4af9-b590-8d27253b9217" />

<img width="563" height="363" alt="image" src="https://github.com/user-attachments/assets/b21304a4-6689-4f17-8fc6-b6cc12ce665b" />

4. forward request và quan sát response trả về

<img width="1189" height="517" alt="image" src="https://github.com/user-attachments/assets/48117519-2823-45d0-8046-ae769a065d58" />

5. Bắt request và thay url thành http://localhost/admin/delete?username=carlos

<img width="999" height="371" alt="image" src="https://github.com/user-attachments/assets/93578708-976c-454b-8ad3-2c8388007828" />
