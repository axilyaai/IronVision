# IronVision

Advanced real-time object detection and recognition system powered by YOLOv8n for precise and efficient computer vision analysis.

## Overview

IronVision is a high-performance object detection application utilizing YOLOv8 Nano (YOLOv8n) model for real-time computer vision tasks. It provides fast, accurate object detection with a lightweight footprint suitable for edge devices and mobile applications.

## Features

🎯 **YOLOv8n Object Detection**
- State-of-the-art object detection model
- Real-time performance on edge devices
- High accuracy with minimal latency

⚡ **High Performance**
- Optimized for speed and accuracy balance
- Lightweight model (nano variant)
- Efficient resource utilization

🔌 **Server Integration**
- REST API for detection requests
- WebSocket support for real-time streams
- Batch processing capabilities

📊 **Comprehensive Analysis**
- Multi-object detection
- Confidence scoring
- Bounding box coordinates
- Class categorization

🌐 **Cross-Platform**
- Client-server architecture
- Multiple client support
- Scalable deployment

## Quick Start

### Prerequisites

- Python 3.8 or higher
- CUDA 11.8+ (optional, for GPU acceleration)
- pip or poetry package manager
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/axilyaai/IronVision.git

# Navigate to directory
cd IronVision

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Configuration

Create a `.env` file:

```env
ENVIRONMENT=development
PORT=8000
YOLO_MODEL=yolov8n
CONFIDENCE_THRESHOLD=0.5
IOU_THRESHOLD=0.4
DEVICE=cpu  # or cuda for GPU
MAX_DETECTIONS=100
```

### Running the Application

```bash
# Start detection server
python main.py

# Start with GPU (if available)
DEVICE=cuda python main.py

# Run with specific model size
# yolov8n (nano), yolov8s (small), yolov8m (medium), yolov8l (large)
YOLO_MODEL=yolov8s python main.py
```

## Project Structure

```
IronVision/
├── src/
│   ├── detection/
│   │   ├── yolo_detector.py
│   │   ├── model_loader.py
│   │   └── post_processing.py
│   ├── api/
│   │   ├── routes.py
│   │   ├── schemas.py
│   │   └── handlers.py
│   ├── client/
│   │   ├── detection_client.py
│   │   └── stream_processor.py
│   └── utils/
│       ├── logger.py
│       └── config.py
├── models/
│   └── yolov8n.pt
├── tests/
├── requirements.txt
├── docker-compose.yml
├── .env.example
└── README.md
```

## API Documentation

### Object Detection Endpoint

**POST** `/api/detect`

```json
{
  "image": "base64_encoded_image",
  "confidence_threshold": 0.5,
  "iou_threshold": 0.4
}
```

Response:
```json
{
  "success": true,
  "detections": [
    {
      "class": "person",
      "confidence": 0.92,
      "bbox": [x, y, width, height],
      "class_id": 0
    },
    {
      "class": "car",
      "confidence": 0.87,
      "bbox": [x, y, width, height],
      "class_id": 2
    }
  ],
  "inference_time_ms": 45
}
```

### Batch Detection

**POST** `/api/detect/batch`

Process multiple images in one request for improved throughput.

### Real-Time Stream

**WebSocket** `/ws/detect`

Stream continuous detections from video source.

## YOLOv8 Model Variants

| Model | Size | Speed | Accuracy |
|-------|------|-------|----------|
| YOLOv8n | ~3.2 MB | Fastest | Good |
| YOLOv8s | ~11.2 MB | Fast | Very Good |
| YOLOv8m | ~25.9 MB | Medium | Excellent |
| YOLOv8l | ~52.9 MB | Slower | Superior |

## Model Download

Models are automatically downloaded on first run. To pre-download:

```bash
python -c "from ultralytics import YOLO; YOLO('yolov8n.pt')"
```

## Performance Metrics

- **Inference Speed**: ~10-50ms per image (YOLOv8n on CPU)
- **GPU Speed**: ~5-15ms per image (NVIDIA GPU)
- **Model Size**: ~3.2MB (YOLOv8n)
- **Memory Usage**: ~200-500MB (varies by model)

## Training Custom Models

```bash
# Train on custom dataset
python train.py --model yolov8n --data data.yaml --epochs 100

# Validate model
python validate.py --model runs/detect/train/weights/best.pt

# Export model
python export.py --model runs/detect/train/weights/best.pt --format onnx
```

## Docker Deployment

```bash
# Build Docker image
docker build -t ironvision .

# Run container
docker run -p 8000:8000 ironvision

# Using Docker Compose
docker-compose up -d
```

## Performance Optimization

- Use YOLOv8n for edge devices
- Enable GPU acceleration when available
- Adjust confidence threshold based on use case
- Batch process when possible
- Implement result caching

## Supported Classes

YOLOv8 is trained on COCO dataset (80 classes):
- Persons, vehicles, animals
- Accessories, sports items
- Household objects, furniture
- Food items and more

## Testing

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src tests/

# Test specific module
pytest tests/test_detection.py

# Integration tests
pytest tests/integration/ -v
```

## Troubleshooting

### Model Download Failed
```bash
# Manually download model
wget https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8n.pt
```

### GPU Not Detected
```bash
# Check CUDA installation
python -c "import torch; print(torch.cuda.is_available())"
```

### Out of Memory
```bash
# Use smaller model or reduce batch size
export YOLO_MODEL=yolov8n
export BATCH_SIZE=4
```

## Security

- Input validation on all API endpoints
- Rate limiting to prevent abuse
- Secure model storage
- API authentication (optional)
- HTTPS support

## Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Code Style

- Follow PEP 8 for Python code
- Use Black for code formatting
- Type hints for functions
- Comprehensive docstrings

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Ultralytics for YOLOv8
- COCO dataset creators
- Computer vision community
- Built with assistance from Claude AI

---

**Made with ❤️ by Ali Sefa AKKAŞ and Claude**

[⬆ Back to top](#ironvision)
