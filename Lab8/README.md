# Báo cáo Lab 8: Video Object Detection

## Tổng quan

Bài lab này tập trung vào nhận diện đối tượng trong video sử dụng deep learning:

* **Lab 08 – Video Detection:** Phát hiện và gán nhãn đối tượng theo thời gian thực trong video bằng mô hình RetinaNet thông qua thư viện ImageAI

---

## Lab 08: Nhận diện đối tượng trong video (Video Object Detection)

### 1. Cài đặt thư viện

Cài đặt các thư viện cần thiết để chạy pipeline nhận diện video.

**Các thư viện sử dụng:**
* `imageai==3.0.3` – Giao diện cấp cao cho nhận diện đối tượng
* `torch`, `torchvision` (CPU build) – Backend deep learning của PyTorch
* `opencv-python` – Đọc/ghi và xử lý video
* `matplotlib` – Trực quan hóa frame kết quả

---

### 2. Tải model weights

Tải trọng số mô hình pretrained RetinaNet từ repository chính thức của ImageAI.

**File model:** `retinanet_resnet50_fpn_coco-eeacb38b.pth` (~130 MB)

**Nguồn:** https://github.com/OlafenwaMoses/ImageAI/releases/download/3.0.0-pretrained/retinanet_resnet50_fpn_coco-eeacb38b.pth

**Cơ chế tải:**
* Kiểm tra nếu file đã tồn tại để tránh tải lại
* Hiển thị tiến trình phần trăm trong quá trình tải

---

### 3. Mô hình RetinaNet

RetinaNet là một mô hình phát hiện đối tượng một giai đoạn (single-stage detector) sử dụng Feature Pyramid Network (FPN) để xử lý đa tỉ lệ.

**Kiến trúc:**

$$\text{RetinaNet} = \text{ResNet-50 Backbone} + \text{FPN} + \text{Classification/Regression Heads}$$

**Đặc điểm:**
* Pretrained trên tập dữ liệu COCO (80 lớp đối tượng phổ biến)
* Sử dụng Focal Loss để xử lý mất cân bằng giữa foreground và background
* Phát hiện đối tượng ở nhiều tỉ lệ kích thước khác nhau

---

### 4. Kiểm tra video đầu vào

Phân tích thông số video `video_predetect.mp4` trước khi đưa vào mô hình.

**Thông tin được kiểm tra:**
* FPS (khung hình/giây)
* Kích thước frame (width × height)
* Tổng số frames và thời lượng video
* Hiển thị 4 frame mẫu phân bố đều trong video

---

### 5. Nhận diện đối tượng trong video

Chạy pipeline nhận diện đối tượng trên toàn bộ video đầu vào.

**Các bước thực hiện:**
* Khởi tạo `VideoObjectDetection` từ ImageAI
* Load mô hình RetinaNet với trọng số COCO pretrained
* Gọi `detectObjectsFromVideo()` với video đầu vào `video_predetect.mp4`
* Xuất video kết quả `video_detected` với bounding box và nhãn được vẽ trên từng frame

**Tham số:**
* `frames_per_second` – Số frame xử lý mỗi giây (giữ nguyên FPS gốc)
* `log_progress=True` – Hiển thị tiến trình xử lý từng frame

---

### 6. Xem kết quả sau khi detect

Trực quan hóa 4 frame mẫu lấy từ video đã qua nhận diện để kiểm tra kết quả.

**Cơ chế:**
* Đọc video output bằng `cv2.VideoCapture`
* Lấy frame ở các vị trí phân bố đều (1/4, 2/4, 3/4, 4/4 tổng số frame)
* Hiển thị kết quả bằng Matplotlib với tiêu đề từng frame

**Kết quả:** Mỗi frame hiển thị bounding box màu sắc khác nhau cho từng loại đối tượng được phát hiện kèm nhãn tên và độ tin cậy (confidence score).

---

### 7. Xuất và tải video kết quả

Đóng gói và tải video kết quả về máy cục bộ.

**Hỗ trợ hai môi trường:**
* **Google Colab:** Tự động gọi `files.download()` để tải xuống
* **Local Jupyter:** In đường dẫn file để copy thủ công

**File đầu ra:** `video_detected.mp4` (hoặc `.avi` tùy ImageAI)

---

## Pipeline tổng thể

```
video_predetect.mp4
        │
        ▼
  [Kiểm tra thông số]
  (FPS, kích thước, số frames)
        │
        ▼
  [Load RetinaNet Model]
  (retinanet_resnet50_fpn_coco-eeacb38b.pth)
        │
        ▼
  [detectObjectsFromVideo()]
  (Vẽ bounding box + nhãn từng frame)
        │
        ▼
  video_detected.mp4
        │
        ▼
  [Xem 4 frame mẫu & Tải về]
```

---

## Công cụ sử dụng

* Python 3.11
* ImageAI (`imageai==3.0.3`) — Pipeline nhận diện đối tượng trong video
* PyTorch (`torch`, `torchvision`) — Backend deep learning
* OpenCV (`cv2`) — Đọc, ghi và xử lý video
* Matplotlib — Trực quan hóa frame kết quả
* Model: RetinaNet ResNet-50 FPN, pretrained trên COCO dataset (80 classes)