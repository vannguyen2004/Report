# Khái niệm cơ bản về Exim và Dovecot

## Exim
Exim là một trong những phần mềm máy chủ email được sử dụng nhiều nhất cho các hệ điều hành giống Unix, chẳng hạn như Linux và Solaris. Nó đóng vai trò như một **Mail Transfer Agent (MTA)** có nhiệm vụ chuyển mail từ người gửi đến người nhận thông qua máy chủ mail.

### Một số ưu điểm của Exim:
- **Cấu hình linh hoạt**: Exim cho phép người quản trị cấu hình chi tiết cách thức gửi nhận email, như các quy tắc bảo mật, xử lý bưu kiện email, hạn chế spam, v.v.
- **Quản lý hàng đợi email**: Exim cho phép quản lý email trong hàng đợi queue, giúp theo dõi và kiểm tra các email bị lỗi và không thể gửi đi.
- **Khả năng mở rộng**: Exim có thể được tích hợp với các công cụ khác như spam filtering, antivirus và các hệ thống phân phối email phức tạp.

## Dovecot
Dovecot là một **Mail Delivery Agent (MDA)**, là một máy chủ IMAP và POP3 nguồn mở dành cho các hệ điều hành giống Unix, cung cấp quyền truy cập cho email qua 2 giao thức trên.

# DKIM, SPF, PTR

## DKIM
**DKIM** (DomainKeys Identified Mail) là phương thức xác thực mail, được thiết kế để phát hiện mail giả mạo.

### Các hoạt động:
DKIM hoạt động theo 2 giai đoạn:
1. **Ký (máy chủ gửi)**: Máy chủ gửi ký email trước khi gửi đi và gắn chữ ký vào phần header của email.
2. **Xác thực (máy chủ nhận)**: Máy chủ nhận kiểm tra chữ ký DKIM bằng khóa công khai trong DNS của domain người gửi.

## SPF
**SPF** (Sender Policy Framework) xác định server nào được phép gửi mail từ một tên miền cụ thể, giúp xác thực mail và kiểm tra tính hợp lệ của email.

### Quá trình xác thực SPF:
1. Quản trị viên thiết lập bảng ghi TXT trong DNS để khai báo danh sách các máy chủ có quyền gửi email thay mặt cho domain.
2. Máy chủ nhận sẽ truy vấn DNS của domain trong email, kiểm tra bảng ghi SPF và xác thực địa chỉ IP của máy chủ gửi.

## PTR (bảng ghi Pointer)
**PTR** là bảng ghi dùng để tra cứu tên miền từ địa chỉ IP, có ứng dụng trong SPF để xác thực địa chỉ IP của máy chủ gửi.

# Cấu hình VPS Mail Server với Exim và Dovecot

## Cấu hình Exim
1. **Cài đặt Exim:**
   - Tải repo Epel: `dnf install -y epel-release`
   - Cài đặt Exim: `dnf install -y exim`

2. **Chỉnh sửa tệp cấu hình Exim** tại `/etc/exim/exim.conf`:
   - Thêm dòng sau vào cấu hình:
     ```bash
     untrusted_set_sender = *
     no_local_from_check
     ```

   - Thiết lập hostname của máy chủ Exim.

   - Thiết lập tên domain cho máy chủ Mail.

3. **Cấu hình local delivery**:
   - Cấu hình lưu trữ email trong thư mục Maildir.
   - Ví dụ cấu hình:
     ```bash
     driver = appendfile
     directory = $home/Maildir
     maildir_format
     maildir_use_size_file
     delivery_date_add
     envelope_to_add
     return_path_add
     group = mail
     mode = 0660
     ```

4. **Kiểm tra cấu hình Exim**:
   - Lệnh kiểm tra cấu hình Exim: `exim -C /etc/exim/exim.conf -bV`

## Cấu hình Dovecot
1. **Cài đặt Dovecot**:
   - Cài đặt Dovecot: `dnf install -y dovecot`

2. **Chỉnh sửa tệp cấu hình Dovecot**:
   - Xác thực trong tệp `/etc/dovecot/conf.d/10-auth.conf`: Cấm xác thực dưới dạng Plain text và kiểu xác thực login.

3. **Chỉ định đường dẫn lấy mail** trong tệp `/etc/dovecot/conf.d/10-mail.conf`.

4. **Quản lý kết nối đến máy chủ khác** trong tệp `/etc/dovecot/conf.d/10-master.conf`:
   - Thêm nội dung cho `service auth` và `auth-worker` để tạo socket liên lạc với Exim.

5. **Bật Dovecot và Exim** bằng systemd.

# Test gửi nhận Mail

## Đăng nhập trên Thunderbird
![Screenshot Thunderbird](https://github.com/user-attachments/assets/1c40da61-16bb-499c-abaf-931d830b9c7a)

## Gửi Nhận Mail
![Screenshot Mail Test](https://github.com/user-attachments/assets/6cf81131-cb00-4d4f-a559-11b007678259)

