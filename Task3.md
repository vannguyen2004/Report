### Load Average là gì? Kiểm tra chỉ báo trong lệnh `top`

**Load Average** là chỉ số dùng để đo lường mức tải trung bình của hệ thống trong ba khoảng thời gian: 1, 5 và 15 phút. Chỉ số này giúp phát hiện mức độ tải của hệ thống và đưa ra các hành động xử lý kịp thời.

#### Đọc và đánh giá chỉ số Load Average:
- **Load Average <= số core**: Hệ thống hoạt động bình thường.
- **Load Average > số core**: Hệ thống đang bị tải cao, hiệu suất có thể giảm.
- **Load Average gấp đôi số core**: Hệ thống quá tải nghiêm trọng, cần can thiệp ngay.

---

#### **Chỉ báo trong lệnh `top`:**

![Screenshot 2025-02-11 142454](https://github.com/user-attachments/assets/3e4fae3b-a930-4f8c-af84-1efc54c9147a)

**Phần header:**
- **Thời gian hiện tại**: **14:24:51**
- **Trạng thái**: **up**
- **Số người dùng đang đăng nhập**: **1 user**
- **Load Average**: **0.00**, **0.00**, **0.00** (tương ứng với thời gian tải trong 1, 5, và 15 phút)

**Tasks**: Số lượng các process
- **Total**: **95**
- **Running**: **1**
- **Sleeping**: **94**
- **Stopped, Zombie**: Dừng và đóng băng.

**Cpu(s):**
- **us**: Tỷ lệ phần trăm CPU đang được sử dụng bởi các tiến trình người dùng.
- **sy**: Tỷ lệ phần trăm CPU đang được sử dụng bởi các tiến trình hệ thống.
- **ni**: Tỷ lệ phần trăm CPU dành cho các tiến trình có mức độ ưu tiên.
- **id**: Tỷ lệ CPU ở trạng thái không hoạt động.
- **wa**: Tỷ lệ phần trăm CPU đang đợi I/O.
- **hi**: Tỷ lệ phần trăm CPU dành thời gian ngắt phần cứng.
- **si**: Tỷ lệ phần trăm CPU dành thời gian ngắt phần mềm.
- **st**: Tỷ lệ phần trăm CPU bị "đánh cắp" bởi máy ảo (stolen).

**Memory:**
- Bao gồm tổng dung lượng bộ nhớ, dung lượng bộ nhớ đang sử dụng, dung lượng còn trống, bộ nhớ đệm và bộ nhớ cached.

**SWAP:**
- Bộ nhớ hoán đổi từ disk thành memory, bao gồm **Total**, **Free**, **Used**, và **Available Memory**.

---

#### **Chi tiết các Process:**
- **PID**: Mã tiến trình, mỗi tiến trình sẽ có PID riêng.
- **USER**: Tên người tạo tiến trình.
- **PR**: Mức độ ưu tiên của tiến trình.
- **VIRT**: Dung lượng bộ nhớ ảo mà tiến trình sử dụng.
- **RES**: Dung lượng bộ nhớ vật lý mà tiến trình đang sử dụng.
- **SHR**: Dung lượng bộ nhớ chia sẻ.
- **S**: Trạng thái của tiến trình (Running, Sleeping, Zombie, ...).
- **%CPU**: Tỷ lệ phần trăm CPU mà tiến trình đang sử dụng.
- **%MEM**: Tỷ lệ phần trăm bộ nhớ mà tiến trình đang sử dụng.
- **TIME+**: Thời gian tổng cộng mà tiến trình đã chạy.
- **COMMAND**: Tên của tiến trình.

**Lệnh `uptime`**: Ngoài lệnh `top`, bạn cũng có thể sử dụng lệnh `uptime` để xem thông tin Load Average của hệ thống.

---

### Nginx Reverse Proxy là gì? Tác dụng của Nginx Reverse Proxy cho Apache

**Nginx** là một máy chủ web mã nguồn mở, và **Nginx Reverse Proxy** là một tính năng cho phép Nginx hoạt động như một cầu nối giữa client và máy chủ backend. Thay vì kết nối trực tiếp với backend, client sẽ gửi yêu cầu đến Nginx, và Nginx sẽ chuyển tiếp yêu cầu đến backend thích hợp.

![673c413af7c9e8a1b4d94709_61ee501466af465f016c81b6_Proxy20vs 203](https://github.com/user-attachments/assets/31f4e91b-27eb-48b4-810d-304d13b39db0)


#### Một số thuật toán cân bằng tải phổ biến:
- **Round Robin**: Lần lượt chuyển các yêu cầu đến các máy chủ theo thứ tự tuần hoàn.
- **Weighted Round Robin**: Một biến thể của Round Robin, trong đó mỗi máy chủ được gán một trọng số khác nhau tùy thuộc vào năng lực xử lý.
- **Least Connections**: Máy chủ có ít kết nối đang mở nhất sẽ được chọn để xử lý yêu cầu kế tiếp.
- **Least Response Time**: Yêu cầu được gửi đến máy chủ có thời gian đáp ứng nhanh nhất.
- **Dựa trên địa chỉ IP của client**: Phân phối yêu cầu tới cùng một máy chủ.
- **URL Hash**: Tương tự IP Hash, nhưng sử dụng URL của yêu cầu để xác định máy chủ.
- **Least Loaded**: Yêu cầu được gửi đến máy chủ có tải thấp nhất tại thời điểm hiện tại, dựa trên các chỉ số như CPU, bộ nhớ, v.v.

---

#### **Tác dụng của Nginx Reverse Proxy cho Apache:**
- **Cân bằng tải**: Nginx sử dụng các thuật toán cân bằng tải để phân phối yêu cầu đến nhiều máy chủ Apache, giúp cải thiện hiệu suất của Web server.
- **Bảo mật**: Nginx hoạt động như một proxy, ngăn chặn kết nối trực tiếp từ client đến Apache. Điều này giúp bảo vệ Apache khỏi các cuộc tấn công DDoS và các lỗ hổng bảo mật có thể bị khai thác.
- **Caching**: Nginx có thể cấu hình cache để giảm bớt yêu cầu mà Apache phải xử lý, từ đó cải thiện tốc độ phản hồi cho người dùng.
- **Cấu hình SSL**: Việc quản lý SSL và mã hóa được thực hiện tập trung tại Nginx, hạn chế quá trình SSL handshake bằng cơ chế keepalive và sử dụng lại các tham số cho kết nối lại hay mở nhiều kết nối song song
- **Giảm nghẻn cổ chai**: do Nginx có hiệu xuất cao có thể xử lí lên đến 10.000 kết nối đồng thời bằng cơ chế hướng sự kiện  `event driven` và kiến trúc không đồ bộ `asynchronous`
- **Hiệu quả các tệp tỉnh**: Nginx rất hiệu quả khi phục vụ các tệp tĩnh nhanh chóng, phân chia công việc với Apache (Nginx sử lý các yêu cầu tĩnh như html, css, hình ảnh, Apache xử lí các yêu cầu động như PHP)
  
---

### Cấu hình Nginx reverse proxy cho LAMP stack  

1. Thay đổi port cho Apache để tránh xung đột với Nginx trong file  `/etc/httpd/conf/httpd.conf`
   
   ![Screenshot 2025-02-13 100225](https://github.com/user-attachments/assets/de1434c2-949f-4e9a-b626-d0726f598318)

2. Tải Nginx `dnf install -y nginx `
   + Cấu hình Reverse Proxy cho Nginx `/etc/nginx/nginx.conf`
     
   ![Screenshot 2025-02-13 101246](https://github.com/user-attachments/assets/4c534d42-692e-42ca-a402-9cddda788111)
   
   + listen: lắng nghe ở port 80 trên IPv4 và IPv6  
   + server_name: Tên máy chủ mà Nginx sẽ xử lí yêu cầu  
   + root: Chỉ thị này chỉ định thư mục gốc mà Nginx sẽ sử dụng để tìm các tệp tĩnh khi có yêu cầu từ người dùng  
   + proxy_redirect off: Nginx là người chung gian và không thay đổi URL được trả về từ backend  
   + proxy_set_header X-Real-IP $remote_addr: Nginx thêm `X-Real-IP` để gửi yêu cầu tới backend (`X-Real-IP` là địa chỉ IP của Client khi yêu càu đến Nginx)  
   + proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for: Chỉ thị này thêm header  `X-Forwarded-For` vào yêu cầu gửi đến backend. Đây là một chuỗi các địa chỉ IP được thêm vào từ phía người dùng đến Nginx, giúp backend biết rõ đường đi của yêu cầu qua các proxy.  
   + proxy_set_header Host $http_host: Head `http_host` được giữ nguyên khi nginx gửi đến backend thông thường sẽ là tên website  
   + proxy_pass là chỉ thị cơ bản để thiết lập reverse proxy, nơi Nginx đóng vai trò làm "người trung gian", nhận yêu cầu và chuyển tiếp đến một máy chủ khác  


