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
- Giao diện Docker Destop:

<img width="1593" height="905" alt="Screenshot 2025-11-03 203055" src="https://github.com/user-attachments/assets/bf185195-9764-472a-b7d1-f2ca56b802b8" />

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
version: "3.8"

services:
  mariadb:
    image: mariadb:10.11
    container_name: mariadb
    restart: unless-stopped
    environment:
      # SỬ DỤNG GIÁ TRỊ CỐ ĐỊNH
      MYSQL_ROOT_PASSWORD: 123456
      MYSQL_DATABASE: web
      MYSQL_USER: khanh
      MYSQL_PASSWORD: 123456
    ports:
      - "3306:3306"
    volumes:
      - ./mariadb_data:/var/lib/mysql
    healthcheck:
      # SỬ DỤNG GIÁ TRỊ CỐ ĐỊNH TRONG LỆNH PING
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-u", "root", "-p123456"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - iot_network

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: unless-stopped
    environment:
      PMA_HOST: mariadb
      PMA_PORT: 3306
      PMA_USER: root
      # SỬ DỤNG GIÁ TRỊ CỐ ĐỊNH
      PMA_PASSWORD: 123456
      PMA_ARBITRARY: 1
    ports:
      - "8080:80"
    depends_on:
      mariadb:
        condition: service_healthy
    networks:
      - iot_network

  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    restart: unless-stopped
    ports:
      - "1880:1880"
    volumes:
      - ./nodered_data:/data
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:1880/"]
      interval: 30s
      timeout: 10s
      retries: 3
    networks:
      - iot_network

  influxdb:
    image: influxdb:2.7
    container_name: influxdb
    restart: unless-stopped
    ports:
      - "8086:8086"
    environment:
      DOCKER_INFLUXDB_INIT_MODE: setup
      DOCKER_INFLUXDB_INIT_USERNAME: admin
      # SỬ DỤNG GIÁ TRỊ CỐ ĐỊNH
      DOCKER_INFLUXDB_INIT_PASSWORD: admin123
      DOCKER_INFLUXDB_INIT_ORG: iot_org
      DOCKER_INFLUXDB_INIT_BUCKET: iot_bucket
      # SỬ DỤNG GIÁ TRỊ CỐ ĐỊNH
      DOCKER_INFLUXDB_INIT_ADMIN_TOKEN: my-token
    volumes:
      - ./influxdb_data:/var/lib/influxdb2
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8086/health"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - iot_network

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
      # SỬ DỤNG GIÁ TRỊ CỐ ĐỊNH
      GF_SECURITY_ADMIN_PASSWORD: admin123
      GF_USERS_ALLOW_SIGN_UP: "false"
    healthcheck:
      test: ["CMD", "wget", "-q", "-O", "/dev/null", "http://localhost:3000/api/health"]
      interval: 15s
      timeout: 5s
      retries: 3
    networks:
      - iot_network

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: unless-stopped
    ports:
      - "8088:80"
      - "4443:443"
    volumes:
      # LƯU Ý: Nginx mặc định dùng conf.d/default.conf, tôi sẽ dùng tên file này.
      # Đảm bảo file cấu hình của bạn là ./nginx/nginx.conf
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro 
      - ./nginx/html:/usr/share/nginx/html:ro
    depends_on:
      - grafana
      - nodered
      - phpmyadmin
    networks:
      - iot_network

networks:
  iot_network:
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
  
<img width="1475" height="293" alt="Screenshot 2025-11-03 223359" src="https://github.com/user-attachments/assets/f62ea1f7-96cb-4d18-8f10-dc42865ef429" />

#### Bước 6: Kiểm tra trạng thái
- Chạy lệnh: docker ps để kiểm tra trạng thái

<img width="1487" height="292" alt="Screenshot 2025-11-03 230457" src="https://github.com/user-attachments/assets/7fbebc5b-6084-449c-8b10-23f1cf372093" />

### 3.4. Kiểm tra kết quả
- Node-RED: http://localhost:1880
- Grafana:	http://localhost:3000
- phpMyAdmin:	http://localhost:8080
- InfluxDB:	http://localhost:8086

<img width="1581" height="894" alt="Screenshot 2025-11-06 065403" src="https://github.com/user-attachments/assets/b9038579-3b5f-41f5-96e8-db8653c15481" />

## 4. Lập trình web frontend+backend:
### 4.1. Tạo CSDL gồm DB: web, các bảng: Categories, Orders, Orders_Items, Products, Users
  + Mở phpMyAdmin http://localhost:8080
  
<img width="1772" height="620" alt="Screenshot 2025-11-06 110613" src="https://github.com/user-attachments/assets/17dc2ca3-c6ff-420c-a9f7-ff2d53d615c4" />

  + Bảng Categories:

<img width="1063" height="816" alt="image" src="https://github.com/user-attachments/assets/c9d94c26-58e9-441a-b3af-3164d9ffb9fc" />

  + Bảng Orders:

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/2a8e6208-ae04-460c-829c-dd4ce643a9eb" />

  + Bảng Orders_Items:

<img width="1534" height="1019" alt="image" src="https://github.com/user-attachments/assets/fe1ea4cf-4796-45c1-b059-35fc848e16d0" />

  + Bảng Products:

<img width="1738" height="1066" alt="image" src="https://github.com/user-attachments/assets/d330968c-9ce9-4350-a0f5-ecb512ca9dae" />

  + Bảng Users:

<img width="1504" height="639" alt="image" src="https://github.com/user-attachments/assets/0977b6cd-9808-41c9-a54d-351c5a530840" />

### 4.2. Tạo Node-RED backend — import flow (REST API)
1. Mở Node-RED http://localhost:1880. Vào Menu → Manage palette install để cài 1 số thư viện:

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/07157958-d5e9-47d6-8e27-2dd59db9426b" />

2. Tạo nodered login:

<img width="1249" height="533" alt="image" src="https://github.com/user-attachments/assets/d06b2763-fb57-4244-a4b9-263047bc35a9" />

3. Tạo nodered liệt kê sản phẩm, các sản phẩm bán chạy nhất, các nhóm sản phẩm

<img width="1426" height="757" alt="image" src="https://github.com/user-attachments/assets/45bf18f3-fd46-4221-96a5-74685a6696d5" />

+ Kiểm tra API:
   + Liệt kê sản phẩm:
   <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ea62321b-5ec7-46bd-ab6f-b6b4eb41053c" />

   + Các sản phẩm bán chạy nhất:
  
   <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/53bfa559-6613-4cfd-8d57-7bae86d80094" />

   + Các nhóm sản phẩm:

   <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/bb7e2df6-2419-4afe-8205-e5e1609fcfc2" />

4. Tạo nodered tìm kiếm sản phẩm:

<img width="1307" height="375" alt="image" src="https://github.com/user-attachments/assets/e7fe33ce-bcaa-4ab6-9812-086ddf3f162e" />

+ Kiểm tra API:

<img width="690" height="1097" alt="image" src="https://github.com/user-attachments/assets/c187f396-f70f-443b-95af-fcd1df2d910a" />

5. Tạo nodered đặt hàng:

<img width="1411" height="483" alt="image" src="https://github.com/user-attachments/assets/0c81e445-46a6-44c3-a4a2-554129fa4b7c" />

6. Tạo nodered xem đơn hàng:

<img width="1211" height="806" alt="image" src="https://github.com/user-attachments/assets/5a19cf44-9b5d-4114-8591-f073d13ed468" />

+ Kiểm tra API:

<img width="690" height="1097" alt="Screenshot 2025-11-06 223419" src="https://github.com/user-attachments/assets/2c2a5e97-a5f6-40aa-b07d-66e84f805fd7" />

### 4.3. Code html
+ code index.html (Giao diện đăng nhập vào hệ thống):

<img width="1919" height="850" alt="image" src="https://github.com/user-attachments/assets/b3e7b434-98c1-4e31-83a7-d0206b42872d" />

+ code dashboard.html (Giao diện web thương mại điện tử sau khi đăng nhập):

<img width="1919" height="855" alt="image" src="https://github.com/user-attachments/assets/192d3253-73ca-4d79-a951-69f0942ebdab" />

### 4.4. Giao diện web
+ Giao diện đăng nhập

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e6bdff16-d422-429c-ab11-67d46e2931bb" />

+ Giao diện web của Admin

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/58c8f99a-a403-4e30-9cfb-3cdd34e77b89" />

+ Giao diện web của Customer

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9d6d5ce7-bcfd-4966-a421-9f4e2bdffcf0" />

+ Tính năng liệt kê các sản phẩm bán chạy ra trang chủ

<img width="1897" height="1053" alt="image" src="https://github.com/user-attachments/assets/237ead2f-f22b-4c38-b42f-7765c31136e0" />

+ Tính năng liệt kê các nhóm sản phẩm

<img width="696" height="755" alt="image" src="https://github.com/user-attachments/assets/3abf712b-5302-446f-a08b-08227385b89a" />

+ Tính năng liệt kê sản phẩm theo nhóm

<img width="1919" height="873" alt="image" src="https://github.com/user-attachments/assets/0f93a1d9-50a0-4bde-990e-49f46b20e959" />

+ Tính năng tìm kiếm sản phẩm

<img width="1919" height="1052" alt="image" src="https://github.com/user-attachments/assets/709dec3d-9bd4-4c19-9575-d103a87c8a58" />

+ Tính năng chọn sản phẩm (đưa sản phẩm vào giỏ hàng, thay đổi số lượng sản phẩm trong giỏ, cập nhật tổng tiền)
  + Thêm sản phẩm vào giỏ hàng
    
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/0b02982e-dbd7-4657-adab-d8652cbe7e8e" /></p>

  <img width="1893" height="1099" alt="Screenshot 2025-11-06 224131" src="https://github.com/user-attachments/assets/0b0f6c73-24af-481e-a512-942038b3a4f7" />

  + Thay đổi số lượng sản phẩ trong giỏ hàng và cập nhật tổng tiền
 
  <img width="1895" height="1046" alt="image" src="https://github.com/user-attachments/assets/195b2ba5-1756-4e2e-a4a2-188d8e51c01e" />

+ Tính năng đặt hàng, nhập thông tin giao hàng

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/587acf76-f1de-44a0-bf2c-6853d97f5e90" />

-> Sau khi đặt hàng, dữ liệu sẽ tự động hiển thị trong sql

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d4ebfc71-fc9c-420d-8c4c-5d507f3a67a0" />

+ Tính năng dành cho admin: Thống kê xem có bao nhiêu đơn hàng, call để xác nhận và cập nhật thông tin đơn hàng. chuyển cho bộ phận đóng gói, gửi bưu điện, cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...

<img width="1881" height="1049" alt="image" src="https://github.com/user-attachments/assets/414a97d6-00de-4455-8381-a11a97bb63f8" />

+ Tính năng dành cho admin: biểu đồ thống kê số lượng mặt hàng bán được trong từng ngày


## 5. Nginx làm web-server
