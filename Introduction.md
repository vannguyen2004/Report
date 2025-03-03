# Các thành phần của Traffic Control
Traffic Ops: là nơi lưu giữ cá cache server và CDN Delivery Service. Cũng là Traffic Óp API có thể sử dụng tool, script hoặc chương trình để truy cập nhiều CDN data  
Traffic Router: Được dùng để định tuyến yêu cầu người dùng đến server gần nhất bằng các phân tích Health, capacity và stat của Cache Server theo Health Protocol và liên quan đến địa lí khoản cách địa lỹ giữa các Cache Group và client  
Traffic Moniter: thăm dò tình trạng của các cache server on trong khoảng thời gian rất xem máy chủ nào cần đươc luân chuyển  
Traffic Stats: Thu thập và lưu giữ thông kê lưu lượng trung bình theo thời gian thực của mỗi Cache server. Dữ liệu được sử dụng bởi Traffic Router đánh giá khả năng của mỗi Cache server nó dùng trong load balance traffic và ngăn chặn overload  
Traffic Porttal: là giao diện web sử dụng Traffic Ops API để trình bài dữ liệu CDN theo giao diện thân thiện với người dùng.  
Traffic Vault: được sử dụng lưu trử SSL private key 
# Content Invalidation Jobs:
Thay vì để dữ liệu trong cache là không hợp lệ theo các thông thường thì ta có thể áp đặt Policy để áp đặt chính sách là không hợp lệ. Đôi khi có những dữ liệu khi thay đổi Origin cần thông báo tới các cache server để thông báo rằng dữ liệu cần thay đổi thay vì phải đợi theo max-age nhưng thông thường.
  - Một số thành phần của Content Invalidaiton Job
    Asset URL: URL của nội dung (asset) mà bạn muốn xóa hoặc làm mất hiệu lực khỏi bộ nhớ cache của CDN. Như /images/logo.png, /css/style.css, /api/data.json
    Create By: Tên người hoặc hệ thống đã tạo yêu cầu invalidation.
    Delivery Service: Tên của dịch vụ CDN chịu trách nhiệm phân phối nội dung mà bạn muốn làm mất hiệu lực
    ID: Mã định danh duy nhất của Content Invalidation Job
    Invalidation Type: Xác định loại invalidation mà bạn đang thực hiện. Invalidate – Đánh dấu nội dung trong cache là hết hạn (stale), CDN sẽ lấy nội dung mới từ origin server khi có request tiếp theo. Purge – Xóa hoàn toàn nội dung khỏi cache ngay lập tức.
    Refresh: Khi bật tùy chọn refresh, CDN sẽ tự động lấy lại nội dung từ origin server và cập nhật vào cache ngay lập tức.
    Refetch:  CDN chủ động gửi yêu cầu đến origin server để lấy nội dung mới và thay thế trong cache.
    Regular Expression:  Cho phép sử dụng biểu thức chính quy để xóa nhiều nội dung cùng lúc.
    Start Time: Thời điểm bắt đầu thực hiện Content Invalidation Job.
    

