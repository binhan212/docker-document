# Buổi 1 - Tổng quan Docker và cài đặt

## Mục tiêu

Sau buổi này, bạn cần nắm được:

- Docker giải quyết vấn đề gì.
- Container khác VM ở đâu.
- Cài Docker thành công trên máy của mình.
- Chạy được các lệnh `docker --version` và `docker info`.

## 1. Docker sinh ra để làm gì?

Docker giúp đóng gói ứng dụng cùng với môi trường cần thiết để chạy.

Ví dụ dễ hình dung:

- Không dùng Docker: mỗi máy tự cài theo kiểu riêng, xong rồi tranh luận xem ai đúng.
- Dùng Docker: cả team dùng cùng image hoặc cùng Dockerfile, bớt cảnh "máy em chạy được".

## 2. Container là gì? VM là gì?

- Container: môi trường gọn nhẹ, dùng chung kernel của máy host.
- VM: máy ảo gần như đầy đủ, có hệ điều hành riêng.

So sánh nhanh:

| Tiêu chí | VM | Container |
|---|---|---|
| Khởi động | Chậm hơn | Nhanh hơn |
| Tài nguyên | Tốn hơn | Gọn hơn |
| Mức độ tách biệt | Mạnh | Tốt |

Ví dụ vui:

- VM giống như thuê nguyên căn hộ.
- Container giống như thuê một quầy nấu ăn trong bếp chung.

## 3. Lợi ích của Docker

- Chạy nhất quán giữa dev, test, production.
- Dựng môi trường nhanh.
- Dễ chia sẻ cho đồng đội.
- Dễ cô lập từng dịch vụ.

## 4. Cài Docker

### Windows

- Cài Docker Desktop.
- Bật WSL 2 nếu được yêu cầu.
- Kiểm tra virtualization nếu Docker không khởi động.

### macOS

- Cài Docker Desktop đúng bản Intel hoặc Apple Silicon.

### Linux

- Cài Docker Engine hoặc Docker CE theo distro.

## 5. Kiểm tra cài đặt

Chạy:

```bash
docker --version
docker info
```

Nếu `docker info` báo không kết nối được daemon, thường là Docker Desktop chưa chạy hoặc service Docker chưa bật.

## 6. Thực hành cuối buổi

1. Cài Docker.
2. Chạy `docker --version`.
3. Chạy `docker info`.
4. Ghi lại một ảnh chụp màn hình hoặc copy kết quả để tự kiểm chứng.

## 7. Bài tập mini

- Tự giải thích lại bằng lời của bạn: image, container, VM.
- Trả lời câu hỏi: vì sao Docker giúp giảm lỗi khác môi trường?

## 8. Chuẩn bị cho buổi sau

Buổi sau bạn sẽ học:

- Image là gì.
- Container là gì.
- Các lệnh Docker cơ bản để kéo image, chạy container và xem log.
