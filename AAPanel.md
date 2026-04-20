**Luồng thực hiện (Workflow)**

Quy trình bắt đầu bằng việc cài đặt aaPanel để chuyển đổi từ quản lý dòng lệnh sang quản trị qua giao diện web, thiết lập môi trường LNMP (Nginx, MySQL, PHP) làm nền tảng cho website. Sau đó, tiến hành khai báo hai tên miền và cài đặt WordPress, sử dụng All-in-One WP Migration để xuất toàn bộ dữ liệu (bao gồm cả mã nguồn và cơ sở dữ liệu đã được mã hóa chuỗi) từ máy chủ cũ và nhập vào máy chủ mới nhằm đảm bảo tính đồng bộ. Khi website đã hoạt động, bước tiếp theo là cài đặt các công cụ chức năng như Rank Math SEO để tối ưu tìm kiếm, Elementor/Divi để thiết kế giao diện kéo thả, và cuối cùng là tối ưu hóa hiệu suất bằng cách phối hợp giữa WP-Optimize (dọn dẹp định kỳ cơ sở dữ liệu) và LiteSpeed Cache (lưu trữ bản sao tĩnh ở tầng máy chủ) để tăng tốc độ tải trang tối đa.

**Các kiến thức trọng tâm cần nắm**

   + Quản trị Control Panel (aaPanel): Cách triển khai môi trường LNMP tự động, quản lý Virtual Host và cơ sở dữ liệu trên giao diện đồ họa.

   + Cơ chế Migration (Di cư dữ liệu): Hiểu cách All-in-One WP Migration đóng gói và tự động thay đổi các đường dẫn (URL) trong Database từ domain cũ sang domain mới thông qua kỹ thuật Serialize/Deserialize dữ liệu.

   + Hệ sinh thái WordPress chuyên nghiệp: Vai trò của các Page Builder (Elementor/Divi) trong thiết kế UX/UI và công cụ SEO (Rank Math) trong việc tối ưu cấu trúc dữ liệu cho bộ máy tìm kiếm.

   + Phân tầng Caching (Bộ nhớ đệm): Phân biệt rõ sự khác biệt giữa tối ưu hóa ở tầng ứng dụng (WP-Optimize: dọn rác, nén ảnh) và tối ưu hóa ở tầng máy chủ (LiteSpeed Cache: phản hồi HTML tĩnh nhanh mà không cần thực thi PHP/SQL).

   + Điều kiện triển khai LiteSpeed Cache: Chỉ hoạt động hiệu quả nhất khi phần cứng máy chủ chạy phần mềm Web Server là LiteSpeed (như hệ thống của Vietnix), giúp tận dụng sức mạnh xử lý trực tiếp từ tầng hệ thống thay vì thông qua mã nguồn PHP thông thường.


# **Giai đoạn 1: Cài đặt hệ quản trị aaPanel lên VPS**

Mục tiêu của giai đoạn này là biến cái VPS chỉ có dòng lệnh đen trắng của bạn thành một trang quản trị có giao diện web dễ dùng.
Bước 1: Chạy lệnh cài đặt

Bạn truy cập vào Terminal (SSH vào root@221.132.21.141) và dán duy nhất lệnh này:
```
wget -O install.sh http://www.aapanel.com/script/install-ubuntu_6.0_en.sh && sudo bash install.sh
```

Hệ thống sẽ hỏi bạn có muốn cài đặt aaPanel vào thư mục /www không? Bạn gõ y rồi nhấn Enter.
Bước 2: Chờ đợi và Lưu thông tin

Quá trình này mất khoảng 2-5 phút. Khi kết thúc, màn hình sẽ hiện lên một cái khung chứa thông tin quan trọng. Bạn phải copy và lưu lại 3 dòng sau:
+  Outer Login URL:  **https://221.132.21.141:31128/e1b82c79**
    
+  username: **rohugz55**
    
+ password: **cbfc5207**

# **Giai đoạn 2: Thiết lập môi trường LNMP**
Bước 1: Bỏ qua bảng đăng ký

Trong hình bạn gửi, có một cái bảng hiện lên yêu cầu đăng ký tài khoản aaPanel.

    Bạn hãy nhấn nút Skip ở góc dưới cùng bên phải của cái bảng đó để vào thẳng bên trong. Không cần thiết phải đăng ký lúc này.

# **Bước 2: Chọn bộ cài đặt LNMP**

Sau khi nhấn Skip, aaPanel sẽ tự động hiện ra một bảng chọn có tiêu đề là "Recommended software stacks". Bạn hãy chọn cột bên trái (LNMP) và chọn các phiên bản như sau:

    Nginx: Chọn bản mới nhất (thường là 1.22 hoặc 1.24).

    MySQL: Chọn bản 5.7 (ổn định và nhẹ nhất cho Lab).

    PHP: Chọn bản 7.4 hoặc 8.1 (để chạy WordPress mượt nhất).

    phpMyAdmin: Chọn bản 5.0 trở lên.

    FTP: Không cần thiết, có thể bỏ qua hoặc để mặc định.

## Tạo Website WordPress (wp.phucan.vietnix.tech)

Tại bảng hiện ra giữ nguyên ở thẻ Create site và điền các nội dung như sau

    Tên miền Nhập chính xác địa chỉ wp.phucan.vietnix.tech vào ô Domain

    Cơ sở dữ liệu Ở phần Database nhấn chọn MySQL. Lúc này hệ thống tự sinh ra các dòng Tên, Người dùng và Mật khẩu.
    🚨 Lưu ý quan trọng Bạn cần sao chép ngay 3 thông tin này vào file Note cá nhân vì đây là thông tin bắt buộc phải dùng ở bước cài đặt WordPress

    Phiên bản PHP Nhấn chọn phiên bản PHP 7.4 hoặc PHP 8.1 tùy theo loại bạn đã cài trên máy chủ

## Thao tác 2: Tạo Website Laravel (laravel.phucan.vietnix.tech)

Tại bảng hiện ra bạn giữ nguyên ở thẻ Create site và nhập chính xác các thông số như sau

    Tên miền Nhập địa chỉ laravel.phucan.vietnix.tech

    Cài đặt SSL Bạn hãy BỎ TÍCH ô Apply for SSL vì hiện tại chưa trỏ IP thật cho tên miền này.

    Mô tả và Đường dẫn Bạn cứ để hệ thống tự động điền theo tên miền.

    Cơ sở dữ liệu Tại phần Database bạn nhấn chọn MySQL. Lúc này hệ thống tự sinh ra Tên, Người dùng và Mật khẩu.
    🚨 Lưu ý cực kỳ quan trọng Bạn hãy sao chép ngay các thông tin Database này đặc biệt là Mật khẩu dán vào file Note. Lát nữa khi tải mã nguồn Laravel lên bạn bắt buộc phải điền thông tin này vào file cấu hình .env thì web mới chạy được.

    Phiên bản PHP Nhấn chọn phiên bản PHP bạn đã cài đặt trên máy chủ thường là PHP 8.1 hoặc PHP 8.2.

