# Báo cáo Lab 6: Keypoint Detection 

## Tổng quan

Bài lab này tập trung vào:

* Phát hiện các điểm đặc trưng (Keypoint Detection) trong ảnh
* Xây dựng và so khớp đặc trưng bất biến với scale và rotation bằng **SIFT**
* Phân tích scale-space và lựa chọn scale tự động
* Ứng dụng keypoint vào các bài toán thực tế: ghép ảnh panorama, tìm kiếm ảnh theo nội dung (CBIR)

---

## Nội dung thực hiện

### 1. Harris Corner Detection

Harris Corner Detector xác định các góc (corners) trong ảnh dựa trên sự thay đổi cường độ pixel theo nhiều hướng.

**Nguyên lý:**

$$R = \det(M) - k\,(\mathrm{trace}\,M)^2$$

* $R > 0$ → **Corner** (góc)
* $R < 0$ → **Edge** (cạnh)
* $R \approx 0$ → **Flat region** (vùng phẳng)

**Các bước thực hiện:**
* Chuyển ảnh sang grayscale, áp dụng `cv2.cornerHarris(gray, blockSize, ksize, k)`
* Dilate kết quả để làm nổi bật corner, đánh dấu bằng màu đỏ với ngưỡng 1% giá trị tối đa
* Tinh chỉnh vị trí corner bằng `cv2.cornerSubPix()` để đạt độ chính xác sub-pixel

**Tham khảo:** https://docs.opencv.org/3.4/dc/d0d/tutorial_py_features_harris.html

---

### 2. Band-pass Filtering – Difference of Gaussians (DoG)

DoG là phép lọc band-pass xấp xỉ **Laplacian of Gaussian (LoG)**, được sử dụng để phát hiện blob và keypoint ở nhiều scale.

$$\mathrm{DoG}(x,y,\sigma) = G(x,y,k\sigma) - G(x,y,\sigma)$$

**Các bước thực hiện:**
* Áp dụng Gaussian Blur với hai cặp sigma khác nhau: $(1,2)$, $(2,4)$, $(4,8)$, $(8,16)$
* Tính hiệu giữa hai ảnh đã làm mờ tương ứng mỗi cặp
* Trực quan hóa DoG response dưới dạng heatmap (`cmap='hot'`)

**Ý nghĩa:** Kernel sigma nhỏ phát hiện đặc trưng chi tiết; kernel sigma lớn phát hiện cấu trúc tổng thể.

---

### 3. Automatic Scale Selection

Chọn scale tối ưu cho từng điểm ảnh bằng cách tìm cực đại của **normalized LoG** trong scale-space:

$$\hat{\sigma}(x,y) = \arg\max_{\sigma}\; \sigma^2 \left|\nabla^2 G_\sigma * I\right|(x,y)$$

**Các bước thực hiện:**
* Xây dựng scale-space với 8 sigma: $\{1.0,\; 1.5,\; 2.0,\; 3.0,\; 4.0,\; 6.0,\; 8.0,\; 12.0\}$
* Tính normalized LoG tại mỗi scale
* Lấy `argmax` theo chiều scale để tìm sigma tốt nhất tại mỗi pixel
* Trực quan hóa bản đồ scale (`cmap='jet'`) và max response (`cmap='hot'`)

---

### 4. Scale Invariant Detection – SIFT

**SIFT (Scale-Invariant Feature Transform)** phát hiện và mô tả keypoint bất biến với scale, rotation và một phần với biến đổi ánh sáng.

**Các bước thực hiện:**
* Khởi tạo detector: `sift = cv2.SIFT_create()`
* Phát hiện keypoint và tính descriptor: `kp, desc = sift.detectAndCompute(gray, None)`
* Vẽ keypoint với `cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS` (hiển thị scale & orientation)
* Phân tích phân phối scale và orientation của keypoint bằng histogram

**Kích thước descriptor:** 128 chiều / keypoint

---

### 5. Scale-Space Blob Detector

Phát hiện các vùng tròn đồng nhất (blob) ở nhiều scale khác nhau bằng `cv2.SimpleBlobDetector`.

**Các tham số cấu hình:**

| Tham số | Giá trị | Ý nghĩa |
|---------|---------|---------|
| `minArea / maxArea` | 50 / 60000 | Lọc theo diện tích |
| `minCircularity` | 0.2 | Lọc theo độ tròn |
| `minConvexity` | 0.4 | Lọc theo độ lồi |
| `minInertiaRatio` | 0.05 | Lọc theo tỉ lệ quán tính |

---

### 6. Bag-of-Words Detection

**Pipeline Bag-of-Words (BoW) với SIFT:**

1. **Trích xuất descriptors:** Dùng SIFT lấy descriptor 128 chiều từ tất cả ảnh trong tập dữ liệu
2. **Xây dựng Visual Vocabulary:** Gom nhóm toàn bộ descriptors thành $K=40$ visual words bằng `MiniBatchKMeans`
3. **Mã hóa ảnh:** Mỗi ảnh được biểu diễn bằng histogram tần suất $K$ visual words (normalized)
4. **So sánh ảnh:** Tính khoảng cách giữa các histogram để đo độ tương đồng

**Bag-of-Words Histogram + SIFT:**
* Descriptor của từng keypoint được ánh xạ về visual word gần nhất (nearest neighbor trong vocabulary)
* Histogram thể hiện phân phối visual words trong ảnh, dùng làm đặc trưng đại diện ảnh

---

### 7. Image Panoramas

Ghép hai ảnh thành panorama sử dụng SIFT feature matching và Homography.

**Quy trình:**
1. Phát hiện keypoint bằng SIFT trên cả hai ảnh
2. So khớp descriptor bằng **FLANN Matcher** (nhanh hơn BFMatcher với dữ liệu lớn)
3. Lọc matches bằng **Lowe's ratio test** (ngưỡng 0.75)
4. Tính **Homography matrix** với RANSAC (`cv2.findHomography`)
5. Warp ảnh thứ hai về không gian ảnh thứ nhất bằng `cv2.warpPerspective`
6. Ghép hai ảnh lên canvas chung và cắt vùng đen

---

### 8. Automatic Mosaicing

Ghép nhiều ảnh tự động thành mosaic bằng `cv2.Stitcher`.

**Các bước thực hiện:**
* Cắt ảnh rộng thành các tile có overlap ~30%
* Gọi `cv2.Stitcher_create(cv2.Stitcher_PANORAMA).stitch(tiles)`
* Fallback thủ công bằng `np.hstack` nếu Stitcher không hội tụ

---

### 9. Wide Baseline Stereo

So khớp hai ảnh chụp từ vị trí hoặc góc nhìn rất khác nhau (góc nhìn rộng).

**Các bước thực hiện:**
* Mô phỏng góc nhìn thứ hai bằng Perspective Transform (`cv2.getPerspectiveTransform`)
* Dùng SIFT + **BFMatcher** với Lowe's ratio test để tìm good matches
* Trực quan hóa top-50 matches giữa hai góc nhìn

**So sánh với Panorama Stitching:**

| Tiêu chí | Panorama | Wide Baseline Stereo |
|----------|----------|----------------------|
| **Góc nhìn** | Nhỏ, có overlap rõ | Lớn, baseline rộng |
| **Matcher** | FLANN | BFMatcher |
| **Mục tiêu** | Ghép ảnh | Phân tích hình học 3D |

---

### 10. CBIR – Content-Based Image Retrieval

Tìm kiếm ảnh dựa trên nội dung bằng BoW histogram và Cosine Similarity.

**Quy trình:**
1. Biểu diễn mỗi ảnh bằng BoW histogram (từ Phần 6)
2. Tính **Cosine Similarity** giữa ảnh truy vấn và toàn bộ tập ảnh
3. Xếp hạng và trả về top-4 ảnh tương tự nhất

$$\text{similarity}(A,B) = \frac{A \cdot B}{\|A\|\,\|B\|}$$

---

## So sánh các phương pháp Keypoint Detection

| Phương pháp | Bất biến Scale | Bất biến Rotation | Tốc độ | Ứng dụng chính |
|---|---|---|---|---|
| **Harris** | ✗ | Một phần | Nhanh | Tracking, calibration |
| **DoG** | ✓ | ✗ | Trung bình | Scale-space analysis |
| **SIFT** | ✓ | ✓ | Chậm | Matching, recognition |
| **Blob** | ✓ | ✓ | Nhanh | Shape analysis |

---

## Kết luận

Qua Lab 6:

* Nắm vững nguyên lý Harris Corner Detection và ý nghĩa của Harris response $R$
* Hiểu cơ chế Difference of Gaussians và vai trò trong xây dựng scale-space
* Biết cách chọn scale tự động bằng normalized LoG (Automatic Scale Selection)
* Thành thạo SIFT: phát hiện keypoint bất biến scale + rotation, trích xuất descriptor 128 chiều
* Xây dựng được pipeline Bag-of-Words hoàn chỉnh từ trích xuất descriptor đến mã hóa histogram
* Ghép ảnh panorama bằng Homography + RANSAC và hiểu từng bước trong pipeline
* Triển khai hệ thống CBIR đơn giản bằng BoW + Cosine Similarity

**Ứng dụng thực tế:**
* Ghép ảnh panorama và 3D reconstruction
* Tìm kiếm ảnh theo nội dung (Image Search Engine)
* Nhận dạng và theo dõi đối tượng trong video
* Định vị camera và xây dựng bản đồ (SLAM)
* Xây dựng pipeline Computer Vision cho hệ thống thị giác máy tính

---

## Công cụ sử dụng

* Python 3.11.5
* OpenCV (cv2) — Keypoint detection, feature matching, image stitching
* NumPy — Xử lý mảng và tính toán ma trận
* Matplotlib — Trực quan hóa kết quả
* scikit-learn — MiniBatchKMeans (Visual Vocabulary), Cosine Similarity (CBIR)