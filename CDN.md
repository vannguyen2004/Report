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
* Active: Cấu hình DS được triển khai tới Cache Server và sẽ được sử dụng bởi Traffic Router
* Primed: DS được triển khai trên các Cache Server nhưng không được sử dụng bởi Traffic Router
* Inactive: DS không được triển khai trên Cache Server và cũng không được sử dụng bởi Traffic Router

Anonymous Blocking: Chặn tất cả các IP ẩn danh (Proxy, VPN, *TOR exit node*)

CDN: Chỉ các CDN mà các DS này thuộc về. Chỉ có các cache server mới có thể định tuyến nội dung cho DS nàynày

Check Path: là URL *Origin* được sử dụng bởi Traffic Ops monitor tình trạng của Origin

Consistent Hashing Regular Expression: thuật toán trong CDN giúp anh xạ yêu cầu của client đến Edge

Consistent Query Parameter: sử dụng tham số truy vấn để xác định Cache Server nào phục vụ Client

DNSpass: CNAME, IP, IPv6, TTL. Khi truy vấn quá giới hạn sẽ chuyển tải sang máy chủ khác thường là vượt quá global max Mbps/ Tps

Global Max Mbps: Là giới hạn tốitối đa mà DS có thể phục vụ cho Cacher Server (Edge) trong CDN nếu băng thông thực tế này vượt quá giới hạn thì. Traffic Router sẽ tự động  chuyển hướng bypass

DNS TTL: DNS routed Delivery Service sẽ phân giả DNS gửi TTL kèm theo phải hồi cho client

DSCP: Đánh dấu các gói tin IP gửi đến Client

EDNS0 Client Subnet Enabled: là giao thức mở rộng của DNS, cho phép truyền các thông tin bổ sung truyền vào DNS (gửi một phần địa chỉ IP của người dùng trong quá trình truy vấn DNS đến máy chủ đích) default: false
Example URL: à các URL được tạo tự động dựa trên cấu hình của DS. Chúng đại diện cho các điểm cuối (endpoints) mà client có thể sử dụng để gửi yêu cầu truy xuất nội dung thông qua CDN.
 
* Scheme (Giao thức): Hỗ trợ HTTP, HTTPS hoặc cả hai.
* Routing Name: Được định nghĩa trong cấu hình của DS.
* xml_id của DS: Một định danh duy nhất cho DS trong hệ thống.
* Tên miền của CDN: Là tên miền mà CDN quản lý.

Fair-Queuing Pacing Rate Bps: Tốc độ tối đa mà Cache Server cung cấp trên mỗi TCP.


Max Origin Connection: Số TCP tối đa giữa Origin và MID

Info URL: là một trường trong cấu hình DS. Để lưu một URL cung cấp thông tin bổ sụng về dịch vụ này

Initial Dispersion: số lượng cache serever xữ lí Request trong Cache group

Logs Enable: bật tắt log trong DNS

Logs Description: mô tả 

Match List: là tập hợp các biểu thức chính quy được traffic router xác định yêu cầu của client có được phục vụ bở DS hay không
* HEADERHEADER
* HOST
* PATH
* STEERING

Max Request Header Bytes: Số byte tối đa trong yêu cầu DS được phép

Origin Server Base URL: Chỉ định URL của máy chủ gốc để lấy nội dung khi lưu trử cache
Origin Shield: lá chắn giữa Orgin và MID
Profile: tên của Profile được sử dụng trong DS 
Protocol: 
* 0: HTTP
* 1: HTTPS
* 2 HTTP & HTTPS
* 3 HTTP to HTTPS

Query String Handling: xử lí chuỗi truy vấn khi phục vụ nội dung DS

Range Request Handling: Yều cầu HTTP cho phép tải xuống 1 phần thay vì toàn bộ tệp (Chia nhỏ)

Raw Remap Text

Regex Remap Expression: Chuyển hướng bằng biểu thưc chính quy
    
 Statis 

Geo Limit: 
   * 0: 
   * 1 CZP
   * 2 CZP + country code








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

   





