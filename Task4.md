# Khái niệm cơ bản về Exim và Dovecot
### Exim ###
  Exim là một trong phần mềm máy chủ email được sử dụng nhiều nhất cho các hệ điều hành giống Unix, chẳng hạn như Linux và Solaris,….Nó đóng vai trò như một Mail Transfer Agent (MTA) có nhiệm vụ chuyển mail từ người gửi đến đến người nhận thông qua máy chủ mail    
  *Một số ưu điểm của Exim*:  
    + Cấu hình linh hoạt: Exim cho phép người quản trị cấu hình chi tiết cách thức gửi nhận email, như các quy tắc bảo mật, xử lý bưu kiện email, hạn chế spam,...   
    + Quản lý hàng đợi email: Exim cho phép quản lí email trong hàng đợi queue, giúp the dõi và kiểm tra các email bị lỗi và không thể gửi đi  
    + Khả năng mở rộng: Exim có thể được tích hợp với các công cụ khác như spam filtering, antivirus và các hệ thống phân phối email phức tạp.  
### Dovecot
  Dovecot là một MDA (Mail Delivery Agent) là một máy chủ IMAP và POP3 nguồn mở dành cho các hệ điều hành giống Unix cung cấp quyền truy cập cho email qua giao 2 giao thức trên  
# DKIM, SPF, PTR
### DKIM
  Là phương thức xác thực Mail, được thiết kế để phát hiện mail giả mạo  
  *Các hoạt động*: theo 2 giai đoạn là ký (máy chủ gửi) và xác thực (máy chủ nhận)  
  Sử dụng bảng ghi DNS để xác thực email
  Khi tạo cặp khóa private và public. Khóa riêng sẽ được thêm vào máy chủ gửi mail và sử dụng nó để ký mail trước khi gửi đi nó được gắn vào phần header của mail. Mail Server của người nhận kiểm tra chữ kí DKIM bằng khóa công khai trong DNS của domain người  
### SPF  
  Xác định Server nào được phép gửi Mail từ một tên miền cụ . Mục đích dùng để xác thực mail kiểm tra tính hợp phép của Mail   
  Quản trị viên sẽ thiết lập bảng ghi TXT trong DNS để khai báo danh sách các máy chủ có quyền gửi Email thay mặc cho domain đó. Khi nhận được Mail máy chủ nhận   sẽ lấy domain trong mail sau đó truy vấn DNS của example.com để lấy bảng ghi SPF nó tìm bảng ghi TXT chứa v=spf1 và lấy danh sách IP cho phép sau đó kiểm tra địa chỉ IP của máy chủ gửi có khớp với địa chỉ IP trong danh sách hay không  
  ## PTR (bảng ghi Pointer)  
  Dùng để tra cứu tên miền từ địa chỉ IP, có ứng dụng trong SPF trong việc dịch địa chỉ sang tên miền được phép gửi mail để tra cứu tính hợp lệ  
  
# Cấu hình VPS Mail Server với Exim và Dovecot
### Cấu hình Exim  
  Tải repo Epel `dnf installl -y epel-release`  
  `dnf install -y exim`  
  Thay đổi tập tin cấu hình của exim `/etc/exim/exim.conf`    
   Thêm mới dòng 4 5  
  + `untrusted_set_sender = * : Exim không thay đổi địa chỉ người gửi trong trường hợp nó không xác minh được địa chỉ từ một nguồn đáng tin  
  + `no_local_from_check` : Exim sẽ không kiểm tra và xử lí các thư có địa chỉ là nội bộ (mặc định trust) 
  
  ![Screenshot 2025-02-14 084337](https://github.com/user-attachments/assets/a009dbd6-39a4-404f-8b1b-4838f19272a4)

  + thiết lập hostname của máy chủ exim

    ![Screenshot 2025-02-14 091616](https://github.com/user-attachments/assets/fb86dca8-99c1-4bbb-9f4e-aa355b6770d3)

  ![Screenshot 2025-02-14 091539](https://github.com/user-attachments/assets/a8783588-0a57-4415-82b4-940dbc02586a)

  + Thiết lập tên domain cho máy chủ Mail
    
![Screenshot 2025-02-14 091724](https://github.com/user-attachments/assets/db00f602-5b75-4b2c-8e56-c9187df988ca)

  + local delivery: là cấu hình liên quan đến việc xử lí thư nội bộ, tức là thư được gửi đến cùng tên. Ý nghĩa của các thông số như:  
      `driver = appendfile`: Exim sử dụng phương thức appendfile để lưu tiệp, phương thức này sẽ lưu nội dung vào cuối tệp thay vì ghi đè    
      `directory = $home/Maildir`: chỉ định vị trí lưu Mail. Trong thư mục Maildir ở thư mục người dùng  
      `maildir_format`: Chỉ định lưu theo format mail  
      `maildir_use_size_file`: Exim có thể tạo và sử dụng một tệp size để lưu trữ kích thước của các thư trong thư mục, giúp theo dõi và quản lý dung lượng thư  
      `delivery_date_add`: Yêu cầu thêm ngày giờ vào thư khi thực hiện gửi  
      `envelope_to_add`: Thêm địa chỉ người nhận vào trong thư   
      `return_path_add`:  Return-Path là địa chỉ mà nếu có lỗi hoặc không thể gửi thư, thư sẽ được trả lại.  
      `group = mail`: chỉ định nhóm người dùng mà thư sẽ được lưu dưới quyền truy cập. Thư sẽ có quyền truy cập cho nhóm mail  
      `mode = 0660`: quyền truy cập thư mục  
        
![image](https://github.com/user-attachments/assets/61094d62-f319-43b4-b887-762a4c690d24) 

+ dovecot_login và dovecot_plan: đây là phương thức xác Exim xác thực dovecot để lấy mail
  *Sự khác nhau giữa login và plain là: login gửi username và password có mã hóa còn plain là không mã hóa
  ![image](https://github.com/user-attachments/assets/1adaef76-6a30-4d18-b165-e5e1eb2fc41a)

+ Kiểm tra cấu hình Exim: `exim -C /etc/exim/exim.conf -bV`  
  ![image](https://github.com/user-attachments/assets/2c6cbb63-d6f7-43cf-8c12-8f9ab16cc780)

### Cấu hình Dovecot
`dnf install -y dovecot`  
+ Chỉnh sử xác thực trong tệp `/etc/dovecot/conf.d/10-auth.conf`  
   Cấm xác thực dưới dạng Plain text và kiểu xác thực login
  ![image](https://github.com/user-attachments/assets/6b038b72-0c8e-4bea-81e6-eff9dfe11125)
  ![image](https://github.com/user-attachments/assets/0e1e4fe5-2912-45fb-9225-8b72292bb595)

+ Chỉ định đường dẫn lấy Mail trong tệp `/etc/dovecot/conf.d/10-mail.conf`  
  ![image](https://github.com/user-attachments/assets/b538c3ad-d9ac-4e4e-892b-a814ca3e6884)

+ Quản lí kết nối đến máy chủ khác trong tệp `/etc/dovecot/conf.d/10-master.conf`  
  Thêm nội dung service auth service auth trong service auth-worker dùng để tạo socket liên lạc với exim  
  ![image](https://github.com/user-attachments/assets/3113def9-d0fb-4fb3-aab4-1d2fe08b35f8)

  bật dovecot và exim bằng systemd  

  # Test gửi nhận Mail
   ### 1 Đăng nhập trên thunderbird
  ![image](https://github.com/user-attachments/assets/1c40da61-16bb-499c-abaf-931d830b9c7a)  

  ### 2 Gửi Nhận Mail
  ![image](https://github.com/user-attachments/assets/6cf81131-cb00-4d4f-a559-11b007678259)  

![image](https://github.com/user-attachments/assets/49c37752-c377-4cf7-beff-3e9a7741b607)  


  
  






  




