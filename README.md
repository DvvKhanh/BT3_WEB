# Đậu Văn Khánh - K225480106099
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

<img width="1584" height="893" alt="Screenshot 2025-11-02 232825" src="https://github.com/user-attachments/assets/8e6516df-3d41-4cd1-aed9-140763b51fb4" />

- Chuyển sang tab Resources -> Chọn WSL Integration:
  + Tick ✅ “Enable integration with my default WSL distro”
  + Bật Ubuntu hoặc Ubuntu-22.04

<img width="1576" height="892" alt="Screenshot 2025-11-02 233059" src="https://github.com/user-attachments/assets/2bb7f2aa-9bb3-4b36-ad0c-8089b1212269" />

- Nhấn Apply & Restart
-> Sau khi restart, Docker Desktop sẽ tự động kết nối với Ubuntu qua WSL2.
 
- Kiểm tra Ubuntu:
  + Mở cmd (quyền Admin) và nhập: wsl --list --verbose
  + Kết quả:

<img width="1096" height="629" alt="image" src="https://github.com/user-attachments/assets/1b7ff9ab-1c66-4281-8eeb-768515eb31b0" />

- Kiểm tra Docker trong Ubuntu
  + Mở Ubuntu (WSL) mà bạn vừa bật integration → gõ: docker version hoặc docker run hello-world

 <img width="1472" height="755" alt="Screenshot 2025-11-02 234312" src="https://github.com/user-attachments/assets/cbe210be-d14e-45a8-90f3-5685a2efbfe3" />

## 2. Cài đặt Docker (nếu dùng docker desktop trên windows thì nó có ngay) (Đã làm chi tiết ở ý 1)
- Truy cập link: https://www.docker.com/ -> nhấn Download
- Sau khi tải về sẽ hiển thị file:<img width="752" height="40" alt="Screenshot 2025-11-02 231058" src="https://github.com/user-attachments/assets/86c138e1-45d5-43f6-b8d6-0287dae9db19" />
- Nhấp đúp vào tệp cài đặt Docker Desktop -> chọn Yes khi hộp thoại cấp quyền quản trị xuất hiện.
- Sau khi Docker Desktop khởi động, chọn Accept để đồng ý với các điều khoản sử dụng.
- Tiếp theo, đăng nhập bằng tài khoản Google/ GitHub của bạn hoặc đăng ký tài khoản mới nếu chưa có.
- Kết quả khi cài đặt xong Docker Destop

<img width="875" height="599" alt="Screenshot 2025-11-02 231639" src="https://github.com/user-attachments/assets/197e0d09-44d5-4608-9b90-83b2b03ee975" />

## 3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: mariadb (3306), phpmyadmin (8080), nodered/node-red (1880), influxdb (8086), grafana/grafana (3000), nginx (80,443)
### 3.1. Mục tiêu và yêu cầu
- Mục đích của phần này là triển khai nhanh hệ thống IoT web server gồm 6 container hoạt động đồng thời bằng Docker Compose, bao gồm:

| Tên dịch vụ    | Cổng host | Mục đích                                       |
| -------------- | --------- | ---------------------------------------------- |
| **MariaDB**    | 3306      | Cơ sở dữ liệu chính                            |
| **phpMyAdmin** | 8080      | Giao diện web quản lý MariaDB                  |
| **Node-RED**   | 1880      | Nền tảng IoT Flow                              |
| **InfluxDB**   | 8086      | Cơ sở dữ liệu time-series lưu dữ liệu cảm biến |
| **Grafana**    | 3000      | Giao diện phân tích, hiển thị dữ liệu InfluxDB |
| **Nginx**      | 80, 443   | Máy chủ web front-end (reverse proxy)          |

- Tất cả được cài đặt thông qua một file docker-compose.yml duy nhất, giúp dễ dàng quản lý và triển khai.

### 3.2. Cấu trúc thư mục dự án
- Tạo cấu trúc thư mục như sau:
<img width="264" height="283" alt="image" src="https://github.com/user-attachments/assets/af597003-2ea5-41f4-85e2-619ce53aea77" />
💡 Các thư mục *_data được mount vào container để lưu dữ liệu bền vững (persistent data).

### 3.3. Các bước thực hiện chi tiết
#### Bước 1: Tạo thư mục dự án và di chuyển vào
```
mkdir ~/iot_docker
cd ~/iot_docker
```
#### Bước 2: Tạo file docker-compose.yml
- Mở Ubuntu
- Nhập lệnh: nano docker-compose.yml để tạo file
- Viết code:
```
  services:
  mariadb:
    image: mariadb:10.11
    container_name: mariadb
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: iotdb
      MYSQL_USER: iotuser
      MYSQL_PASSWORD: iotpass
    ports:
      - "3306:3306"
    volumes:
      - ./mariadb_data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 5

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: unless-stopped
    environment:
      PMA_HOST: mariadb
      PMA_USER: root
      PMA_PASSWORD: root123
    ports:
      - "8080:80"
    depends_on:
      mariadb:
        condition: service_healthy

  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    restart: unless-stopped
    ports:
      - "1880:1880"
    volumes:
      - ./nodered_data:/data
    healthcheck:
      test: ["CMD-SHELL", "node -v || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 3

  influxdb:
    image: influxdb:2.7
    container_name: influxdb
    restart: unless-stopped
    ports:
      - "8086:8086"
    environment:
      DOCKER_INFLUXDB_INIT_MODE: setup
      DOCKER_INFLUXDB_INIT_USERNAME: admin
      DOCKER_INFLUXDB_INIT_PASSWORD: admin123
      DOCKER_INFLUXDB_INIT_ORG: iot_org
      DOCKER_INFLUXDB_INIT_BUCKET: iot_bucket
      DOCKER_INFLUXDB_INIT_ADMIN_TOKEN: my-token
    volumes:
      - ./influxdb_data:/var/lib/influxdb2
    healthcheck:
      test: ["CMD-SHELL", "curl -sfS http://localhost:8086/health || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 5

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: unless-stopped
    ports:
      - "3000:3000"
    depends_on:
      - influxdb
    volumes:
      - ./grafana_data:/var/lib/grafana
    environment:
      GF_SECURITY_ADMIN_USER: admin
      GF_SECURITY_ADMIN_PASSWORD: admin123
      GF_USERS_ALLOW_SIGN_UP: "false"
    healthcheck:
      test: ["CMD-SHELL", "grafana-cli -v || exit 1"]
      interval: 15s
      timeout: 5s
      retries: 3

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: unless-stopped
    ports:
      - "8088:80"    # nếu 80 bị chiếm, dùng 8088 trên host
      - "4443:443"   # https host port 4443
    volumes:
      - ./nginx_conf:/etc/nginx/conf.d:ro
      - ./html:/usr/share/nginx/html:ro
    depends_on:
      - grafana
      - nodered
      - phpmyadmin

networks:
  default:
    driver: bridge
```
-> Nhấn Ctrl + O -> Enter để lưu lại, Ctrl + X để thoát ra

#### Bước 3: Tạo các thư mục dữ liệu: mkdir -p mariadb_data nodered_data influxdb_data grafana_data nginx

#### Bước 4: Tạo file cấu hình nginx.conf
- Chạy lệnh: nano nginx/nginx.conf để tạo file
- Chạy code:
```
events { }

http {
    server {
        listen 80;
        location / {
            root /usr/share/nginx/html;
            index index.html;
        }

        location /grafana/ {
            proxy_pass http://grafana:3000/;
        }

        location /nodered/ {
            proxy_pass http://nodered:1880/;
        }
    }
}
```
-> Nhấn Ctrl + O -> Enter để lưu lại, Ctrl + X để thoát ra

#### Bước 5: Khởi động tất cả container
- Chạy lệnh: docker compose up -d để khởi động


#### Bước 6: Kiểm tra trạng thái
- Chạy lệnh: docker ps để kiểm tra trạng thái

![Uploading image.png…]()

### 5. Kiểm tra kết quả
- Node-RED: http://localhost:1880
- Grafana:	http://localhost:3000
- phpMyAdmin:	http://localhost:8080
- InfluxDB:	http://localhost:8086
- Nginx:	http://localhost
