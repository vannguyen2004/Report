# CDN: 
Là công nghệ giúp kết nối kiểu one-to-many hiệu quả. CDN là hệ thống phân tán của các máy chủ để vận chuyển nội dung qua HTTP. Các máy chủ được triển khai ở nhiều vị trí khác nhau với mục đích tối ưu việc truyền tải nội dung đến người dùng cuối, trong khi giảm thiểu lưu lượng trên mạng.

# Đặc trưng: 
Cache Server: Là máy chủ vừa làm nhiệm vụ proxy các yêu cầu, vừa lưu trữ dữ liệu để tái sử dụng.

Content Router: Đảm bảo rằng các người dùng cuối được kết nối đến các Cache Server tối ưu cho vị trí và khả năng có sẵn nội dung (Traffic Router)

The Health Protocol: Giao thức Health giám sát việc sử dụng các Cache Server và các máy chủ thuê bao (tenants) trong mạng CDN.

Configuration Management System: Hệ thống quản lí CDN (Traffic Portal)

Log File Analysis System: Thống kê phân tích trong việc quản lý, giám sát CDN (Traffic Stats.)
 
Các kiến thức cơ bản cần nắm: HTTP/1.1, Cache control headers và revalidation


# Mô hình:
Phân cấp: Orgin -> [(mid -> edge) Cache ServerServer]

Orgin gửi dữ liệu cho mid, mid gửi dữ liệu cho edge. Edge tiếp nhận yêu cầu. Kiểm tra dữ liệu bằng các gửi đến Mid chứ không cần đến Origin 

# Topology
{Division [Region (Location)]} 

## Delivery Service Request
Là yêu cầu thay đổi một Delivery Service - một cấu hình điều hướng lưu lượng của CDN để phân phối nội dung đến người dùng cuối (bao gồm tạo mới DSR, xóa, chỉnh sửa)

# Một số thành phần của TRAFFIC CONTROL

# Delivery Service: 
Là tập hợp các quy tắc và cấu hình giúp phân phối nội dung mạng CDN. Đóng vai trò cốt lõi trong ATC. Vì nó quyết định các nội dung được lưu trữ, bảo vệ và truyền tải đến người dùng cuối.

Trạng thái: 
* Active
* Primed
* Inactive


Còn nhiều thứ khác nữa




## Traffic Monitor: 
là dịch vụ HTTP để giám sát các Cache Server trong mạng phân phối CDN dựa trên nhiều chỉ số khác nhau. Các chỉ số này để xác định tình trạng của Cache Server cũng như các DS liên quan. Traffic monitor kết hợp với dữ liệu của chính mình để có cái nhìn tổng quát và nhất quán về tình trạng máy chủ CDN
* Health Protocol: Quy định tính khả dụng của máy chủ Cache và DS cung cấp thông tin tìn trạng của CDN thông qua **Endpoint Restful JSONJSON**

Traffic Monitor: kiểm tra bằng cách gửi 
* Thoughput: Byte in byte out
* Transaction: mã phản hồi của HTTP: 2xx, 3xx, 4xx, 
* Connection: kết nối client, Orgin, parent cache ...
* Cache Performance: Hits (yêu cầu nội dung có sẳn), misses (yêu cầu nội dung khôngkhông có sẳn), Refresh (Số lần nội dung trong cache được cập nhật hoặc làm mới),.. 
* Storage Performance: Write, Read, Frags,
* System Performance: load average, networking interface throuhput

Sử dụng:
* Optimistic ApproachApproach để đánh giá sự khả dụng của Cache Server hoặc DS. Nếu có 4 Traffic Monitor 3 cái nói Cache server chết nhưng 1 con nói chưa thì nó chưa.

* Optimistic Quorum: ngặn chặn tình trạng slip-bran (số node > node /2 )

## Traffic Router 
Chức năng chính gửi client đến server tối ưu nhất.
Khái niệm tối ưu ở đây là khoảng cách ở đâu không phải khoảng cách địa lí mà là hop layer. Càng ít bước nhảy càng cải thiện hiệu suất

## Traffic Ops
Là công cụ quản trị (cấu hình và giám sát) các thành phần trong CDN. Traffic Portal sử dụng Traffic Ops API để quản lý máy chủ,... Trong nhiều trường hợp cần áp dụng lên nhiều máy chủ cache hoặc toàn bộ hệ thống. Traffic Ops đảm bảo tính nhất quán của cần thiết giữa các thành phần của CDN. 
* Traffic Ops sử dụng PostgreSQL để lưu trữ cấu hình và kết hợp các Mojolicious framework và Go.
* Traffic Ops có nhiệm vụ tạo ra tất cả các tệp cấu hình dành riêng cho từng ứng dụng chạy trên các máy chủ. Các máy chủ định kì kiểm tra traffic Ops để xem có tệp cấu hình mới cần áp dụng hay không. Quá trình này được thực hiện thông qua script ORT

## Traffic Portal
Là một ứng dụng khách AngularJS 1.x được phục vụ từ máy chủ web nodejs được thiết kế để sử dụng API traffic Ops 

## Traffic Stats
Là một chương trình được viết bằng ngôn ngữ Go để thu thập dữ liệu và thống kê từ các CDN. Traffic Stats lấy dữ liệu API của traffic Monitor và lưu trữ trong InfluxDB hoặc Kafka 
 *Các thông tin Traffic Stats phân tích như:*
 * Cache_stats theo dõi hiệu suất của từng máy chủ cache
* DDeliveryservice_stats giám sát hiệu suất của từng dịch vụ phân phối nội dung (Delivery Service), đo lường lưu lượng và số lượng phản hồi HTTP theo từng nhóm mã trạng thái (2xx, 3xx, 4xx, 5xx)
* daily_stats tổng hợp dữ liệu mỗi ngày để tạo báo cáo về băng thông cao nhất và tổng dữ liệu đã phục vụ.

## Traffic Vault
Là nơi lưu trữ các thông tin nhạy cảm:
* SSL Certificates
* DNSSEC Keys
* URL Signing Keys

   





