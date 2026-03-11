# Báo cáo Lab 5: Lọc không gian và Phát hiện đặc trưng ảnh

## Tổng quan

Bài lab này tập trung vào:

* Thực hiện các phép lọc không gian (Spatial Filtering) trên ảnh
* So sánh hai thư viện xử lý ảnh: **PIL (Pillow)** và **OpenCV**
* Phát hiện đặc trưng ảnh (Feature Detection)
* Ứng dụng các bộ lọc tuyến tính và phi tuyến tính trong xử lý ảnh

---

## Cấu trúc file

* **2_5_1_Spatial_Filtering-PIL.ipynb**
  Thực hiện các phép lọc không gian trên ảnh sử dụng thư viện PIL/Pillow

* **2_5_2_Spatial_Filtering.ipynb**
  Thực hiện các phép lọc không gian trên ảnh sử dụng thư viện OpenCV

* **Lab03_Feature_Detection.doc**
  Bài làm Lab 03 về phát hiện đặc trưng trong ảnh (Feature Detection)

---

## Nội dung thực hiện

### 1. Lọc không gian (Spatial Filtering)

Các phép toán không gian sử dụng các pixel lân cận để xác định giá trị pixel hiện tại. Ứng dụng bao gồm lọc nhiễu, làm sắc nét ảnh, phát hiện cạnh — là các bước quan trọng trong Computer Vision và các thuật toán AI.

#### a. Lọc tuyến tính (Linear Filtering)

##### Lọc nhiễu (Filtering Noise)
* Làm mờ ảnh bằng cách trung bình hóa các pixel trong vùng lân cận (Smoothing / Low-pass Filter)
* Kernel đơn giản lấy trung bình cộng các pixel trong vùng kernel
* Kernel nhỏ giữ ảnh sắc nét hơn nhưng lọc nhiễu kém hơn kernel lớn

**PIL/Pillow:**
* Sử dụng `ImageFilter.BLUR` và `ImageFilter.BoxBlur`
* Hỗ trợ kernel tùy chỉnh qua `ImageFilter.Kernel`

**OpenCV:**
* Sử dụng `cv2.filter2D()` với kernel tùy chỉnh
* Thực hiện tích chập 2D (2D convolution) giữa ảnh và kernel trên từng kênh màu

##### Gaussian Blur
* Làm mờ ảnh với Gaussian kernel, lọc nhiễu hiệu quả hơn đồng thời bảo toàn cạnh tốt hơn Mean Filter
* Giá trị Sigma ảnh hưởng đến mức độ làm mờ: sigma càng lớn, ảnh càng mờ

**PIL/Pillow:**
* Sử dụng `ImageFilter.GaussianBlur(radius)`

**OpenCV:**
* Sử dụng `cv2.GaussianBlur(src, ksize, sigmaX, sigmaY)`
* Cho phép điều chỉnh độ lệch chuẩn theo trục X và Y riêng biệt

##### Làm sắc nét ảnh (Image Sharpening)
* Kết hợp làm mịn ảnh và tính đạo hàm để tạo hiệu ứng sắc nét
* Áp dụng Sharpening Kernel tùy chỉnh thông qua tích chập

**PIL/Pillow:**
* Sử dụng `ImageFilter.SHARPEN` hoặc kernel tùy chỉnh

**OpenCV:**
* Sử dụng `cv2.filter2D()` với Sharpening kernel

#### b. Phát hiện cạnh (Edges)

* Cạnh là nơi cường độ pixel thay đổi đột ngột
* Sử dụng gradient của hàm để xấp xỉ sự thay đổi cường độ ảnh grayscale
* Ảnh được làm mịn trước để giảm nhiễu trước khi phát hiện cạnh

**Sobel Edge Detector:**
* Xấp xỉ đạo hàm theo chiều X hoặc Y
* Kết hợp nhiều tích chập để tìm cả hướng và độ lớn của gradient

**PIL/Pillow:**
* Sử dụng `ImageFilter.FIND_EDGES` và `ImageFilter.EDGE_ENHANCE`

**OpenCV:**
* Sử dụng `cv2.Sobel(src, ddepth, dx, dy, ksize)`
* Tham số `dx`, `dy` xác định bậc đạo hàm theo từng chiều

#### c. Lọc Median (Median Filter)

* Thay thế mỗi pixel bằng giá trị trung vị (median) của các pixel trong vùng kernel
* Hiệu quả trong việc khử nhiễu salt-and-pepper mà vẫn bảo toàn cạnh
* Là bộ lọc phi tuyến tính (non-linear filter)

**PIL/Pillow:**
* Sử dụng `ImageFilter.MedianFilter(size)`

**OpenCV:**
* Sử dụng `cv2.medianBlur(src, ksize)`

#### d. Ngưỡng hóa (Threshold) *(chỉ trong OpenCV)*

* Phân chia pixel thành hai nhóm dựa trên ngưỡng giá trị
* Ứng dụng trong tách vật thể khỏi nền và phân vùng ảnh

**OpenCV:**
* Sử dụng `cv2.threshold(src, thresh, maxval, type)`
* Các kiểu threshold: BINARY, BINARY_INV, TRUNC, TOZERO, OTSU

---

### 2. Phát hiện đặc trưng (Feature Detection) — Lab03

Phát hiện đặc trưng là kỹ thuật xác định các điểm, vùng, hoặc cấu trúc đặc biệt trong ảnh có tính ổn định và khả năng phân biệt cao, phục vụ cho các bài toán nhận dạng và so khớp ảnh.

#### Các kỹ thuật phát hiện đặc trưng:
* **Harris Corner Detector**: Phát hiện góc và điểm đặc trưng dựa trên sự thay đổi cường độ theo nhiều hướng
* **SIFT (Scale-Invariant Feature Transform)**: Trích xuất đặc trưng bất biến với tỉ lệ và góc xoay
* **Keypoint Detection**: Xác định các điểm đặc trưng nổi bật trong ảnh
* **Feature Descriptors**: Mô tả đặc trưng xung quanh mỗi keypoint để phục vụ so khớp

#### Ứng dụng:
* So khớp ảnh (Image Matching)
* Nhận dạng đối tượng (Object Recognition)
* Stitching ảnh panorama
* Tracking đối tượng trong video

---

### 3. So sánh PIL và OpenCV trong Spatial Filtering

| Tiêu chí | PIL/Pillow | OpenCV |
|----------|------------|---------|
| **Định dạng ảnh** | RGB (Red, Green, Blue) | BGR (Blue, Green, Red) |
| **Cấu trúc dữ liệu** | PIL Image object | NumPy array |
| **Gaussian Blur** | `ImageFilter.GaussianBlur` | `cv2.GaussianBlur` |
| **Median Filter** | `ImageFilter.MedianFilter` | `cv2.medianBlur` |
| **Edge Detection** | `ImageFilter.FIND_EDGES` | `cv2.Sobel`, `cv2.Canny` |
| **Threshold** | Không hỗ trợ trực tiếp | `cv2.threshold` |
| **Tùy biến kernel** | Hạn chế | Linh hoạt, đa dạng |
| **Hiệu năng** | Chậm hơn với ảnh lớn | Nhanh hơn, tối ưu |
| **Ứng dụng** | Xử lý ảnh cơ bản | Computer Vision, Video |

**Lưu ý quan trọng:**
* Khi sử dụng OpenCV cần chuyển đổi BGR → RGB để hiển thị đúng màu với Matplotlib
* PIL phù hợp cho xử lý ảnh đơn giản và nhanh chóng
* OpenCV cung cấp nhiều tham số tinh chỉnh hơn, phù hợp cho xử lý ảnh nâng cao

---

## Kết luận

Qua Lab 5:

* Nắm vững các phép lọc không gian cơ bản: Mean Filter, Gaussian Blur, Median Filter, Sharpening
* Hiểu cơ chế tích chập (convolution) và vai trò của kernel trong lọc ảnh
* Biết cách phát hiện và tăng cường cạnh trong ảnh bằng Sobel Operator
* Áp dụng ngưỡng hóa (thresholding) để phân vùng ảnh
* Phân biệt lọc tuyến tính và phi tuyến tính, ưu nhược điểm của từng loại
* Nắm được các kỹ thuật phát hiện đặc trưng cơ bản trong ảnh

**Ứng dụng thực tế:**
* Tiền xử lý ảnh cho các mô hình Machine Learning
* Khử nhiễu và cải thiện chất lượng ảnh
* Phát hiện cạnh phục vụ phân vùng ảnh (Image Segmentation)
* Nhận dạng và so khớp đối tượng trong Computer Vision
* Xây dựng pipeline xử lý ảnh cho hệ thống thị giác máy tính

---

## Công cụ sử dụng

* Python 3.11.5
* PIL/Pillow — Xử lý ảnh cơ bản và lọc không gian
* OpenCV (cv2) — Computer Vision và lọc ảnh nâng cao
* NumPy — Xử lý mảng và tính toán ma trận
* Matplotlib — Trực quan hóa kết quả