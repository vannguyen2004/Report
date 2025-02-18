# 1.Khái niệm cơ bản về Exim và Dovecot
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
  
# 2.Cấu hình VPS Mail Server với Exim và Dovecot
### Cấu hình DNS 
![image](https://github.com/user-attachments/assets/debc4373-8f7e-4da0-9a5e-9b7e9244f760)

### Cấu hình Exim  
  Tải repo Epel và exim `dnf installl -y epel-release`   `dnf install -y exim`  
  Thay đổi tập tin cấu hình của exim `/etc/exim/exim.conf`    
   Thêm mới dòng 4 5  
  + `untrusted_set_sender = *` : Exim không thay đổi địa chỉ người gửi trong trường hợmiền. Ý nghĩa của các thông số như:  
1. `driver = appendfile`: Exim sử dụng phương thức `appendfile` để lưu tệp. Phương thức này sẽ lưu nội dung vào cuối tệp thay vì ghi đè lên tệp hiện tại.

2. `directory = $home/Maildir`: Chỉ định vị trí lưu Mail. Thư sẽ được lưu trong thư mục `Maildir` của người dùng.

3. `maildir_format`: Chỉ định Exim lưu email theo định dạng Maildir.

4. `maildir_use_size_file`: Exim có thể tạo và sử dụng một tệp `size` để lưu trữ kích thước của các thư trong thư mục, giúp theo dõi và quản lý dung lượng thư.

5. `delivery_date_add`: Yêu cầu thêm ngày giờ vào thư khi thực hiện gửi.

6. `envelope_to_add`: Thêm địa chỉ người nhận vào trong thư.

7. `return_path_add`: `Return-Path` là địa chỉ mà nếu có lỗi hoặc không thể gửi thư, thư sẽ được trả lại.

8. `group = mail`: Chỉ định nhóm người dùng mà thư sẽ được lưu dưới quyền truy cập. Thư sẽ có quyền truy cập cho nhóm `mail`.

9. `mode = 0660`: Quyền truy cập thư mục, chỉ định quyền đọc và ghi cho chủ sở hữu và nhóm, nhưng không có quyền cho người dùng khác.
        
![image](https://github.com/user-attachments/assets/61094d62-f319-43b4-b887-762a4c690d24) 

+ dovecot_login và dovecot_plan: đây là phương thức xác Exim xác thực dovecot để lấy mail  
  *Sự khác nhau giữa login và plain là: login gửi username và password có mã hóa còn plain là không mã hóa
  
  **dovecot login**  
  `driver = dovecot`: Điều này chỉ ra rằng Exim sử dụng Dovecot làm trình xác thực.
  
  `public_name = LOGIN`: Đây là tên phương thức xác thực công khai mà Exim sử dụng trong giao tiếp SMTP. Khi máy khách yêu cầu xác thực, Exim sẽ quảng bá phương thức LOGIN.
  
  `server_socket = /var/run/dovecot/auth-client`: Đây là đường dẫn tới socket của Dovecot (một tập tin đặc biệt dùng để giao tiếp giữa các chương trình). Đây là nơi Exim sẽ kết nối với DDovecot để thực hiện việc xác thực.
  
  `server_set_id = $auth1`: Đây là một cấu hình để chỉ định ID của người dùng sẽ được xác thực. Biến $auth1 chứa thông tin liên quan đến người dùng sẽ được Dovecot xử lý và xác thực.
    
![{4F89FC75-C284-4928-AF81-E124436E3791}](https://github.com/user-attachments/assets/e37449f3-64b2-463d-83ee-4f270edd24bc)

+ Kiểm tra cấu hình Exim: `exim -C /etc/exim/exim.conf -bV`: được sử dụng để kiểm tra và hiển thị thông tin cấu hình của Exim (một MTA - Mail Transfer Agent) trên hệ thống Linux.
  
![image](https://github.com/user-attachments/assets/2c6cbb63-d6f7-43cf-8c12-8f9ab16cc780)

### Cấu hình Dovecot
`dnf install -y dovecot`  
+ Chỉnh sử xác thực trong tệp `/etc/dovecot/conf.d/10-auth.conf`  
  chỉ định các cơ chế xác thực mà Dovecot sẽ hỗ trợ khi người dùng kết nối đến máy chủ qua IMAP hoặc POP3.  
  
![image](https://github.com/user-attachments/assets/01a7ea99-e963-4250-9e64-735386e516d8)

![image](https://github.com/user-attachments/assets/860b5a2b-b940-4349-b284-4bc0d84a2f97)


+ Chỉ định đường dẫn lấy Mail trong tệp `/etc/dovecot/conf.d/10-mail.conf`  
![image](https://github.com/user-attachments/assets/b538c3ad-d9ac-4e4e-892b-a814ca3e6884)

+ Quản lí kết nối đến máy chủ khác trong tệp `/etc/dovecot/conf.d/10-master.conf`  
  Thêm nội dung service auth service auth trong service auth-worker dùng để tạo socket liên lạc với exim  
![image](https://github.com/user-attachments/assets/3113def9-d0fb-4fb3-aab4-1d2fe08b35f8)

  bật dovecot và exim bằng systemd  

  # 3.Test gửi nhận Mail
### 1 Đăng nhập trên thunderbird
![image](https://github.com/user-attachments/assets/1c40da61-16bb-499c-abaf-931d830b9c7a)  

### 2 Gửi Nhận Mail
![image](https://github.com/user-attachments/assets/436aedb6-df74-426a-8283-406515bd3b85)

#### Phân tích gửi mail
![image](https://github.com/user-attachments/assets/17041489-9eb0-4fcc-bf38-88700e67b2e0)

#### Giải thích Log Exim

Dưới đây là giải thích chi tiết về các trường trong log Exim mà bạn cung cấp:

##### Dòng đầu tiên trong log

###### Giải thích:

1. **2025-02-18 01:11:48**: Thời gian email được xử lý (ngày tháng và giờ).
2. **1tk5au-000000000g2-1HNZ**: ID duy nhất của email trong hệ thống Exim, dùng để theo dõi email.
3. **<= test1@nguyenhv.id.vn**: Người gửi email là **test1@nguyenhv.id.vn**.
4. **H=([192.168.1.104])**: Địa chỉ IP hoặc hostname của máy gửi email là **[192.168.1.104]**.
5. **[171.252.154.180]**: Địa chỉ IP nguồn của máy gửi email là **171.252.154.180**.
6. **P=esmtpsa**: Phương thức kết nối SMTP bảo mật sử dụng **STARTTLS và xác thực** (SMTP over TLS với xác thực người dùng).
7. **X=TLS1.3:TLS_AES_128_GCM_SHA256:128**: Chi tiết về **cipher suite** được sử dụng:
   - **TLS1.3**: Phiên bản TLS được sử dụng.
   - **TLS_AES_128_GCM_SHA256**: Cơ chế mã hóa AES 128-bit trong chế độ GCM với SHA-256 là hàm băm.
8. **CV=no**: Không kiểm tra chứng chỉ TLS (chứng chỉ không được kiểm tra trong kết nối này).
9. **A=dovecot_plain:test1**: Cơ chế xác thực sử dụng là **dovecot_plain**, với người dùng **test1**.
10. **S=640**: Kích thước của email là **640 byte**.
11. **id=b791c657-59a3-4dcc-ad45-3addfdda736d@nguyenhv.id.vn**: Mã định danh duy nhất (message ID) của email này.

###### Dòng thứ hai trong log  

###### Giải thích:

1. **=> test2 <test2@nguyenhv.id.vn>**: Email được gửi đến người nhận **test2@nguyenhv.id.vn**.
2. **R=localuser**: Đây là phương thức định tuyến (route) email, có nghĩa là email sẽ được chuyển đến người dùng nội bộ **localuser** (người nhận trên cùng một máy chủ).
3. **T=local_delivery**: Transport type là **local_delivery**, có nghĩa là email sẽ được giao cho người nhận nội bộ trong hệ thống.

##### Dòng thứ ba trong log  

1. **Completed**: Quá trình xử lý email đã hoàn tất, tức là email đã được gửi đi và giao đến người nhận thành công.

---
#### Phân tích nhận Mail
![image](https://github.com/user-attachments/assets/50224b00-730e-4a5e-8af2-04505844bf19)

![image](https://github.com/user-attachments/assets/6a9f005c-3a01-40cd-98df-a4ce69ad3c64)

##### Log nhận Mail của Dovecot
![image](https://github.com/user-attachments/assets/b165c867-a1c8-4935-8a29-f9d2cf0c7824)

#####  Dòng đầu thứ nhấtnhất
Feb 18 01:20:22 mail dovecot[2510]: imap-login: Login: user=<test2>, method=PLAIN, rip=171.252.154.180, lip=103.112.211.194, mpid=2777, TLS, session=<gazWk1ouKV2r/Jq0>  
###### Giải thích:

1. **Feb 18 01:20:22**: Thời gian của sự kiện (ngày tháng và giờ).
2. **mail**: Tên máy chủ hoặc dịch vụ (tên của hệ thống nơi Dovecot đang chạy).
3. **dovecot[2510]**: Chương trình **Dovecot** với ID tiến trình (PID) là `2510` đang ghi nhận sự kiện.
4. **imap-login**: Đây là dịch vụ **imap-login**, tức là Dovecot đang xử lý yêu cầu đăng nhập IMAP.
5. **Login**: Đăng nhập của người dùng.
6. **user=<test2>**: Tên người dùng đang đăng nhập là **test2**.
7. **method=PLAIN**: Phương thức xác thực là **PLAIN**, tức là tên người dùng và mật khẩu được gửi dưới dạng văn bản thuần (plaintext).
8. **rip=171.252.154.180**: **Địa chỉ IP của người gửi** (remote IP), tức là máy tính hoặc máy chủ đang kết nối đến Dovecot là **171.252.154.180**.
9. **lip=103.112.211.194**: **Địa chỉ IP của máy chủ** (local IP), tức là địa chỉ của máy chủ Dovecot là **103.112.211.194**.
10. **mpid=2777**: ID tiến trình (MPID) của quá trình xử lý đăng nhập là **2777**.
11. **TLS**: Kết nối được bảo mật bằng **TLS**, tức là email được gửi qua một kết nối bảo mật (Encryption).
12. **session=<gazWk1ouKV2r/Jq0>**: **Session ID** duy nhất cho phiên đăng nhập này là **gazWk1ouKV2r/Jq0**.

---

##### Dòng thứ hai
Feb 18 01:20:22 mail dovecot[2510]: imap(test2)<2777><gazWk1ouKV2r/Jq0>: Disconnected: Logged out in=9 out=482 deleted=0 expunged=0 trashed=0 hdr_count=0 hdr_bytes=0 body_count=0 body_bytes=0

###### Giải thích:
1. **imap(test2)**: Lần đăng nhập của người dùng **test2** thông qua giao thức IMAP.
2. **<2777><gazWk1ouKV2r/Jq0>**: Đây là ID tiến trình (2777) và Session ID (gazWk1ouKV2r/Jq0) của phiên đăng nhập.
3. **Disconnected**: Kết nối bị ngắt hoặc người dùng đã **đăng xuất**.
4. **Logged out**: Người dùng đã đăng xuất thành công.
5. Các thống kê:
   - **in=9**: Số byte đã được nhận từ máy khách trong phiên này (9 byte).
   - **out=482**: Số byte đã được gửi từ máy chủ đến máy khách trong phiên này (482 byte).
   - **deleted=0**: Số email đã bị xóa trong phiên này (0).
   - **expunged=0**: Số email bị loại bỏ vĩnh viễn (0).
   - **trashed=0**: Số email đã bị đưa vào thùng rác (0).
   - **hdr_count=0**: Số tiêu đề email đã được xử lý (0).
   - **hdr_bytes=0**: Số byte của các tiêu đề email (0).
   - **body_count=0**: Số email đã được xử lý trong phần thân (0).
   - **body_bytes=0**: Số byte của phần thân email (0).

---

##### Dòng log thứ ba

Feb 18 01:20:22 mail dovecot[2510]: imap-login: Login: user=<test2>, method=PLAIN, rip=171.252.154.180, lip=103.112.211.194, mpid=2780, TLS, session=<0m7ck1ouJXqr/Jq0>

Dòng này tương tự như Dòng log 1, có nghĩa là người dùng **test2** đã thực hiện một lần đăng nhập IMAP mới từ địa chỉ IP **171.252.154.180** và địa chỉ IP máy chủ **103.112.211.194**. Phương thức xác thực là **PLAIN**, và kết nối được bảo mật bằng **TLS**.

---
























  
  






  




