# Phân tích và dự đoán rủi ro tài chính cá nhân

Dự án này tập trung vào việc xây dựng mô hình học máy (Machine Learning) nhằm phân loại và dự đoán mức độ rủi ro tài chính của các cá nhân dựa trên dữ liệu nhân khẩu học và hồ sơ tài chính.

## Mục tiêu nghiên cứu (Research Objectives)

Kết quả của mô hình được thiết kế để giải quyết các bài toán cốt lõi trong đánh giá tín dụng và quản trị rủi ro:
* **Phân loại rủi ro chính xác:** Phát triển mô hình dự đoán xếp hạng rủi ro (Risk Rating) thành 3 nhóm (High, Medium, Low).
* **Xử lý dữ liệu mất cân bằng:** Áp dụng kỹ thuật `SMOTE` để giải quyết sự chênh lệch lớn giữa nhóm đa số (Low - 60%) và nhóm thiểu số (High - 10%, Medium - 30%), qua đó cải thiện các chỉ số Recall và Precision.
* **Tối ưu hóa và đánh giá mô hình:** Tập trung vào các chỉ số như F1-Score macro/weighted để đảm bảo mô hình hoạt động hiệu quả trên tất cả các lớp.
* **Xác định đặc trưng quan trọng (Feature Importance):** Nhận diện các yếu tố ảnh hưởng mạnh nhất đến rủi ro tài chính cao (High Risk) để hỗ trợ quá trình ra quyết định.

## Cấu trúc dữ liệu

Bộ dữ liệu `financial_risk_assessment.csv` (nguồn Kaggle) cung cấp thông tin chi tiết với 15.000 bản ghi và 20 trường dữ liệu (cột), bao gồm:
* **Thông tin nhân khẩu học:** `Age`, `Gender`, `Education Level`, `Marital Status`, v.v.
* **Hồ sơ tài chính & Tín dụng:** `Income`, `Credit Score`, `Loan Amount`, `Debt-to-Income Ratio`, `Assets Value`, `Previous Defaults`, v.v.
* **Biến mục tiêu (Target Variable):** `Risk Rating` (Low, Medium, High).

## Quy trình thực thi

### 1. Làm sạch & Tiền xử lý dữ liệu (Preprocessing)
* **Xử lý dữ liệu khuyết thiếu (Missing Values):** Định dạng lại các giá trị không hợp lệ (ví dụ: chuyển đổi "Non-binary" thành `NaN`) và sử dụng các thuật toán điền khuyết như `KNNImputer`.
* **Mã hóa đặc trưng (Encoding):** Sử dụng `LabelEncoder` để biến đổi các biến phân loại.
* **Chuẩn hóa dữ liệu (Scaling):** Đưa các đặc trưng số học về cùng một thang đo bằng `StandardScaler`.

### 2. Xử lý mất cân bằng lớp (Handling Class Imbalance)
* Nhận thấy sự mất cân bằng nghiêm trọng của tập dữ liệu, dự án áp dụng thuật toán **SMOTE (Synthetic Minority Over-sampling Technique)** để sinh thêm dữ liệu tổng hợp cho nhóm thiểu số. Việc này giúp mô hình học tốt hơn các đặc điểm của lớp High Risk thay vì bị thiên lệch về nhóm Low Risk.

### 3. Mô hình dự đoán (Modeling)
* Xây dựng và đánh giá các mô hình phân loại học máy.
* Phân tích điểm hạn chế của hệ thống đối với các nhóm thiểu số (sự nhầm lẫn giữa Medium và Low) và đề xuất các hướng cải tiến chuyên sâu, bao gồm tạo ra các đặc trưng mới (`Feature Engineering`) từ phân tích chuyên sâu và thử nghiệm các kiến trúc Ensemble phức tạp như Stacking.

## Hướng dẫn sử dụng

1. Khởi tạo môi trường lập trình và cài đặt các thư viện Data Science cơ bản (`pandas`, `numpy`, `scikit-learn`, `seaborn`, `matplotlib`).
2. Tải và đặt file `financial_risk_assessment.csv` vào cùng thư mục chứa mã nguồn.
3. Mở file `HoangNgocBaoTran_FinancialRisk.ipynb` trên Jupyter Notebook hoặc Google Colab.
4. Chạy tuần tự các khối lệnh (Run All) để trải nghiệm toàn bộ quy trình: từ Exploratory Data Analysis (EDA), tiền xử lý, chạy SMOTE, đến khâu huấn luyện và xuất báo cáo đánh giá mô hình.
