# 1. Docker là gì?
+ Docker là một nền tảng mã nguồn mở cho phép ảo hóa ở cấp độ hệ điều hành (Containerization). Nó giúp các nhà phát triển đóng gói ứng dụng cùng với tất cả các thư viện, môi trường phụ thuộc (dependencies) cần thiết vào trong một đơn vị duy nhất gọi là Container.
+ Khác với Máy ảo (Virtual Machine) phải mang theo cả một hệ điều hành khách (Guest OS) nặng nề, Docker Container chia sẻ chung nhân (Kernel) của hệ điều hành máy chủ, giúp nó hoạt động cực kỳ nhẹ, khởi động trong vài giây và tiêu tốn rất ít tài nguyên.
# 2. Các keyword phổ biến trong docker-compose.yml
+ docker-compose.yml là file cấu hình định dạng YAML dùng để định nghĩa và vận hành hệ thống gồm nhiều container cùng lúc.
  + A. Nhóm mô tả Dịch vụ (Services)
    + version: Định nghĩa phiên bản cấu hình của Docker Compose sử dụng (Ví dụ: '3.8').
    + services: Nhóm gốc chứa tất cả các container cấu thành ứng dụng.
    + image: Chỉ định Docker Image được tải về từ Docker Hub để dựng container.
    + container_name: Đặt tên tường minh cho container thay vì để Docker tự sinh tên ngẫu nhiên.
    + ports: Ánh xạ cổng (Port Forwarding) từ máy host vào bên trong container theo cú pháp [Cổng_Máy_Host]:[Cổng_Container].
    + environment: Thiết lập các biến môi trường cấu hình cho ứng dụng (User, Password, Config,...).
    + depends_on: Chỉ định thứ tự khởi động giữa các dịch vụ (Ví dụ: App chỉ khởi động sau khi Database đã chạy).
    + restart: Chính sách tự động khởi động lại container nếu bị lỗi hoặc máy chủ reboot (Ví dụ: always, unless-stopped).
  + B. Nhóm mô tả Mạng (Networks) và Ổ đĩa (Volumes)
    + networks: Định nghĩa mạng nội bộ cô lập để các container có thể giao tiếp với nhau bằng tên dịch vụ (Service Name) mà không cần mở port ra ngoài máy host.
    + volumes: Gắn một thư mục từ máy host hoặc ổ đĩa ảo của Docker vào trong container nhằm giữ lại dữ liệu (Data Persistence), không bị mất dữ liệu khi container bị xóa/nâng cấp.
# 3. Ưu điểm khi triển khai ứng dụng sử dụng Docker
+ Giải quyết triệt để lỗi môi trường: Đảm bảo ứng dụng chạy đồng nhất 100% từ laptop cá nhân của lập trình viên đến máy chủ thử nghiệm và máy chủ production thật ("Run anywhere").
+ Tối ưu hóa tài nguyên phần cứng: Chạy hàng chục container trên cùng một máy chủ mà không bị quá tải như máy ảo VM do chia sẻ chung tài nguyên OS.
+ Triển khai và mở rộng cực nhanh: Khởi động/Hạ dịch vụ chỉ tính bằng giây. Dễ dàng nâng cấp hoặc scale-up bằng lệnh đơn giản.
+ Kiến trúc Microservices cô lập: Các dịch vụ chạy hoàn toàn độc lập. Lỗi của container này (ví dụ Node-RED) không làm sập container khác (ví dụ MariaDB).
# 4. Quy trình triển khai App lên máy chủ thật KHÔNG CÓ INTERNET (Môi trường Air-Gapped)
+ Khi máy chủ mục tiêu bị ngắt Internet hoàn toàn, bạn cần áp dụng quy trình đóng gói offline theo 5 bước sau:
  + Bước 1 (Tại máy cá nhân có Internet): Build và chạy thử ứng dụng thành công bằng Docker Compose.
  + Bước 2 (Đóng gói Image thành file nén): Sử dụng lệnh docker save để xuất các Image cần thiết ra file .tar.
    + docker save -o my_app_images.tar node-red:latest mariadb:10.11 influxdb:2.7 
  + Bước 3 (Di chuyển file vật lý): Copy file my_app_images.tar và file docker-compose.yml vào ổ cứng di động/USB hoặc đẩy qua mạng LAN nội bộ vào máy chủ thật.
  + Bước 4 (Tải Image vào máy chủ thật): Trên máy chủ thật, chạy lệnh docker load để giải nén nạp lại các Image vào bộ nhớ Docker của máy chủ:
    + docker load -i my_app_images.tar
  + Bước 5 (Khởi chạy dịch vụ): Chạy lệnh docker compose up -d ngay tại thư mục chứa file cấu hình. Hệ thống tự lên mà không cần tải bất kỳ byte dữ liệu nào từ Internet


## Thực hành: 
# tắt môi trường cũ và khởi tạo môi trường cho bài 5
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/11ffcfdb-30f7-49d8-bf5a-09f9952130b6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0b0a5578-8a98-4a6c-a15b-4f5770aaf146" />
# Tạo file cấu hình Routing cho Nginx
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f0cc7c20-bfad-43ce-ac94-13520c2bd7b6" />
# Tạo giao diện Web Front-end lấy số realtime
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c73e108a-0749-4b8a-973f-6abafed974ee" />
# Kích hoạt hệ thống và chuẩn bị làm phần Logic

