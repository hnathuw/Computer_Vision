# Báo cáo Lab: Xử lý Ảnh Cơ Bản với PIL và OpenCV

## Tổng quan
Bài lab này nhằm giúp làm quen với **xử lý ảnh cơ bản trong Python** thông qua hai thư viện phổ biến là **Pillow (PIL)** và **OpenCV**.  
Nội dung tập trung vào việc:
- Đọc và hiển thị ảnh
- Hiểu cấu trúc ảnh số dưới dạng ma trận
- Thao tác với các kênh màu
- Quan sát sự khác biệt giữa PIL và OpenCV trong xử lý ảnh

---

## Cấu trúc file
- **2.2.1_basic_image_manipulation_PIL.ipynb**  
  Thực hành xử lý ảnh cơ bản với thư viện Pillow (PIL)

- **2.2.2_basic_image_manipulation_open_CV.ipynb**  
  Thực hành xử lý ảnh cơ bản với thư viện OpenCV

---

## Nội dung thực hiện

### Notebook sử dụng PIL
Trong file `2.2.1_basic_image_manipulation_PIL.ipynb`, các nội dung chính bao gồm:
- Mở và hiển thị ảnh bằng Pillow
- Truy cập và thao tác pixel của ảnh
- Thực hiện một số thao tác xử lý ảnh cơ bản
- Quan sát ảnh sau khi xử lý thông qua Matplotlib

Ảnh được làm việc dưới dạng đối tượng `Image`, giúp việc thao tác trở nên đơn giản và trực quan.

---

### Notebook sử dụng OpenCV
Trong file `2.2.2_basic_image_manipulation_open_CV.ipynb`, các nội dung chính bao gồm:
- Đọc ảnh bằng OpenCV (`cv2.imread`)
- Hiển thị ảnh kết hợp OpenCV và Matplotlib
- Làm việc với các kênh màu của ảnh
- Chuyển đổi không gian màu từ BGR sang RGB bằng `cv2.cvtColor()`

Ảnh trong OpenCV được lưu dưới dạng mảng NumPy 3 chiều *(height × width × channels)*, giúp dễ dàng thao tác trực tiếp trên từng kênh màu.

---

## So sánh PIL và OpenCV

| Tiêu chí | Pillow (PIL) | OpenCV |
|--------|-------------|--------|
| Mục đích | Xử lý ảnh cơ bản, chỉnh sửa đơn giản | Xử lý ảnh và Computer Vision |
| Kiểu dữ liệu | Đối tượng `Image` | Mảng `numpy.ndarray` |
| Cú pháp | Đơn giản, dễ hiểu | Nhiều hàm, nhiều tham số |
| Hệ màu | RGB | BGR |
| Mức độ phù hợp | Học nhập môn xử lý ảnh | Nền tảng cho Computer Vision |

---

## Kết luận
Qua bài lab:
- Hiểu cách ảnh số được biểu diễn và xử lý trong Python
- Nhận biết sự khác nhau giữa PIL và OpenCV
- Nắm được cách thao tác với kênh màu và hiển thị ảnh đúng không gian màu

PIL phù hợp cho các bài tập xử lý ảnh cơ bản và học tập ban đầu, trong khi OpenCV là công cụ mạnh hơn cho các bài toán xử lý ảnh và thị giác máy tính ở mức nâng cao.

---

## Công cụ sử dụng
- Python 3.11.9  
- Pillow (PIL)  
- OpenCV  
- NumPy  
- Matplotlib  
- Visual Studio Code
