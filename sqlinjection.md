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

### SQL injection vulnerability allowing login bypass

- Mô tả:

Bài lab có lỗ hổng SQL injection trong chức năng đăng nhập. Để hoàn thành phải đăng nhập vào tài khoản administrator

- POC:

1. Mở trang login

2. Username, viết payload ' OR 1=1-- - sau administrator

<img width="926" height="382" alt="image" src="https://github.com/user-attachments/assets/ede1d34c-cdef-496a-830f-c9900d80d538" />

3. Password, nhập chuỗi bất kỳ .

4. Nhấn Login.

5. Quan sát kết quả

<img width="940" height="564" alt="image" src="https://github.com/user-attachments/assets/c8e1dd77-d988-4323-9123-796db233b7e2" />

### SQL injection attack, querying the database type and version on Oracle

- Mô tả:

Mục tiêu của bài lab: hiển thị chuỗi phiên bản cơ sở dữ liệu (database version string). Trên Oracle, mọi SELECT phải chỉ rõ bảng FROM. Nếu UNION SELECT không truy vấn từ bảng nào, cần thêm FROM <table>, Oracle có bảng tích hợp tên dual để dùng trong trường hợp này.

- POC:

1. Kiểm tra số cột

/filter?category=Accessories' union all select NULL,NULL FROM dual-- -

<img width="1885" height="847" alt="image" src="https://github.com/user-attachments/assets/d2acda80-58e1-4291-8f7e-ee47f7767459" />

response không lỗi (2 cột NULL hợp lệ).

2. Lấy version trên Oracle: Oracle có view v$version chứa banner. Vì Oracle bắt buộc phải có FROM nên payload truyền vào sau Accessories:

/filter?category=Accessories'union all select 'aaa',banner FROM v$version-- -

(Giải thích: UNION ALL SELECT 'aaa', banner FROM v$version — cột đầu là một chuỗi tạm để phù hợp 2 cột, cột thứ hai lấy banner từ v$version.)

3. Send và quan sát response

<img width="1853" height="739" alt="image" src="https://github.com/user-attachments/assets/951ccedc-c428-4d59-9a8b-5a166410d269" />
