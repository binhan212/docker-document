# Buổi 4 - Docker Compose cơ bản

## Mục tiêu

Sau buổi này, bạn cần:

- Hiểu vì sao cần Docker Compose.
- Biết các thành phần `services`, `networks`, `volumes`.
- Dùng được `docker compose up`, `down`, `ps`, `logs`, `up --build`.

## 1. Vì sao cần Compose?

Nếu ứng dụng có nhiều dịch vụ như app, database, redis, logging thì gõ từng lệnh `docker run` sẽ rất dài và dễ sai.

Compose cho phép mô tả toàn bộ bằng một file YAML.

## 2. Cấu trúc cơ bản

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
```

## 3. Các thành phần chính

### services

Nơi định nghĩa các container.

### networks

Nơi định nghĩa mạng cho container giao tiếp.

### volumes

Nơi định nghĩa dữ liệu bền vững.

## 4. Các thuộc tính thường dùng trong service

- `image`
- `build`
- `ports`
- `volumes`
- `environment`
- `depends_on`
- `restart`

## 5. Các lệnh quan trọng

```bash
docker compose up
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
docker compose up --build -d
```

## 6. Điểm cần nhớ

- Compose mới thường dùng lệnh `docker compose`.
- Nhiều tài liệu cũ vẫn dùng `docker-compose`.
- `depends_on` không đảm bảo service đã sẵn sàng hoàn toàn.

## 7. Thực hành nhanh

- Mở file `practice/redis/docker-compose.yml`.
- Chạy `docker compose up -d` trong thư mục đó.
- Dùng `docker compose ps` để kiểm tra.
- Dùng `docker compose down` để dừng.

## 8. Bài tập mini

- Thêm `container_name` vào một service.
- Thêm `restart: unless-stopped`.
- Đổi cổng Redis sang cổng khác ở phía host.
