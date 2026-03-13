# Buổi 2 - Image, container và lệnh Docker cơ bản

## Mục tiêu

Sau buổi này, bạn cần làm được:

- Phân biệt image và container.
- Dùng được các lệnh `docker pull`, `docker images`, `docker run`, `docker ps`, `docker logs`, `docker exec`.
- Hiểu các tùy chọn `-d`, `-it`, `-p`, `-v`, `--name`.

## 1. Image và container

- Image là bản mẫu.
- Container là thực thể chạy từ image.

Ví dụ:

- Image giống công thức nấu ăn.
- Container giống món ăn đang bày trên bàn.

## 2. Registry là gì?

Registry là nơi lưu image, phổ biến nhất là Docker Hub.

Ví dụ kéo image Redis:

```bash
docker pull redis:7
```

## 3. Các lệnh quan trọng

### Quản lý image

```bash
docker pull nginx:latest
docker images
docker rmi nginx:latest
```

### Quản lý container

```bash
docker run -d -p 8080:80 --name web-demo nginx:latest
docker ps
docker ps -a
docker logs web-demo
docker stop web-demo
docker start web-demo
docker rm web-demo
```

### Chui vào container

```bash
docker exec -it web-demo sh
```

## 4. Các tùy chọn rất hay gặp

- `-d`: chạy nền.
- `-it`: tương tác với terminal.
- `-p`: map cổng host:container.
- `-v`: mount volume hoặc thư mục.
- `--name`: đặt tên container.

## 5. Thực hành bắt buộc

### Bài thực hành 1

Chạy Nginx:

```bash
docker pull nginx:latest
docker run -d -p 8080:80 --name web-demo nginx:latest
```

Sau đó:

```bash
docker ps
docker logs web-demo
```

Mở trình duyệt tại:

```text
http://localhost:8080
```

### Bài thực hành 2

Chạy Ubuntu tương tác:

```bash
docker run -dit --name ubuntu-demo ubuntu bash
docker exec -it ubuntu-demo bash
```

## 6. Điều dễ nhầm

- `localhost` trong container là chính container đó.
- `EXPOSE` không tự map cổng.
- Container dừng không đồng nghĩa container bị xóa.

## 7. Bài tập mini

- Kéo image `redis:7`.
- Chạy Redis với tên `redis-demo`.
- Xem log rồi dừng và xóa container.
