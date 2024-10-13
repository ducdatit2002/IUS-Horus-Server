# IUS Horus Server

## Run the Docker Compose Services
0. cd
```bash
cd /home/datpham/datpham/IUSHorus/Video-Search
```
2. Run the service container
```bash
docker compose -f services/docker-compose.yml up
```
2. Thông port 
```bash
ssh -L 9001:127.0.0.1:9001 datpham@192.168.100.123
```
3. Truy cập
```bash
http://localhost:9001
```

## Run dev container

1. Start container
```bash
docker run --gpus '"device=0"' -it --name vbs_container --network services_milvus-network --env-file .env \
-v ./:/app \
-v /home/datpham/datpham/IUSHorus:/home/datpham/datpham/IUSHorus \
taindp98/aic-vbs:gpu /bin/bash
```
2. Run container
```bash
docker start vbs_container
```
```bash
docker exec -it vbs_container /bin/bash
```
3. Environment
```bash
conda activate vbs
```
4. Check connection
```bash
docker network inspect services_milvus-network
```
5. Run 01
```bash
scripts/monitor.sh
```
6. Run 02
```bash
scripts/run.sh
```
7. Run Frame Selection
```bash
scripts/frame_selection_1.sh
```

### Server anh Tiến

1. Docker run
```bash
docker run --gpus '"device=0"' -it --name vbs_container --env-file .env \
-v ./:/app \
-v /root/IUSHorus:/root/IUSHorus \
taindp98/aic-vbs:gpu /bin/bash
```
2. Cài thư viện
```
pip install torch==2.1.0 torchvision==0.16.0 torchaudio==2.1.0 --index-url https://download.pytorch.org/whl/cu121
pip install decord==0.6.0
```
3. Cài ffmpeg
```bash
conda install -c conda-forge ffmpeg
``` 
4. GPU
```bash
pip install psutil gputil
```
5. Ingestion
```bash
scripts/ingestion.sh
```
6. Copy data từ source a Tài qua source Đạt
```bash
cp -r /root/IUSHorus/taindp/Video-Search/services/volumes /root/IUSHorus/Video-Search/services/
```
7. Chunk volumes
```bash
split -n 5 -d -a 1 volumes.zip volumes_chunk_
```
### Cài docker
1. Xoá phiên bản cũ
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```
2. Xoá đường dẫn
```bash
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
3. Cài đặt các gói cần thiết: Trước tiên, hãy chắc chắn rằng bạn đã cài đặt các gói cần thiết cho việc cài Docker:
```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
4. Thêm khóa GPG của Docker: Thêm khóa GPG chính thức của Docker:
```bash
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
5. Thêm repository Docker: Thêm địa chỉ Docker vào danh sách các nguồn phần mềm:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
6. Cài đặt Docker
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
7. Cài đặt NVIDIA Container Toolkit
```bash
sudo apt-get install -y nvidia-docker2
```
8. Chạy Docker không cần sudo
```bash
sudo usermod -aG docker $USER
```
