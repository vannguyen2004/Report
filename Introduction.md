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

# Profile and Parameters
Profile và Parameter: được dùng để cấu hình 
Mối quan hệ giữa Profile và parameter: Profile là tập hợp các Parameter
Parameter: Là những giá trị cụ thể giúp Profile hoạt động theo yêu cầu
Một profile có thể áp dụng cho máy chủ, Delivery Request hoặc cache Group trong CDN

  
### Profile
Properties: Profile là tập hợp ác cấu hình được gán cho các thành phần như: 
+ Máy chủ CDN
+ Delivery Service
+ Cache Group  

Mỗi Profile chứa các tham số để quyết định cách CDN hoạt động. Giúp chuẩn hóa cấu hình cho các thành phần trong CDN
Các thành phần của Profile như:
  + Description: mô tả
  + ID: Định danh cho môt Profile duy nhất
  + Name: Tên của Profile đó (ten khong được có space)
  + Routing disabled: Vô hiệu hóa định tuyến kh đi qua CDN. Nó cần thiết có trên bất kì **Type** 
  + Type: Xác định loại nội dung hoặc cách hoạt động cảu profile trong đó

### Parameter trong CDN
Parameter trong CDN: là giá trị cấu hình cụ thể bên trong một profile   
Mỗi Profile có thể chứa nhiều Parameter để kiểm soát chức năng của CDN  
** Mối quan hệ của Profile và Parameter**: Prfole là tập hợp các parameter  

## Role
Là bộ sưu tập **quyền** có thể là 0 hoặchoặc nhiều người dùng
Description: là mô tả giúp con người hiểu 
Last Update: ngày giờ điều chỉnh role
Name: tên của role đó
Admin Role: là role có tất cả các quyền
Permissions: Role Permission là tập hợp các quyền tương tác với API

# Traffic Monitor: 

  Là dịch vụ giám sác các Cache Server
  
  Nó thăm dò các cache server với 2 trạng thái Reported và Admin_DOWN theo tham số được cấu hình trong Traffic. Nếu một máy chủ được đánh dấu là ADMIN_DOWN thì nó không còn khả dụng *nhưg vẫn được thăm dò để kiểm tra tính khả dụng và thu thập số liệu thống kê*  
  
  Còn nếu máy chủ là ONLINE hay OFFLINE thì máy chủ sẽ không thăm giò và nó được coi là đã cấu hình và không kiểm 
  
  Traffic Monitor kiểm tra bằng cách gửi :  
     + Thông lượng: số byte vào số byte ra  
     + Transaction: mã phản hồi của HTTP 2xx 3xx 4xx  
     + Connection: 
     + Cache perfomace: 
     + Hiệu suất bộ nhớ đệm
     + Hiệu suất lưu trữ
     + Hiệu suất hệ thống  
     
 Healthy Protocol: Các máy chủ Traffic Monitor được phòng hoạt động độc lập với nhau nhưg khi được yêu cầu cung cấp thông tin trạng thái, thì chúng sẽ xem luôn các Traffic Monitor khác. Trong quá trình giám sát cache (Cache Monitoring) mỗi traffic Monitor duy nhất sẽ đánh giá trạng thái của máy chủ Cache theo quan điểm của nó.  
 
 Giao thức Health: mở rộng cách xác định CDN bằng cách kết hợp trạng thái của tất cả các Traffic monitor thành trạng thái tổng thể nó sử dụng Optimistic Approach khi đánh giá một máy chủ Cache hoặc một Delivery Service có thực sự không khả dụng hay không
 
 Trong quá trình hoạt động. Traffic Monitor tổng hợp tất cả các trạng thái của của peer kể cả nó bằng cách thăm dò peer có trang thái.
 Nếu có bốn Traffic Monitors, trong đó ba cái đánh dấu một máy chủ cache hoặc Delivery Service là không khả dụng, nhưng một cái vẫn coi nó là khả dụng, thì nó vẫn được coi là khả dụng.
 
Cách tiếp cận lạc quan này trái ngược với triết lý "fail fast" (ngừng hoạt động ngay lập tức khi phát hiện lỗi), nhưng nó rất hữu ích cho các hệ thống mạng lớn có địa lý phức tạp và định tuyến đa dạng

Lợi ích chính của Optimistic Health Protocol là:
Giúp tránh gián đoạn không cần thiết: Nếu có lỗi mạng hoặc độ trễ trong một khu vực, hệ thống vẫn tiếp tục hoạt động bình thường mà không ảnh hưởng đến toàn bộ lưu lượng truy cập.
Tạo ra một cái nhìn toàn diện hơn về sức khỏe hệ thống: Vì mỗi Traffic Monitor có thể có một góc nhìn khác nhau về mạng khi được triển khai ở các vị trí địa lý khác nhau.

Optimistic Quorum:  ngăn chặn tình trạng "split-brain" (phân tách bộ nhớ giám sát), cần có ít nhất ba máy chủ Traffic Monitor để giám sát một hệ thống CDN và tính năng "optimistic quorum" nên được kích hoạt.

Tính năng optimistic quorum có thể được sử dụng bằng cách đặt giá trị peer_optimistic_quorum_min trong traffic_monitor.cfg thành một số lớn hơn 0. Giá trị này đại diện cho số lượng tối thiểu các peer cần phải có sẵn để tham gia vào Optimistic Health Protocol.
*Nếu Traffic Monitor phát hiện số lượng peer khả dụng ít hơn giá trị đã đặt, nó sẽ tự động rút khỏi giao thức giám sát sức khỏe bằng cách trả về mã lỗi 503 khi có yêu cầu kiểm tra trạng thái sức khỏe của máy chủ cache, cho đến khi kết nối được khôi phục.*

## Protocol  Engagement
* Việc sử dụng chu kỳ kiểm tra (polling intervals) ngắn đối với cả máy chủ cache và các Traffic Monitor peer giúp giảm thiểu tác động đến khách hàng trong trường hợp có sự cố. 
Việc một Traffic Monitor đánh dấu một cache server là không khả dụng là điều bình thường trong hệ thống CDN. Ví dụ:

Nếu một video được nhiều người xem khiến một cache server gần đạt đến giới hạn băng thông của nó, thì Health Protocol sẽ tự động kích hoạt.
Lúc này, Traffic Monitor sẽ đánh dấu cache server đó là không khả dụng.
Khi khách hàng mới yêu cầu cùng video đó, Traffic Router sẽ chuyển hướng họ đến một cache server khác trong cùng Cache Group, giúp chia sẻ tải giữa các máy chủ cache.

Khi các khách hàng đã xem xong video trên máy chủ cache bị quá tải trước đó, băng thông của nó giảm xuống dưới ngưỡng giới hạn và nó được đánh dấu là khả dụng trở lại. Lúc này, Traffic Router sẽ tiếp tục gửi khách hàng mới đến nó.
Việc một Delivery Service bị đánh dấu là không khả dụng hiếm khi xảy ra.

Các ngưỡng giới hạn của Delivery Service thường chỉ được sử dụng trong các trường hợp quá tải cực đoan.
Điều này nhằm bảo vệ các dịch vụ khác trong CDN khỏi bị ảnh hưởng khi có lưu lượng truy cập đột biến.

# Traffic OPs

* Traffic Ops là công cụ quản trị (cấu hình và giám sát) tất cả các thành phần trong một hệ thống CDN sử dụng Traffic Control. 

Traffic Portal sử dụng Traffic Ops API để quản lý máy chủ (servers), nhóm bộ nhớ đệm (Cache Groups), dịch vụ phân phối nội dung (Delivery Services), v.v.
Trong nhiều trường hợp, một thay đổi cấu hình cần được truyền đến nhiều, hoặc thậm chí tất cả, các máy chủ cache và phải được thực hiện theo thứ tự trước hoặc sau khi thay đổi đó được áp dụng cho Traffic Router.

Traffic Ops đảm bảo tính nhất quán của cấu hình giữa các thành phần khác nhau trong CDN:   
* Lưu trữ dữ liệu và bảo mật
* Tạo và cập nhật cấu hình
* Kiểm tra tình trạng hoạt động

 Traffic Ops giống như trung tâm điều hành, nơi quản lý đội xe, tuyến đường, trạng thái giao hàng, v.v.
Traffic Portal là giao diện web để bạn điều khiển Traffic Ops.
Các xe tải giao hàng (cache servers) sẽ liên lạc với trung tâm điều hành để nhận hướng dẫn tuyến đường mới (cấu hình cập nhật).
Nếu một xe tải gặp sự cố, Traffic Ops sẽ kiểm tra và đánh giá tình trạng hoạt động rồi điều chỉnh lộ trình phù hợp.
Dữ liệu quan trọng (ví dụ: danh sách khách VIP hoặc mã xác thực bảo mật) được lưu trữ trong Traffic Vault, giống như một két sắt riêng biệt để bảo vệ thông tin quan trọng.

Traffic Portal: Traffic Portal là một ứng dụng khách AngularJS 1.x được phục vụ từ máy chủ web Node.js được thiết kế để sử dụng API Traffic Ops. Đây là sự thay thế chính thức cho Traffic Ops UI cũ.

# Traffic Router
* Traffic router có chức năng gửi client đến các Cache Server tối ưu nhất 

Chức năng của Traffic Router là định tuyến khách hàng đến máy chủ cache tối ưu nhất.
Trong trường hợp này, "tối ưu" được xác định dựa trên một số yếu tố:

* Khoảng cách giữa máy chủ cache và khách hàng 

Không nhất thiết là khoảng cách vật lý, mà thường được tính theo số bước nhảy mạng (Layer 3 network hops).
Ít bước nhảy hơn giữa khách hàng và máy chủ cache giúp cải thiện hiệu suất và giảm tải mạng.
Traffic Router giúp khách hàng kết nối với máy chủ cache có hiệu suất tốt nhất và chi phí mạng thấp nhất cho vị trí của họ.
 Tình trạng của máy chủ cache và mức tải xử lý/mạng

Một vấn đề phổ biến trong Internet và truyền hình trực tuyến là nhiều khách hàng cùng lúc tải nội dung giống nhau.
Traffic Router giúp khách hàng tránh các máy chủ cache bị quá tải hoặc bị vô hiệu hóa có chủ đích.
 Sự sẵn có của nội dung trên máy chủ cache

Việc tái sử dụng nội dung (cache hits) là yếu tố cải thiện hiệu suất quan trọng nhất trong CDN.
Traffic Router sẽ hướng khách hàng đến máy chủ cache có nhiều khả năng đã lưu trữ nội dung cần tìm, giúp tăng tốc độ truy xuất.

## DNS Content Routing
Định tuyến nội dung bằng DNS (DNS Content Routing)

* Đối với một DNS Delivery Service, khách hàng có thể nhận được một URL như: http://video.demo1.mycdn.ciab.test/.
* Khi máy chủ LDNS (Local DNS Server) tìm kiếm IP của video.demo1.mycdn.ciab.test, nó sẽ gửi truy vấn đến Traffic Router vì Traffic Router là DNS có thẩm quyền (authoritative DNS) của miền mycdn.ciab.test và các miền con.
* Traffic Router phản hồi bằng danh sách địa chỉ IP của các máy chủ cache đủ điều kiện, dựa trên vị trí của LDNS.

Lưu ý:

Khi phản hồi, Traffic Router không biết IP thực tế của khách hàng cũng như đường dẫn cụ thể mà khách hàng sẽ yêu cầu.
Quyết định chọn IP của máy chủ cache hoàn toàn dựa vào vị trí của LDNS và trạng thái sức khỏe của các máy chủ cache.

HTTP Content Routing:
Đối với Dịch vụ phân phối HTTP, máy khách có thể nhận được URL như http://video.demo1.mycdn.ciab.test/. LDNS giải quyết video.demo1.mycdn.ciab.test này thành một địa chỉ IP, nhưng trong trường hợp này, Traffic Router trả về địa chỉ IP của riêng nó. Máy khách mở kết nối đến cổng 80 (HTTP) hoặc cổng 443 (HTTPS) trên địa chỉ IP của Traffic Router và gửi yêu cầu của nó.


# Traffic Stats
Traffic Stats là một chương trình được viết bằng Go, dùng để thu thập và lưu trữ số liệu thống kê về các CDN (Mạng phân phối nội dung) do Traffic Control quản lý. Traffic Stats lấy dữ liệu từ API của Traffic Monitor và lưu trữ trong InfluxDB hoặc Kafka. Dữ liệu thường được lưu trong InfluxDB trong thời gian ngắn (dưới 30 ngày), trong khi dữ liệu Kafka có sẵn để tiêu thụ dựa trên thời gian lưu trữ được cấu hình

Traffic Stats có hai chức năng chính:

Thu thập số liệu thống kê cho các máy chủ cache ở tầng biên (Edge-tier) và các Dịch vụ Phân phối (Delivery Services) theo khoảng thời gian có thể cấu hình (mặc định là 10 giây), rồi lưu trữ trong InfluxDB hoặc Kafka.
Tóm tắt số liệu thống kê hàng ngày (khoảng nửa đêm UTC), tạo báo cáo hàng ngày chứa băng thông tối đa (Max Gbps Served) và tổng số byte đã phục vụ (Total Bytes Served).

Traffic Stats giúp lưu trữ và phân loại dữ liệu thống kê liên quan đến hệ thống CDN. Cụ thể:

* Cache_stats theo dõi hiệu suất của từng máy chủ cache, giúp quản trị viên biết được lưu lượng băng thông, số lượng kết nối khách hàng, và nhóm cache mà máy chủ thuộc về
* deliveryservice_stats giám sát hiệu suất của từng dịch vụ phân phối nội dung (Delivery Service), đo lường lưu lượng và số lượng phản hồi HTTP theo từng nhóm mã trạng thái (2xx, 3xx, 4xx, 5xx)
* daily_stats tổng hợp dữ liệu mỗi ngày để tạo báo cáo về băng thông cao nhất và tổng dữ liệu đã phục vụ.



# Traffic Vault
Traffic Vault is a data store used for storing the following types of sensitive
information: 
* SSL Certificates
    * Private Key
    * Certificate
    * CSR
* DNSSEC Keys
    * Key Signing Key
        * private key
        * public key
    * Zone Signing Key
        * private key
        * public key
* URL Signing Keys

















