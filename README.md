# Chapter 8 Compose Yolo

This repository contains the Compose tutorial with YOLO mentioned in **Chapter 8** of the book:

[**Cloud Computing for Artificial Intelligence: Concepts, Methods, and Practical Tools**](https://amzn.eu/d/02lMPIKf)

# YOLO Image Processing with RabbitMQ and FastAPI

This repository demonstrates a distributed image processing pipeline using Docker Compose. It includes:

- A FastAPI uploader service that accepts image uploads and queues them via RabbitMQ.
- A YOLO worker that consumes images from the queue, performs object detection, and stores annotated results.
- A RabbitMQ message broker for communication between services.

## Project Structure

```
CloudComputingW5ComposeYolo/
├── docker-compose.yml
├── bus.jpg
├── uploader_service/
│   ├── Dockerfile
│   ├── main.py
├── yolo_worker/
│   ├── Dockerfile
│   ├── consumer.py
│   ├── db.py
│   └── data/
```

## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/CloudComputingW5ComposeYolo.git
cd CloudComputingW5ComposeYolo
```

### 2. Build and Start the Services
```bash
sudo docker compose build
sudo docker compose up
```

### 3. Upload an Image
Use a tool like `curl` or Postman to send a POST request:
```bash
curl -X POST "http://localhost:8010/upload/" -F "file=@bus.jpg"
```

## Services Overview

### RabbitMQ
- Provides message queuing for asynchronous communication.
- Management UI available at `http://localhost:15672` (user: guest, pass: guest).

### Uploader Service (FastAPI)
- Accepts image uploads via `/upload/` endpoint.
- Publishes image bytes to RabbitMQ queue `image_tasks`.

### YOLO Worker
- Listens to `image_tasks` queue.
- Uses YOLOv8 to detect objects in images.
- Annotates and saves images to `data/images/`.
- Stores detection metadata in a database.

## Docker Compose Commands

### Build and Run
```bash
sudo docker compose build
sudo docker compose up
```

### Start in Detached Mode
```bash
sudo docker compose up -d
```

### Stop and Remove Containers
```bash
sudo docker compose down
```

### View Logs
```bash
sudo docker compose logs
```

### Execute Commands Inside Container
```bash
sudo docker compose exec uploader bash
sudo docker compose exec worker bash
```

### Clean Up Unused Resources
```bash
sudo docker system prune
```

## Notes
- Ensure `yolov8n.pt` is available for the YOLO worker.
- Annotated images are saved in `yolo_worker/data/images/`.
