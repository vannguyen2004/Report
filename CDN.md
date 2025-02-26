# CDN là gì

Hiện tại trên mạng internet, phần lớn dữ liệu media (thường là video hoặc audio) được truyền từ một nguồn (Content Provider) đến hàng triệu người dùng (Content Customer). CDN là công nghệ giúp kết nối kiểu one-to-many hiệu quả. CDN là hệ thống phân tán của các máy chủ để vận chuyển nội dung qua HTTPS. Các máy chủ được triển khai ở nhiều vị trí khác nhau với mục đích tối ưu việc truyền tải nội dung đến người dùng cuối, trong khi giảm thiểu lưu lượng trên mạng.

## Đặc trưng của CDN

Các thành phần chính của CDN bao gồm:

- **Cache Server**: Là một máy chủ vừa làm nhiệm vụ proxy các yêu cầu, vừa lưu trữ dữ liệu để tái sử dụng. Traffic Control sử dụng **Apache Traffic Server** để cung cấp các Cache Server.
  
- **Content Router**: Đảm bảo rằng các người dùng cuối được kết nối đến các Cache Server tối ưu cho vị trí và khả năng có sẵn nội dung. Traffic Control sử dụng **Traffic Router** như một Content Router.

- **The Health Protocol**: Giao thức Health giám sát việc sử dụng các Cache Server và các máy chủ thuê bao (tenants) trong mạng CDN.

- **Configuration Management System**: Trong nhiều trường hợp, CDN có thể bao gồm hàng trăm hoặc thậm chí hàng triệu server phân bố trên khu vực địa lý rộng lớn. Trong trường hợp này, việc cấu hình thủ công là không thực tế, do đó, sẽ có một cơ quan trung ương (central authority) quản lý cấu hình được sử dụng để tự động hóa càng nhiều công việc càng tốt. Traffic Ops quản lý điều này thông qua **Traffic Portal**.

- **Log File Analysis System**: Thống kê và phân tích rất quan trọng trong việc quản lý và giám sát CDN. Transaction logs và Usage Statistics cho Traffic Control được gói gọn trong **Traffic Stats**.

## HTTP/1.1

Để có cái nhìn tổng quan về traffic control, một điều quan trọng là hiểu cơ bản giao thức **HTTP/1.1** hoạt động như thế nào và chức năng của **Cache Server**.

### Chuỗi sự kiện khi yêu cầu nội dung qua HTTP/1.1:
1. Client gửi yêu cầu đến LDNS server để phân giải tên ***www.origin.com*** thành địa chỉ IP và gửi HTTP request đến địa chỉ IP đó. (Một phản hồi DNS kèm với TTL chỉ ra kết quả phân giải tên miền được coi là hợp lệ, mặc dù TTL dài như một ngày hoặc lâu hơn là khá phổ biến trong các trường hợp khác. Tuy nhiên, trong CDN, TTL DNS thường là một phút.)
  
2. Server tại ***www.origin.com*** tìm kiếm đường dẫn (path) và gửi nó trong phản hồi đến Client.

## Cache Control Headers và Revalidation

Cache Control Header là một phần trong tiêu đề HTTP mà máy chủ gốc (Origin Server) sử dụng để chỉ dẫn cách thức và khi nào dữ liệu được lưu trữ trong bộ nhớ đệm (cache) và khi  nào nó cần được làm  mới hoặc tải từ máy chủ gốc. *Tóm lại máy chủ gốc có thể kiểm soát cách mà các máy chủ proxy cache hoặc trình duyệt của bạn)

Cache Control Header là một phần của tiêu đề HTTP (HTTP header) mà máy chủ gốc (origin server) gửi đến trình duyệt hoặc các máy chủ proxy trung gian. Nó giống như một "hướng dẫn" để nói cho các thiết bị này biết:

Cách lưu trữ dữ liệu: Dữ liệu có được phép lưu vào bộ nhớ cache không, và nếu có thì lưu ở đâu (ví dụ: trình duyệt người dùng hay proxy công cộng).
Khi nào làm mới dữ liệu: Dữ liệu trong cache có thể được dùng trong bao lâu trước khi cần kiểm tra lại hoặc tải lại từ máy chủ gốc.

Vai trò của máy chủ proxy và hệ thống cache
Máy chủ proxy: Đây là một máy chủ trung gian nằm giữa người dùng (client) và máy chủ gốc. Nó có thể lưu trữ dữ liệu tạm thời để phục vụ người dùng nhanh hơn mà không cần liên hệ trực tiếp với máy chủ gốc mỗi lần.
Hệ thống cache: Bao gồm bộ nhớ cache của trình duyệt hoặc của proxy, nơi dữ liệu được lưu tạm để tái sử dụng.  
### Cách thức lưu trữ và khi nào làm mới
1. Cách thức lư trữ dữ liệu: có thể được chỉ định lưu ở đâu và có được lưu hay không. Các giá trị phổ biến như:
   Public: cho phép lưu cache ở bất kì đâu, bao gồm các các proxy công cộng
   Private: dữ liệu chỉ được lưu trong cache riêng của người dùng (như trình duyệt, không được lưu trong proxy công cộng)
   No-store: dữ liệu không được lư ở bất kì cache nào
2. Khi nào cần làm mới dữ liệu:
   Cache control header cũng quyết định thời gian dữ liệu trong cache còn tươi và khi nào cần liên hệ với máy chủ gốc để cập nhật lại. Có một số giá trị như:
       max-age=giay : dữ liệu có thể được dùng trong khoản thời gian này mà không cần kiểm tra lại
       no-cache: dữ liệu được lưu trong cache nhưng mỗi lần cần phải kiểm tra với máy chủ gốc trước khi sử 
    Ngoài ra còn có thêm Expire: thời gian hết hạn của cache thường nhỏ hơn max-age khi cache quá thời gian expire thì sẽ không bị xóa khỏi cache nhưng nếu có yêu cầu mới thì cần phải truy cập vao phản hồi đã hết hạn, cache sẽ thực hiện kiểm tra lại

### Cách ATS (Apache Traffic Server) xử lý kiểm tra lại nội dung:

Thay vì yêu cầu lại đối tượng từ máy chủ gốc, cache sẽ gửi yêu cầu đến máy chủ gốc, thông báo phiên bản của phản hồi mà cache đang có và hỏi xem nó có thay đổi hay không. Nếu có thay đổi, máy chủ sẽ gửi phản hồi 200 OK kèm dữ liệu mới. Nếu không thay đổi, máy chủ gốc sẽ gửi phản hồi 304 Not Modified, thông báo rằng phản hồi vẫn hợp lệ và cache có thể đặt lại bộ đếm thời gian cho việc hết hạn của phản hồi.

Để chỉ ra phiên bản mà khách hàng (cache) có, nó sẽ thêm một tiêu đề **If-Not-Modified-Since** hoặc **If-None-Match**. Ví dụ, trong trường hợp **If-None-Match**, máy chủ gốc sẽ gửi một tiêu đề **ETag** để xác định duy nhất phản hồi. Sau đó, khách hàng có thể sử dụng giá trị **ETag** này trong yêu cầu kiểm tra lại (revalidation request) để kiểm tra xem **ETag** của nội dung yêu cầu có thay đổi hay không.
