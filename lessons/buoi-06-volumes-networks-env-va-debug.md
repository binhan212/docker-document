# Buổi 6 - Volumes, networks, biến môi trường và debug

## Mục tiêu

Sau buổi này, bạn cần:

- Hiểu vì sao volume quan trọng.
- Hiểu container giao tiếp qua network thế nào.
- Dùng `.env` để cấu hình linh hoạt.
- Biết cách debug lỗi Docker cơ bản.

## 1. Volumes

Nếu không dùng volume, dữ liệu trong container có thể mất khi container bị xóa.

Lệnh hữu ích:

```bash
docker volume ls
docker volume inspect redis-data
```

## 2. Networks

Trong Compose, các service thường nằm cùng một network mặc định.

Ví dụ app muốn gọi Redis thì dùng:

```text
redis:6379
```

Không dùng `localhost:6379` trong container app.

## 3. Biến môi trường và .env

Ví dụ `.env`:

```env
SEQ_PORT=8081
SEQ_INGEST_PORT=5341
```

Ví dụ Compose:

```yaml
ports:
  - "${SEQ_PORT}:80"
  - "${SEQ_INGEST_PORT}:5341"
```

## 4. Các lỗi rất hay gặp

- Quên map port.
- Dùng nhầm `localhost`.
- Thay code nhưng chưa build lại image.
- Xóa container xong mất dữ liệu vì chưa dùng volume.
- Tin rằng `depends_on` sẽ lo hết mọi chuyện.

## 5. Quy trình debug đề xuất

1. Xem `docker ps` hoặc `docker compose ps`.
2. Xem log bằng `docker logs` hoặc `docker compose logs -f`.
3. Kiểm tra port mapping.
4. Kiểm tra biến môi trường.
5. Nếu cần, `docker exec -it` vào container để soi bên trong.

## 6. Bài tập mini

- Tự mô tả một lỗi bạn từng gặp và cách dùng log để tìm ra nguyên nhân.
- Đổi giá trị cổng trong file `.env` của Seq rồi chạy lại Compose để xác nhận cấu hình mới đã được áp dụng.
- Giải thích vì sao volume làm dữ liệu bền hơn container.
