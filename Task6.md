## Tạo Email trên DA
Ta vào DirectAdmin bằng User
Vào phần E-mail Manager
  Chọn E-mail Accounts
  Chọn Create Account

  ![image](https://github.com/user-attachments/assets/4986e570-0c05-48d5-9877-a94a2f0d9e02) 

  Điền các thông tin liên quan bao gồm: username, password, E-mail quota (giới hạn dung lượng lưu trữ email), send limit (giới hạn số thư được gửi đi)-> Sau đó bấm Create

  ![image](https://github.com/user-attachments/assets/b220ad20-cb7a-4604-a7f1-55de361edb2c)
  ![image](https://github.com/user-attachments/assets/a53d7032-ca54-49a6-976d-ebb679ea5ef8)
  
  danh sách các email hiện có trên domain project2.nguyenhv.id.vn
  
  ![image](https://github.com/user-attachments/assets/9e8d561f-ef90-4680-bac9-2b932fb1ef02)
  
  Ta vào phần ex features -> Webmail roundcube -> Đăng nhập
  
  ![image](https://github.com/user-attachments/assets/ebe8a796-2cb7-4c69-a7f7-16bcb5e09e74)
  ![image](https://github.com/user-attachments/assets/6898cfc8-ccaa-4c96-95b6-3f2f72fa871d)
  
  Chọn Compose để soạn mail mới 
  
  ![image](https://github.com/user-attachments/assets/efff2577-a7e4-41ea-8451-ea54659f2cba)
  
  Kiểm tra bên mail người nhận
  
  ![image](https://github.com/user-attachments/assets/0daee93e-47be-4cf3-8e7f-26bfc7c31cd6)

  Xem thông tin Mail đã gửi 
  ![image](https://github.com/user-attachments/assets/c270f674-7985-4b77-8d79-08f5204023eb)
  Ta lấy thông tin trường ID và tìm kiếm trong Log Exim và Dovecot
  1tnC0h-0000CY-OZ


  Log file exim
 ![image](https://github.com/user-attachments/assets/1876b1af-bda1-4981-ba58-ed62c3d1a539)

Dòng đầu tiên:
2025-02-26 14:39:16 1tnCOh-0000CY-Oz <= nguyen1@project2.nguyenhv.id.vn H=(49157.vpsvinahost.vn) [127.0.0.1] P=esmtpa A=login:nguyen1@project2.nguyenhv.id.vn S=623 id=56fd56b49ecb6484906f915b6e8445c5@project2.nguyenhv.id.vn T="TEST" from <nguyen1@project2.nguyenhv.id.vn> for nguyen3@project2.nguyenhv.id.vn  

Timestamp: 2025-02-26 14:39:16
Message ID: 1tnCOh-0000CY-Oz
Người gửi: nguyen1@project2.nguyenhv.id.vn
Host gửi: 49157.vpsvinahost.vn (địa chỉ IP 127.0.0.1, có thể gửi từ localhost)
Phương thức gửi: esmtpa (SMTP có xác thực)
Xác thực với tài khoản: nguyen1@project2.nguyenhv.id.vn
Kích thước email: 623 bytes
Message ID: 56fd56b49ecb6484906f915b6e8445c5@project2.nguyenhv.id.vn
Tiêu đề: "TEST"
Người nhận: nguyen3@project2.nguyenhv.id.vn
🡆 Đây là quá trình Exim nhận email từ nguyen1@project2.nguyenhv.id.vn để gửi đến nguyen3@project2.nguyenhv.id.vn.
Dòng thứ hai:
2025-02-26 14:39:16 cwd=/var/spool/exim 3 args: /usr/sbin/exim -Mc 1tnCOh-0000CY-Oz
Timestamp: 2025-02-26 14:39:16
Exim làm việc trong thư mục: /var/spool/exim
Lệnh được thực thi: /usr/sbin/exim -Mc 1tnCOh-0000CY-Oz
Dòng thứ ba
Timestamp: 2025-02-26 14:39:17
Message ID: 1tnCOh-0000CY-Oz
Người nhận: nguyen3@project2.nguyenhv.id.vn
Router: virtual_user (email được gửi đến hộp thư của user ảo)
Transport: dovecot_lmtp_udp (giao thức LMTP của Dovecot)
Kích thước email: 786 bytes
Phản hồi từ Dovecot: 250 2.0.0 ... Saved

dòng thứ 4 
2025-02-26 14:39:17 1tnCOh-0000CY-Oz Completed
Timestamp: 2025-02-26 14:39:17
Message ID: 1tnCOh-0000CY-Oz
Trạng thái: Completed

Log Dovecot
![image](https://github.com/user-attachments/assets/2ee64286-440d-4cb6-b620-f98a63520585)
Ta chú ý hai session này vì lúc này chính là lúc client lấy thư về
Dòng thứ nhất

Feb 26 14:39:56 49157 dovecot[834]: imap-login: Login: user=<nguyen3@project2.nguyenhv.id.vn>, method=PLAIN, rip=127.0.0.1, lip=127.0.0.1, mpid=836, secured, session=<e5MLrgYvcMZ/AAAB>

Feb 26 14:39:56:
Đây là thời điểm xảy ra sự kiện, cụ thể là ngày 26 tháng 2, 14 giờ 39 phút 56 giây (theo giờ hệ thống).
49157:
Đây là hostname của máy chủ chạy dịch vụ Dovecot. Trong trường hợp này, 49157 có thể là tên máy hoặc một định danh nội bộ.
dovecot[834]:
Dịch vụ Dovecot đang chạy với process ID (PID) là 834. Đây là tiến trình chính của Dovecot trên máy chủ.
imap-login: Login::
Chỉ ra rằng đây là sự kiện đăng nhập vào dịch vụ IMAP.
user=<nguyen3@project2.nguyenhv.id.vn>:
Tên người dùng đăng nhập là nguyen3@project2.nguyenhv.id.vn. Đây thường là địa chỉ email hoặc định danh của người dùng trong hệ thống mail.
method=PLAIN:
Phương thức xác thực được sử dụng là PLAIN, tức là mật khẩu được gửi dưới dạng văn bản thuần (không mã hóa trong chính giao thức IMAP). Tuy nhiên, vì có trường secured (xem bên dưới), kết nối có thể đã được mã hóa qua SSL/TLS.
rip=127.0.0.1:
Địa chỉ IP của client (remote IP) là 127.0.0.1, tức là localhost. Điều này cho thấy client đang chạy trên cùng máy chủ với Dovecot, có thể là một ứng dụng hoặc script nội bộ.
lip=127.0.0.1:
Địa chỉ IP của server (local IP) cũng là 127.0.0.1, xác nhận đây là kết nối nội bộ giữa client và server trên cùng một máy.
mpid=836:
Đây là PID của tiến trình IMAP cụ thể xử lý phiên đăng nhập này. Mỗi phiên IMAP sẽ có một tiến trình riêng để quản lý.
secured:
Kết nối được bảo mật, có thể qua SSL/TLS. Điều này đảm bảo rằng dù phương thức xác thực là PLAIN, thông tin đăng nhập vẫn được mã hóa khi truyền qua mạng.
session=<e5MLrgYvcMZ/AAAB>:
Đây là mã phiên (session ID) duy nhất được gán cho phiên IMAP này. Nó giúp theo dõi phiên trong các log liên quan.


Dòng thứ 2
Feb 26 14:39:56 49157 dovecot[834]: imap(nguyen3@project2.nguyenhv.id.vn)<836><e5MLrgYvcMZ/AAAB>: Disconnected: Logged out in=469 out=2105 deleted=0 expunged=0 trashed=0 hdr_count=1 hdr_bytes=391 body_count=1 body_bytes=31
imap(nguyen3@project2.nguyenhv.id.vn):
Chỉ ra rằng đây là phiên IMAP của người dùng nguyen3@project2.nguyenhv.id.vn.
<836>:
PID của tiến trình IMAP xử lý phiên này là 836, trùng với mpid trong dòng đăng nhập.
<e5MLrgYvcMZ/AAAB>:
Session ID của phiên này, khớp với dòng đăng nhập, dùng để liên kết hai sự kiện (đăng nhập và đăng xuất).
Disconnected: Logged out:
Phiên IMAP đã bị ngắt kết nối với lý do Logged out. Điều này có nghĩa là client đã gửi lệnh LOGOUT để chủ động kết thúc phiên.
in=469:
Tổng số byte dữ liệu được gửi từ client đến server trong phiên này là 469 byte. Đây là dữ liệu của các lệnh IMAP mà client thực hiện.
out=2105:
Tổng số byte dữ liệu được gửi từ server đến client là 2105 byte. Đây là dữ liệu của các phản hồi từ server (ví dụ: nội dung email, header...).
deleted=0:
Số email bị đánh dấu xóa trong phiên này là 0. Người dùng không thực hiện lệnh xóa email nào.
expunged=0:
Số email bị xóa vĩnh viễn (expunged) là 0. Trong IMAP, lệnh EXPUNGE được dùng để xóa hoàn toàn email đã bị đánh dấu xóa, nhưng không có hành động này trong phiên.
trashed=0:
Số email được di chuyển vào thùng rác là 0. Không có email nào bị chuyển vào thư mục "Trash".
hdr_count=1:
Số lượng header email được tải trong phiên là 1. Client đã yêu cầu thông tin header của một email.
hdr_bytes=391:
Tổng kích thước của header email được tải là 391 byte.
body_count=1:
Số lượng phần thân (body) email được tải là 1. Client đã yêu cầu nội dung của một email.
body_bytes=31:
Tổng kích thước của phần thân email được tải là 31 byte.
  

