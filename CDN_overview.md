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

# Traffic Control Overview

## 1. Cache Group - Ostensibly
Là nhóm máy chủ Cache. Trong CDN của Traffic Control, mọi máy chủ phải thuộc về Cache Group (Kể cả khi chúng không làm nhiệm vụ Cache). Thông thường một Cache group là nhóm máy chủ có sẵn nằm cùng một đơn vị địa lí cụ thể.

Mặc dù các máy chủ có vị trí vật lí khác nhau, tuy nhiên khi máy chủ được chọn để phục vụ client thì sẽ dựa trên Cache group chứ không phải vị trí thật của máy chủ đó.

Các loại Cache Group phổ biến nhất là:
- **Edge_loc**: bao gồm các máy chủ Cache ở tầng Edge
- **Mid_Log**: tầng Mid sẽ là cha của Parent của một hoặc nhiều Cache Group ở tầng Edge tạo thành hệ thống phân cấp Cache hai tầng

### Vai trò của hai tầng:
- **Tầng Edge**: gần người dùng, phản hồi nhanh.
- **Tầng Mid**: Hỗ trợ nhiều Edge, giảm tải cho Origin.
- **Origin**: Máy chủ gốc, nơi lưu toàn bộ nội dung chính thức. Chỉ được hỏi khi cả Edge và Mid không có nội dung.

![image](https://github.com/user-attachments/assets/765b0b9f-2457-4c12-bcee-3846c5c3ea43)

## Regions, Divisions and Locations

Thêm vào Cache group, tất cả server có vị trí vật lí riêng (bao gồm kinh độ và vĩ độ). Mỗi Physical Location thuộc về một **Region (Vùng)**, và mỗi Region thuộc về một **Division (Phân khu)**.

![image](https://github.com/user-attachments/assets/840d7117-0f68-4fd0-9792-fba1df2723e7)

## Properties
Cache Group được định nghĩa và lưu trữ trong hệ thống **ATC (Apache Traffic Control)**, nhưng nó xuất hiện ở nhiều nơi với các viết khác nhau:
- **Traffic Ops database**: Đây là cơ sở dữ liệu chính lưu thông tin về Cache Group.
- **Traffic Portal**: Giao diện người dùng (dạng bảng, biểu mẫu) để quản lý Cache Group.
- **Traffic Ops API**: Các phiên bản API (giao diện lập trình) dùng để truy cập Cache Group từ mã nguồn, đặc biệt trong phiên bản mới viết bằng ngôn ngữ Go.

### Tại sao phải cần tổng hợp thuộc tính?
Vì Cache Group xuất hiện ở nhiều nơi (Database, Portal, API), mỗi nơi có thể gọi các thuộc tính của nó (như tên, vị trí, loại) bằng những tên khác nhau.

### Cách viết tên dễ đọc:
- **camelCased**: Viết liền, chữ cái đầu của từ thứ hai trở đi in hoa (ví dụ: `cacheGroupName`).
- **snake_cased**: Dùng dấu gạch dưới giữa các từ (ví dụ: `cache_group_name`).

### Bí danh (Aliases)
Một số thuộc tính có thể có nhiều tên khác nhau (bí danh) ở các phần khác nhau của hệ thống.
Nếu bí danh không chỉ là thay đổi kiểu chữ (như từ `cacheGroup` thành `CacheGroup`), chúng sẽ được liệt kê trong một bảng riêng để bạn biết.

## ASNs
Một Cache Group có thể được gán **zero hoặc nhiều ASN (Autonomous System Numbers - Số Hệ thống Tự trị)**, được sử dụng để phân loại lưu lượng truy cập đi qua một CDN. 

Chúng thường không được thể hiện trực tiếp trên đối tượng Cache Group, mà thay vào đó là một đối tượng riêng biệt biểu thị mối quan hệ, ví dụ trong các yêu cầu và phản hồi của điểm cuối `asns`.

Thông tin về ASN không được ghi thẳng vào đối tượng Cache Group (ví dụ: không có trường `ASN` trong thông tin của Cache Group).

## Coordinate
Tọa độ của Cache Group xác định vị trí địa lý của Cache group, được sử dụng để định tuyến khách hàng thông qua vị trí địa lý. Nó cũng được dùn để xác định Cache Group khác gần nhất nhằm dự phòng theo khoảng cách gần nhất (failback to closeset) 
Thông thường điều này được biểu diễn dưới dạng số nguyên duy nhất dùng để nhận diện Coordinate được liên kết với cache group. Tuy nhiên thì nó cũng có thể là tên  
## Fallback
Là nhóm 0 hoặc nhiều Cache Group được xem xét định tuyến khi một cache group không khả dụng do quá tải hoặc bảo trì kéo dài. Chúng thường được biểu diễn dưới dạng một mảng chứa ID của từng cache group nhưng đôi khi có thể xuất hiện dưới dạng tên hoặc tên viết tắt của từng Cache Group.  
![image](https://github.com/user-attachments/assets/e9d6d706-62ac-49a7-a94e-b4b2a14f7b79)

### Fallback to Closest
Trường này mang giá trị boolean, nếu nó mang giá trị true, True vv sẽ khiến quá trình định tuyến tuyến dự phòng sang cache Group gần nhất về mặt địa lý khi Cache Group natf trở nên không khả dụng hay tải 
(Nói chung Fallback hay Fallback to closest là các mà máy chủ điều hướng sang một máy chủ khác khi nó không khả dụng nữa)
### ID 
Tất cả các Cache Group đều có một số nguyên duy nhất được sử dụng chủ yếu để tham chiều trong traffic Ops API  
Mặc dù tên của một Cache Group phải là duy nhất, nhưng đây là định danh Cache Group được sử dụng phổ biến trong ngữ cảnh của ATC. Một ngoại lệ đáng chú ý là trong CDN Snapshot và cấu hình định tuyến  được sử dụng bởi Traffic Router  
### Latitude
Vĩ độ địa từ của Cache Group được sử dụng trong định tuyến và cho mục đích Dự phòng theo khoảng cách gần nhất (Fallback to Closest).  
### Localization Methods
 Các phương thức định vị của một nhóm Cache xác định các để Traffic Router được phép định tuyến khách hàng đến các máy chủ Cache trong nhóm. Đây là phương thức đươc cho phép và cacs giá trị trong tập hợp này bị giới hạn bởi các giá trị sau: (
   Coverage Zone File: Cho phép các Traffic Router định tuyến khách hàng đến Cache Group nếu địa chỉ IP của họ được gán ví trí địa lí bằng cách tra cứu trong Coverage Zone File.  
    Deep Coverage Zone File: giống như Coverage Zone File, tuy nhiên tùy chọn này không có bất kì tác dụng nào. DO đó , nó ẽ không xuất hiện trong các biểu mẫu của Traffic Portal.  
    Geo-IP database: Cho phép Traffic Router định tuyến khách hàng đến nhóm Cache này nếu địa chỉ IP của ho được tra cứu trong cơ sở dữ liệu anh xạ địa chỉ IP với vị trí địa lý , nhằm xác định vị trí địa lý của khách hàng
### Longitude (vĩ độ)
### Name
Tên unique cho Cache Group
### Parent
Cache Group có thể là cha của Cache Group. Nó có nghĩa khác nhau dựa vào Cache  `type`. EDGE_LOC phải có Cache group cha là MID_LOG
 EDGE_LOC: Cache Server ở biên
 MID_LOC: Cache Server ở trung gian 
 MID_LOG phải  có Cache Server ORG_LOC. Nếu MID_LOG không có cha hay toàn có cha nhưng không phải và ORG_LOG thì tất cả các nhóm G_LOG (thậm chí khác CDN) sẽ được coi là cha của nó.
 Secondary Parent:
Secondary Parent của Cache group được sử dụng trong fallback dùng để dữ phòng các Cache Server với nhau (sau khi quá trình định tuyến diễn ra khác Fallback và Fallback to Closest
### Server

 Mỗi Mục đích chính của một Cache Group là chứa các máy chủ. Trong hầu hết các trường hợp, người ta ngầm hiểu hoặc giả định rằng các máy chủ này là cache servers, nhưng điều này không bắt buộc. Trên thực tế, không hiếm trường hợp các máy chủ bên trong một Cache Group thuộc loại khác.
 Mỗi cache group có thể không hoặc nhiều máy chủ gán vào nó nhưng mỗi máy chủ chỉ được ở trong một cache group

# Delivery Service Request

**Delivery Service Request** trong CDN là một yêu cầu để tạo hoặc thay đổi một **Delivery Service** - một cấu hình điều hướng lưu lượng của CDN để phân phối nội dung đến người dùng cuối (bao gồm tạo mới DSR, xóa, chỉnh sửa).

## Cấu trúc của DSR trong phiên bản mới nhất của Traffic Óp API và Typescript Interface

- **Assignee**: chỉ định tên người dùng của người dùng được chỉ định trong. Nó có thể được để trống nếu không gán cho bất kỳ DSR nào.
- **Author**: là username của người tạo DSR.
- **Change Type**: chuỗi biểu thị hành động sẽ thực hiện trong sự kiện của DSR được đáp ứng. Bao gồm:
  - **Create**: New Delivery Service Request sẽ được tạo.
  - **Delete**: xóa.
  - **Update**: chỉnh sửa Delivery hiện tại.
- **Created At**: Đây là ngày giờ mà DSR được tạo.
- **ID**: định danh duy nhất cho DSR.
- **Last Edited By**: username người cuối cùng chỉnh sửa DSR.
- **Original**: Nếu thuộc tính này của DSR tồn tại thì nó đại diện cho DSR ban đầu trước khi DSR đã/được/đang thực hiện. Thuộc tính này chỉ tồn tại trên các DSR có **Change Type** là **Update** và **Delete**.

  **Lưu ý**: `original` là thuộc tính trong DSR, dùng để lưu trữ bản sao đầy đủ của Delivery Service trước khi có sự thay đổi.
  
  - Chỉ xuất hiện khi **Change Type** là **update** hoặc **delete**, giúp hệ thống ghi nhớ thông tin trước khi áp dụng thay đổi.
    - ✅ Nếu **Change Type** = "update", `original` chứa dữ liệu trước khi cập nhật.
    - ✅ Nếu **Change Type** = "delete", `original` chứa dữ liệu của Delivery Service trước khi bị xóa.
    - ✅ Nếu **Change Type** = "create", `original` không tồn tại vì không có gì trước đó.

- **Request**: là thuộc tính trong DSR, chứa thông tin của **Delivery Service** mà người dùng muốn tạo hoặc cập nhật.

  **Lưu ý**: Chỉ xuất hiện khi **Change Type** là **update** hoặc **create**, giúp hệ thống biết được thông tin mới mà người dùng muốn áp dụng.
  
  - ✅ Nếu **Change Type** = "update", `requested` chứa dữ liệu sau khi cập nhật.
  - ✅ Nếu **Change Type** = "create", `requested` chứa dữ liệu của Delivery Service mới cần tạo.
  - ✅ Nếu **Change Type** = "delete", `requested` không tồn tại vì không có gì để thay thế.


**Status**: Trạng thái là một chuỗi ký tự cho biết vị trí của một DSR trong vòng đời quy trình công việc DSR. Thông thường, một DSR có thể ở trạng thái “mở” (open), có nghĩa là nó có thể được chỉnh sửa, xem xét và có thể hoàn thành hoặc bị từ chối, hoặc ở trạng thái “đóng” (closed), có nghĩa là nó đã được hoàn thành hoặc bị từ chối. Cụ thể hơn, các DSR ở trạng thái “mở” có một trong các trạng thái sau:

 - Dự thảo (draft): DSR chưa sẵn sàng để hoàn thành hoặc xem xét có thể dẫn đến việc từ chối, vì nó vẫn đang được làm việc tích cực.

 - Đã gửi (submitted): DSR đã được gửi đi để xem xét, nhưng chưa được xem xét.

 - Trong khi đó, một DSR ở trạng thái “đóng” có một trong các trạng thái sau:

 - Hoàn thành (complete): DSR đã được phê duyệt và hành động đã tuyên bố được thực hiện.

 - Đang chờ (pending): DSR đã được phê duyệt và các thay đổi đã được áp dụng, nhưng cấu hình mới chưa được phân phát đến các thành phần khác của ATC - thường có nghĩa là nó không thể được coi là hoàn thành cho đến khi một Snapshot được thực hiện hoặc một Queue Update được thực hiện.

 - Bị từ chối (rejected): DSR đã bị từ chối và đóng lại; nó không thể được hoàn thành.

 - Một DSR ở trạng thái “đóng” không thể được chỉnh sửa - ngoại trừ việc thay đổi trạng thái “đang chờ” thành “hoàn thành” hoặc “bị từ chối”.



 
 
 
    

