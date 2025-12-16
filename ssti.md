### 1.Basic server-side template injection

- Mô tả: Bài lab dễ bị tấn công bằng cách chèn mẫu phía máy chủ do việc xây dựng mẫu ERB không an toàn. Để giải bài lab phải xóa tệp morale.txt khỏi thư mục chính của Carlos.

- POC:

1. Truy cập trang web vào sản phẩm đầu tiên có hiển thị message Unfortunately this product is out of stock

<img width="1252" height="371" alt="image" src="https://github.com/user-attachments/assets/158c250f-1f2c-43dc-97cb-66b9d6ece462" />

2. Send request to repeater

<img width="843" height="551" alt="image" src="https://github.com/user-attachments/assets/5112f6eb-01c4-4699-85cc-0d102e1471bc" />

3. Thay Unfortunately%20this%20product%20is%20out%20of%20stock bằng câu lệnh ruby <%= File.delete('/home/carlos/morale.txt')%>

<img width="1423" height="509" alt="image" src="https://github.com/user-attachments/assets/012771d9-afa6-4d57-aee9-483c24c95a82" />

4. Sau khi xóa file, xóa lần nữa đã hiển thị lỗi file không tồn tại và solved this lab

<img width="1508" height="465" alt="image" src="https://github.com/user-attachments/assets/6f93d70a-1d85-4a10-b670-a0d6edb07f44" />

