# Docker compose

## Docker Compose là gì?

![image](https://github.com/thangdtph27626/DockerCompose/assets/109157942/2eef9eba-4d91-4d38-b94a-2e11812beacd)

Docker Compose là một công cụ hỗ trợ xác định và chạy các ứng dụng multi-container . Docker Compose có thể xử lý đồng thời multi-container trong sản xuất, staging, phát triển, thử nghiệm và CI.

Docker Compose hoạt động bằng cách áp dụng các quy tắc được xác định trong tệp docker-compose.yaml.

## Các lệnh cơ bản trong Docker Compose

- docker-compose up: Khởi động các container
- docker-compose down: Dừng và xóa các container
- docker-compose ps: Hiển thị trạng thái của các container
- docker-compose build: Tạo image từ Dockerfile trong mỗi dịch vụ
- docker-compose restart: Khởi động lại các container
- docker-compose stop: Dừng các container
- docker-compose rm: Xóa các container không sử dụng
- docker-compose logs: Hiển thị các logs của các container
- docker-compose config: Hiển thị các cấu hình của Docker Compose
- docker-compose exec: Thực thi một lệnh trên một container
- docker-compose port: Hiển thị các port của các container
- docker-compose top: Hiển thị các process đang chạy trong các container

> Lưu ý: Các lệnh trên phải được thực hiện trong thư mục chứa file docker-compose.yml.

## Cài đặt Docker Compose

![image](https://github.com/thangdtph27626/DockerCompose/assets/109157942/316ee71b-1720-493a-80d2-6e7067ccae05)

Theo từng hệ điều hành sẽ có bước cài đặt Docker Compose khác nhau. Bạn nên tải phiên bản mới nhất của Docker Compose từ trang web chính thức của Docker Compose.

Cài đặt Docker Compose trên macOS
Để sử dụng Docker Compose trên macOS, bạn chỉ cần cài đặt Docker Desktop cho Mac và không cần cài đặt riêng Docker Compose.

Cài đặt Docker Compose trên Windows
Để sử dụng Docker Compose trên Windows, bạn chỉ cần cài đặt Docker Desktop cho Windows và không cần cài đặt riêng Docker Compose.

## Sử dụng Docker Compose

Để sử dụng Docker Compose, bạn cần thực hiện các bước sau:

### Bước 1: Cài đặt Docker Compose
Bạn cần cài đặt Docker Compose trên máy tính của mình. Bạn có thể tải từ trang chủ của Docker hoặc có thể cài đặt thông qua các gói phần mềm của hệ điều hành.

Tiếp đến, bạn sẽ tạo Dockerfile để xác định environment. Mỗi dịch vụ sẽ chạy trên một container riêng và sử dụng image tương ứng.

### Bước 2: Tạo file docker-compose.yml
Bạn cần tạo một file docker-compose.yml để định nghĩa các container và cấu hình của chúng. Ví dụ, tệp sẽ bao gồm các nội dung sau:

```
version: '3'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./web:/usr/share/nginx/html
    networks:
      - webnet
    depends_on:
      - db
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: example
      POSTGRES_PASSWORD: example
      POSTGRES_DB: example
    volumes:
      - dbdata:/var/lib/postgresql/data
    networks:
      - webnet

networks:
  webnet:

volumes:
  dbdata:

```

Ý nghĩa của các giá trị trong file docker-compose:

- version: phiên bản của Docker Compose file. Ở đây, chúng ta sử dụng phiên bản 3.
- services: là khu vực khai báo các services cần thiết cho ứng dụng.
- web: dịch vụ web, sử dụng image nginx, chia sẻ volume và network với dịch vụ db. Cổng 80 được định nghĩa để web có thể truy cập được từ bên ngoài.
- db: dịch vụ db, sử dụng image postgresql, chia sẻ volume và network với dịch vụ web. Các biến môi trường cài đặt cho dịch vụ postgresql được định nghĩa ở phần environment.
- networks: danh sách các networks được sử dụng cho container.
- webnet: mạng webnet để chia sẻ giữa dịch vụ web và db.
- volumes: là option nên config, volumes cho phép mount data từ container ra máy local. Khi config option này thì mỗi lần stop container data của container đó sẽ không bị mất đi.
- dbdata: là container chứa thông tin về database.
Trong ví dụ này, Docker Compose sẽ khởi tạo 2 container, một container sử dụng image nginx và một container sử dụng image postgresql. Container sử dụng image nginx sẽ được kết nối với container sử dụng image postgresql thông qua mạng webnet. Sau đó, chúng sẽ chia sẻ volume dbdata để lưu trữ dữ liệu của postgresql.

### Bước 3: Chạy lệnh docker-compose up
Sử dụng lệnh docker-compose up để khởi động các container được định nghĩa trong file docker-compose.yml. Nếu các image không được tải xuống trước đó, Docker Compose sẽ tự động tải chúng xuống và khởi động các container.

Quản lý các container
Bạn có thể quản lý các container bằng các lệnh Docker Compose như docker compose stop để dừng các container, docker compose start khởi động sau khi dừng các container, docker compose restart để khởi động lại các container đã dừng, docker compose -f docker.yaml down để xóa các container đã dừng, docker-compose ps để hiển thị trạng thái của các container, ...

Tùy chỉnh cấu hình
Nếu bạn muốn thay đổi cấu hình của các container, bạn chỉ cần chỉnh sửa file docker-compose.yml và chạy lại lệnh docker-compose up.

Để dừng các dịch vụ đang hoạt động một cách an toàn, chúng ta có thể sử dụng lệnh dừng để bảo toàn các thùng chứa, khối lượng và mạng cùng với mọi sửa đổi được thực hiện đối với chúng:

> docker-compose stop
