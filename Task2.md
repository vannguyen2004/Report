# 1. Mô hình LAMP stack

**Khái niệm về Web stack**: là tập hợp những phần mềm được sử dụng để chạy các ứng dụng web động.  
  *Một `Web Stack`* thông thường sẽ bao gồm các thành phần như:  
    + Hệ điều hành.  
    + Máy chủ Web.  
    + Hệ thống quả trị cơ sở dữ liệu.  
    + Ngôn ngữ lập trình.  

### **Khái niệm về LAMP Stack**: 
LAMP là bộ công nghệ bao gồm các thành phần chính của một ứng dụng web động. LAMP là viết tắt của các thành phần sau:
  + **Linux**: Hệ điều hành mã nguồn mở.
  + **Apache**: Máy chủ Web mã nguồn mở.
  + **MySQL/MariaDB**: Hệ quản trị cơ sở dữ liệu quan hệ (RDBMS) mã nguồn mở.
  + **PHP**: Ngôn ngữ lập trình mã nguồn mở dùng để phát triển ứng dụng web.

### *Ưu điểm của LAMP Stack*:
  + **Chi phí thấp** (opensource).
  + **Khả năng tùy chỉnh và tính linh hoạt cao** đối với các phần mềm mã nguồn mở.
  + **Cộng đồng hỗ trợ lớn**.
# 2. Xem các thông tin của VPS
### CPU: 
![Screenshot 2025-02-11 112349](https://github.com/user-attachments/assets/7e5615d7-5a49-4995-b5ab-1efb65ae12d3)
### RAM
![Screenshot 2025-02-11 134051](https://github.com/user-attachments/assets/b340fe2d-d04f-48e9-9394-1f0652814a4f)
### DISK
![Screenshot 2025-02-11 112412](https://github.com/user-attachments/assets/42befdc5-0f3e-4586-a43b-0998b912d2f2)
### Banwidth
![Screenshot 2025-02-11 112316](https://github.com/user-attachments/assets/d419a9b2-0988-4f53-9d43-f5c125af9192)
### IOPS: Sử dụng FIO
IOPS read max= 25k write max= 8k4
![Screenshot 2025-02-11 133826](https://github.com/user-attachments/assets/78868210-e38a-44f8-b8ef-765a0f963939)

# 3. Wordpress
*Khái niệm về wordpress*: là hệ thống mã nguồn mỡ dùng để sản xuất blog và website được viết bằng ngôn ngữ lập trình PHP và cơ sở dữ liệu MySQL. WordPress được coi là một CMS (Content Management System phần mềm giúp người dùng tạo và quản lí sửa đổi nội dung trên một sang web mà không cần phải có kiến thức sâu về lập trình) miễn phí nhưng tốt.  
### Quá trình cài đặt wordpress  
####  Cài đặt web server (apache)
 + `dnf install -y httpd`
 + Kiểm tra lại `rpm -qa | grep httpd`  
   ![Screenshot 2025-02-12 114125](https://github.com/user-attachments/assets/51ac314d-54a3-4c6d-a92d-9b7542c6e703)
 + Khởi động Apache bằng câu lệnh `systemctl enable --now httpd`
#### Cài đặt Database MariaDB
 + `dnf install -y mariadb-server`
 + Thiết lập bộ mã ký tự cho MariaDB sử dụng utf8mb4 phiên bản mở rộng của utf8
   
![Screenshot 2025-02-12 114718](https://github.com/user-attachments/assets/8ebf9cd6-45a7-486a-9688-66c110dd7c20)  
 + Khởi động MariaDB bằng câu lệnh `systemctl enable --now mariadb-server`  
 + Thiết lập cơ sở dữ liệu MariaDB bằng câu lệnh `mysql_secure_installation` sẽ có thông báo hỏi có muốn thay đổi một số thiết lập  

![Screenshot 2025-02-12 115308](https://github.com/user-attachments/assets/496848d5-75b1-42c4-ae25-058542035ae6)

![Screenshot 2025-02-12 115508](https://github.com/user-attachments/assets/ab1c3b66-347a-4697-852a-77a98416d4dd)  

![Screenshot 2025-02-12 115514](https://github.com/user-attachments/assets/9a71f8e6-e1f2-4ab6-98a3-6e1c1ac68bc1)

![Screenshot 2025-02-12 115613](https://github.com/user-attachments/assets/88646e22-da30-4da9-a170-677b21e6473d)

![Screenshot 2025-02-12 115626](https://github.com/user-attachments/assets/7d834836-dcc2-45f4-aeb8-53ee5ec4e30a)

![Screenshot 2025-02-12 115944](https://github.com/user-attachments/assets/b96ee1e3-b05e-4106-a92e-4d0625a58eba)  
 + Sau đó Tạo database và user cho Wordpress, cấp quyền cho user trên database vừa tạo  
 + User = wordpress ; password = wordpress; database = wordpress  
 + Tài khoản này cho phép wordpress lấy dữ liệu từ database
   
![Screenshot 2025-02-12 124051](https://github.com/user-attachments/assets/94b30216-6e95-40c4-a993-bea9339426a2)

#### Thiết lập PHP  
 + Tải PHP về máy bằng câu lệnh: `dnf install -y php`  
 + Tải các module PHP: `dnf -y install php-pear php-mbstring php-pdo php-gd php-mysqlnd php-enchant enchant hunspell`
 + Điều chỉnh các thông số hoạt động của PHP khi xử lí các yêu cầu từ web server trong tệp /etc/php-fpm.d/www.conf.  
 + Sau đó khởi động lại module php-fpm  
   ![Screenshot 2025-02-12 121021](https://github.com/user-attachments/assets/d7e71447-88a7-49fd-b7c2-474e467db106)

#### Tải Wordress và cấu hình  
+ Sử dụng công cụ wget để tải file từ internet  
![Screenshot 2025-02-12 124522](https://github.com/user-attachments/assets/403380d6-303d-4c60-9f4e-b6ebb50294ab)

+ Xã nén file vừa tải vào thư mục /var/www/ bằng câu lệnh `tar zxvf latest.tar.gz -C /var/www/`  
+ Thay đổi quyền truy cập thư mục /var/www/wordpress cho thuộc người dùng apache.
  
![Screenshot 2025-02-12 124721](https://github.com/user-attachments/assets/bf8773c2-49fe-4a38-b5de-abf44244638e)

+ Cấu hình trang web Wordpress trong file `/etc/httpd/conf.d/wordpress.conf`

![Screenshot 2025-02-12 125312](https://github.com/user-attachments/assets/cc9e3e03-8960-43db-8991-c56d1bdd1dca)

+ Timout: thời gian chờ tối đa cho một kết nối
+ ProxyTimeout: Thời gian khi Apache hoạt động như nột Proxy
+ Alias: giúp ánh xạ URL /wordpress đến “/var/www/wordpress”
+ Directory: định nghĩa các quyền cho thư mục
+ Reload http bằng câu lệnh systemctl reload httpd
  
 Sau đó dùng trình duyệt truy cập vào địa chỉ http://103.112.211.194/wordpress/ và bắt đầu thiết lập  
 
![Screenshot 2025-02-12 131515](https://github.com/user-attachments/assets/86691acd-3f1f-4580-b278-c4663af6b553)

# 4. Thêm 2 domain mỗi domain một site  
**Tên Website thứ nhất là `www.web1.vn`**  
**Tên Website thứ nhất là `www.web2.vn`**  
+ Tập tin cấu hình của virtual trong dường dẫn `/etc/httpd/conf.d/vhost.conf`
  *Ngoài ra cần tạo site wordpress thứ 2 giống như phần  **tải wordpress và cấu hình** ở mục 3 Wordpress
   
![Screenshot 2025-02-12 170553](https://github.com/user-attachments/assets/ab38d3ce-b20a-477b-8963-6419d4aa290c)

+ <VirtualHost *:80> định nghĩa các Virtual Hosts  
+ DocumentRoot: Chỉ định thư mục gốc chứa các tệp tin của website  
+ ServerName: Đây là tên miền của website  
+ ErrorLog: Đường dẫn đến tệp log ghi lại các lỗi của website  
+ CustomLog: Đường dẫn đến tệp log ghi lại các yêu cầu của người dùng  
+ Các phần <Directory> chỉ định quyền truy cập và các tùy chọn cho thư mục cụ thể
+ Options FollowSymLinks: Cho phép Apache theo dõi các liên kết tượng trưng (symlinks) trong thư mục này
+ AllowOverride All: Cho phép các tệp .htaccess trong thư mục này ghi đè cấu hình của Apache

  ##### Web1
  
![Screenshot 2025-02-12 171444](https://github.com/user-attachments/assets/155438b7-2893-4a28-98d0-7a0842335c5f)  

  ##### Web2
  
![Screenshot 2025-02-12 171449](https://github.com/user-attachments/assets/cac3f404-ebb2-4d03-8857-361236784896)


