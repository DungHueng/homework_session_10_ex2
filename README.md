# Bài 2 – Đóng gói Web Backend bằng Single-stage Dockerfile

## 1. Mục tiêu

- Xây dựng ứng dụng REST API bằng Node.js và Express.js.
- Đóng gói ứng dụng thành Docker Image bằng Dockerfile single-stage.
- Chạy ứng dụng trong Docker Container.
- Kiểm tra hoạt động của API sau khi triển khai.

## 2. Công nghệ sử dụng

- **Node.js:** 18 Alpine
- **Express.js:** 4.x
- **Docker:** Docker Desktop
- **Dockerfile:** Single-stage build

## 3. Cấu trúc dự án

```text
ex2/
├── Dockerfile
├── package.json
├── server.js
└── README.md
```

## 4. Nội dung Dockerfile

Sử dụng một stage duy nhất để cài đặt các thư viện cần thiết và chạy ứng dụng Node.js.

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

## 5. Build Docker Image

Mở CMD tại thư mục chứa Dockerfile và thực hiện lệnh:

```cmd
docker build -t hw-backend:v1 .
```

Trong đó:

- `docker build`: Xây dựng Docker Image.
- `-t hw-backend:v1`: Đặt tên image là `hw-backend` với tag `v1`.
- `.`: Sử dụng thư mục hiện tại làm build context.

Kiểm tra image sau khi build:

```cmd
docker images
```

## 6. Chạy Docker Container

Khởi chạy container từ image vừa tạo:

```cmd
docker run -d --name hw-backend-container -p 3000:3000 hw-backend:v1
```

Trong đó:

- `-d`: Chạy container ở chế độ nền.
- `--name hw-backend-container`: Đặt tên container.
- `-p 3000:3000`: Ánh xạ cổng 3000 trên máy host tới cổng 3000 trong container.
- `hw-backend:v1`: Image được sử dụng để chạy container.

## 7. Kiểm tra hoạt động

### 7.1. Kiểm tra container

```cmd
docker ps
```

Lệnh này hiển thị các container đang chạy, bao gồm tên container, image và cổng được ánh xạ.

### 7.2. Kiểm tra API

Truy cập endpoint được khai báo trong `server.js` bằng trình duyệt hoặc lệnh:

```cmd
curl http://localhost:3000
```

**Lưu ý:** Đường dẫn `/` phải được khai báo trong `server.js`. Nếu ứng dụng sử dụng endpoint khác, hãy thay URL bằng endpoint tương ứng.

### 7.3. Xem log ứng dụng

```cmd
docker logs hw-backend-container
```

Lệnh này giúp kiểm tra thông báo khởi động và phát hiện lỗi trong quá trình chạy ứng dụng.