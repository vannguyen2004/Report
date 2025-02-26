# Traffic Control Overview
1. Cache Group - Ostensibly
  Là nhóm máy chủ Cache. Trong CDN của Traffic Control  mọi máy chủ phải thuộc về Cache Group (Kể cả khi chúng không làm nhiệm vụ Cache). Thông thường một Cache group là nhóm máy chủ có sẵn nằm cùng một đơn vị địa lí cụ thể.
Mặc dù các máy chủ có vị trí vật lí khác nhau, tuy nhiên khi máy chủ được chọn để phục vụ client thì sẽ dựa trên Cache group chứ không phải vị trí thật của máy chủ đó.
  Các loại Cache Group phổ biến nhất là Edge_loc bao gồm các máy chủ Cache ở tâng Edge và Mid_Log. Mỗi Cache group tầng Mid sẽ là cha của Parent của một hoặc nhiều Cache Group ở tâng Edge tạo thành hệ thống phân cấp Cache hai tầng trong
  Vai trò của hai tầng:
    + Tầng Edge: gần người dùng, phản hồi nhanh.
    + Tầng Mid: Hỗ trợ nhiều Edge, giảm tải cho Origin
    + Origin: Máy chủ gốc, nơi lưu toàn bộ nội dung chính thức. Chỉ được hỏi khi cả Edge và mid không có nội dung.

 ![image](https://github.com/user-attachments/assets/765b0b9f-2457-4c12-bcee-3846c5c3ea43)

 Regions, Divisions and Locations

  Thêm vào Cache group, tất cả server có vị trí vật lí riêng (bao gồm kinh độ và vĩ độ). Mỗi Physical Location thuộc về một Region (Vùng), và mỗi Region thuộc về một Division (Phân khu)
  ![image](https://github.com/user-attachments/assets/840d7117-0f68-4fd0-9792-fba1df2723e7)

Properties
  Cache Group được định nghĩa và lưu trữ trong hệ thống ATC (Apache Traffic Control), nhưng nó xuất hiện ở nhiều nơi với các viét khác nhau
  Nó xuất hiện trọng
    - Traffic Ops database: Đây là cơ sở dữ liệu chính lưu thông tin về Cache Group
    - Traffic Portal: Giao diện người dùng (dạng bảng, biểu mẫu) để quản lý Cache Group
    - Traffic Ops API: Các phiên bảng API (giao diện lập trình) dùng để truy cập Cache Group từ mã nguồn, đặt biệt trong phiên bảng mới viết bằng ngôn ngữ Go
    Tại sao phải cần tổng hợp thuộc tính: Vì Cache Group xuất hiện ở nhiều nơi (database, Portal, API) mỗi nơi có thể gọi các thuộc tính của nó (như tên vị trí loại) bằng những tên khác nhau.
    Cách viết tên dễ đọc, kiểu viết
      + camelCased: Viết liền, chữ cái đầu của từ thứ hai trở đi in hoa (ví dụ: cacheGroupName).
      + snake_cased: Dùng dấu gạch dưới giữa các từ (ví dụ: cache_group_name).
    Bí danh (Aliases)
      Một số thuộc tính có thể có nhiều tên khác nhau (bí danh) ở các phần khác nhau của hệ thống.
Nếu bí danh không chỉ là thay đổi kiểu chữ (như từ cacheGroup thành CacheGroup), chúng sẽ được liệt kê trong một bảng riêng để bạn biết.
ASNs 
  Một Cache Group có thể được gán zéro hoặc nhiều ASN (Autonomous System Numbers - Số Hệ thống Tự trị), được sử dụng để phân loại lưu lượng truy cập đi qua một CDN. Chúng thường không được thể hiện trực tiếp trên đối tượng Cache Group, mà thay vào đó là một đối tượng riêng biệt biểu thị mối quan hệ, ví dụ trong các yêu cầu và phản hồi của điểm cuối asns.
  Thông tin về ASN không được ghi thẳng vào đối tượng Cache Group (ví dụ: không có trường "ASN" trong thông tin của Cache Group).
  

 
 
