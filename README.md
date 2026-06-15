# IronVision

Real-time object detection HUD for Android. Your phone's camera streams frames to a YOLOv8 server running on your PC — detections appear as animated overlays on screen.

```
Android Camera → Wi-Fi → YOLOv8 (PC) → Detection Results → HUD Overlay
```

---

## What it looks like

- Full-screen camera feed
- Corner markers animate around detected objects
- Labels show object name + confidence score
- Small status bar in the top-left: connection state, object count, FPS, latency
- Connection dialog on launch — enter your PC's IP and go

---

## Requirements

**Android**
- Android 8.0+ (API 26)
- Camera permission
- Same Wi-Fi network as the PC running the server

**PC (server)**
- Python 3.12.2
- NVIDIA GPU recommended (CUDA) — CPU works too, just slower
- Same Wi-Fi network as the phone

---

## Setup

### 1. Clone the repo

```bash
git clone https://github.com/axilyaai/IronVision.git
cd IronVision
```

### 2. Start the server

```bash
cd server
pip install -r requirements.txt
python server.py
```

On first run, `yolov8s.pt` (~22 MB) downloads automatically.

**With NVIDIA GPU:**
```bash
pip uninstall torch torchvision -y
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
python server.py
```

**Verify GPU is available:**
```bash
python -c "import torch; print(torch.cuda.is_available())"
# Should print: True
```

### 3. Find your PC's IP address

**Windows:**
```
ipconfig
```
Look for `IPv4 Address` under your Wi-Fi adapter (e.g. `192.168.1.104`).

### 4. Build and install the Android app

Open the project in Android Studio, build and run on your device.

On first launch, enter your PC's IP in the connection dialog and tap **CONNECT**.

---

## Server options

```bash
# Default (yolov8s, CUDA, port 8765)
python server.py

# Use a different model
python server.py --model yolov8n.pt   # faster, less accurate
python server.py --model yolov8m.pt   # slower, more accurate

# Force CPU
python server.py --device cpu

# Adjust confidence threshold
python server.py --conf 0.35

# Custom port
python server.py --port 9000
```

| Model | Size | GPU Speed | Accuracy |
|-------|------|-----------|----------|
| yolov8n | 6 MB | ~3ms | Good |
| yolov8s | 22 MB | ~5ms | Better ✓ |
| yolov8m | 50 MB | ~10ms | Best |

---

## Tech stack

**Android**
| | |
|---|---|
| Language | Kotlin |
| UI | Jetpack Compose |
| Camera | CameraX |
| Networking | OkHttp WebSocket |
| DI | Hilt |
| Architecture | MVVM |

**Server**
| | |
|---|---|
| Language | Python 3.12.2 |
| Detection | YOLOv8s (Ultralytics) |
| Transport | WebSocket (`websockets`) |
| Acceleration | CUDA (optional) |

---

## Project structure

```
IronVision/
├── app/src/main/java/com/example/ironvision/
│   ├── core/
│   │   ├── camera/        # CameraX setup, frame capture
│   │   └── network/       # WebSocket client
│   ├── domain/model/      # DetectedObject, HudState
│   └── feature/hud/
│       ├── HudViewModel.kt
│       └── ui/
│           ├── HudScreen.kt
│           ├── ServerConnectDialog.kt
│           └── overlay/   # TargetingOverlay
└── server/
    ├── server.py
    └── requirements.txt
```

---

## Troubleshooting

**Can't connect to server**
- Make sure phone and PC are on the same Wi-Fi network
- Allow port 8765 through Windows Firewall:
  ```
  netsh advfirewall firewall add rule name="IronVision" dir=in action=allow protocol=TCP localport=8765
  ```

**"Torch not compiled with CUDA enabled"**
- Reinstall PyTorch with CUDA support (see Setup → Step 2)

**Low FPS**
- Switch to `yolov8n.pt` for faster inference
- Make sure GPU is being used (`--device cuda`)

---

## License

MIT — see [LICENSE](LICENSE).
