### Kiểm tra VPS + DirectAdmin gói này có từ gói VPS nào

#### VPS + DirectAdmin:
- **VPS giá rẻ**: miễn phí từ `cheap-SSD2` trở lên (SSD1 có phí)
- **VPS cao cấp**: tất cả các gói
- **VPS NVME**: tất cả các gói
- **VPS MMO, Minecraft và GPU**: không có DirectAdmin  
  *Tuy nhiên những bản DirectAdmin là bản no lisence (lisence shared), vẫn khyến khích khách hàng sử dụng bản có lisence.*  
### Tìm hiểu sơ qua về DirectAdmin
- **DirectAdmin** là một công cụ quản lý web hosting (tương tự như cPanel, Flesk, aaPanel,...). So với các công cụ khác như cPanel, công cụ quản lý này được đánh giá là nhẹ, tiết kiệm tài nguyên và dễ sử dụng.
- Hỗ trợ nhiều ngôn ngữ, cả tiếng Việt. Nó cung cấp nhiều tính năng như:
  - Quản lý domain, DNS, FTP, MySQL
  - Tạo email theo tên miền, SSH, SSL
  - Quản lý file thông qua File Manager.
- Hoạt động tốt trên **Hệ điều hành Linux** và không hỗ trợ trên hệ điều hành windows.

#### Những cấp độ user trong DirectAdmin
- **Administrator**:
  - Cấp độ user cao nhất của DirectAdmin  
  - Có quyền chỉnh sửa và thay đổi cấu hình của toàn bộ hệ thống  
  - Có thể tạo và quản lí Reseller và User  

- **Reseller**:
  - Cấp bật sau Administrator  
  - Chỉ có quyền quản trị và thay đổi cấu hình của nhóm User mà Reseller tạo ra  
  - Không có quyền can thiệp vào cấu hình của các Reseller khác  

- **User**:
  - Cấp độ có quyền hạn thấp nhất  
  - Được tạo ra bởi Admin hoặc Reseller  
  - Chỉ có quyền thay đổi thông tin liên quan đến tài khoản của mình  

### Giao diện các tính năng DirectAdmin
Ở Level Admin, **DirectAdmin** có 6 mục chính như sau:

#### 1. **Account Manager**
Liên quan đến các vấn đề về tài khoản:
- Tạo tài khoản **Administrator**
- Thay đổi mật khẩu
- Tạo **Reseller**
- Quản lý gói của **Reseller**
- Liệt kê người dùng của **Admin** hoặc **Reseller**
- Hiển thị tất cả người dùng
- Di chuyển người dùng giữa các **Reseller**

---

#### 2. **Server Manager**
Quản lý phía server và các dịch vụ:
- **Administrator Setting**: Cài đặt của Admin lên toàn bộ hệ thống.
- Tùy chỉnh cấu hình các dịch vụ như:
  - **HTTPD**
  - **DNS Administrator**
  - **IP Management**
  - **Multi Server Setup**
  - **PHP config**
  - **SSH key**

---

#### 3. **Admin Tool**
Các công cụ hỗ trợ việc quản lý server:
- **Backup/Transfer**
- **Brute Force Monitor**
- **Process Monitor**
- **Mail Queue**
- **Service Monitor**
- **System Backup**
- **Customize Evolution Skin**

---

#### 4. **System Info & File**
Thông tin hệ thống và các tập tin:
- **Tự động hóa công việc của tất cả người dùng**
- **Quản lý file, chỉnh sửa file**
- **Thông tin hệ thống**
- **Log**
- **Thống kê sử dụng**

---

#### 5. **Extra Features**
Các tính năng mở rộng:
- **Webmail** (RoundCube)
- **phpMyAdmin**
- **Plugin Manager**
- **Security & Firewall**
- **CustomBuild**

---

#### 6. **Support & Help**
Các công cụ hỗ trợ và giúp đỡ:
- **Help**
- **Manage Tickets**
- **Licensing/Update**

***Tuy nhiên, mỗi Level thì người dùng sẽ có khác biệt về giao diện cũng như tính năng.***

### Kiểm tra Webstack trên DirectAdmin  

Web Stack mặc định trên DirectAdmin là Nginx Reverse Proxy LAMP Stack.

#### Lí do
Ta vào phần **Service Monitor** để xem các dịch vụ đang chạy trên máy.

![image](https://github.com/user-attachments/assets/ee0220c2-0fdb-4c3f-b0f7-feeac5164d7b)

Ta thấy được các dịch vụ đang chạy bao gồm:
- SMTP
- DirectAdmin
- Dovecot
- Exim
- Httpd
- lfd
- MySQL
- Named
- Nginx
- PHP-fpm56
- Pure_FTP
- SSH  

![image](https://github.com/user-attachments/assets/ad8c9a7c-8b79-4cac-a4ae-72f0a8411051)

Tuy nhiên, ở đây ta thấy có hai dịch vụ Web server đang chạy là **Nginx** và **Apache**.

Ta kiểm tra tiếp port mà 2 Service Web hiện tại đang chạy.

![image](https://github.com/user-attachments/assets/f1d1d7e2-3899-492b-915e-9a54b4351c3a)

- Hiện tại, Nginx đang chạy ở **port 80 và 443** giao thức **http và https** và **Apache** đang chạy ở 2 port là 8080 và 8081. Khả năng có thể là Mô hình LEMP stack hoặc Nginx Reverse Proxy LAMP stack (Do Nginx đang chạy ở port web thông dụng).  
- Ta kiểm tra file nginx-vhost.conf. Ở phần location ta thấy có proxy_pass (Nginx thực hiện chuyển dữ liệu tới backend) -> Nginx có chức năng là Proxy.

![image](https://github.com/user-attachments/assets/3fe66836-b615-48ba-bc75-17affdbd4cee)

![image](https://github.com/user-attachments/assets/dc41435e-8e0e-47ea-833a-4f051160cc27)

Cách nhanh hơn là ở phần Server Manager chỉ có phần Custom HTTPD configurations mà không có nhắc gì đến Nginx. Khi nhấp vào thì ta thấy conf. file là httpd.conf nginx proxy.

![image](https://github.com/user-attachments/assets/27f7625c-5f23-4058-8f8a-9bc76813cea0)

![image](https://github.com/user-attachments/assets/ec1a5d95-e70a-45fe-985e-6bce0f080559)

### Cài đặt WordPress trên DA
Em sẽ dùng user Admin ở level user để cài đặt WordPress và domain là `project2.nguyenhv.id.vn`. Đây là user có quyền cao nhất, và khi tạo một user thì ta cần là reseller hoặc admin chuyển sang level reseller và tạo package trước khi tạo user.

Đầu tiên vào phần file manager trong **System Info & File**.

![image](https://github.com/user-attachments/assets/a27771dc-0b99-4656-83ec-a4046c8a7f78)

Vào folder `public_html` upload mã nguồn WordPress (nếu chưa giải nén thì giải nén và y tất cả mã nguồn ra `public_html/`).

![image](https://github.com/user-attachments/assets/d38331c1-2f2c-416e-beed-dfb957430fdb)

Tạo user, database và gán quyền cho cho user trên database đó.

![image](https://github.com/user-attachments/assets/26ea8d33-3796-4bdf-83c2-2f161ff5b18b)

![image](https://github.com/user-attachments/assets/dbe7023e-17b2-4ff0-b6f0-477b66699253)

Ta truy cập vào URL: `http://project2.nguyenhv.id.vn/wp-admin` để setup WordPress.

![image](https://github.com/user-attachments/assets/08c179c4-3b0c-4531-b599-0e7b6fc3e8a9)

Sau khi setup database username mật khẩu và thông tin quản trị website.

![image](https://github.com/user-attachments/assets/299d48bb-13e8-4c0f-9059-7577ef134766)

### CustomBuild
**CustomBuild** là một công cụ được sử dụng để cài đặt và quản lý các phần mềm và dịch vụ trên máy chủ bằng giao diện đồ họa. Một số tính năng của CustomBuild như:
- Cài đặt, xóa và cập nhật phần mềm
- Chỉnh sửa các thông số của dịch vụ
- Chỉnh sửa phiên bản
- Cập nhật cấu hình dịch vụ
- Xây dựng thêm các chức năng

Việc sử dụng **CustomBuild** giúp quản lý các dịch vụ trở nên dễ dàng và trực quan hơn, tuy nhiên nếu cần cấu hình nâng cao thì cần vào giao diện dòng lệnh để cấu hình.
