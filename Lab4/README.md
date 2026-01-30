# Báo cáo Lab 4: Biến đổi hình học và Phép toán ma trận trên ảnh

## Tổng quan

Bài lab này tập trung vào:

* Áp dụng các phép biến đổi hình học trên ảnh (Geometric Transformations)
* Thực hiện các phép toán mảng và ma trận trên ảnh
* So sánh hai thư viện xử lý ảnh: **PIL (Pillow)** và **OpenCV**
* Nén ảnh bằng phương pháp **SVD (Singular Value Decomposition)**

---

## Cấu trúc file

* **2_4_1_Gemetric_trasfroms_PIL.ipynb**
  Thực hiện các phép biến đổi hình học và toán học trên ảnh sử dụng thư viện PIL/Pillow

* **2_4_2_Gemetric_trasfroms_OpenCV.ipynb**
  Thực hiện các phép biến đổi hình học và toán học trên ảnh sử dụng thư viện OpenCV

* **2374802010492_HuynhNgocAnhThu_252_71ITAI40603_0101_Lab02.ipynb**
  Bài làm Lab 02 về xử lý ảnh và các bộ lọc (filters)

---

## Nội dung thực hiện

### 1. Biến đổi hình học (Geometric Transformations)

Các phép biến đổi hình học cho phép thay đổi kích thước, vị trí và hướng của ảnh trong không gian.

#### a. Scaling (Thay đổi kích thước)
* Phóng to hoặc thu nhỏ ảnh theo tỷ lệ
* Sử dụng các phương pháp nội suy khác nhau (nearest, bilinear, bicubic)
* So sánh chất lượng ảnh sau khi scale với các phương pháp khác nhau

**PIL/Pillow:**
* Sử dụng phương thức `resize()` với các chế độ resampling
* Hỗ trợ: NEAREST, BILINEAR, BICUBIC, LANCZOS

**OpenCV:**
* Sử dụng hàm `cv2.resize()` 
* Hỗ trợ: INTER_NEAREST, INTER_LINEAR, INTER_CUBIC, INTER_AREA

#### b. Translation (Dịch chuyển)
* Di chuyển ảnh theo trục x và y
* Sử dụng ma trận affine transformation để thực hiện dịch chuyển
* Xử lý vùng trống sau khi dịch chuyển

**PIL/Pillow:**
* Sử dụng phương thức `transform()` với ma trận affine

**OpenCV:**
* Sử dụng `cv2.warpAffine()` với ma trận translation 2×3

#### c. Rotation (Xoay)
* Xoay ảnh theo góc xác định (độ hoặc radian)
* Xoay quanh tâm ảnh hoặc điểm tùy chỉnh
* Xử lý việc mất thông tin sau khi xoay

**PIL/Pillow:**
* Sử dụng phương thức `rotate()` với tham số góc xoay
* Hỗ trợ expand để không cắt ảnh

**OpenCV:**
* Sử dụng `cv2.getRotationMatrix2D()` để tạo ma trận xoay
* Áp dụng với `cv2.warpAffine()`

---

### 2. Phép toán toán học (Mathematical Operations)

#### a. Array Operations (Phép toán mảng)
* Cộng, trừ, nhân, chia pixel-wise
* Thay đổi độ sáng và độ tương phản
* Kết hợp nhiều ảnh với nhau (blending)
* Các phép toán logic: AND, OR, XOR, NOT

**Ứng dụng:**
* Tăng/giảm độ sáng: thêm/trừ giá trị constant
* Tăng độ tương phản: nhân với hệ số > 1
* Kết hợp ảnh: weighted sum của hai ảnh

#### b. Matrix Operations (Phép toán ma trận)
* **Singular Value Decomposition (SVD)**
* Phân tích ma trận ảnh thành tích của 3 ma trận: U, Σ, V^T
* Nén ảnh bằng cách giữ lại k singular values lớn nhất

**Quy trình SVD:**
1. Chuyển ảnh sang ma trận (grayscale)
2. Áp dụng SVD: A = U × Σ × V^T
3. Chọn k thành phần chính (singular values)
4. Tái tạo ảnh: A_approx = U × Σ_k × V^T

**Kết quả:**
* Với 1-10 components: ảnh rất mờ, khó nhận dạng
* Với 100 components: ảnh đã khá rõ nét
* Với 200 components: ảnh gần như giống gốc
* Với 500 components: ảnh tương đương ảnh gốc

**Lợi ích SVD:**
* Giảm dung lượng lưu trữ đáng kể
* Giữ được đặc trưng quan trọng của ảnh
* Ứng dụng trong nén ảnh và khử nhiễu

---

### 3. So sánh PIL và OpenCV

| Tiêu chí | PIL/Pillow | OpenCV |
|----------|------------|---------|
| **Định dạng ảnh** | RGB (Red, Green, Blue) | BGR (Blue, Green, Red) |
| **Cấu trúc dữ liệu** | PIL Image object | NumPy array |
| **Dễ sử dụng** | Đơn giản, trực quan | Mạnh mẽ, phức tạp hơn |
| **Hiệu năng** | Chậm hơn với ảnh lớn | Nhanh hơn, tối ưu |
| **Chức năng** | Cơ bản | Toàn diện, chuyên sâu |
| **Ứng dụng** | Xử lý ảnh đơn giản | Computer Vision, Video |

**Lưu ý quan trọng:**
* Khi sử dụng OpenCV cần chuyển đổi BGR → RGB để hiển thị đúng màu
* PIL phù hợp cho xử lý ảnh cơ bản và web
* OpenCV phù hợp cho xử lý ảnh nâng cao và real-time

---

### 4. Bài làm Lab 02 - Xử lý ảnh và Filters

Notebook này thực hành về các bộ lọc (filters) trong xử lý ảnh:

#### Các loại filter được thực hiện:
* **Averaging Filter**: Làm mờ ảnh bằng trung bình cộng
* **Gaussian Filter**: Làm mờ ảnh với Gaussian kernel (3×3, 5×5, 9×9)
* **Median Filter**: Khử nhiễu salt-and-pepper
* **Bilateral Filter**: Làm mờ nhưng giữ được cạnh
* **Filter 2D**: Áp dụng kernel tùy chỉnh
* **Sharpening Filter**: Làm sắc nét ảnh
* **Phép toán số học**: Cộng, trừ, nhân, chia ảnh

#### Các kỹ thuật khác:
* Crop ảnh về kích thước cố định
* Bitwise operations (AND, OR, XOR, NOT)
* Image blending với trọng số
* So sánh kết quả các bộ lọc khác nhau

---

## Kết luận

Qua Lab 4:

* Nắm vững các phép biến đổi hình học cơ bản: scaling, translation, rotation
* Hiểu cách thức hoạt động của ma trận affine transformation
* Biết cách áp dụng các phép toán mảng và ma trận lên ảnh
* Hiểu nguyên lý và ứng dụng của SVD trong nén ảnh
* Phân biệt được ưu nhược điểm của PIL và OpenCV
* Thực hành với các bộ lọc xử lý ảnh thực tế
* Áp dụng các phép toán logic và số học trên ảnh

**Ứng dụng thực tế:**
* Image augmentation cho Machine Learning
* Chỉnh sửa và xử lý ảnh
* Nén và lưu trữ ảnh hiệu quả
* Computer Vision và Object Detection
* Khử nhiễu và cải thiện chất lượng ảnh

---

## Công cụ sử dụng

* Python 3.11.5
* PIL/Pillow - Xử lý ảnh cơ bản
* OpenCV (cv2) - Computer Vision
* NumPy - Xử lý mảng và ma trận
* Matplotlib - Trực quan hóa
