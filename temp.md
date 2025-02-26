# Tạo Email trên DirectAdmin

## 1. Đăng nhập vào DirectAdmin
- Truy cập DirectAdmin bằng tài khoản user.
- Vào phần **E-mail Manager** → **E-mail Accounts** → **Create Account**.

![image](https://github.com/user-attachments/assets/4986e570-0c05-48d5-9877-a94a2f0d9e02)

## 2. Tạo tài khoản email
- Điền các thông tin liên quan:
  - **Username**
  - **Password**
  - **E-mail quota** (giới hạn dung lượng lưu trữ email)
  - **Send limit** (giới hạn số thư được gửi đi)
- Bấm **Create** để tạo tài khoản.

![image](https://github.com/user-attachments/assets/b220ad20-cb7a-4604-a7f1-55de361edb2c)
![image](https://github.com/user-attachments/assets/a53d7032-ca54-49a6-976d-ebb679ea5ef8)

## 3. Xem danh sách email hiện có
- Hiển thị danh sách các email trên domain **project2.nguyenhv.id.vn**.

![image](https://github.com/user-attachments/assets/9e8d561f-ef90-4680-bac9-2b932fb1ef02)

## 4. Đăng nhập Webmail (Roundcube)
- Vào **Extra Features** → **Webmail Roundcube**.
- Đăng nhập vào Webmail bằng tài khoản vừa tạo.

![image](https://github.com/user-attachments/assets/ebe8a796-2cb7-4c69-a7f7-16bcb5e09e74)
![image](https://github.com/user-attachments/assets/6898cfc8-ccaa-4c96-95b6-3f2f72fa871d)

## 5. Soạn và gửi email
- Chọn **Compose** để soạn thư mới.

![image](https://github.com/user-attachments/assets/efff2577-a7e4-41ea-8451-ea54659f2cba)

## 6. Kiểm tra email nhận được
- Kiểm tra hộp thư bên người nhận để xem email đến.

![image](https://github.com/user-attachments/assets/0daee93e-47be-4cf3-8e7f-26bfc7c31cd6)

## 7. Xem thông tin email đã gửi
- Lấy thông tin **Message ID** và tìm kiếm trong log của **Exim** và **Dovecot**.
- **Message ID**: `1tnC0h-0000CY-OZ`

![417039624-c270f674-7985-4b77-8d79-08f5204023eb](https://github.com/user-attachments/assets/beba111b-adf3-4648-b619-8f68483605da)


### Log file Exim

```log
2025-02-26 14:39:16 1tnCOh-0000CY-Oz <= nguyen1@project2.nguyenhv.id.vn H=(49157.vpsvinahost.vn) [127.0.0.1] P=esmtpa A=login:nguyen1@project2.nguyenhv.id.vn S=623 id=56fd56b49ecb6484906f915b6e8445c5@project2.nguyenhv.id.vn T="TEST" from <nguyen1@project2.nguyenhv.id.vn> for nguyen3@project2.nguyenhv.id.vn
```

**Phân tích:**
- **Timestamp:** `2025-02-26 14:39:16`
- **Message ID:** `1tnCOh-0000CY-Oz`
- **Người gửi:** `nguyen1@project2.nguyenhv.id.vn`
- **Host gửi:** `49157.vpsvinahost.vn` (IP `127.0.0.1` - localhost)
- **Phương thức gửi:** `esmtpa` (SMTP có xác thực)
- **Xác thực với tài khoản:** `nguyen1@project2.nguyenhv.id.vn`
- **Kích thước email:** `623 bytes`
- **Tiêu đề:** `TEST`
- **Người nhận:** `nguyen3@project2.nguyenhv.id.vn`

---

```log
2025-02-26 14:39:16 cwd=/var/spool/exim 3 args: /usr/sbin/exim -Mc 1tnCOh-0000CY-Oz
```

- **Thời gian:** `2025-02-26 14:39:16`
- **Exim làm việc trong thư mục:** `/var/spool/exim`
- **Lệnh thực thi:** `/usr/sbin/exim -Mc 1tnCOh-0000CY-Oz`

---

```log
2025-02-26 14:39:17 1tnCOh-0000CY-Oz Completed
```

- **Thời gian:** `2025-02-26 14:39:17`
- **Trạng thái:** `Completed`

### Log file Dovecot

```log
Feb 26 14:39:56 49157 dovecot[834]: imap-login: Login: user=<nguyen3@project2.nguyenhv.id.vn>, method=PLAIN, rip=127.0.0.1, lip=127.0.0.1, mpid=836, secured, session=<e5MLrgYvcMZ/AAAB>
```

- **Thời gian:** `Feb 26 14:39:56`
- **Người dùng:** `nguyen3@project2.nguyenhv.id.vn`
- **Phương thức xác thực:** `PLAIN` (mật khẩu không mã hóa trong giao thức IMAP nhưng có SSL/TLS bảo mật)
- **Địa chỉ IP client:** `127.0.0.1`
- **Địa chỉ IP server:** `127.0.0.1`
- **Session ID:** `<e5MLrgYvcMZ/AAAB>`

---

```log
Feb 26 14:39:56 49157 dovecot[834]: imap(nguyen3@project2.nguyenhv.id.vn)<836><e5MLrgYvcMZ/AAAB>: Disconnected: Logged out in=469 out=2105 deleted=0 expunged=0 trashed=0 hdr_count=1 hdr_bytes=391 body_count=1 body_bytes=31
```

- **Người dùng:** `nguyen3@project2.nguyenhv.id.vn`
- **Số email đã xem:** `1`
- **Kích thước tiêu đề email:** `391 bytes`
- **Kích thước nội dung email:** `31 bytes`
- **Phiên IMAP kết thúc với trạng thái:** `Logged out`

---

## Kết luận
- Email đã được tạo thành công trên **DirectAdmin**.
- Người dùng có thể đăng nhập vào **Webmail Roundcube** để gửi và nhận email.
- Email gửi đi được ghi nhận trong **log của Exim**.
- Email nhận được và tải về được ghi nhận trong **log của Dovecot**.
- Mọi quá trình gửi/nhận email đều hoạt động bình thường.
