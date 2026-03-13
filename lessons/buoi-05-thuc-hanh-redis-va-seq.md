# Buổi 5 - Thực hành Redis và Seq

## Mục tiêu

Sau buổi này, bạn cần:

- Đọc được file Compose Redis.
- Đọc được file Compose Seq và seq-input-gelf.
- Hiểu port mapping, named volume, biến môi trường và `depends_on` trong ví dụ thật.

## 1. Redis compose mẫu

File: `practice/redis/docker-compose.yml`

Điểm cần chú ý:

- `image: redis:7`
- `ports: "6379:6379"`
- `volumes: redis-data:/data`
- `restart: unless-stopped`

Mục tiêu thực hành:

```bash
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
```

## 2. Seq compose mẫu

File: `practice/seq/docker-compose.yml`

Dịch vụ gồm:

- `seq`
- `seq-input-gelf`

Điểm cần chú ý:

- `ACCEPT_EULA=Y`
- `SEQ_ADDRESS=http://seq:80`
- `12201:12201/udp`
- volume `seq-data:/data`

## 3. Vì sao `SEQ_ADDRESS` dùng `seq` thay vì `localhost`

Vì trong cùng Compose project, các service nói chuyện với nhau qua tên service.

- Đúng: `http://seq:80`
- Sai trong trường hợp này: `http://localhost:80`

## 4. Thực hành bắt buộc

### Redis

- Chạy stack Redis.
- Kiểm tra container và log.
- Dừng stack.

### Seq

- Chạy stack Seq.
- Truy cập giao diện tại `http://localhost:8081`.
- Kiểm tra service GELF đã mở cổng UDP chưa.

## 5. Bài tập mini

- Đổi cổng giao diện Seq từ `8081` sang `8082`.
- Chuyển file `practice/seq/.env.example` thành `.env` rồi dùng biến môi trường trong Compose.
