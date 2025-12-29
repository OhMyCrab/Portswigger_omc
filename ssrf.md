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

### 2.Basic SSRF against another back-end system

- Mô tả: Bài lab có tính năng kiểm tra kho hàng, lấy dữ liệu từ một hệ thống nội bộ. Để giải bài lab cần sử dụng chức năng kiểm tra kho để quét phạm vi nội bộ 192.168.0.X, tìm giao diện quản trị trên cổng 8080, sau đó sử dụng nó để xóa người dùng carlos.

- POC:

1. Mở lab https://0aae005a03ffc78f83966967000a00b9.web-security-academy.net

2. Bắt request chuyển vào intruder

<img width="1152" height="463" alt="image" src="https://github.com/user-attachments/assets/a0e72d84-9d77-47b8-aa4a-d6d7e07e69bf" />

<img width="822" height="577" alt="image" src="https://github.com/user-attachments/assets/637e2be1-8621-4427-b2b2-1fb029ba7a4d" />

3. Kết thúc intruder attack và quan sát response trả về

<img width="1297" height="473" alt="image" src="https://github.com/user-attachments/assets/66ee6b14-053f-4db6-a0d7-82859cfa5727" />

### 3.SSRF with blacklist-based input filter

- Mô tả: Bài lab này có tính năng kiểm tra kho hàng, lấy dữ liệu từ một hệ thống nội bộ. Để giải quyết bài cần thay đổi URL kiểm tra kho để truy cập giao diện quản trị http://localhost/adminvà xóa người dùng carlos.

- POC:

1. Mở lab https://0af9009f0471efa1813821bd00810014.web-security-academy.net/

2. Bắt request và send to repeater

<img width="754" height="392" alt="image" src="https://github.com/user-attachments/assets/746ede91-cdc0-4c9b-9ac3-5be872c0150d" />

3. encode url admin 2 lần và quan sát response trả về

<img width="1267" height="402" alt="image" src="https://github.com/user-attachments/assets/ec673c64-27e9-4b21-bb5b-62a1861f1c54" />

4. thêm /delete?username=carlos vào http://127.1/%2561%2564%256d%2569%256e và quan sát response trả về

<img width="1463" height="581" alt="image" src="https://github.com/user-attachments/assets/f1db1ec4-44d6-433f-85d5-ae1538893ce0" />



