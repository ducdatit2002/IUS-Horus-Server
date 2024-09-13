# IUS Horus Server

## Run the Docker Compose Services

1. Run the service container
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
2. Run
```bash
bash scripts/run.sh
```

