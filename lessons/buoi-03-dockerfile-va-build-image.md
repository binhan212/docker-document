# Buổi 3 - Dockerfile và build image

## Mục tiêu

Sau buổi này, bạn cần:

- Hiểu Dockerfile là gì.
- Biết các lệnh `FROM`, `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `WORKDIR`, `ENV`, `EXPOSE`, `VOLUME`.
- Build được image đầu tiên của mình.

## 1. Dockerfile là gì?

Dockerfile là file mô tả cách tạo image.

Ví dụ cơ bản:

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

## 2. Ý nghĩa các lệnh chính

- `FROM`: chọn base image.
- `RUN`: chạy lệnh khi build.
- `CMD`: lệnh mặc định khi container khởi động.
- `ENTRYPOINT`: lệnh chính của container.
- `COPY`: copy file vào image.
- `WORKDIR`: thư mục làm việc.
- `ENV`: biến môi trường.
- `EXPOSE`: khai báo cổng ứng dụng dùng.
- `VOLUME`: khai báo chỗ lưu dữ liệu.

## 3. Build image

```bash
docker build -t hello-docker:1.0 .
```

Nếu Dockerfile có tên khác:

```bash
docker build -f Dockerfile.dev -t hello-docker:dev .
```

## 4. .dockerignore

Ví dụ:

```gitignore
node_modules
.git
.vscode
.env
dist
```

Mục đích:

- Build nhanh hơn.
- Tránh copy file rác.
- Giảm nguy cơ lộ dữ liệu nhạy cảm.

## 5. Thực hành với file mẫu

Dùng thư mục `practice/nginx-static` trong workspace.

Bên trong đã có:

- Dockerfile
- index.html
- .dockerignore

Chạy trong thư mục đó:

```bash
docker build -t hello-docker:1.0 .
docker run -d -p 8081:80 --name hello-site hello-docker:1.0
```

Mở trình duyệt tại:

```text
http://localhost:8081
```

## 6. Best practices cơ bản

- Ưu tiên image nhẹ khi phù hợp.
- Tối ưu cache layer bằng cách copy dependency trước.
- Không nhét secret vào image.
- Một container nên có một trách nhiệm chính.

## 7. Bài tập mini

- Sửa nội dung `index.html` rồi build lại image.
- Đổi tag từ `1.0` sang `1.1`.
- So sánh hai image bằng `docker images`.
