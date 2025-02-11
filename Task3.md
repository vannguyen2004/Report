### Load Average là gì? Kiểm tra chỉ báo trong lệnh top ###
**Load Average**: là chỉ số dùng để đo lường mức tải trung bình của hệ thống trong một khoản thời gian nhất định (1, 5, 15 phút). Load Average giúp phát hiện mức tải của hệ thống để có những hành động xử lí kịp thời.
Đọc và đánh giá chỉ số Load Average
  + Load Average <= số core, bình thường
  + Load Average > số core, đang bị tải cao, giảm hiệu suất
  + Load Average gấp đôi số core, quá tải nghiêm trong cần được can thiệp ngay
**Chỉ báo trong lệnh top**:
![Screenshot 2025-02-11 142454](https://github.com/user-attachments/assets/3e4fae3b-a930-4f8c-af84-1efc54c9147a)
phần header:
  + Thời gian hiện tại: **14:24:51**
  + Trạng thái: **up**
  + Số người dùng đang đăng nhập: **1 user**
  + load average: **0.00**,**0.00**,**0.00** tương ứng thời tải lần lượt trong 1, 5, và 15 phút
tasks: Số lượng các process
  + Total **95**
  + Running **1**
  + Sleeping **94**
  + Stopped, Zombie (dừng và đóng băng)
Cpu(s)
  + us: Tỷ lệ phần trăm CPU đang được sử dụng bởi các tiến trình người dùng 
  + sy: Tỷ lệ phần trăm CPU đang được sử dụng bởi các tiến trình hệ thống
  + ni: Tỷ lệ phần trăm CPU dành cho các tiến trình có mức độ ưu tiên
  + id: Tỷ lệ CPU ở trạng thái không hoạt động 
  + wa: Tỷ lệ phần trăm CPU đang đợi I/O
  + hi: Tỷ lệ phần trăm CPU dành thời gian ngắt phần cứng
  + si: Tỷ lệ phần trăm CPU dành thời gian ngắt phần 
  + st: Tỷ lệ phần trăm CPU bị "đánh cắp" bởi máy ảo (stolen).
Memory: bao gồm (Tổng dung lượng  bộ nhớ, Dung lượng bộ nhớ đang sử dụng, Dung lượng còn trống, bộ nhớ đệm, bộ nhớ cached)
SWAP (Bộ nhớ được hoán đổi từ disk thành memory): bao gồm Total, Free và Used và aval Memory)
Chi tiết các Process:
  + PID: mã tiến trình sẽ không có 2 tiến trình có cùng PIP
  + USER: Tên người tạo tiến trình
  + PR: Mức độ ưu tiên tiến trình
  + Giá trị ưu tiên
  + VIRT: Dung lượng bộ nhớ ảo mà tiến trình sử dụng
  + RES: Dung lượng bộ nhớ vật lí mà tiến trình đang sử dụng
  + SHR: Dung lượng bộ nhớ chia sẻ
  + S: Trạng thái của tiến trình (Running, Sleeping, Zombie...).
  + %CPU: Tỷ lệ phần trăm CPU mà tiến trình đang sử dụng.
  + %MEM: Tỷ lệ phần trăm bộ nhớ mà tiến trình đang sử dụng.
  + TIME+: Thời gian tổng cộng mà tiến trình đã chạy.
  + COMMAND: tên tiến
***Ngoài top thì có lệnh có lệnh uptime để xem load average***
### Nginx Reverse Proxy là gì, tác dụng của Nginx Reverse Proxy cho Apache ###
Nginx: là máy chủ Web mã nguồn mỡ và Nginx Reverse là một tính năng của Nginx cho phép nó hoạt động như một cầu nối giữa client và máy chủ backend. Thay vì kết nối trực tiếp với backend, client gửi yêu cầu đến Nginx và Nginx sẽ chuyển tiếp yêu cầu đến back end thích hợp.
Lợi ích của reverse proxy:
  + Cải thiện hiệu xuất: Nhờ cache, nén dữ liệu và giảm tải cho backend.
  + Bảo mật: Ẩn thông tin của backend, tránh được các cuộc tấng công trực tiếp.
  + Cân bằng tải: Phân phối lưu lượng đến nhiều máy chủ backend.
  + Đơn giản hóa cấu hình SSL/TLS: Quản lí mã hóa tập trung tại Nginx.
Một số thuật toán cân bằng phổ biến:
  + Round Robin: Lần lượt chuyển các yêu cầu đến những máy chủ theo thứ tự tuần hoàn.
  +  Weighted Round Robin: Một biến thể của Round Robin, trong đó mỗi máy chủ được gán một trọng số khác nhau dựa trên năng lực xử lý của chúng.
  + Least Connections: Máy chủ có ít kết nối đang mở nhất sẽ được chọn để xử lý yêu cầu kế tiếp.
  + Least Response Time: Yêu cầu sẽ được gửi đến máy chủ có thời gian đáp ứng nhanh nhất
  + Dựa trên địa chỉ IP của client để phân phối yêu cầu tới cùng một máy chủ.
  + URL Hash: Tương tự IP Hash nhưng sử dụng URL của yêu cầu để xác định máy chủ thích hợp.
