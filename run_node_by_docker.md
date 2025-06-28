# Hướng dẫn chạy Nexus CLI bằng Docker trên Ubuntu 22.04
- Dưới đây là các bước chi tiết để chạy Nexus CLI trên Ubuntu 22.04 sử dụng Dockerfile bạn cung cấp:
---
## Bước 1: Cài đặt Docker trên Ubuntu 22.04
### Cài đặt docker
- Cập nhật hệ thống:
- Cài đặt phục thuộc
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg
```
- Thêm GPG key của Docker:
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
- Thêm repository Docker:
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
- Cài đặt docker:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
docker --version
docker run hello-world
sudo usermod -aG docker $USER
newgrp docker
```
## Bước 2: Chuẩn bị mã nguồn và Dockerfile
- Clone repository Nexus CLI:
```bash
git clone https://github.com/nexus-xyz/nexus-cli.git
cd nexus-cli/clients/cli
```
## Bước 3: Build Docker image
- Build image:
```bash
docker build -t nexus-cli .
```
- Kiểm tra image:
```bash
docker images
```
Bạn nên thấy nội dung như sau:
```
REPOSITORY                            TAG                      IMAGE ID       CREATED          SIZE
nexus-cli                             latest                   dc712e28e6a4   10 minutes ago   36.3MB
```
## Bước 4: Chạy container
Chạy node :
```bash
docker run -it --rm nexus-cli start --node-id 8199216
```

