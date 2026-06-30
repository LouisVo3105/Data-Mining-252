# 🏠 Dự đoán Giá Nhà (House Pricing Prediction)

Mục tiêu của dự án là xây dựng và đánh giá các mô hình học máy để dự đoán giá nhà dựa trên các đặc tính bất động sản.

## 1. Vấn đề giải quyết
Định giá nhà chính xác là thách thức lớn đối với thị trường bất động sản. Dự án giải quyết bài toán hồi quy: **"Dự đoán giá bán của một căn nhà dựa trên các đặc trưng diện tích, vị trí và tiện nghi"**.

## 2. Dữ liệu đầu vào
Dữ liệu từ Kaggle (`Housing.csv`) gồm **545 mẫu** và 14 cột đặc tính (ví dụ: `price`, `area`, `bedrooms`, `bathrooms`, `stories`, `parking`, `airconditioning`,...).

## 3. Phân tích khám phá dữ liệu (EDA)
Trước khi huấn luyện mô hình, nhóm đã thực hiện phân tích sâu để hiểu bản chất dữ liệu:
*   **Phân phối biến mục tiêu:** Giá nhà (`price`) có phân phối **lệch phải**, tập trung ở phân khúc 3-6 triệu USD, với nhiều giá trị ngoại lệ (outliers) ở phân khúc cao cấp.
*   **Ma trận tương quan:** Xác định các biến ảnh hưởng mạnh nhất đến `price` là `area` (r=0.54), `bathrooms` (r=0.52), `stories` (r=0.45) và `airconditioning` (r=0.42).
*   **Mối quan hệ biến số:** Biểu đồ cặp (Pairplots) xác nhận mối quan hệ thuận giữa diện tích (`area`) và giá (`price`), đồng thời cho thấy các biến rời rạc tạo ra các "dải" phân bố giá trị đặc trưng trong tập dữ liệu.
*   **Xử lý biến phân loại:** Nhóm đã áp dụng **One-Hot Encoding** (với `drop_first=True` để tránh đa cộng tuyến) cho các biến dạng `object` (như `mainroad`, `furnishingstatus`).

## 4. Cách giải quyết vấn đề
*   **Tiền xử lý:** Làm sạch, mã hóa biến phân loại và chuẩn hóa dữ liệu (`StandardScaler`).
*   **Mô hình hóa:** 
    *   **Linear Regression:** Mô hình cơ sở.
    *   **Random Forest & XGBoost:** Tối ưu hóa siêu tham số qua **GridSearchCV**.
    *   **Mạng Nơ-ron (MLP):** Kiến trúc 2 tầng ẩn (64-128 nơ-ron), tối ưu hóa bằng Adam.

## 5. Outcome & Kết quả đánh giá
| Phương pháp | RMSE | $R^2$ | MAE |
| :--- | :--- | :--- | :--- |
| **Linear Regression** | **1,324,506** | **0.653** | 970,043 |
| **Random Forest** | 1,400,619 | 0.612 | 1,010,525 |
| **XGBoost** | 1,352,015 | 0.640 | 976,624 |
| **Neural Network** | 1,347,227 | 0.640 | **956,810** |

## 6. Recommendation & Insight
*   **Mô hình hiệu quả:** **Linear Regression** đạt kết quả tốt nhất do dữ liệu mang tính tuyến tính cao.
*   **Lựa chọn chuyên biệt:** **MLP** phù hợp nếu muốn tối ưu sai số tuyệt đối (MAE) cho các căn nhà phổ thông.
*   **Kết luận:** Việc phân tích kỹ các biến có tương quan mạnh (`area`, `bathrooms`) đã giúp nhóm định hình tốt cấu trúc mô hình, tránh việc sử dụng các biến có tương quan yếu (như `hotwaterheating`, `basement`).
