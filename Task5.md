### Kiểm tra VPS + DirectAdmin gói này có từ gói VPS nào
#### VPS + DirectAdmin:
- **VPS giá rẻ**: miễn phí từ `cheap-SSD2` trở lên (SSD1 có phí)
- **VPS cao cấp**: tất cả các gói
- **VPS NVME**: tất cả các gói
- **VPS MMO, Minecraft và GPU**: không có DirectAdmin

### Tìm hiểu sơ qua về DirectAdmin
- **DirectAdmin** là một công cụ quản lý web hosting (tương tự như cPanel, Flesk, aaPanel,...). So với các công cụ khác như cPanel, công cụ quản lý này được đánh giá là nhẹ, tiết kiệm tài nguyên và dễ sử dụng.
- Hỗ trợ nhiều ngôn ngữ, cả tiếng Việt. Nó cung cấp nhiều tính năng như:
  - Quản lý domain, DNS, FTP, MySQL
  - Tạo email theo tên miền, SSH, SSL
  - Quản lý file thông qua File Manager.
- Hoạt động tốt trên **Hệ điều hành Linux** và không được hỗ trợ trên các hệ điều hành khác.

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

Trong **DirectAdmin**, có 6 mục chính như sau:

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
- Tự động hóa công việc của tất cả người dùng
- Quản lý file, chỉnh sửa file
- Thông tin hệ thống
- Log
- Thống kê sử dụng

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

### Kiểm tra Webstack trên directadmin  

### Web Stack mặc định trên DirectAdmin là Nginx Reverse Proxy LAMP Stack

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

Ta kiểm tra tiếp port mà 2 Service Web hiện tại đang chạy

![image](https://github.com/user-attachments/assets/f1d1d7e2-3899-492b-915e-9a54b4351c3a)


  - Hiện tại, Nginx đang chạy ở **port 80 và 443** giao thức **http và https** và **Apache** đang chạy ở 2 port là 8080 và 8081. Khả năng có thể là Mô hình LEMP stack hoặc Nginx Reverse Proxy LAMP stack (Do Nginx đang chạy ở port web thông dụng)  
  - Ta kiểm tra file nginx-vhost.conf  
  Ở phần location ta thấy có proxy_pass (Nginx thực hiện chuyển dữ liệu tới backend) -> Nginx có chức năng là Proxy
![image](https://github.com/user-attachments/assets/3fe66836-b615-48ba-bc75-17affdbd4cee)

![image](https://github.com/user-attachments/assets/dc41435e-8e0e-47ea-833a-4f051160cc27)

Cách nhanh hơn là ở phần Server Manager chỉ có phầm Custom HTTPD configurations mà không có nhắc gì đến Nginx. Khi nhấp vào thì ta thấy conf. file là httpd.conf nginx proxy

![image](https://github.com/user-attachments/assets/27f7625c-5f23-4058-8f8a-9bc76813cea0)

![image](https://github.com/user-attachments/assets/ec1a5d95-e70a-45fe-985e-6bce0f080559)







### CustomBuild
CustomBuild là một công cụ được sử dụng để cài đặt và quản lý các phần mềm và dịch vụ trên máy chủ bằng giao diện đồ họa
Một số tính năng của CustomBuild như: 
  + Cài đặt xóa và cập nhật phần
  + Chỉnh sửa cá thông số của dịch
  + Chỉnh sửa version
  + Cập nhật cấu hình dịch vụ
  + Xây dựng thêm các chức năng
* Việc sử dụng Custombuild giúp quản lí các dịch vụ trở nên dễ dàng và trực quan hơn, tuy nhiên nếu cần cấu hình nâng cao thì cần vào giao diện dòng lệnh để cấu hình
* 

