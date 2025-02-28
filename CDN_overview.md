
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



 
 
 
    

