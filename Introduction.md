# Các thành phần của Traffic Control

## Traffic Ops
Traffic Ops là nơi lưu giữ các cache server và CDN Delivery Service. Đây cũng là Traffic Ops API, có thể sử dụng tool, script hoặc chương trình để truy cập nhiều dữ liệu CDN.

## Traffic Router
Traffic Router được dùng để định tuyến yêu cầu người dùng đến server gần nhất bằng cách phân tích:
- **Health**
- **Capacity**
- **Stat của Cache Server** theo Health Protocol
- **Khoảng cách địa lý** giữa các Cache Group và client

## Traffic Monitor
Traffic Monitor thăm dò tình trạng của các cache server trong một khoảng thời gian ngắn để xác định máy chủ nào cần được luân chuyển.

## Traffic Stats
Traffic Stats thu thập và lưu giữ thống kê lưu lượng trung bình theo thời gian thực của mỗi Cache Server. Dữ liệu này được sử dụng bởi Traffic Router để:
- Đánh giá khả năng của mỗi Cache Server
- Hỗ trợ cân bằng tải (Load Balancing)
- Ngăn chặn tình trạng quá tải (Overload)

## Traffic Portal
Traffic Portal là giao diện web sử dụng Traffic Ops API để trình bày dữ liệu CDN một cách thân thiện với người dùng.

## Traffic Vault
Traffic Vault được sử dụng để lưu trữ **SSL private key**.

---

# Content Invalidation Jobs
Thay vì để dữ liệu trong cache trở nên không hợp lệ theo cách thông thường, chúng ta có thể áp đặt chính sách để xác định dữ liệu nào là không hợp lệ. Đôi khi, khi dữ liệu thay đổi trên **Origin Server**, cần thông báo tới các Cache Server để cập nhật thay vì phải đợi **max-age** hết hạn.

## Một số thành phần của Content Invalidation Job
- **Asset URL**: URL của nội dung (asset) cần xóa hoặc làm mất hiệu lực khỏi bộ nhớ cache của CDN. Ví dụ:
  - `/images/logo.png`
  - `/css/style.css`
  - `/api/data.json`
- **Created By**: Tên người hoặc hệ thống đã tạo yêu cầu invalidation.
- **Delivery Service**: Tên của dịch vụ CDN chịu trách nhiệm phân phối nội dung.
- **ID**: Mã định danh duy nhất của Content Invalidation Job.
- **Invalidation Type**:
  - `Invalidate` – Đánh dấu nội dung trong cache là hết hạn (*stale*), CDN sẽ lấy nội dung mới từ origin server khi có request tiếp theo.
  - `Purge` – Xóa hoàn toàn nội dung khỏi cache ngay lập tức.
- **Refresh**: Khi bật tùy chọn `refresh`, CDN sẽ tự động lấy lại nội dung từ origin server và cập nhật vào cache ngay lập tức.
- **Refetch**: CDN chủ động gửi yêu cầu đến origin server để lấy nội dung mới và thay thế trong cache.
- **Regular Expression**: Cho phép sử dụng **biểu thức chính quy** để xóa nhiều nội dung cùng lúc.
- **Start Time**: Thời điểm bắt đầu thực hiện Content Invalidation Job.
