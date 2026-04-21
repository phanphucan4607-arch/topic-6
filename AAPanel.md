<img width="851" height="551" alt="image" src="https://github.com/user-attachments/assets/889eb2fa-5162-4d5b-baed-c01a39eb0563" />**Luồng thực hiện (Workflow)**

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

## Thao tác 1 Tạo Website WordPress (wp.phucan.vietnix.tech)
**Bước 1: Tạo Website & Database trên aaPanel**
+ vào website -> add site.

+ nhập domain: wp phucan.vietnix.tech

+ Mục **Database**: chọn MSQL --> đặt tên: sql_wp_phhucan

+ Mục **PHP Version**: chọn 8.1 hoặc 3.3

+  Nhấn Submit

**Bước 2: Xử lý bộ code (Source Code)**
+ Vào Files -> Truy cập thư mục web vừa tạo.

+ Xóa sạch các file mặc định mà aaPanel tự tạo (như index.html, 404.html).

+ Upload file source code (.zip) của bạn lên và Extract (Giải nén).

+ Đảm bảo các file nằm ngay thư mục gốc (vừa mở thư mục web là thấy wp-admin, wp-content liền).

**Bước 3: Cấu hình "đầu não" wp-config.php**


Mở file wp-config.php cấu hình tên biến 

```
<?php
// 1. Nhận diện HTTPS từ Nginx
if (isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https') {
    $_SERVER['HTTPS'] = 'on';
}

// ** Database settings ** //
define( 'DB_NAME', 'sql_wp_phucan' );
define( 'DB_USER', 'sql_wp_phucan' );
define( 'DB_PASSWORD', 'mAL55pnBNsCTskXW' );
define( 'DB_HOST', 'localhost' );
define( 'DB_CHARSET', 'utf8' );
define( 'DB_COLLATE', '' );

// ** Secret Keys ** //
define('AUTH_KEY',         ']6uS9Z56Y_yl_3:x986a_|%TTAFQ8#B&8GE+UgJKmHQ|a-c%r|o5oj5wuvpPNHv!');
define('SECURE_AUTH_KEY',  'fms|Kl3002+&%Z#)q!DX@-Ro*(I]hZHLw(&&+ztLJqb#X(hK5I_Da03IQ7-3W0Ta');
define('LOGGED_IN_KEY',    's5)O)24E]R~z83DRh]9DSJuhU|YJIB~-zBqH(r114kO/:bcT-f0;s54(aVgG%&X[');
define('NONCE_KEY',        '+G*aVr/55Ek+xc4D(!B71d~z&6dUGZ(6qP4l)A7K9lc6n8101l1!+u7V29[E(ILd');
define('AUTH_SALT',        '[)_C%6L9;IM3Q71Q!24VqH1[gPU~__[Pw15W19oLsA/&W-S7l%y|ewS)SGwhPWfq');
define('SECURE_AUTH_SALT', '55:xB9i049U/1_Q+G-2A;[/;6|[f91#%g6O8Gr&a#4l16e&oG9A5OO/GT~;Em@/p');
define('LOGGED_IN_SALT',   'b:u&k~wIUAl1Gz000x#Y7k5U_PB&P20i!*p)Vz:O+0j*R##8#ac-!Ps|z57J8ND8');
define('NONCE_SALT',       'on%eawi9n4_7Fl3E@vrdt|9-w4sK7m!u/p6sh(1:FKl_|14h%3l;:kC!+DKA;c6#');

$table_prefix = 'Sa3QIZ_';

// ** Debugging ** //
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', false );
define( 'WP_DEBUG_DISPLAY', false );
@ini_set( 'display_errors', 0 );

// ** Tên miền chuẩn ** //
define( 'WP_HOME', 'https://wp.phucan.vietnix.tech' );
define( 'WP_SITEURL', 'https://wp.phucan.vietnix.tech' );

/* That's all, stop editing! Happy publishing. */

if ( ! defined( 'ABSPATH' ) ) {
    define( 'ABSPATH', __DIR__ . '/' );
}
require_once ABSPATH . 'wp-settings.php';
```
**Bước 4: Phân quyền File (Permission)**

+ Tại thư mục web, tích chọn tất cả file/thư mục.

+ Bấm Permission.

+ Thiết lập: 755 và Owner là www. Tích chọn Apply to subdirectories.

**Bước 5: URL Rewrite (Để vào được các trang con)**

Vào Website -> Click vào tên miền -> URL Rewrite.

Chọn mẫu wordpress từ danh sách thả xuống.

Nhấn Save.

<img width="1013" height="278" alt="image" src="https://github.com/user-attachments/assets/a006b3aa-0658-458b-902e-012e6df7f92b" />

+ Tắt Anti-XSS attack

<img width="851" height="551" alt="image" src="https://github.com/user-attachments/assets/c9453255-9933-44c0-b750-3c41669ffeae" />

**Kiểm tra truy cập**

https://wp.phucan.vietnix.tech/
    
<img width="1810" height="1008" alt="image" src="https://github.com/user-attachments/assets/65717a51-8b14-4d26-8c3f-93794551e725" />

   
## Thao tác 2: Tạo Website Laravel (laravel.phucan.vietnix.tech)
Làm tuong tự như wp lưu ý lưu lại thong tin su khi tạo xong

Database user profile

User: sql_laravel_phucan_vietnix_tech

Password: f01b8f2a88da

<img width="1865" height="1002" alt="Screenshot from 2026-04-21 02-34-55" src="https://github.com/user-attachments/assets/40bbda9e-76a7-4fe8-a789-90899873c3ee" />

## Cấu hình Document Root và URL Rewrite cho Laravel
Bước 1 Truy cập thư mục Laravel

Tại menu cột trái bạn chọn mục Files. Tiếp theo bạn tìm và nhấn mở thư mục laravel.phucan.vietnix.tech để vào bên trong.
Bước 2 Dọn dẹp thư mục

Bạn hãy chọn và xóa bỏ hai file index.html và file 404.html mặc định đang có sẵn trong thư mục này để tránh bị xung đột khi chạy website.
Bước 3 Tải lên mã nguồn

Nhấn vào nút Upload ở thanh công cụ phía trên. Sau đó bạn chọn file mã nguồn Laravel có định dạng .zip từ máy tính của bạn để bắt đầu tải lên máy chủ.
Bước 4 Giải nén mã nguồn

Sau khi quá trình tải lên hoàn tất bạn nhấn chuột phải vào file .zip vừa tải xong. Cuối cùng bạn chọn lệnh Unzip để giải nén toàn bộ mã nguồn website ra thư mục hiện tại.

và sau khi giải nén xong chúng ta coppy ra thư mục cha y như wp

<img width="1865" height="1002" alt="image" src="https://github.com/user-attachments/assets/ab5df8da-6961-445e-8dc5-7f0c6ee24eac" />

## cấu hình Document Root và URL Rewrite 
Bước 1 Mở bảng cấu hình website

Tại menu cột trái bạn quay lại mục Website. Sau đó bạn nhấn thẳng vào tên miền laravel.phucan.vietnix.tech hoặc nhấn chữ Conf ở cột thao tác bên phải để mở bảng cài đặt.
Bước 2 Cấu hình đường dẫn chạy web

Tại menu bên trái của bảng cấu hình vừa hiện ra bạn chọn mục Site directory.
Bước 3 Chọn thư mục chạy công khai

Ở dòng Running directory bạn nhấn nút xổ xuống. Lúc này danh sách sẽ hiện ra mục /public. Bạn hãy nhấn chọn đúng mục /public này.
Bước 4 Lưu cài đặt

Sau khi đã chọn xong bạn nhấn nút Save để hệ thống xác nhận đường dẫn chạy cho mã nguồn Laravel.

<img width="1865" height="1002" alt="image" src="https://github.com/user-attachments/assets/9537b665-8436-4336-8487-59e1b66bc1b4" />

<img width="1865" height="1002" alt="image" src="https://github.com/user-attachments/assets/f6296b4d-d972-44a0-86cc-8b2b4b0591c5" />

##  Nối Database và Sửa file hosts

Bước 1 Tìm và mở file cấu hình

Tại giao diện aaPanel bạn vào menu Files. Sau đó bạn mở thư mục laravel.phucan.vietnix.tech. Bạn tìm file có tên là .env (file này có dấu chấm ở đầu tên). Hãy nhấp đúp vào file đó để mở cửa sổ soạn thảo.
Bước 2 Nhập thông tin Database

Trong cửa sổ soạn thảo vừa hiện ra bạn cuộn xuống tìm đoạn có chữ DB_DATABASE và các dòng liên quan. Bạn tiến hành xóa thông tin cũ và điền thông tin của bạn vào 3 dòng như sau

    DB_DATABASE sql_laravel_phucan_vietnix_tech

    DB_USERNAME sql_laravel_phucan_vietnix_tech

    DB_PASSWORD (Dán mật khẩu Database Laravel bạn đã lưu vào đây)

🚨 Lưu ý quan trọng Khi dán bạn phải cẩn thận không để dính khoảng trắng ở đầu hay cuối của mật khẩu.
Bước 3 Lưu và đóng file

Sau khi đã điền xong bạn nhìn lên góc trên cùng bên trái của cửa sổ soạn thảo nhấn vào biểu tượng đĩa mềm hoặc nhấn phím tắt Ctrl S để lưu lại. Cuối cùng bạn đóng cửa sổ soạn thảo đó đi là hoàn thành.

<img width="1865" height="1002" alt="image" src="https://github.com/user-attachments/assets/f74827c3-c392-428e-8101-4d08321c205e" />

## sửa lại file hosts lần nữa thêm ip của laravel vào 

<img width="1168" height="803" alt="image" src="https://github.com/user-attachments/assets/9c37c686-9600-476e-bcd3-6298bce71119" />
Bước 1: Truy cập trực tiếp vào Database (phpMyAdmin)
🔑 Thông tin đăng nhập phpMyAdmin

    Username: sql_laravel_phucan_vietnix_tech

    Password: a04d91dd20d468

Tại màn hình bạn đang mở ở hình image_65e0c1.jpg:

    Nhấn vào nút Password-free access màu xám. Nó sẽ mở ra một tab mới là giao diện phpMyAdmin.

    Ở cột bên trái, bạn nhấn vào tên database: sql_laravel_phucan_vietnix_tech.

    Kiểm tra: Nếu nó hiện "No tables found", nghĩa là database đang rỗng tuếch.

Bước 2: Import thủ công (Cách này chắc chắn nhất)

Nếu database trống, bạn làm ngay trong tab phpMyAdmin đó:

    Nhấn vào menu Import ở thanh công cụ phía trên.

    Nhấn Choose File và chọn file laravel_db_vps.sql từ máy tính của bạn.

    Kéo xuống dưới cùng nhấn nút Go (hoặc Import).

    Quan trọng: Sau khi chạy xong, hãy nhìn cột bên trái xem có hiện ra danh sách các bảng (tables) như users, products, sliders... chưa.

<img width="1837" height="1002" alt="image" src="https://github.com/user-attachments/assets/1234e9c8-4d9d-42be-9e80-16910bb98d2f" />


Kiểm tra đường dẫn

Bạn gõ địa chỉ sau vào thanh địa chỉ của trình duyệt

http://laravel.phucan.vietnix.tech

<img width="1837" height="1002" alt="image" src="https://github.com/user-attachments/assets/75e718a7-6f2f-43ae-947d-f2edb159d030" />



tiến hành import và giai nén 

<img width="1174" height="475" alt="image" src="https://github.com/user-attachments/assets/03f17b76-9a12-4add-824f-3a01c50d951e" />

<img width="1174" height="475" alt="image" src="https://github.com/user-attachments/assets/db78ad3f-ce1e-4c86-bf66-1d46b821a821" />


