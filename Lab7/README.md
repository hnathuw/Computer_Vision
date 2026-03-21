# Báo cáo Lab 7: Image Segmentation & Image Detection

## Tổng quan

Bài lab này gồm 2 phần:

* **Lab 05 – Image Segmentation:** Phân đoạn ảnh bằng các kỹ thuật ngưỡng, phân cụm và phát triển vùng
* **Lab 06 – Image Detection:** Nhận diện đối tượng và các bộ phận khuôn mặt người bằng Haar Cascade

---

## Lab 05: Phân đoạn ảnh (Image Segmentation)

### 1. Thresholding

Phân đoạn ảnh bằng cách so sánh giá trị pixel với một ngưỡng cố định hoặc ngưỡng cục bộ.

**Các phương pháp thực hiện:**
* Binary Thresholding (ngưỡng = 127)
* Binary Inverse Thresholding
* Truncated Thresholding
* Adaptive Mean Thresholding (`blockSize=11, C=2`)
* Adaptive Gaussian Thresholding (`blockSize=11, C=2`)

**Tham khảo:** https://docs.opencv.org/4.x/d7/d4d/tutorial_py_thresholding.html

---

### 2. Otsu's Algorithm

Tự động tìm ngưỡng tối ưu dựa trên histogram ảnh, phù hợp với ảnh có histogram hai đỉnh (bimodal) như ảnh vân tay.

**Nguyên lý:**

$$\sigma^2_B = w_0 w_1 (\mu_0 - \mu_1)^2$$

Tìm ngưỡng $t$ để tối đa hóa phương sai giữa hai lớp $\sigma^2_B$.

**Các bước thực hiện:**
* Áp dụng `cv2.THRESH_OTSU` để tự tìm ngưỡng tối ưu
* So sánh kết quả trước và sau khi lọc Gaussian Blur
* Trực quan hóa histogram và ngưỡng Otsu

---

### 3. K-Means Clustering

Phân đoạn ảnh dựa trên phân cụm màu sắc pixel.

**Các bước thực hiện:**
* Reshape ảnh thành mảng 2D (pixels × channels)
* Chạy `cv2.kmeans()` với K = 2, 3, 4, 5
* Gán màu trung tâm cụm về lại từng pixel
* So sánh kết quả với các giá trị K khác nhau

**Nhận xét:** K càng lớn, ảnh càng chi tiết nhưng thời gian xử lý tăng.

---

### 4. Region Growing

Phát triển vùng từ một điểm khởi đầu (seed point) dựa trên độ tương đồng cường độ pixel.

**Nguyên lý:**
* Bắt đầu từ seed point, duyệt 8-connected neighbors
* Chấp nhận pixel láng giềng nếu `|pixel - seed_val| ≤ threshold`
* Tiếp tục mở rộng cho đến khi không còn pixel đủ điều kiện

**Các tham số thử nghiệm:** threshold = 10, 20, 30

---

### 5. Split and Merge (Quadtree)

Phân đoạn ảnh bằng cách chia đệ quy thành các khối nhỏ (quadtree), sau đó gộp lại các vùng đồng nhất.

**Tiêu chí đồng nhất:** Độ lệch chuẩn (std) của khối ≤ `homogeneity_thresh`

**Các bước thực hiện:**
* Chia ảnh thành 4 quadrant đệ quy nếu vùng không đồng nhất
* Gán giá trị trung bình cho vùng đồng nhất
* Trực quan hóa ranh giới quadtree

---

### 6. Edge-Based Segmentation

Phân đoạn ảnh dựa trên phát hiện cạnh.

**Các phương pháp thực hiện:**

| Phương pháp | Mô tả |
|-------------|-------|
| Sobel X/Y | Gradient theo trục ngang và dọc |
| Sobel tổng hợp | `cv2.magnitude(sobelx, sobely)` |
| Laplacian | Đạo hàm bậc 2, nhạy với nhiễu |
| Canny (3 mức) | Ngưỡng thấp (50–150), trung (100–200), cao (150–250) |
| Watershed | Kết hợp morphology + distance transform + markers |

---

## Lab 06: Nhận diện ảnh (Image Detection)

### 1. Code mẫu – Nhận diện biển báo STOP

Sử dụng Haar Cascade Classifier với file `stop_data.xml` để phát hiện biển báo STOP trong ảnh.

**Các bước thực hiện:**
* Chuyển ảnh sang grayscale
* Load `cv2.CascadeClassifier('stop_data.xml')`
* Gọi `detectMultiScale(img_gray, minSize=(20, 20))`
* Vẽ khung xanh lá quanh vùng phát hiện

**Kết quả:** Phát hiện được 1 biển báo STOP.

---

### 2. Nhận diện khuôn mặt (Face Detection)

Phát hiện khuôn mặt chính diện bằng `haarcascade_frontalface_default.xml`.

**Tham số:**
* `scaleFactor = 1.1` – thu nhỏ ảnh 10% mỗi bước
* `minNeighbors = 5` – số hàng xóm tối thiểu để xác nhận
* `minSize = (30, 30)` – kích thước khuôn mặt tối thiểu

**Kết quả:** Phát hiện được 4 khuôn mặt trong ảnh tập thể.

---

### 3. Nhận diện mắt và nụ cười

Phát hiện mắt và nụ cười bên trong từng vùng khuôn mặt đã xác định (ROI).

**File XML sử dụng:**
* Mắt: `haarcascade_eye.xml`
* Nụ cười: `haarcascade_smile.xml`

**Kỹ thuật:**
* Cắt ROI (Region of Interest) từng khuôn mặt
* Tìm mắt trong toàn bộ ROI khuôn mặt
* Tìm nụ cười trong nửa dưới ROI khuôn mặt

**Kết quả:** 7 mắt, 1 nụ cười trong ảnh tập thể.

---

### 4. Nhận diện mũi và miệng

Phát hiện mũi và miệng bên trong vùng khuôn mặt.

**File XML sử dụng:**
* Mũi: `haarcascade_mcs_nose.xml`
* Miệng: `haarcascade_mcs_mouth.xml`

**Kỹ thuật:**
* Mũi: tìm trong vùng 1/4 đến 3/4 chiều cao khuôn mặt
* Miệng: tìm trong nửa dưới khuôn mặt

---

## So sánh các phương pháp phân đoạn ảnh

| Phương pháp | Ưu điểm | Nhược điểm | Ứng dụng |
|-------------|---------|------------|----------|
| **Thresholding** | Đơn giản, nhanh | Nhạy với nhiễu, ánh sáng | Ảnh nhị phân đơn giản |
| **Otsu** | Tự động chọn ngưỡng | Chỉ hoạt động tốt với histogram bimodal | Ảnh vân tay, tài liệu |
| **K-Means** | Phân đoạn theo màu | Phụ thuộc K, chậm | Ảnh màu, phân vùng đối tượng |
| **Region Growing** | Bảo toàn hình dạng vùng | Phụ thuộc seed point | Ảnh y tế, MRI |
| **Split & Merge** | Không cần seed | Tốn bộ nhớ (đệ quy) | Phân tích cấu trúc ảnh |
| **Edge-based** | Phát hiện ranh giới rõ | Nhạy với nhiễu | Phát hiện biên, đường viền |

---

## Công cụ sử dụng

* Python 3.11
* OpenCV (`cv2`) — Xử lý ảnh, phát hiện đối tượng
* NumPy — Xử lý mảng và tính toán
* Matplotlib — Trực quan hóa kết quả 
* scikit-learn — K-Means Clustering
* scikit-image — Xử lý ảnh nâng cao
