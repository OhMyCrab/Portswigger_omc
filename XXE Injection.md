### 1.Exploiting XXE using external entities to retrieve files

- Mô tả: Bài lab có tính năng "Check stock" phân tích cú pháp đầu vào XML và trả về bất kỳ giá trị không mong muốn nào trong phản hồi. Cần inject an XML external entity để lấy nội dung của tệp /etc/passwd.

- POC:

1. Mở trang lab trong trình duyệt: https://0a85006a03f1916a821a74f300100063.web-security-academy.net/product?productId=2 và bật Burp Proxy để bắt request.

<img width="753" height="978" alt="image" src="https://github.com/user-attachments/assets/37f038ec-4a3d-4df1-8974-c6e32afaa1e3" />

2. Trong request, thêm: `<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>` để định nghĩa entity xxe và thay giá trị của `<productId>` bằng &xxe; 

<img width="773" height="419" alt="image" src="https://github.com/user-attachments/assets/d9427689-d35c-46ed-97e5-3fdcb51f7420" />

3. Send request trong Repeater và quan sát response trả về

<img width="1221" height="530" alt="image" src="https://github.com/user-attachments/assets/32dbccde-13f5-4326-ba1f-65e4766b8a68" />

### 2.Exploiting XXE to perform SSRF attacks

- Mô tả: Ứng dụng có chức năng "Check stock" cho phép người dùng gửi dữ liệu dưới dạng XML. Tuy nhiên, máy chủ không cấu hình đúng bộ phân tích XML, dẫn đến việc không chặn external entity declarations. Điều này tạo ra lỗ hổng XML External Entity (XXE). Trong bài lab này, máy chủ còn chạy EC2 Metadata Service giả lập tại địa chỉ mặc định: http://169.254.169.254/ . Điểm cuối này cung cấp metadata của instance, bao gồm thông tin IAM. Do XXE có thể ép máy chủ gửi request đến một URL tùy ý, nên hacker có thể thực hiện Server-Side Request Forgery (SSRF) nhằm truy xuất metadata. Bằng cách gọi đến: http://169.254.169.254/latest/meta-data/iam/security-credentials/admin . Hacker có thể thu được Access Key ID, Secret Access Key và Token của role IAM gán cho instance.

- POC:

1. Mở trang lab trong trình duyệt: https://0ad200f803ce0a6680580deb00c60016.web-security-academy.net/product?productId=1 và bật Burp Proxy để bắt request.

<img width="764" height="395" alt="image" src="https://github.com/user-attachments/assets/e26e63a1-e949-4cd2-9e1c-ad5408c0e211" />

2. Trong request, thêm: `<!DOCTYPE foo [<!ENTITY xxe SYSTEM "http://169.254.169.254"> ]>` để định nghĩa entity xxe và thay giá trị của `<productId>` bằng &xxe; 

<img width="1185" height="500" alt="image" src="https://github.com/user-attachments/assets/4c86234f-1c61-477f-97c7-cbefc32ef8f5" />

3. Gửi SYSTEM entity tới: http://169.254.169.254/latest/meta-data/iam/security-credentials/

<img width="734" height="572" alt="image" src="https://github.com/user-attachments/assets/3d82fe51-b180-47d7-a4b4-3962985a4d39" />

4. Quan sát response trả về khi gửi SYSTEM entity tới http://169.254.169.254/latest/meta-data/iam/security-credentials/admin

<img width="843" height="616" alt="image" src="https://github.com/user-attachments/assets/8ae57a22-8ed6-4fbb-8ec4-798101aac5bd" />

### 3.Exploiting XXE via image file upload

- Mô tả: Bài lab cho phép người dùng upload ảnh đại diện vào bình luận và sử dụng thư viện Apache Batik để xử lý các tệp ảnh đại diện. Để hoàn thành bài lab, tải lên hình ảnh hiển thị nội dung của tệp /etc/hostname sau khi xử lý.

- POC:

1. Tạo 1 file ảnh xxe.svg chứa payload xxe để in nội dung file /etc/hostname

<img width="604" height="205" alt="image" src="https://github.com/user-attachments/assets/461052c0-0d79-4358-9720-b401dc8c8b3b" />

2. Post comment và f12 để xem phần hình ảnh của bình luận đã gửi

<img width="1377" height="753" alt="image" src="https://github.com/user-attachments/assets/ff4a347c-7c5c-4dab-bca1-f39a05e60121" />

3. Truy cập đường dẫn https://0a7c008d045823a880a7089d009e00c9.web-security-academy.net/post/comment/avatars?filename=1.png để xem ảnh

<img width="1016" height="663" alt="image" src="https://github.com/user-attachments/assets/c9312ad8-d70d-4ec9-b9f3-5453c49ad1a3" />

Giá trị trong ảnh là 351080d9fd17

4. Nhập giá trị trong ảnh vào submit solution

<img width="737" height="614" alt="image" src="https://github.com/user-attachments/assets/fc490eb1-12d2-4abe-868e-b5cf13b6ce43" />

5. Quan sát kết quả trả về

<img width="948" height="190" alt="image" src="https://github.com/user-attachments/assets/6c3f8069-2d4a-43f8-bc21-90a9ad26fe31" />

