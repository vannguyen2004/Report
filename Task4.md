# Khái niệm cơ bản về EXIM và Dovecot
### Exim ###
  Exim là một trong phần mềm máy chủ email được sử dụng nhiều nhất cho các hệ điều hành giống Unix, chẳng hạn như Linux và Solaris,….Nó đóng vai trò như một Mail Transfer Agent (MTA) có nhiệm vụ chuyển mail từ người gửi đến đến người nhận thông qua máy chủ mail  
  *Một số ưu điểm của Exim*:  
    + Cấu hình linh hoạt: Exim cho phép người quản trị cấu hình chi tiết cách thức gửi nhận email, như các quy tắc bảo mật, xử lý bưu kiện email, hạn chế spam,...    
    + Quản lý hàng đợi email: Exim cho phép quản lí email trong hàng đợi queue, giúp the dõi và kiểm tra các email bị lỗi và không thể gửi đi  
    + Khả năng mở rộng: Exim có thể được tích hợp với các công cụ khác như spam filtering, antivirus và các hệ thống phân phối email phức tạp.  
### Dovecot
  Dovecot là một MDA (Mail Delivery Agent) là một máy chủ IMAP và POP3 nguồn mở dành cho các hệ điều hành giống Unix cung cấp quyền truy cập cho email qua giao 2 giao thức trên  
# DKIM, SPF, PTR
### DKIM
  Là phương thức xác thực Mail, được thiết kế để phát hiện mail giả mạo
  *Các hoạt động*: theo 2 giai đoạn là ký (máy chủ gửi) và xác thực (máy chủ nhận)
  Sử dụng bảng ghi DNS để xác thực email
  Khi tạo cặp khóa private và public. Khóa riêng sẽ được thêm vào máy chủ gửi mail và sử dụng nó để ký mail trước khi gửi đi nó được gắn vào phần header của mail. Mail Server của người nhận kiểm tra chữ kí DKIM bằng khóa công khai trong DNS của domain người  
### SPF
  Xác định Server nào được phép gửi Mail từ một tên miền cụ . Mục đích dùng để xác thực mail kiểm tra tính hợp phép của Mail  
  Quản trị viên sẽ thiết lập bảng ghi TXT trong DNS để khai báo danh sách các máy chủ có quyền gửi Email thay mặc cho domain đó. Khi nhận được Mail máy chủ nhận sẽ lấy domain trong mail sau đó truy vấn DNS của example.com để lấy bảng ghi SPF nó tìm bảng ghi TXT chứa v=spf1 và lấy danh sách IP cho phép sau đó kiểm tra địa chỉ IP của máy chủ gửi có khớp với địa chỉ IP trong danh sách hay không
  ## PTR (bảng ghi Pointer)
    Dùng để tra cứu ngược từ địa chỉ IP sang tên miền
  
