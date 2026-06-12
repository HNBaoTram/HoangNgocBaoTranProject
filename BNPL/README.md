# Dự báo rủi ro tín dụng của giao dịch Mua trước trả sau (BNPL)

Dự án này tập trung vào việc tiền xử lý, phân tích khám phá dữ liệu (EDA) và xây dựng chiến lược, lộ trình để phát triển các mô hình học máy (Machine Learning) nhằm dự báo rủi ro tín dụng trong các giao dịch "Mua trước trả sau" (Buy Now Pay Later - BNPL).

## Mục tiêu nghiên cứu (Research Objectives)

Kết quả của pha phân tích này đóng vai trò nền tảng để giải quyết các bài toán đánh giá tín dụng trong bối cảnh Fintech:
* **Nhận diện rủi ro tiềm ẩn:** Trực quan hóa và phân tích sâu các hành vi dẫn đến trễ hạn thanh toán hoặc bùng nợ trong giao dịch BNPL.
* **Định hình chiến lược mô hình hóa (Model Plan):** Thay vì vội vàng huấn luyện mô hình, dự án vạch ra lộ trình bài bản bao gồm việc lựa chọn thuật toán (dự kiến XGBoost/LightGBM), chiến lược kỹ nghệ đặc trưng (Feature Engineering), và các tiêu chí đánh giá mô hình.
* **Đảm bảo tính công bằng (Fairness):** Đặt ra yêu cầu kiểm tra tính thiên vị của mô hình đối với các nhóm khách hàng theo độ tuổi, giới tính hoặc vị trí địa lý.
* **Hệ thống giám sát rủi ro:** Đề xuất cơ chế theo dõi hiệu suất mô hình liên tục (model drift) khi triển khai thực tế.

## Cấu trúc dữ liệu

Bộ dữ liệu `Buy_Now_Pay_Later_BNPL_CreditRisk_Dataset.csv` (nguồn Kaggle) cung cấp hơn 10.000 bản ghi chi tiết về các giao dịch BNPL, bao gồm các nhóm thông tin chính:
* **Đặc trưng nhân khẩu học & Thu nhập:** `age`, `employment_type`, `monthly_income`, `location`.
* **Lịch sử tín dụng & Nợ:** `credit_score`, `debt_to_income_ratio`.
* **Hành vi giao dịch BNPL:** `purchase_amount`, `product_category`, `bnpl_installments` (số kỳ trả góp), `app_usage_frequency`.
* **Hành vi thanh toán (Key Indicators):** `repayment_delay_days`, `missed_payments`, `default_flag`.
* **Biến mục tiêu (Target Variables):** `risk_score` (điểm rủi ro) và `customer_segment` (Phân khúc khách hàng: High Risk, Medium Risk, Low Risk).

## Quy trình thực thi

### 1. Tiền xử lý dữ liệu (Preprocessing)
* Làm sạch dữ liệu, xử lý các giá trị khuyết thiếu (nếu có) và định dạng lại các trường dữ liệu thời gian (`transaction_date`).
* Chuyển đổi các biến phân loại (Categorical variables) như `employment_type`, `product_category` để chuẩn bị cho bước phân tích.

### 2. Phân tích khám phá & Trực quan hóa (EDA & Visualization)
* Đánh giá sự phân phối của các nhóm rủi ro (Customer Segments) và các mức độ thu nhập.
* Phân tích mối tương quan giữa tần suất sử dụng app, độ trễ thanh toán (`repayment_delay_days`) với điểm rủi ro tín dụng (`risk_score`).
* Trực quan hóa dữ liệu để tìm ra những "red flags" (dấu hiệu cảnh báo) của các hồ sơ có khả năng vỡ nợ (default).

### 3. Lộ trình phát triển mô hình (Modeling Roadmap)
* **Xây dựng mô hình baseline:** Thử nghiệm với các thuật toán Gradient Boosting (XGBoost, LightGBM) để đánh giá khả năng phân loại hồ sơ rủi ro.
* **Bổ sung dữ liệu:** Đề xuất thu thập thêm dữ liệu thực tế và tạo ra các biến mới như khoảng thời gian từ lần thanh toán gần nhất, số lần giao dịch liên tiếp.
* **Retrain & Monitor:** Xây dựng pipeline để cập nhật và retrain mô hình khi có dữ liệu mới, ngăn ngừa hiện tượng suy giảm hiệu suất (Model Drift).

## Hướng dẫn sử dụng

1. Khởi tạo môi trường lập trình Python và cài đặt các thư viện: `pandas`, `numpy`, `matplotlib`, `seaborn`.
2. Tải file dữ liệu `Buy_Now_Pay_Later_BNPL_CreditRisk_Dataset.csv` và lưu vào cùng thư mục với notebook.
3. Mở file `Prepro_Visualize_ModelPlan.ipynb` thông qua Jupyter Notebook hoặc Google Colab.
4. Chạy từng ô lệnh (cell) để xem chi tiết quá trình tiền xử lý, các biểu đồ trực quan hóa rủi ro và phần trình bày về lộ trình xây dựng mô hình ở cuối file.
