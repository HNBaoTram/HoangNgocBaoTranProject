# Hệ thống thương mại điện tử - Khai phá dữ liệu lớn kinh tế

Dự án này tập trung vào việc áp dụng các kỹ thuật Khai phá Dữ liệu Lớn (Big Data Mining) để phân tích hành vi mua sắm trên hệ thống thương mại điện tử.

## Mục tiêu kinh doanh (Business Objectives)

Mục đích cốt lõi của dự án là tăng giá trị đơn hàng trung bình (AOV - Average Order Value) bằng cách xây dựng hệ thống gợi ý sản phẩm phù hợp thông qua các chiến lược Cross-sell và Up-sell. Kết quả của mô hình giúp doanh nghiệp tối ưu hóa chiến lược định giá chéo và biến dữ liệu lớn thành lợi nhuận kinh tế thực tế.

## Hướng dẫn chuẩn bị dữ liệu (Data Setup)

Dự án này sử dụng bộ dữ liệu **Olist Brazilian E-Commerce Dataset** (dữ liệu lịch sử giao dịch thương mại điện tử thực tế). Vì tập dữ liệu chứa hàng trăm ngàn bản ghi đơn hàng mang dung lượng khá lớn, các tệp dữ liệu thô không được đính kèm trực tiếp vào mã nguồn để tối ưu hóa không gian lưu trữ.

Để chuẩn bị nguyên liệu và khởi chạy mô hình, bạn vui lòng thực hiện theo các bước sau:

**1. Chuẩn bị các tệp dữ liệu thô (Raw Data)**
Đảm bảo bạn đã tải xuống và thu thập đủ **9 tệp tin `.csv`** cốt lõi của bộ dữ liệu Olist, bao gồm:
1. `olist_orders_dataset.csv`
2. `olist_order_items_dataset.csv`
3. `olist_products_dataset.csv`
4. `product_category_name_translation.csv`
5. `olist_order_reviews_dataset.csv`
6. `olist_order_payments_dataset.csv`
7. `olist_customers_dataset.csv`
8. `olist_sellers_dataset.csv`
9. `olist_geolocation_dataset.csv`

**2. Thiết lập không gian làm việc (Workspace Setup)**
* **Nếu chạy trên Google Colab (Khuyến nghị cho PySpark):** Tải toàn bộ 9 tệp CSV trên lên một thư mục trên Google Drive cá nhân của bạn. Khi mở file Notebook (`Preprocessing_Dashboard.ipynb`), hãy cấp quyền `drive.mount` để hệ thống tự động ánh xạ (mount) và đọc dữ liệu trực tiếp từ Drive vào môi trường Colab.
* **Nếu chạy trên máy tính cá nhân (Local Localhost):** Tạo một thư mục con tên là `data/` nằm ngay bên trong thư mục chứa mã nguồn (`.ipynb`). Di chuyển tất cả 9 tệp CSV vào thư mục này trước khi chạy code.

**3. Khởi chạy luồng Tiền xử lý (Data Preprocessing)**
Trước khi chạy mô hình Học máy ở Chương 4, bạn **bắt buộc** phải mở file `Preprocessing_Dashboard.ipynb` và chạy tuần tự các ô mã lệnh. 
Quá trình này sẽ thực hiện làm sạch dữ liệu (Data Cleaning), nối các bảng quan hệ và kết xuất ra một tệp dữ liệu tổng hợp (ví dụ: `master_df_cleaned.csv` hoặc `master_df_final_engineered`). Đây chính là nguồn dữ liệu chuẩn bị sẵn sàng (ML-ready data) để đưa vào thuật toán khai phá luật kết hợp FP-Growth.

## Quy trình thực thi (Execution Process)

Dự án được chia thành 5 bước triển khai kỹ thuật chuyên sâu:

1. Kỹ thuật đặc trưng và tăng cường dữ liệu (Feature Engineering & Enrichment)
* Phân khúc giá tự động: Sử dụng PySpark và hàm approxQuantile để tính toán mốc phân vị, tự động dán nhãn sản phẩm thành các nhóm: Budget (Giá rẻ), Mid-range (Tầm trung) và Premium (Cao cấp).
* Tạo định danh phức hợp: Ghép nối tên danh mục với phân khúc giá (ví dụ: bed_bath_table_Premium) để khai phá các hành vi ẩn sâu sắc hơn thay vì chỉ dùng thông tin phẳng.
* Chuẩn bị giỏ hàng: Gom nhóm dữ liệu giao dịch thành các mảng giỏ hàng (Nested Arrays) và loại bỏ các đơn hàng chỉ có 1 sản phẩm.
* Tối ưu lưu trữ: Xuất dữ liệu sẵn sàng cho Machine Learning ra định dạng Parquet để tối ưu hóa RAM (lưu trữ dạng cột) và bảo toàn cấu trúc dữ liệu mảng.

2. Huấn luyện và đối chứng thuật toán (Model Training & Benchmarking)
* Thực hiện A/B Testing giữa thuật toán tối ưu cho Big Data (FP-Growth trên PySpark) và thuật toán truyền thống (Apriori trên Python/mlxtend).
* So sánh hiệu năng thực tế về thời gian thực thi và lượng RAM tiêu thụ, qua đó chứng minh ưu thế vượt trội về tốc độ của FP-Growth khi mở rộng trên Spark, dù cả hai cho ra số lượng luật kết hợp giống hệt nhau.

3. Đánh giá và chọn lọc luật (Rule Evaluation)
* Sử dụng chỉ số Lift > 1 làm thước đo để loại bỏ các quy luật ngẫu nhiên, chỉ giữ lại "Luật kết hợp tinh hoa" (Golden Rules) có sức mạnh cộng hưởng thực sự.
* Việc áp dụng phân khúc giá giúp giảm số lượng luật nhiễu nhưng tăng chất lượng (Lift) lên gấp gần 3 lần so với mô hình không phân khúc.

4. Trích xuất tri thức kinh doanh (Business Insights)
* Trực quan hóa dữ liệu bằng Biểu đồ phân tán (Scatter Plot) giữa Support và Confidence.
* Sử dụng Biểu đồ mạng lưới (Network Graph) có hướng để phân tích luồng mua chéo.
* Insight cốt lõi: Hành vi mua chéo thực sự mạnh mẽ (Lift lên tới 14.84) chỉ xuất hiện trong phân khúc Premium (mua Premium kèm Premium). Khách hàng Budget rất ít có hành vi mua chéo mang tính liên kết.

5. Tác động kinh tế (Economic Impact)
* Xây dựng mô phỏng tính toán doanh thu tăng thêm dựa trên Tiền đề (Antecedent-based Calculation).
* Đánh giá định lượng tác động của hệ thống gợi ý lên tổng doanh thu và chỉ số AOV với một tỷ lệ chuyển đổi giả định (ví dụ: 5%).

## Hướng dẫn sử dụng mô hình

1. Hoàn thành các bước trong phần Hướng dẫn chuẩn bị dữ liệu (Data Setup) ở trên.
2. Mở file Notebook trên Google Colab hoặc môi trường Jupyter cục bộ có hỗ trợ PySpark.
3. Chạy từng Cell theo trình tự từ file `Model.ipynb`: Khởi tạo Spark -> Xử lý dữ liệu Bước 1 -> Huấn luyện A/B Test Bước 2 -> Lọc luật Bước 3 -> Trực quan hóa và Mô phỏng Bước 4 & 5.
4. Kiểm tra các thư mục Output (như /content/chapter4_step2_fp_growth/) để xem các tệp .csv chứa bộ Golden Rules đã được xuất ra.
