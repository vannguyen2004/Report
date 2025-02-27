
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
 Các phương thức định vị của một nhóm Cache xác định các để Traffic Router được phép định tuyến khách hnagf đến các máy chủ Cache trong nhóm. Đây là phương thức đươc cho phép và cacs giá trị trong tập hợp này bị giới hạn bởi các giá trị sau:
   Coverage Zone File: Cho phép các Traffic Router định tuyến khách hàng đến Cache Group nếu địa chỉ IP của họ được gán ví trí địa lí bằng cách tra cứu trong Coverage Zone File.  
    Deep Coverage Zone File: giống như Coverage Zone File, tuy nhiên tùy chọn này không có bất kì tác dụng nào. DO đó , nó ẽ không xuất hiện trong các biểu mẫu của Traffic Portal.  
    Geo-IP database: Cho phép Traffic Router định tuyến khách hàng đến nhóm Cache này nếu địa chỉ IP của ho được tra cứu trong cơ sở dữ liệu anh xạ địa chỉ IP với vị trí địa lý , nhằm xác định vị trí địa lý của khách hàng
    

