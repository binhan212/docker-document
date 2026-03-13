# Tài liệu học Docker từ cơ bản đến thực hành

> Phiên bản tài liệu: 13/03/2026  
> Mục tiêu: đọc một mạch, làm theo được luôn, không cần vừa học vừa "cầu nguyện máy đừng lỗi".

## Cách dùng bộ tài liệu trong workspace

Ngoài file tài liệu tổng này, trong workspace hiện đã có sẵn thêm 2 nhóm tài nguyên để bạn học liền mạch hơn:

### 1. Bộ bài học theo từng buổi

- [lessons/README.md](lessons/README.md)
- [lessons/buoi-01-tong-quan-va-cai-dat.md](lessons/buoi-01-tong-quan-va-cai-dat.md)
- [lessons/buoi-02-image-container-va-lenh-co-ban.md](lessons/buoi-02-image-container-va-lenh-co-ban.md)
- [lessons/buoi-03-dockerfile-va-build-image.md](lessons/buoi-03-dockerfile-va-build-image.md)
- [lessons/buoi-04-docker-compose-co-ban.md](lessons/buoi-04-docker-compose-co-ban.md)
- [lessons/buoi-05-thuc-hanh-redis-va-seq.md](lessons/buoi-05-thuc-hanh-redis-va-seq.md)
- [lessons/buoi-06-volumes-networks-env-va-debug.md](lessons/buoi-06-volumes-networks-env-va-debug.md)

Gợi ý cách học:

1. Đọc file này một lượt để có bức tranh tổng thể.
2. Học tiếp theo từng buổi trong thư mục `lessons`.
3. Sau mỗi buổi, chạy file tương ứng trong thư mục `practice`.

### 2. Bộ file thực hành mẫu

- [practice/nginx-static/Dockerfile](practice/nginx-static/Dockerfile)
- [practice/nginx-static/index.html](practice/nginx-static/index.html)
- [practice/nginx-static/.dockerignore](practice/nginx-static/.dockerignore)
- [practice/redis/docker-compose.yml](practice/redis/docker-compose.yml)
- [practice/seq/docker-compose.yml](practice/seq/docker-compose.yml)
- [practice/seq/.env.example](practice/seq/.env.example)

Bạn có thể dùng ngay các file trên để thực hành build image, chạy Redis bằng Compose, và chạy Seq cùng seq-input-gelf mà không phải tự gõ lại từ đầu.

## 1. Giới thiệu về Docker và containerization

### Container là gì? So sánh với máy ảo (VM)

Hiểu đơn giản:

- **Container** là một môi trường chạy ứng dụng gọn nhẹ, đóng gói sẵn code, thư viện và cấu hình cần thiết.
- **Máy ảo (VM)** là một máy tính giả lập gần như đầy đủ, có cả hệ điều hành riêng.

Ví dụ vui:

- **VM** giống như bạn thuê nguyên một căn hộ: có phòng khách, phòng ngủ, nhà bếp, nội thất đầy đủ.
- **Container** giống như bạn thuê một quầy bếp trong khu food court: chỉ lấy đúng phần cần để nấu món của mình.

So sánh nhanh:

| Tiêu chí | VM | Container |
|---|---|---|
| Mức độ nặng | Nặng | Nhẹ |
| Khởi động | Chậm hơn | Nhanh hơn |
| Tài nguyên | Tốn nhiều RAM/CPU | Tiết kiệm hơn |
| Cách ly | Mạnh | Tốt, nhưng phụ thuộc host OS |
| Hệ điều hành | Mỗi VM có OS riêng | Dùng chung kernel của host |

Kết luận ngắn:

- Nếu cần mô phỏng hẳn một máy độc lập: nghĩ đến **VM**.
- Nếu cần đóng gói và chạy ứng dụng nhanh, gọn, nhất quán: nghĩ đến **Docker container**.

### Lợi ích của Docker trong phát triển và triển khai phần mềm

Docker giúp giải quyết câu nói kinh điển:

> "Máy em chạy được mà anh."

Một số lợi ích lớn:

- **Chạy nhất quán**: máy dev, máy test, máy production giống nhau hơn.
- **Dễ chia sẻ môi trường**: chỉ cần image hoặc Dockerfile.
- **Triển khai nhanh**: kéo image về và chạy.
- **Dễ scale**: nhiều container có thể chạy cùng một ứng dụng.
- **Dễ cô lập dịch vụ**: app, database, redis, message queue tách riêng.

Ví dụ thực tế:

Một dự án web có thể gồm:

- 1 container cho ứng dụng Node.js
- 1 container cho PostgreSQL
- 1 container cho Redis
- 1 container cho Nginx

Thay vì cài từng thứ thủ công lên máy, bạn chỉ cần mô tả chúng bằng Docker.

### Khi nào nên dùng Docker?

Docker rất hợp khi:

- Muốn dựng môi trường phát triển nhanh.
- Muốn đồng bộ môi trường giữa team.
- Muốn đóng gói ứng dụng để deploy.
- Muốn chạy nhiều dịch vụ phụ trợ mà không làm máy host thành "nồi lẩu thập cẩm".

Docker không phải lúc nào cũng là lựa chọn tốt nhất nếu:

- Bạn đang học những khái niệm nền tảng của hệ điều hành và muốn tự cài mọi thứ bằng tay.
- Ứng dụng quá phụ thuộc GUI desktop.
- Bạn cần mô phỏng nhiều hệ điều hành tách biệt ở mức sâu như VM.

---

## 2. Cài đặt Docker

### Hướng dẫn cài Docker trên Windows, macOS, Linux

### Windows

Cách phổ biến nhất là cài **Docker Desktop**.

Yêu cầu thường gặp:

- Windows 10/11
- Bật virtualization trong BIOS nếu cần
- Hỗ trợ WSL 2

Các bước:

1. Tải Docker Desktop từ trang chủ Docker.
2. Cài đặt theo wizard.
3. Nếu được hỏi, bật **WSL 2 integration**.
4. Khởi động lại máy nếu cần.
5. Mở Docker Desktop và chờ trạng thái running.

Lưu ý:

- Docker trên Windows thường dùng **WSL 2 backend** để chạy ổn định hơn.
- Nếu máy báo lỗi liên quan đến virtualization hoặc WSL, xử lý hai phần này trước.

### macOS

Các bước:

1. Tải Docker Desktop cho macOS.
2. Chọn đúng bản theo chip Intel hoặc Apple Silicon.
3. Kéo ứng dụng vào Applications.
4. Mở Docker Desktop và cấp quyền nếu hệ thống yêu cầu.

### Linux

Trên Linux, thường sẽ cài Docker Engine.

Ví dụ Ubuntu:

```bash
sudo apt update
sudo apt install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Sau đó kiểm tra:

```bash
docker --version
```

### Kiểm tra cài đặt thành công với docker --version

Lệnh:

```bash
docker --version
```

Ví dụ kết quả:

```bash
Docker version 27.x.x, build abcdefg
```

Kiểm tra thêm:

```bash
docker info
```

Nếu chạy được, nghĩa là Docker daemon và CLI đang nói chuyện được với nhau.

### Bài thực hành nhanh

1. Cài Docker.
2. Chạy `docker --version`.
3. Chạy `docker info`.
4. Nếu lỗi, ghi lại đúng thông báo lỗi. Đừng sửa kiểu đoán mò.

---

## 3. Các khái niệm cốt lõi

### Image là gì?

**Image** là bản mẫu dùng để tạo container.

Bạn có thể hình dung:

- Image giống như **công thức nấu ăn**.
- Container là **món ăn đã nấu xong và đang nằm trên bàn**.

Ví dụ:

- `nginx:latest` là một image
- `redis:7` là một image
- `python:3.12-slim` là một image

Image thường chứa:

- Base OS tối giản
- Runtime cần thiết
- Thư viện
- Ứng dụng của bạn
- Lệnh khởi động mặc định

### Container là gì?

Container là một instance đang chạy từ image.

Ví dụ:

- Từ image `redis:7`, bạn có thể tạo 1 container tên `redis-dev`.
- Bạn cũng có thể tạo 3 container khác nhau từ cùng image đó.

Nói vui:

- Image là khuôn bánh.
- Container là cái bánh đã nướng xong.
- Nướng bao nhiêu cái tùy bạn, miễn máy còn chịu nổi.

### Registry là gì? Docker Hub là gì?

**Registry** là nơi lưu trữ image.

Phổ biến nhất là:

- **Docker Hub**

Ngoài ra còn có:

- GitHub Container Registry
- GitLab Container Registry
- Azure Container Registry
- Amazon ECR
- Google Artifact Registry

Luồng thường gặp:

1. Bạn build image cục bộ.
2. Push image lên registry.
3. Server khác pull image về chạy.

### Docker daemon và CLI

Docker có hai phần bạn cần hiểu:

- **Docker CLI**: công cụ dòng lệnh bạn gõ, ví dụ `docker ps`
- **Docker daemon**: tiến trình nền xử lý việc build, run, stop container

Khi bạn gõ:

```bash
docker ps
```

Thì CLI gửi yêu cầu tới daemon, daemon trả kết quả về.

Nếu daemon không chạy, CLI sẽ báo lỗi kiểu:

```bash
Cannot connect to the Docker daemon
```

---

## 4. Các lệnh Docker cơ bản và thường dùng

## 4.1. Quản lý image

### docker pull

Tải image từ registry về máy:

```bash
docker pull redis:7
```

Nếu không ghi tag, Docker thường dùng `latest`:

```bash
docker pull nginx
```

Nhưng trong thực tế, **nên ghi tag rõ ràng** để tránh hôm nay kéo được, mai kéo ra phiên bản khác rồi ngồi nhìn nhau như phim trinh thám.

### docker images

Xem danh sách image đã có:

```bash
docker images
```

### docker rmi

Xóa image:

```bash
docker rmi redis:7
```

Nếu image đang bị container sử dụng, Docker có thể không cho xóa.

---

## 4.2. Quản lý container

### docker run

Tạo và chạy container mới.

Ví dụ:

```bash
docker run redis:7
```

Chạy nginx ở chế độ nền và map cổng:

```bash
docker run -d -p 8080:80 --name my-nginx nginx:latest
```

Giải thích:

- `-d`: chạy nền
- `-p 8080:80`: map cổng máy host 8080 vào cổng 80 trong container
- `--name my-nginx`: đặt tên container

### docker ps

Xem container đang chạy:

```bash
docker ps
```

Xem cả container đã dừng:

```bash
docker ps -a
```

### docker stop

Dừng container:

```bash
docker stop my-nginx
```

### docker start

Chạy lại container đã dừng:

```bash
docker start my-nginx
```

### docker rm

Xóa container:

```bash
docker rm my-nginx
```

Nếu container đang chạy, phải dừng trước hoặc dùng force.

### docker logs

Xem log container:

```bash
docker logs my-nginx
```

Xem log realtime:

```bash
docker logs -f my-nginx
```

### docker exec

Chạy lệnh bên trong container đang chạy:

```bash
docker exec -it my-nginx sh
```

Nếu image có bash:

```bash
docker exec -it my-nginx bash
```

Rất hữu ích khi bạn muốn "chui vào bụng container" để xem chuyện gì đang diễn ra.

---

## 4.3. Các tùy chọn hay dùng

### -d

Detach mode, chạy nền.

```bash
docker run -d nginx
```

### -it

Kết hợp `-i` và `-t` để chạy tương tác với terminal:

```bash
docker run -it ubuntu bash
```

### -p

Map cổng:

```bash
docker run -p 3000:3000 my-app
```

Cú pháp:

```text
host_port:container_port
```

### -v

Mount volume hoặc bind mount:

```bash
docker run -v mydata:/data redis
```

Hoặc mount thư mục hiện tại:

```bash
docker run -v ${PWD}:/app node:20
```

### --name

Đặt tên container:

```bash
docker run --name demo-redis redis:7
```

Không đặt tên cũng được, nhưng Docker sẽ tự sinh tên kiểu rất nghệ sĩ như `happy_turing`, `sleepy_einstein`. Nhìn vui nhưng khi debug nhiều container thì cũng hơi đau tim.

---

## 4.4. Viết tắt lệnh

Một số lệnh có dạng mới và dạng viết tắt:

| Dạng đầy đủ | Dạng quen dùng |
|---|---|
| `docker container ls` | `docker ps` |
| `docker container rm` | `docker rm` |
| `docker image ls` | `docker images` |
| `docker image rm` | `docker rmi` |

Hiểu được cả hai dạng sẽ giúp bạn đọc tài liệu dễ hơn.

### Bài thực hành

1. Kéo image nginx.
2. Chạy container tên `web-demo` với cổng `8080:80`.
3. Mở trình duyệt vào `http://localhost:8080`.
4. Xem log.
5. Dừng và xóa container.

Lệnh mẫu:

```bash
docker pull nginx:latest
docker run -d -p 8080:80 --name web-demo nginx:latest
docker ps
docker logs web-demo
docker stop web-demo
docker rm web-demo
```

---

## 5. Dockerfile – cách viết và các lệnh quan trọng

### Cấu trúc cơ bản của Dockerfile

Ví dụ Dockerfile đơn giản cho ứng dụng Node.js:

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

Luồng đọc Dockerfile từ trên xuống dưới:

1. Chọn base image
2. Tạo thư mục làm việc
3. Copy file cần thiết
4. Cài dependency
5. Copy mã nguồn
6. Khai báo cổng
7. Chỉ định lệnh chạy mặc định

### Các lệnh chính trong Dockerfile

### FROM

Chọn image nền:

```dockerfile
FROM python:3.12-slim
```

Đây là câu đầu tiên rất quan trọng. Chọn base image giống như chọn nền nhà. Nền yếu thì vừa kê tủ đã lún.

### RUN

Thực thi lệnh lúc build image:

```dockerfile
RUN apt-get update && apt-get install -y curl
```

### CMD

Lệnh mặc định khi container khởi động:

```dockerfile
CMD ["python", "app.py"]
```

### ENTRYPOINT

Định nghĩa chương trình chính của container.

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Khi kết hợp như trên, container sẽ chạy tương đương:

```bash
python app.py
```

Khác biệt ngắn gọn:

- `CMD`: dễ bị override khi chạy container
- `ENTRYPOINT`: thường dùng để cố định lệnh chính

### COPY

Copy file từ máy build context vào image:

```dockerfile
COPY . .
```

### ADD

Giống `COPY` nhưng có thêm một số khả năng như giải nén file nén cục bộ hoặc tải từ URL trong một số trường hợp.

Tuy nhiên, **thực tế nên ưu tiên `COPY`** cho dễ đoán và rõ ràng.

### WORKDIR

Thiết lập thư mục làm việc:

```dockerfile
WORKDIR /app
```

### ENV

Thiết lập biến môi trường:

```dockerfile
ENV NODE_ENV=production
```

### EXPOSE

Khai báo cổng ứng dụng dùng bên trong container:

```dockerfile
EXPOSE 3000
```

Lưu ý: `EXPOSE` **không tự map cổng ra ngoài host**. Muốn truy cập từ host, bạn vẫn cần `-p` hoặc `ports` trong Compose.

### VOLUME

Khai báo điểm mount volume:

```dockerfile
VOLUME ["/data"]
```

### Best practices khi viết Dockerfile

### 1. Dùng image nhẹ khi phù hợp

Ví dụ:

- `node:20-alpine`
- `python:3.12-slim`

### 2. Tối ưu cache layer

Ví dụ tốt:

```dockerfile
COPY package*.json ./
RUN npm install
COPY . .
```

Lý do:

- Khi code thay đổi nhưng dependency chưa đổi, layer `npm install` có thể được cache.

### 3. Tránh copy file rác

Dùng `.dockerignore` để bỏ qua:

- `node_modules`
- `.git`
- `dist`
- file log

### 4. Không nhét bí mật vào image

Không hard-code:

- mật khẩu
- token
- secret key

### 5. Một container nên có một trách nhiệm chính

Ví dụ:

- app container chạy app
- db container chạy database

Đừng gom tất cả vào một container kiểu "tiện tay làm luôn". Tiện lúc đầu, mệt lúc sau.

---

## 6. Xây dựng image với docker build

### Cú pháp cơ bản

```bash
docker build -t my-app:1.0 .
```

Giải thích:

- `docker build`: build image
- `-t my-app:1.0`: đặt tên và tag
- `.`: build context là thư mục hiện tại

### Build từ Dockerfile khác tên

```bash
docker build -f Dockerfile.dev -t my-app:dev .
```

### .dockerignore – loại bỏ file không cần thiết

Ví dụ file `.dockerignore`:

```gitignore
node_modules
.git
.vscode
.env
npm-debug.log
dist
coverage
```

Lợi ích:

- Build nhanh hơn
- Context nhỏ hơn
- Tránh lộ file nhạy cảm
- Giảm nguy cơ image phình to như vali đi 2 ngày nhưng nhét đồ cho 3 tháng

### Bài thực hành

Tạo 3 file:

#### Dockerfile

```dockerfile
FROM nginx:latest
COPY . /usr/share/nginx/html
```

#### index.html

```html
<h1>Hello Docker</h1>
```

#### .dockerignore

```gitignore
.git
.vscode
```

Build image:

```bash
docker build -t hello-docker:1.0 .
```

Chạy:

```bash
docker run -d -p 8081:80 --name hello-site hello-docker:1.0
```

Mở trình duyệt:

```text
http://localhost:8081
```

---

## 7. Docker Compose – cấu trúc file và các thành phần

### Giới thiệu Docker Compose và file docker-compose.yml

Docker Compose giúp quản lý **nhiều container cùng lúc** bằng một file YAML.

Thay vì gõ 5 lệnh `docker run` dài như sớ Táo quân, bạn chỉ cần một file cấu hình.

Lưu ý hiện đại:

- Lệnh mới thường là `docker compose ...`
- Nhiều tài liệu cũ dùng `docker-compose ...`
- Về mặt ý tưởng, chúng phục vụ cùng mục đích

Ví dụ file Compose đơn giản:

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
```

Chạy:

```bash
docker compose up
```

### Các phiên bản (version) của compose file

Trong tài liệu cũ bạn sẽ hay thấy:

```yaml
version: "3.8"
```

Ví dụ đầy đủ hơn:

```yaml
version: "3.8"
services:
  web:
    image: nginx:latest
```

Tuy nhiên, với Docker Compose mới, trường `version` thường **không còn bắt buộc** nữa.

Khuyến nghị thực tế:

- Nếu team đang dùng file cũ có `version`, vẫn đọc hiểu bình thường.
- Nếu tạo file mới, có thể bỏ `version` nếu môi trường hiện tại hỗ trợ.

### Các thành phần chính: services, networks, volumes

### services

Khai báo các container/dịch vụ:

```yaml
services:
  app:
    image: my-app:latest
```

### networks

Khai báo mạng để container giao tiếp với nhau:

```yaml
networks:
  app-net:
```

### volumes

Khai báo nơi lưu dữ liệu bền vững:

```yaml
volumes:
  redis-data:
```

### Các thuộc tính thường dùng trong service

### image

Dùng image có sẵn:

```yaml
image: redis:7
```

### build

Build từ Dockerfile cục bộ:

```yaml
build:
  context: .
  dockerfile: Dockerfile
```

### ports

Map cổng:

```yaml
ports:
  - "3000:3000"
```

### volumes

Mount volume:

```yaml
volumes:
  - redis-data:/data
```

### environment

Khai báo biến môi trường:

```yaml
environment:
  NODE_ENV: production
```

Hoặc:

```yaml
environment:
  - NODE_ENV=production
```

### depends_on

Khai báo phụ thuộc khởi động:

```yaml
depends_on:
  - redis
```

Lưu ý: `depends_on` giúp về **thứ tự khởi động**, nhưng không đảm bảo service phụ thuộc đã "sẵn sàng phục vụ" hoàn toàn.

### restart

Chính sách tự khởi động lại:

```yaml
restart: unless-stopped
```

---

## 8. Phân tích chi tiết các file docker-compose mẫu

Do bạn chưa gửi file cụ thể, phần này dùng **2 file mẫu rất sát thực tế** để bạn học cấu trúc và cách đọc từng dòng.

## 8.1. File thứ nhất: Redis

### File mẫu

```yaml
version: "3.8"
services:
  redis:
    image: redis:7
    container_name: redis-local
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    restart: unless-stopped

volumes:
  redis-data:
```

### Giải thích từng dòng

```yaml
version: "3.8"
```

- Khai báo phiên bản schema Compose theo kiểu tài liệu cũ.
- Với Compose mới, dòng này có thể không bắt buộc.

```yaml
services:
```

- Bắt đầu phần định nghĩa các dịch vụ/container.

```yaml
  redis:
```

- Tên service là `redis`.
- Tên này cũng có thể được dùng để các container khác gọi qua network nội bộ.

```yaml
    image: redis:7
```

- Dùng image `redis` tag `7`.
- Việc ghi rõ tag tốt hơn `latest` để giảm bất ngờ.

```yaml
    container_name: redis-local
```

- Đặt tên container cố định là `redis-local`.
- Không bắt buộc, nhưng giúp dễ nhớ khi debug.

```yaml
    ports:
      - "6379:6379"
```

- Map cổng 6379 của máy host sang 6379 trong container.
- Redis client trên máy bạn có thể kết nối qua `localhost:6379`.

```yaml
    volumes:
      - redis-data:/data
```

- Mount named volume `redis-data` vào thư mục `/data` trong container.
- Dữ liệu Redis sẽ bền hơn nếu container bị xóa.

```yaml
    restart: unless-stopped
```

- Nếu Docker hoặc máy khởi động lại, container có thể được chạy lại.
- Trừ khi bạn chủ động stop nó.

```yaml
volumes:
  redis-data:
```

- Khai báo named volume `redis-data`.
- Docker sẽ tự quản lý volume này.

### Cách chạy file Redis

Giả sử file tên `docker-compose.yml`, chạy:

```bash
docker compose up -d
```

Kiểm tra:

```bash
docker compose ps
docker logs redis-local
```

Dừng:

```bash
docker compose down
```

Nếu muốn dừng và xóa luôn volume:

```bash
docker compose down -v
```

Cẩn thận: `-v` sẽ xóa dữ liệu trong volume nếu volume thuộc project đó.

---

## 8.2. File thứ hai: Seq và seq-input-gelf

### Bối cảnh

- **Seq** là công cụ thu thập và xem log.
- `seq-input-gelf` là input cho log định dạng GELF.

Nói dễ hiểu:

- Ứng dụng của bạn là người nói nhiều.
- Seq là thư ký chuyên ghi chép.
- `seq-input-gelf` là cái tai nghe đúng chuẩn để hiểu kiểu log GELF.

### File mẫu

```yaml
version: "3.8"
services:
  seq:
    image: datalust/seq:latest
    container_name: seq
    environment:
      ACCEPT_EULA: "Y"
    ports:
      - "8081:80"
      - "5341:5341"
    volumes:
      - seq-data:/data
    restart: unless-stopped

  seq-input-gelf:
    image: datalust/seq-input-gelf:latest
    container_name: seq-input-gelf
    environment:
      SEQ_ADDRESS: http://seq:80
    depends_on:
      - seq
    ports:
      - "12201:12201/udp"
    restart: unless-stopped

volumes:
  seq-data:
```

### Giải thích từng phần

#### Service `seq`

```yaml
  seq:
```

- Tên service là `seq`.

```yaml
    image: datalust/seq:latest
```

- Dùng image Seq chính thức từ Datalust.

```yaml
    container_name: seq
```

- Đặt tên container là `seq`.

```yaml
    environment:
      ACCEPT_EULA: "Y"
```

- Chấp nhận điều khoản sử dụng của Seq.
- Nhiều image yêu cầu biến môi trường để kích hoạt hoặc cấu hình.

```yaml
    ports:
      - "8081:80"
      - "5341:5341"
```

- Cổng `8081` trên host map tới `80` trong container để mở giao diện web Seq.
- Cổng `5341` thường dùng cho ingestion API của Seq.

```yaml
    volumes:
      - seq-data:/data
```

- Dữ liệu Seq được lưu ở named volume `seq-data`.
- Nếu không dùng volume, xóa container có thể mất dữ liệu.

```yaml
    restart: unless-stopped
```

- Tự chạy lại khi phù hợp.

#### Service `seq-input-gelf`

```yaml
  seq-input-gelf:
```

- Service nhận log theo định dạng GELF và chuyển vào Seq.

```yaml
    image: datalust/seq-input-gelf:latest
```

- Image cho GELF input.

```yaml
    container_name: seq-input-gelf
```

- Tên container dễ nhớ.

```yaml
    environment:
      SEQ_ADDRESS: http://seq:80
```

- Chỉ định địa chỉ Seq mà service này sẽ gửi dữ liệu tới.
- Ở đây dùng `http://seq:80` vì trong cùng Docker network, service `seq` có thể được gọi bằng tên `seq`.

```yaml
    depends_on:
      - seq
```

- Khởi động `seq` trước `seq-input-gelf`.
- Nhắc lại: không đồng nghĩa Seq đã chắc chắn sẵn sàng nhận log ngay lập tức.

```yaml
    ports:
      - "12201:12201/udp"
```

- Mở cổng UDP 12201 cho GELF input.
- Phần `/udp` rất quan trọng vì giao thức ở đây là UDP.

```yaml
    restart: unless-stopped
```

- Chính sách tự khởi động lại.

#### Khai báo volume

```yaml
volumes:
  seq-data:
```

- Khai báo volume để lưu dữ liệu Seq.

### Lưu ý quan trọng về version, biến môi trường, volume mapping

### Version

- File cũ hay có `version: "3.8"`.
- File mới có thể bỏ nếu Compose phiên bản mới hỗ trợ.
- Quan trọng là team thống nhất một cách dùng để đỡ loạn.

### Biến môi trường

- Dùng để cấu hình mềm dẻo.
- Không nên hard-code thông tin nhạy cảm trực tiếp trong file nếu đẩy lên Git.
- Có thể đưa vào file `.env`.

### Volume mapping

Có hai kiểu phổ biến:

#### Named volume

```yaml
volumes:
  - seq-data:/data
```

- Docker tự quản lý.
- Dễ dùng, gọn.

#### Bind mount

```yaml
volumes:
  - ./logs:/data/logs
```

- Ánh xạ thư mục thật trên máy host.
- Hữu ích khi cần xem file trực tiếp.

---

## 9. Chạy và quản lý ứng dụng với Docker Compose

### Các lệnh cơ bản

### docker compose up

Khởi động dịch vụ:

```bash
docker compose up
```

Chạy nền:

```bash
docker compose up -d
```

### docker compose down

Dừng và xóa container, network của project:

```bash
docker compose down
```

### docker compose ps

Xem trạng thái dịch vụ:

```bash
docker compose ps
```

### docker compose logs

Xem log:

```bash
docker compose logs
```

Theo dõi realtime:

```bash
docker compose logs -f
```

Chỉ xem một service:

```bash
docker compose logs -f seq
```

### Chạy ở chế độ nền (-d) và dừng

Khởi động nền:

```bash
docker compose up -d
```

Dừng và dọn:

```bash
docker compose down
```

### Xây dựng lại image khi có thay đổi

Nếu service dùng `build`, khi source code đổi bạn có thể:

```bash
docker compose build
```

Hoặc:

```bash
docker compose up --build
```

Khuyên dùng khi bạn thay đổi:

- Dockerfile
- dependency
- source code được copy vào image

### Quy trình thực hành đề xuất

1. Viết `docker-compose.yml`
2. Chạy `docker compose up -d`
3. Chạy `docker compose ps`
4. Xem log nếu có lỗi
5. Sửa file cấu hình hoặc code
6. Chạy `docker compose up --build -d`
7. Kiểm tra lại

---

## 10. Một số chủ đề nâng cao liên quan

## 10.1. Volumes – lưu trữ dữ liệu bền vững

Container có tính chất khá "ngắn hạn".

Nếu không lưu dữ liệu ra volume, khi container bị xóa thì dữ liệu có thể bay màu nhanh hơn lương mới về đã hết.

Ví dụ:

- Redis, PostgreSQL, MySQL, Seq đều nên gắn volume nếu cần giữ dữ liệu.

Các lệnh hữu ích:

```bash
docker volume ls
docker volume inspect redis-data
docker volume rm redis-data
```

Lưu ý: xóa volume là xóa dữ liệu. Hãy bình tĩnh trước khi enter.

## 10.2. Networks – giao tiếp giữa các container

Docker Compose thường tạo network riêng cho project.

Lợi ích:

- Service có thể gọi nhau bằng tên service.
- Ví dụ app gọi Redis qua hostname `redis` thay vì `localhost`.

Ví dụ:

- Trong container app, muốn kết nối Redis thì dùng `redis:6379`
- Không dùng `localhost:6379`, vì `localhost` trong container là chính nó, không phải máy host

Đây là lỗi rất phổ biến cho người mới.

### Ví dụ compose có app và redis

```yaml
services:
  app:
    build: .
    depends_on:
      - redis

  redis:
    image: redis:7
```

Trong app config:

```text
REDIS_HOST=redis
REDIS_PORT=6379
```

## 10.3. Biến môi trường và file .env

Bạn có thể dùng file `.env` để tách cấu hình ra khỏi file compose.

### Ví dụ file `.env`

```env
SEQ_PORT=8081
SEQ_INGEST_PORT=5341
```

### Dùng trong compose

```yaml
services:
  seq:
    image: datalust/seq:latest
    ports:
      - "${SEQ_PORT}:80"
      - "${SEQ_INGEST_PORT}:5341"
```

Lợi ích:

- Dễ đổi cấu hình theo môi trường
- File compose gọn hơn
- Giảm hard-code

Lưu ý:

- `.env` không phải nơi lý tưởng để lưu secret quan trọng nếu quy trình bảo mật của bạn chưa rõ ràng.
- Nếu dùng Git, cân nhắc thêm `.env` vào `.gitignore` khi chứa dữ liệu nhạy cảm.

---

## 11. Những lỗi thường gặp cho người mới học Docker

### 1. Container chạy rồi nhưng không truy cập được

Thường do:

- Chưa map port
- Map sai port
- App trong container không lắng nghe đúng cổng

Cách kiểm tra:

```bash
docker ps
docker logs <container_name>
```

### 2. Dùng localhost sai chỗ

- Trên máy host, `localhost` là máy bạn.
- Trong container, `localhost` là chính container đó.

### 3. Sửa code nhưng container không đổi

Có thể do:

- Chưa build lại image
- Chưa mount source code
- Docker cache layer cũ

### 4. Xóa container xong mất dữ liệu

Có thể do:

- Chưa dùng volume
- Dùng volume nhưng lại `down -v`

### 5. Depends_on không đảm bảo service sẵn sàng

Ví dụ database khởi động chậm hơn app. App chạy trước và báo lỗi kết nối.

Cách xử lý:

- Healthcheck
- Retry logic trong app
- Wait-for script trong một số trường hợp

---

## 12. Lộ trình thực hành liền mạch đề xuất

Nếu bạn muốn học từ dễ đến chắc, đi theo thứ tự này:

### Bước 1: Làm quen với image và container

Thực hành:

```bash
docker pull nginx:latest
docker run -d -p 8080:80 --name nginx-demo nginx:latest
docker ps
docker logs nginx-demo
docker stop nginx-demo
docker rm nginx-demo
```

Mục tiêu:

- Hiểu image
- Hiểu container
- Hiểu port mapping

### Bước 2: Chui vào container

```bash
docker run -dit --name ubuntu-demo ubuntu bash
docker exec -it ubuntu-demo bash
```

Mục tiêu:

- Hiểu container là môi trường chạy độc lập
- Hiểu `exec`

### Bước 3: Tự build image đầu tiên

Tạo một web tĩnh đơn giản với Nginx và `Dockerfile`.

Mục tiêu:

- Hiểu `FROM`, `COPY`, `CMD`, `EXPOSE`
- Hiểu `docker build`

### Bước 4: Dùng Docker Compose với Redis

Tạo file Compose Redis như ví dụ trên.

Mục tiêu:

- Hiểu `services`
- Hiểu `volumes`
- Hiểu `ports`

### Bước 5: Dùng Docker Compose với nhiều service

Chạy Seq và `seq-input-gelf`.

Mục tiêu:

- Hiểu giao tiếp nội bộ qua service name
- Hiểu biến môi trường
- Hiểu `depends_on`

### Bước 6: Áp dụng vào dự án thật

Ví dụ:

- app + redis
- app + database
- app + seq

Lúc này Docker mới thật sự phát huy giá trị.

---

## 13. Cheat sheet ngắn gọn

### Image

```bash
docker pull nginx:latest
docker images
docker rmi nginx:latest
```

### Container

```bash
docker run -d -p 8080:80 --name web nginx:latest
docker ps
docker ps -a
docker stop web
docker start web
docker rm web
docker logs -f web
docker exec -it web sh
```

### Build

```bash
docker build -t my-app:1.0 .
```

### Compose

```bash
docker compose up -d
docker compose down
docker compose ps
docker compose logs -f
docker compose up --build -d
```

---

## 14. Kết luận

Sau khi học xong tài liệu này, bạn nên nắm được:

- Docker giải quyết vấn đề gì
- Phân biệt image, container, registry
- Dùng được các lệnh cơ bản
- Viết được Dockerfile đơn giản
- Build được image
- Đọc và viết được file Docker Compose cơ bản
- Chạy được Redis, Seq và các service liên quan
- Hiểu volume, network, environment variable ở mức thực hành

Nếu nhớ một câu duy nhất, hãy nhớ câu này:

> Docker không làm ứng dụng của bạn tự hết bug. Nó chỉ giúp bug chạy rất nhất quán ở mọi nơi.

Đó vừa là tin tốt, vừa là tin hơi đau lòng.

---

## 15. Bài tập tự luyện

### Bài 1

Chạy Nginx bằng `docker run`, map cổng `8080:80`, sau đó xem log và xóa container.

### Bài 2

Viết `Dockerfile` cho một web tĩnh đơn giản và build thành image riêng.

### Bài 3

Viết file Compose để chạy Redis có volume lưu dữ liệu.

### Bài 4

Viết file Compose cho ứng dụng giả lập gồm:

- 1 service app
- 1 service redis
- app phụ thuộc redis
- app đọc biến môi trường `REDIS_HOST=redis`

### Bài 5

Tạo file `.env` để cấu hình cổng cho Seq rồi dùng lại trong Compose.

---

## 16. Đáp án mẫu tham khảo

Phần này là đáp án mẫu để đối chiếu. Không nhất thiết phải giống từng ký tự, miễn đúng ý và chạy được.

### Đáp án Bài 1

Chạy Nginx bằng `docker run`, map cổng `8080:80`, xem log và xóa container:

```bash
docker pull nginx:latest
docker run -d -p 8080:80 --name web-demo nginx:latest
docker ps
docker logs web-demo
docker stop web-demo
docker rm web-demo
```

### Đáp án Bài 2

Bạn có thể dùng luôn bộ mẫu ở [practice/nginx-static/Dockerfile](practice/nginx-static/Dockerfile).

Dockerfile mẫu:

```dockerfile
FROM nginx:latest
COPY . /usr/share/nginx/html
```

Build và chạy:

```bash
docker build -t hello-docker:1.0 .
docker run -d -p 8081:80 --name hello-site hello-docker:1.0
```

### Đáp án Bài 3

Bạn có thể dùng luôn file ở [practice/redis/docker-compose.yml](practice/redis/docker-compose.yml).

Ví dụ:

```yaml
services:
  redis:
    image: redis:7
    container_name: redis-local
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    restart: unless-stopped

volumes:
  redis-data:
```

Chạy:

```bash
docker compose up -d
docker compose ps
docker compose down
```

### Đáp án Bài 4

Ví dụ file Compose cho app và Redis:

```yaml
services:
  app:
    image: nginx:latest
    container_name: app-demo
    environment:
      REDIS_HOST: redis
    ports:
      - "8080:80"
    depends_on:
      - redis

  redis:
    image: redis:7
    container_name: redis-demo
    ports:
      - "6379:6379"
```

Nếu app của bạn là Node.js, Python hoặc .NET thì service `app` có thể đổi sang `build: .` hoặc image riêng của bạn. Ý chính của bài là:

- app đọc biến môi trường `REDIS_HOST=redis`
- app gọi Redis bằng hostname `redis`
- app phụ thuộc Redis ở mức thứ tự khởi động

### Đáp án Bài 5

Bạn có thể tham khảo file ở [practice/seq/docker-compose.yml](practice/seq/docker-compose.yml) và [practice/seq/.env.example](practice/seq/.env.example).

File `.env` mẫu:

```env
SEQ_PORT=8081
SEQ_INGEST_PORT=5341
SEQ_GELF_PORT=12201
```

File Compose mẫu:

```yaml
services:
  seq:
    image: datalust/seq:latest
    environment:
      ACCEPT_EULA: "Y"
    ports:
      - "${SEQ_PORT}:80"
      - "${SEQ_INGEST_PORT}:5341"
    volumes:
      - seq-data:/data

  seq-input-gelf:
    image: datalust/seq-input-gelf:latest
    environment:
      SEQ_ADDRESS: http://seq:80
    depends_on:
      - seq
    ports:
      - "${SEQ_GELF_PORT}:12201/udp"

volumes:
  seq-data:
```

---

## 17. Gợi ý học tiếp sau tài liệu này

Sau khi xong phần cơ bản, bạn có thể học tiếp:

1. Multi-stage build
2. Healthcheck
3. Docker network nâng cao
4. Docker volumes nâng cao
5. Tối ưu image cho production
6. Docker registry riêng
7. Kubernetes sau khi đã vững Docker

Nếu học Docker mà chưa vững phần image, container, port, volume, network thì đừng vội nhảy sang Kubernetes. Không thì cảm giác sẽ giống đang học đi xe đạp mà bị mời lái trực thăng.
