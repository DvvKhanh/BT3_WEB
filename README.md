<img width="1584" height="894" alt="image" src="https://github.com/user-attachments/assets/03fa1b39-8d19-4f61-b290-f901193cd4d2" /># Đậu Văn Khánh - K225480106099
# YÊU CẦU: LẬP TRÌNH ỨNG DỤNG WEB trên nền linux
## 1. Cài đặt môi trường linux: SV chọn 1 trong các phương án
 - enable wsl: cài đặt docker desktop
 - enable wsl: cài đặt ubuntu
 - sử dụng Hyper-V: cài đặt ubuntu
 - sử dụng VMware : cài đặt ubuntu
 - sử dụng Virtual Box: cài đặt ubuntu
## 2. Cài đặt Docker (nếu dùng docker desktop trên windows thì nó có ngay)
## 3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: 
   mariadb (3306), phpmyadmin (8080), nodered/node-red (1880), influxdb (8086), grafana/grafana (3000), nginx (80,443)
## 4. Lập trình web frontend+backend:
 SV chọn 1 trong các web sau:
 4.1 Web thương mại điện tử
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - Có tính năng liệt kê các sản phẩm bán chạy ra trang chủ
 - Có tính năng liệt kê các nhóm sản phẩm
 - Có tính năng liệt kê sản phẩm theo nhóm
 - Có tính năng tìm kiếm sản phẩm
 - Có tính năng chọn sản phẩm (đưa sản phẩm vào giỏ hàng, thay đổi số lượng sản phẩm trong giỏ, cập nhật tổng tiền)
 - Có tính năng đặt hàng, nhập thông tin giao hàng => được 1 đơn hàng.
 - Có tính năng dành cho admin: Thống kê xem có bao nhiêu đơn hàng, call để xác nhận và cập nhật thông tin đơn hàng. chuyển cho bộ phận đóng gói, gửi bưu điện, cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...
 - Có tính năng dành cho admin: biểu đồ thống kê số lượng mặt hàng bán được trong từng ngày. (sử dụng grafana)
 - backend: sử dụng nodered xử lý request gửi lên từ javascript, phản hồi về json.
 4.2 Web IOT: Giám sát dữ liệu IOT.
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - hiển thị giá trị mới nhất của các thông số đang giám sát, khi click vào thì hiển thị đồ thị lịch sử quá trình thay đổi (gọi grafana iframe để hiển thị)
 - backend: Sử dụng nodered để đọc dữ liệu từ các cảm biến (có thể dùng api online để lấy dữ liệu theo giời gian thực), 
   nodered sẽ lưu dữ liệu mới nhất (dạng update) vào cơ sở dữ liệu mariadb (sử dụng phpmyadmin để tạp table và quản trị lần đầu)
   nodered sẽ lưu dữ liệu (insert) vào influxdb để lưu giá trị lịch sử, để cho grafana dùng để hiển thị biểu đồ.
## 5. Nginx làm web-server
 - Cấu hình nginx để chạy được website qua url http://fullname.com  (thay fullname bằng chuỗi ko dấu viết liền tên của bạn)
 - Cấu hình nginx để http://fullname.com/nodered truy cập vào nodered qua cổng 80, (dù nodered đang chạy ở port 1880)
 - Cấu hình nginx để http://fullname.com/grafana truy cập vào grafana qua cổng 80, (dù grafana đang chạy ở port 3000)

# BÀI LÀM
## 1. Cài đặt môi trường linux
### Bước 1: Cài đặt Ubuntu:
- Mở cmd (quyền Admin) và gõ: wsl --install để cài Ubuntu
- Sau khi cài xong gõ: wsl -d Ubuntu để mở
- Sau đó nhập lần lượt: Enter new UNIX username và New password
- Sau khi nhập xong username và password sẽ hiển thị: khanh@DESKTOP-7I4R9SM:/mnt/c/Windows/System32$

<img width="1098" height="430" alt="Screenshot 2025-11-02 223419" src="https://github.com/user-attachments/assets/d6cd50be-ba50-4767-bca9-be42567144c9" />

- Sau đó chạy lệnh: sudo apt update và sudo apt upgrade -y

### Bước 2: Cài đặt Docker Destop
- Truy cập link: https://www.docker.com/ -> nhấn Download
- Sau khi tải về sẽ hiển thị file:<img width="752" height="40" alt="Screenshot 2025-11-02 231058" src="https://github.com/user-attachments/assets/86c138e1-45d5-43f6-b8d6-0287dae9db19" />
- Nhấp đúp vào tệp cài đặt Docker Desktop -> chọn Yes khi hộp thoại cấp quyền quản trị xuất hiện.
- Sau khi Docker Desktop khởi động, chọn Accept để đồng ý với các điều khoản sử dụng.
- Tiếp theo, đăng nhập bằng tài khoản Google/ GitHub của bạn hoặc đăng ký tài khoản mới nếu chưa có.
- Kết quả khi cài đặt xong Docker Destop

<img width="875" height="599" alt="Screenshot 2025-11-02 231639" src="https://github.com/user-attachments/assets/197e0d09-44d5-4608-9b90-83b2b03ee975" />

### Bước 3: Bật tích hợp Docker với Ubuntu (trong Docker Desktop)
- Mở Docker Desktop
- Vào Settings -> Chọn General: Tick ✅ “Use the WSL 2 based engine”
<img width="1095" height="636" alt="image" src="https://github.com/user-attachments/assets/e7befc1d-68e9-41bb-a9e5-2074ce0f5049" />

- Chuyển sang tab Resources -> Chọn WSL Integration:
  + Tick ✅ “Enable integration with my default WSL distro”
  + Tick ✅ dòng Ubuntu hoặc Ubuntu-22.04

 <img width="1584" height="894" alt="image" src="https://github.com/user-attachments/assets/cc97860c-6ac7-4dd6-9d59-ac481a3f9fa8" />

- Nhấn Apply & Restart
-> Sau khi restart, Docker Desktop sẽ tự động kết nối với Ubuntu qua WSL2.
 
- Kiểm tra Ubuntu:
  + Mở cmd (quyền Admin) và nhập: wsl --list --verbose
  + Kết quả:

![Uploading Screenshot 2025-11-02 234006.png…]()

- Kiểm tra Docker trong Ubuntu
  + Mở Ubuntu (WSL) mà bạn vừa bật integration → gõ: docker version hoặc docker run hello-world

 ![Uploading Screenshot 2025-11-02 234312.png…]()

