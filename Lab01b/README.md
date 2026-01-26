# LAB01: LÀM QUEN VỚI ẢNH SỐ TRONG COMPUTER VISION

## Mô tả bài lab

Lab này giúp làm quen với xử lý ảnh số cơ bản trong Computer Vision sử dụng OpenCV và Python.

## Nội dung thực hành

### 1. Cài đặt và import thư viện
- Cài đặt opencv-python
- Import các thư viện cần thiết (cv2, matplotlib)

### 2. Kiểm tra thông tin ảnh
- Đọc ảnh từ file
- Hiển thị các thông tin: shape, chiều cao, chiều rộng, số kênh màu
- Kiểm tra kiểu dữ liệu và kích thước

### 3. Hiển thị ảnh và xử lý màu sắc
- Hiển thị ảnh gốc (BGR)
- **Cách 1:** Đảo chiều matrix để chuyển BGR sang RGB
- **Cách 2:** Sử dụng cv.cvtColor() để convert màu
- **Nhận xét:** OpenCV đọc ảnh theo BGR, Matplotlib hiển thị theo RGB

### 4. Phóng to/Thu nhỏ ảnh
- Sử dụng `cv.resize()` để thay đổi kích thước
- Phóng to gấp 2 lần (scale_factor = 2)
- Thu nhỏ còn 0.5 lần
- Các phương pháp interpolation: INTER_LINEAR, INTER_AREA

### 5. Crop (Cắt) ảnh
- Crop ảnh ở giữa (center crop)
- Crop góc trên trái
- Sử dụng slicing của numpy: `img[start_row:end_row, start_col:end_col]`

### 6. Ghép ảnh
- **Ghép ngang (Horizontal):** Sử dụng `np.hstack()`
- **Ghép dọc (Vertical):** Sử dụng `np.vstack()`
- Resize ảnh về cùng kích thước trước khi ghép

## Yêu cầu kỹ thuật

### Môi trường thực hành
- **Platform:** Google Colab
- **Python version:** 3.x
- **Thư viện chính:**
  - opencv-python
  - matplotlib
  - numpy

### Cài đặt thư viện
```python
!pip install opencv-python
```

## Các hàm chính được sử dụng

| Hàm | Mô tả |
|-----|-------|
| `cv.imread()` | Đọc ảnh từ file |
| `cv.cvtColor()` | Chuyển đổi không gian màu |
| `cv.resize()` | Thay đổi kích thước ảnh |
| `plt.imshow()` | Hiển thị ảnh |
| `plt.figure()` | Tạo figure mới |
| `plt.subplot()` | Tạo subplot |
| `np.hstack()` | Ghép ảnh ngang |
| `np.vstack()` | Ghép ảnh dọc |

