# Cross-site scripting
## Apprentice
### Lab Reflected XSS into HTML context with nothing encoded

- URL

https://0a6700640377321d80b71c4000f600d9.web-security-academy.net/?search=a

- Loại lỗ hổng

Reflected Cross-Site Scripting

- Mô tả chi tiết (Detailed description)

Giá trị của tham số a được chèn trực tiếp vào nội dung HTML mà không filter/escape các ký tự đặc biệt. Do đó, kẻ tấn công có thể gửi một payload chứa thẻ <script> để thực thi JavaScript trong trình duyệt của nạn nhân khi nạn nhân mở URL chứa payload đó.

- PoC

1. Bật Burp Suite, truy cập trang tìm kiếm để tạo request tới /search.

2. Bắt request bằng Proxy và send to Repeater.

3. Thay a bằng payload: <code><script>alert(1)</script></code>

GET /?search=a HTTP/2 → GET /?search=<code><script>alert(1)</script></code> HTTP/2

4. Send → kiểm tra response:

<img width="916" height="755" alt="image" src="https://github.com/user-attachments/assets/c286ed34-31b7-43af-af12-c95fc264fca2" />

<img width="932" height="561" alt="image" src="https://github.com/user-attachments/assets/e33d82b9-2cf4-4ae6-91dc-b6fe198a3f72" />

### Lab Stored XSS into HTML context with nothing encoded

-URL

https://0a4500e203175ec9809608ed001100e3.web-security-academy.net/post/comment

- Loại lỗ hổng

Stored Cross-Site Scripting

- Mô tả chi tiết

Giá trị của trường comment và name được người dùng gửi qua form bình luận, được lưu lại trong cơ sở dữ liệu và có thể được render nguyên vẹn vào HTML trả về khi trang bài được tải mà không có bất kỳ filter/escape ký tự đặc biệt nào. Do đó kẻ tấn công có thể lưu một payload chứa thẻ <script> (ví dụ <script>alert(1)</script>) vào cơ sở dữ liệu; payload này sẽ được trình duyệt của mọi người truy cập trang bài parse và thực thi, gây ra Stored Cross-Site Scripting.

- PoC

1. Bật Burp Proxy, mở form bình luận để tạo request POST tới endpoint comment.

2. Bắt request bằng Proxy và send to Repeater.

3. Thay body (tham số) bằng payload:

csrf=ZJVq5Yx7wlJB38HHaBivutfzYNqXEMfk&postId=9&comment=<code><script>alert(1)</script></code>&name=<code><script>alert(1)</script></code>&email=aa%40gmail.com&website=https%3A%2F%2Fa.com

<img width="450" height="561" alt="image" src="https://github.com/user-attachments/assets/0329e4c4-a03e-4110-9372-d7499fd36759" />


4. Send → mở lại trang bài → kiểm tra response:

<img width="907" height="550" alt="image" src="https://github.com/user-attachments/assets/3703e6fb-74dd-46d0-bf43-b31b08759023" />

- name đã được HTML-encoded (hiện &lt;script&gt;...) → không thực thi.

- comment không được encoded và chứa <script> → sẽ chạy khi tải trang -> Stored XSS.

### Lab Reflected XSS into attribute with angle brackets HTML-encoded

- URL

https://0a8600e4033fc2f981fc2536009b0013.web-security-academy.net/?search=aaa

- Loại lỗ hổng

Reflected Cross-Site Scripting — attribute injection / attribute-breaking (event handler onmouseover).

- Mô tả chi tiết (Detailed description)

Giá trị tham số search được phản chiếu vào một thuộc tính HTML mà không escape dấu nháy. Payload chứa dấu nháy " để đóng attribute hiện tại và thêm thuộc tính sự kiện onmouseover="alert(1)". Khi textbox được người dùng di chuột vào, trình duyệt sẽ gọi handler và thực thi alert(1) → Reflected XSS

<img width="649" height="497" alt="image" src="https://github.com/user-attachments/assets/3cc6fdce-38a7-415c-abc4-695643bafec3" />

- PoC

1. Bật Burp Suite, truy cập trang tìm kiếm để tạo request tới /search.

2.  Bắt request bằng Proxy và send to Repeater.

3. Trong Repeater, thay "onmouseover="alert(1) vào sau aaa:

GET /?search=aaa"onmouseover="alert(1) HTTP/1.1

4. Send và quan sát response

<img width="410" height="185" alt="image" src="https://github.com/user-attachments/assets/ccdc5d91-63ba-4f78-ac1c-83e36f3cc66d" />

<img width="849" height="577" alt="image" src="https://github.com/user-attachments/assets/f3479cab-c754-4d7a-9f80-a0ef91fff13a" />

### DOM XSS in document.write sink using source location.search


