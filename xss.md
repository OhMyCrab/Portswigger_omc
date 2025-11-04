## Apprentice
### 1.Reflected XSS into HTML context with nothing encoded

- Mô tả

Giá trị của tham số a được chèn trực tiếp vào nội dung HTML mà không filter/escape các ký tự đặc biệt. Do đó, kẻ tấn công có thể gửi một payload chứa thẻ <script> để thực thi JavaScript trong trình duyệt của nạn nhân khi nạn nhân mở URL chứa payload đó.

- PoC

1. Bật Burp Suite, truy cập trang tìm kiếm để tạo request tới /search.

2. Bắt request bằng Proxy và send to Repeater.

3. Thay a bằng payload: <code><script>alert(1)</script></code>

GET /?search=a HTTP/2 → GET /?search=<code><script>alert(1)</script></code> HTTP/2

4. Send → kiểm tra response:

<img width="916" height="755" alt="image" src="https://github.com/user-attachments/assets/c286ed34-31b7-43af-af12-c95fc264fca2" />

<img width="932" height="561" alt="image" src="https://github.com/user-attachments/assets/e33d82b9-2cf4-4ae6-91dc-b6fe198a3f72" />

### 2.Stored XSS into HTML context with nothing encoded

- Mô tả

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

### 3.Reflected XSS into attribute with angle brackets HTML-encoded

- Mô tả

Giá trị tham số search được phản chiếu vào một thuộc tính HTML mà không escape dấu nháy. Payload chứa dấu nháy " để đóng attribute hiện tại và thêm thuộc tính sự kiện onmouseover="alert(1)". Khi textbox được người dùng di chuột vào, trình duyệt sẽ gọi handler và thực thi alert(1) → Reflected XSS

<img width="649" height="497" alt="image" src="https://github.com/user-attachments/assets/3cc6fdce-38a7-415c-abc4-695643bafec3" />

- PoC

1. Bật Burp Suite, truy cập trang tìm kiếm để tạo request tới /search.

2. Bắt request bằng Proxy và send to Repeater.

3. Trong Repeater, thay "onmouseover="alert(1) vào sau aaa:

GET /?search=aaa"onmouseover="alert(1) HTTP/1.1

4. Send và quan sát response

<img width="410" height="185" alt="image" src="https://github.com/user-attachments/assets/ccdc5d91-63ba-4f78-ac1c-83e36f3cc66d" />

<img width="849" height="577" alt="image" src="https://github.com/user-attachments/assets/f3479cab-c754-4d7a-9f80-a0ef91fff13a" />

### 4.DOM XSS in document.write sink using source location.search

- Mô tả

Bài lab sử dụng hàm document.write để ghi dữ liệu ra trang. Hàm document.write này được gọi với dữ liệu từ location.search, có thể kiểm soát bằng cách sử dụng URL của trang web.

- POC

1. Bắt request và quan sát response trả về khi search từ bất kỳ

<img width="705" height="139" alt="image" src="https://github.com/user-attachments/assets/2635c93a-107d-4680-b752-9f69ecfc0261" />

2. Dùng tham số search với một giá trị đóng chuỗi rồi nối mã JavaScript

"> để đóng chuỗi đằng trước, và nối với <script>alert(1)</script>

<img width="1060" height="524" alt="image" src="https://github.com/user-attachments/assets/38350cf4-7f8c-4ab5-9f1b-c3d3df224a19" />

### 5.DOM XSS in innerHTML sink using source location.search

- Mô tả

Bài lab sử dụng innerHTML, thay đổi nội dung HTML của một phần tử div, sử dụng dữ liệu từ location.search.

- POC

1. Bắt request và quan sát response trả về khi search từ bất kỳ

<img width="640" height="143" alt="image" src="https://github.com/user-attachments/assets/a47a6783-543c-49f6-acb9-9d22e8bd8de1" />

Dữ liệu từ URL (search) được đưa thẳng vào innerHTML của phần tử DOM mà không escape, nên attacker có thể gửi HTML/JS trong search thì trình duyệt sẽ thực thi

2. Truyền payload `<img src=a onerror=alert(1) />` trực tiếp vào tham số search

<img width="1327" height="627" alt="image" src="https://github.com/user-attachments/assets/f61275dd-3a53-4d5a-ac89-d283861cd354" />

### 6.DOM XSS in jQuery anchor href attribute sink using location.search source

- Mô tả

Tạo alert "back" document.cookie

- POC

1. Bắt request và quan sát response trả về từ trang feedback

<img width="682" height="116" alt="image" src="https://github.com/user-attachments/assets/eaca447c-a579-4fdb-a706-a2dcc4d12ba8" />

Đoạn code lấy tham số returnPath từ URL hiện tại bằng window.location.search.

→ https://0a7600b304bfa091800b17a1002f004a.web-security-academy.net/feedback?returnPath=/

→ returnPath = /

Sau đó, gán trực tiếp giá trị này vào thuộc tính href của phần tử có id="backLink".

2. Truyền vào tham số returnPath payload javascript:alert(1)

<img width="922" height="985" alt="image" src="https://github.com/user-attachments/assets/464326b7-120f-42af-aac7-37794ad1c693" />

→ $('#backLink').attr("href", "javascript:alert(1)")

3. Khi nhấn nút back đoạn mã alert(1) được thực thi

<img width="971" height="967" alt="image" src="https://github.com/user-attachments/assets/b1b37f57-2c9a-4565-bab6-b5f103151eaa" />

### 7.Stored XSS into anchor href attribute with double quotes HTML-encoded

- Mô tả

Gửi một bình luận có chức năng gọi hàm cảnh báo khi tên tác giả bình luận được nhấp vào

- POC

1. Post 1 comment bình thường và quan sát 

<img width="799" height="619" alt="image" src="https://github.com/user-attachments/assets/420a7004-93d8-42a1-8305-3fcec5b064a7" />

2. Nhấp vào tên tác giả bình luận sẽ link sang trang khác

<img width="542" height="229" alt="image" src="https://github.com/user-attachments/assets/58c275d0-8fdb-4dab-8650-9cdd092f1070" />

3. Khi nhập đường link Website sẽ trở thành href trong thẻ a của tên

<img width="468" height="99" alt="image" src="https://github.com/user-attachments/assets/440b6a00-8e37-416c-9666-73e6e931a0d3" />

Đôi khi ngữ cảnh XSS nằm trong một loại thuộc tính thẻ HTML mà bản thân nó có thể tạo ra một ngữ cảnh có thể lập trình được. Ở đây, có thể thực thi JavaScript mà không cần phải chấm dứt giá trị thuộc tính.

4. Thay thế đường dẫn https://abcd → javascript:alert(1)

<img width="848" height="636" alt="image" src="https://github.com/user-attachments/assets/fee5313d-8fb1-4e52-8664-a820faa05679" />

5. Nhấp vào tên tác giả bình luận sẽ thực thi payload

<img width="882" height="587" alt="image" src="https://github.com/user-attachments/assets/315b801e-68b6-436a-9bae-b52352bf0ea2" />

https: → trình duyệt gửi yêu cầu tới server và tải nội dung (an toàn hơn nếu URL chỉ là một địa chỉ).

javascript: → trình duyệt không tải trang mới, nó chạy trực tiếp mã JavaScript trong ngữ cảnh trang hiện tại

### 8.Reflected XSS into a JavaScript string with angle brackets HTML encoded

- Mô tả

Thoát khỏi chuỗi JavaScript và gọi alert.Một số cách hữu ích để thoát khỏi chuỗi ký tự là:

    '-alert(1)-'

';alert(1)//

- POC

1. gửi chuỗi ký tự bất kỳ và quan sát response trả về

<img width="612" height="77" alt="image" src="https://github.com/user-attachments/assets/586904ef-27d6-4d70-a8cc-67fff2544be8" />

quan sát thấy aaa đang nằm trong chuỗi, 1 số cách hữu ích để thoát khỏi chuỗi và thực thi lệnh javascript: '-alert(1)-' và ';alert(1)//

2. viết payload vào ô search và quan sát kết quả trả về

<img width="884" height="323" alt="image" src="https://github.com/user-attachments/assets/969c4d43-56a0-49c2-9eb9-2acb26729154" />

<img width="1372" height="589" alt="image" src="https://github.com/user-attachments/assets/d571bf2b-2a40-4b0a-b04f-757e31572170" />

### 9.Reflected XSS into a JavaScript string with single quote and backslash escaped

- Mô tả

XSS xảy ra bên trong một chuỗi JavaScript với dấu ' và \ được escaped.

- POC

<img width="1302" height="703" alt="image" src="https://github.com/user-attachments/assets/0b9fb8a5-0bc6-479b-aa64-1b962d028dda" />

