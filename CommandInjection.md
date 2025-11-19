### 1.OS command injection, simple case

- Mô tả: Ứng dụng thực thi lệnh shell chứa ID sản phẩm và ID cửa hàng do người dùng cung cấp và trả về kết quả, thực hiện lệnh whoami để xác định tên của người dùng hiện tại.

- POC:

1. Mở trang lab trong trình duyệt: https://0a85006a03f1916a821a74f300100063.web-security-academy.net/product?productId=2 và bật Burp Proxy để bắt request.

<img width="597" height="133" alt="image" src="https://github.com/user-attachments/assets/99dabc43-7f2f-40a8-b83b-7dfcde85bf62" />

2. Trong Repeater, sửa tham số storeID thêm dấu | để nối chuỗi whoami

storeId=1|whoami

3. Send request trong Repeater và quan sát response trả về

<img width="948" height="390" alt="image" src="https://github.com/user-attachments/assets/9bd6c71c-4440-4222-b34f-3e9b1a012f71" />

### 2.Blind OS command injection with time delays

- Mô tả: Ứng dụng thực thi lệnh shell chứa thông tin chi tiết do người dùng cung cấp. Kết quả từ lệnh không được trả về trong phản hồi, khai thác lỗ hổng blind os command injection để gây ra độ trễ 10 giây.

- POC:

1. Mở trang lab trong trình duyệt: https://0a71007603fad2e980947be6007000df.web-security-academy.net/feedback và bật Burp Proxy để bắt request.

<img width="861" height="687" alt="image" src="https://github.com/user-attachments/assets/24410109-6068-4b7e-a37e-fd574c344b98" />

2. Truyền vào tham số name, subject, message đều dính Blind OS command injection, payload truyền vào: "; sleep 10;" hoặc "; ping -c 10 127.0.0.1 ;"

<img width="387" height="69" alt="image" src="https://github.com/user-attachments/assets/3b5793c2-155f-4d55-97a0-0c3633abf027" />

4. Send request trong Repeater và quan sát response trả về

<img width="734" height="690" alt="image" src="https://github.com/user-attachments/assets/abf708d5-aeaf-4b2b-b6b9-b7c504b80cd8" />

