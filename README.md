# 🏠 Dự đoán Giá Nhà (House Pricing Prediction)

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-1582BA?style=for-the-badge&logo=xgboost&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=white)

## 1. Vấn đề giải quyết
Định giá nhà chính xác là thách thức lớn đối với thị trường bất động sản. Dự án giải quyết bài toán hồi quy: **"Dự đoán giá bán của một căn nhà dựa trên các đặc trưng diện tích, vị trí và tiện nghi"**.

## 2. Dữ liệu đầu vào
Dữ liệu từ Kaggle (`Housing.csv`) gồm **545 mẫu** và 14 cột đặc tính (ví dụ: `price`, `area`, `bedrooms`, `bathrooms`, `stories`, `parking`, `airconditioning`...).


## 3. Phân tích khám phá dữ liệu (EDA)
Trước khi huấn luyện mô hình, dữ liệu được phân tích sâu để hiểu rõ bản chất:
*   **Phân phối biến mục tiêu:** Giá nhà (`price`) có phân phối **lệch phải**, tập trung ở phân khúc thấp và trung bình, với nhiều giá trị ngoại lệ (outliers) ở phân khúc cao cấp.
*   **Ma trận tương quan:** Các biến ảnh hưởng mạnh nhất đến `price` là `area` (r=0.54), `bathrooms` (r=0.52), `airconditioning` (r=0.45) và `stories` (r=0.42).
*   **Mối quan hệ biến số:** Biểu đồ phân tán và biểu đồ hộp xác nhận mối quan hệ thuận giữa diện tích (`area`) và giá (`price`), đồng thời cho thấy các biến rời rạc tạo ra các "dải" phân bố giá trị đặc trưng.
*   **Xử lý biến phân loại:** Áp dụng **One-Hot Encoding** (với `drop_first=True` để tránh đa cộng tuyến) cho các biến dạng `object`.

## 4. Cách giải quyết vấn đề
*   **Tiền xử lý:** Làm sạch, mã hóa biến phân loại và chuẩn hóa dữ liệu (`StandardScaler`).
*   **Mô hình hóa:** 
    *   **Linear Regression:** Mô hình cơ sở[cite: 3].
    *   **Random Forest & XGBoost:** Tối ưu hóa siêu tham số qua **GridSearchCV**.
    *   **Mạng Nơ-ron (MLP):** Kiến trúc 2 tầng ẩn (64 và 128 nơ-ron), tối ưu hóa bằng thuật toán Adam.

## 5. Outcome & Kết quả đánh giá
| Phương pháp | RMSE | R-squared | MAE |
| :--- | :--- | :--- | :--- |
| **Linear Regression** | **1,324,506** | **0.653** | 970,043 |
| **Random Forest** | 1,400,619 | 0.612 | 1,010,525 |
| **XGBoost** | 1,352,015 | 0.640 | 976,624 |
| **Neural Network (MLP)**| 1,347,227 | 0.640 | **956,810** |

*Bảng kết quả đánh giá hiệu suất các mô hình*

## 6. Recommendation & Insight
*   **Mô hình hiệu quả:** **Linear Regression** đạt kết quả tổng thể tốt nhất do dữ liệu đầu vào mang tính tuyến tính cao.
*   **Lựa chọn chuyên biệt:** **MLP** là giải pháp xuất sắc nếu hệ thống triển khai thực tế ưu tiên việc tối ưu sai số tuyệt đối (MAE) cho các căn nhà phổ thông.
*   **Kết luận:** Phân tích kỹ các biến có tương quan mạnh (`area`, `bathrooms`) giúp định hình tốt cấu trúc mô hình, tránh nhiễu từ các biến có tương quan yếu (như `hotwaterheating`, `basement`).
