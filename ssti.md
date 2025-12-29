### 1.Basic server-side template injection

- Mô tả: Bài lab dễ bị tấn công bằng cách chèn mẫu phía máy chủ do việc xây dựng mẫu ERB không an toàn. Để giải bài lab phải xóa tệp morale.txt khỏi thư mục chính của Carlos. Ứng dụng sử dụng ERB template để render nội dung hiển thị cho người dùng nhưng không lọc đầu vào, khiến dữ liệu do người dùng kiểm soát được xử lý như mã Ruby. Cú pháp <%= %> trong ERB cho phép thực thi biểu thức Ruby trong quá trình render template.

- POC:

1. Truy cập trang web vào sản phẩm đầu tiên có hiển thị message Unfortunately this product is out of stock

<img width="843" height="551" alt="image" src="https://github.com/user-attachments/assets/5112f6eb-01c4-4699-85cc-0d102e1471bc" />

2. Send request to repeater

<img width="1252" height="371" alt="image" src="https://github.com/user-attachments/assets/158c250f-1f2c-43dc-97cb-66b9d6ece462" />

3. Thay `Unfortunately%20this%20product%20is%20out%20of%20stock` bằng câu lệnh ruby `<%= File.delete('/home/carlos/morale.txt')%>`

<img width="1423" height="509" alt="image" src="https://github.com/user-attachments/assets/012771d9-afa6-4d57-aee9-483c24c95a82" />

4. Sau khi xóa file, xóa lần nữa đã hiển thị lỗi file không tồn tại và solved this lab

<img width="1508" height="465" alt="image" src="https://github.com/user-attachments/assets/6f93d70a-1d85-4a10-b670-a0d6edb07f44" />

### 2.Basic server-side template injection (code context)

- Mô tả: Bài lab này dễ bị tấn công chèn mẫu phía máy chủ do cách sử dụng mẫu Tornado không an toàn. Để giải quyết bài thực hành cần xóa tệp morale.txt khỏi thư mục chính của Carlos. Ứng dụng sử dụng Tornado template engine với cách render không an toàn, cho phép kẻ tấn công chèn mã template vào ngữ cảnh xử lý phía server. Payload kiểm tra ban đầu `}}{{2*2}}` xác nhận rằng biểu thức template được thực thi.

- POC:

1. Truy cập trang web, vào phần thông tin cá nhân và nhấn submit

<img width="767" height="510" alt="image" src="https://github.com/user-attachments/assets/f58b5cb7-e207-4b64-bfa0-ca7442a9c5d3" />

2. Send request to repeater

<img width="766" height="460" alt="image" src="https://github.com/user-attachments/assets/5bf91fa6-f878-479c-8f09-7ecc39b2c917" />

3. Test payload `}}{{2*2}}` truyền vào blog-post-author-display 

<img width="738" height="422" alt="image" src="https://github.com/user-attachments/assets/67cede68-a6c8-478e-b39a-e282aca3253f" />

<img width="765" height="476" alt="image" src="https://github.com/user-attachments/assets/4decb8cf-5740-44d8-9996-276fb3c8cba6" />

4. Sửa payload `}}{{2*2}}` thành `}}{%+import+os+%}{{os.popen("rm+/home/carlos/morale.txt")}}`

- Lệnh `{% import os %}` cho phép nhúng module os của Python vào template, để có thể gọi các hàm hệ thống.

- Hàm `{{ os.popen("rm+/home/carlos/morale.txt")}}`: `os.popen()`: mở một subprocess chạy lệnh shell. ; `"rm+/home/carlos/morale.txt"`: lệnh xóa file /home/carlos/morale.txt.

<img width="760" height="442" alt="image" src="https://github.com/user-attachments/assets/963b4f6a-d4ae-451b-bc47-92f27eab8395" />

5. Sau khi xóa file, xóa lần nữa đã hiển thị lỗi file không tồn tại và solved this lab

<img width="1222" height="326" alt="image" src="https://github.com/user-attachments/assets/fda886ad-20ae-4309-9491-66e6632f7ed1" />
