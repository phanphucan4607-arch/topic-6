**Luồng thực hiện (Workflow)**

Quy trình bắt đầu bằng việc cài đặt aaPanel để chuyển đổi từ quản lý dòng lệnh sang quản trị qua giao diện web, thiết lập môi trường LNMP (Nginx, MySQL, PHP) làm nền tảng cho website. Sau đó, tiến hành khai báo hai tên miền và cài đặt WordPress, sử dụng All-in-One WP Migration để xuất toàn bộ dữ liệu (bao gồm cả mã nguồn và cơ sở dữ liệu đã được mã hóa chuỗi) từ máy chủ cũ và nhập vào máy chủ mới nhằm đảm bảo tính đồng bộ. Khi website đã hoạt động, bước tiếp theo là cài đặt các công cụ chức năng như Rank Math SEO để tối ưu tìm kiếm, Elementor/Divi để thiết kế giao diện kéo thả, và cuối cùng là tối ưu hóa hiệu suất bằng cách phối hợp giữa WP-Optimize (dọn dẹp định kỳ cơ sở dữ liệu) và LiteSpeed Cache (lưu trữ bản sao tĩnh ở tầng máy chủ) để tăng tốc độ tải trang tối đa.

**Các kiến thức trọng tâm cần nắm**

   + Quản trị Control Panel (aaPanel): Cách triển khai môi trường LNMP tự động, quản lý Virtual Host và cơ sở dữ liệu trên giao diện đồ họa.

   + Cơ chế Migration (Di cư dữ liệu): Hiểu cách All-in-One WP Migration đóng gói và tự động thay đổi các đường dẫn (URL) trong Database từ domain cũ sang domain mới thông qua kỹ thuật Serialize/Deserialize dữ liệu.

   + Hệ sinh thái WordPress chuyên nghiệp: Vai trò của các Page Builder (Elementor/Divi) trong thiết kế UX/UI và công cụ SEO (Rank Math) trong việc tối ưu cấu trúc dữ liệu cho bộ máy tìm kiếm.

   + Phân tầng Caching (Bộ nhớ đệm): Phân biệt rõ sự khác biệt giữa tối ưu hóa ở tầng ứng dụng (WP-Optimize: dọn rác, nén ảnh) và tối ưu hóa ở tầng máy chủ (LiteSpeed Cache: phản hồi HTML tĩnh nhanh mà không cần thực thi PHP/SQL).

   + Điều kiện triển khai LiteSpeed Cache: Chỉ hoạt động hiệu quả nhất khi phần cứng máy chủ chạy phần mềm Web Server là LiteSpeed (như hệ thống của Vietnix), giúp tận dụng sức mạnh xử lý trực tiếp từ tầng hệ thống thay vì thông qua mã nguồn PHP thông thường.
