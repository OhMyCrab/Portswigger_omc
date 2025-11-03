### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

- Mô tả:

Khi người dùng chọn một danh mục, ứng dụng sẽ thực hiện một truy vấn SQL:

SELECT * FROM products WHERE category = 'Gifts' AND released = 1

Khi nhấp vào các nút, kết quả sẽ được lọc theo danh mục:

<img width="1882" height="589" alt="image" src="https://github.com/user-attachments/assets/8b1cd033-f310-47da-9d81-d0f922e1a683" />

- POC:

1. Mở trang lab trong trình duyệt: https://<lab-host>/filter?category=Gifts và bật Burp Proxy để bắt request.

2. Thao tác chọn danh mục “Gifts” trên giao diện để tạo request, hoặc mở URL gốc.

3. Trong Proxy → HTTP history, tìm request GET tới /filter với tham số category. Send to Repeater.

4. Trong Repeater, sửa tham số category thành payload nhằm phá vỡ chuỗi và thêm điều kiện luôn đúng

/filter?category=Accessories'OR 1=1-- - 

5. Send request trong Repeater và quan sát response trả về

<img width="1887" height="755" alt="image" src="https://github.com/user-attachments/assets/33d8e050-e165-4d80-aef1-e41e84dc81d7" />
