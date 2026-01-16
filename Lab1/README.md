# Báo cáo Lab: Xử lý Ảnh với Pillow và OpenCV

## Tổng quan

Bài lab thực hành các kỹ thuật xử lý hình ảnh cơ bản và nâng cao sử dụng thư viện Pillow (PIL) và OpenCV. Nội dung tập trung vào việc hiểu cấu trúc dữ liệu ảnh dưới dạng ma trận và áp dụng các phép biến đổi hình thái học.

## Cấu trúc file

- `2.1.1_Images_with_python_library_PIL.ipynb` - Thực hành với thư viện Pillow
- `2.1.2_Images_with_python_library_CV.ipynb` - Thực hành với OpenCV

## Nội dung thực hiện

Các notebook đã được thực hiện theo quy trình:

**Chạy tuần tự các cell** để quan sát biến đổi hình ảnh qua từng bước xử lý.

**Chú thích code chi tiết** giải thích chức năng hàm, ý nghĩa tham số và cách dữ liệu thay đổi. Ví dụ: `cv2.cvtColor()` dùng để chuyển đổi không gian màu giữa BGR và RGB.

**Phân tích ma trận** bằng Numpy để thao tác trực tiếp với các kênh màu, hiểu rõ cấu trúc mảng 3 chiều (height × width × channels) của ảnh số.

## So sánh Pillow vs OpenCV

| Tiêu chí | Pillow (PIL) | OpenCV |
|----------|--------------|---------|
| **Mục đích** | Chỉnh sửa ảnh thông thường | Thị giác máy tính, xử lý real-time |
| **Cú pháp** | Đơn giản, Pythonic | Phức tạp, nhiều tính năng |
| **Kiểu dữ liệu** | Đối tượng Image | Mảng Numpy ndarray |
| **Hiệu suất** | Trung bình | Cao, tối ưu cho phép toán ma trận |
| **Hệ màu** | RGB | BGR (cần convert khi hiển thị) |
| **Ứng dụng** | Input cho Deep Learning | Computer Vision, video processing |

## Kết luận

Lab đã giúp nắm vững cách xử lý kênh màu, hiểu sự khác biệt về định dạng dữ liệu giữa các thư viện. Pillow phù hợp cho các tác vụ đơn giản (resize, crop, rotate), còn OpenCV là lựa chọn tối ưu cho ứng dụng Computer Vision phức tạp và xử lý video real-time.

---

**Công cụ:** Python 3.11.9, Pillow, OpenCV, Numpy, Matplotlib, Visual Studio Code