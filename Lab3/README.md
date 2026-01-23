# Báo cáo Lab 3: Histogram, Biến đổi cường độ và Phân tích dữ liệu Uber

## Tổng quan

Bài lab này tập trung vào:

* Phân tích phân phối dữ liệu bằng histogram
* Thực hiện các phép biến đổi cường độ
* Phân tích dữ liệu Uber dựa trên **thời gian** và **vị trí địa lý**

---

## Cấu trúc file

* **2.3.2_Histograms_and_Intensity_Transformations.ipynb**
  Khảo sát histogram và các phép biến đổi cường độ dữ liệu/ảnh

* **Consumption.ipynb**
  Phân tích dữ liệu Uber theo **thời gian** và **không gian (tọa độ, bản đồ)**

---

## Nội dung thực hiện

### 1. 2.3.2_Histograms_and_Intensity_Transformations.ipynb

Notebook này tập trung vào việc **khảo sát phân phối dữ liệu thông qua histogram**.

Các nội dung chính:

* Vẽ histogram biểu diễn phân phối cường độ
* Phân tích độ tập trung và độ phân tán của dữ liệu
* Nhận diện dạng phân phối (lệch trái, lệch phải)
* Thực hiện các phép biến đổi cường độ
* So sánh dữ liệu trước và sau biến đổi bằng biểu đồ

Histogram giúp hiểu rõ cấu trúc dữ liệu và hỗ trợ tiền xử lý cho các bài toán phân tích và học máy.

---

### 2. Consumption.ipynb

Notebook này sử dụng **dữ liệu Uber** để phân tích hành vi di chuyển theo **thời gian** và **không gian địa lý**.

Các nội dung chính:

* Xử lý dữ liệu thời gian (datetime)
* Phân tích số chuyến Uber theo giờ trong ngày
* So sánh xu hướng giữa ngày thường và cuối tuần
* Phân tích khoảng cách từ điểm đón/trả đến các địa điểm quan tâm
* Trực quan hóa dữ liệu bằng:

  * Biểu đồ đường (line chart)
  * Bản đồ với **Folium**
  * Heatmap và Heatmap theo thời gian
* Kiểm định thống kê (t-test) để đánh giá sự khác biệt hành vi theo giờ

Notebook giúp làm rõ tính **chu kỳ theo thời gian** và **phân bố không gian** của các chuyến Uber.

---

## Kết luận

Qua Lab 3:

* Hiểu cách sử dụng histogram để phân tích phân phối dữ liệu
* Nắm được vai trò của các phép biến đổi cường độ
* Biết cách phân tích dữ liệu theo thời gian và vị trí địa lý
* Làm quen với trực quan hóa bản đồ và heatmap
* Áp dụng kiểm định thống kê trong phân tích dữ liệu thực tế

---

## Công cụ sử dụng

* Python 3.11.5
* NumPy
* Matplotlib
* Open CV
* SciPy
* Folium
* Jupyter Notebook

