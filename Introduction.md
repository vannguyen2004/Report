# Các thành phần của Traffic Control
Traffic Ops: là nơi lưu giữ cá cache server và CDN Delivery Service. Cũng là Traffic Óp API có thể sử dụng tool, script hoặc chương trình để truy cập nhiều CDN data  
Traffic Router: Được dùng để định tuyến yêu cầu người dùng đến server gần nhất bằng các phân tích Health, capacity và stat của Cache Server theo Health Protocol và liên quan đến địa lí khoản cách địa lỹ giữa các Cache Group và client  
Traffic Moniter: thăm dò tình trạng của các cache server on trong khoảng thời gian rất xem máy chủ nào cần đươc luân chuyển  
Traffic Stats: Thu thập và lưu giữ thông kê lưu lượng trung bình theo thời gian thực của mỗi Cache server. Dữ liệu được sử dụng bởi Traffic Router đánh giá khả năng của mỗi Cache server nó dùng trong load balance traffic và ngăn chặn overload  
Traffic Porttal: là giao diện web sử dụng Traffic Ops API để trình bài dữ liệu CDN theo giao diện thân thiện với người dùng.  
Traffic Vault: được sử dụng lưu trử SSL private key 
# Content Invalidation Jobs:
Thay vì để dữ liệu trong cache là không hợp lệ theo các thông thường thì ta có thể áp đặt Policy để áp đặt chính sách là không hợp lệ. Đôi khi có những dữ liệu khi thay đổi Origin cần thông báo tới các cache server để thông báo rằng dữ liệu cần thay đổi thay vì phải đợi theo max-age nhưng thông thường.

