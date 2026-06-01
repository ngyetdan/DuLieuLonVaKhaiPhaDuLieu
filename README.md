# 🛒 Superstore Sales Analytics

Dự án phân tích dữ liệu bán lẻ toàn diện trên bộ dữ liệu **Superstore**, bao gồm các kỹ thuật khai phá dữ liệu, học máy và phân tích OLAP.

---

## 📁 Cấu trúc dự án

```
├── train.csv                    # Dữ liệu gốc
├── clean_data.py                # Bước 1: Tiền xử lý dữ liệu
├── association_rules.py         # Bước 2: Khai phá luật kết hợp
├── customer_clustering.py       # Bước 3: Phân cụm khách hàng (RFM)
├── customer_classification.py   # Bước 4: Phân lớp khách hàng
├── sales_forecast.py            # Bước 5: Dự báo doanh số
└── olap.py                      # Bước 6: Phân tích OLAP
```

---

## 🚀 Hướng dẫn chạy

> Chạy theo đúng thứ tự vì các bước sau phụ thuộc vào output của bước trước.

```bash
python clean_data.py
python association_rules.py
python customer_clustering.py
python customer_classification.py
python sales_forecast.py
python olap.py
```

---

## 📦 Yêu cầu thư viện

```bash
pip install pandas numpy scikit-learn mlxtend
```

---

## 🔍 Mô tả từng module

### 1. `clean_data.py` — Tiền xử lý dữ liệu
- Xử lý missing values (Median cho số, Mode cho chữ)
- Xử lý outlier bằng phương pháp **Capping** (giữ lại dữ liệu, không xóa)
- Chuẩn hóa định dạng ngày tháng
- Tự động tạo cột `Profit` nếu thiếu
- **Output:** `clean_superstore.csv`

---

### 2. `association_rules.py` — Khai phá luật kết hợp
- Xây dựng **Market Basket** theo `Sub-Category`
- Áp dụng thuật toán **Apriori** (`min_support = 0.005`)
- Sinh luật kết hợp theo độ đo **Lift**
- **Output:** `association_rules_result.csv`

> Ví dụ: *Khách mua [Chairs] → Thường mua thêm [Tables] (Lift: 2.5)*

---

### 3. `customer_clustering.py` — Phân cụm khách hàng (RFM)
- Tính chỉ số **RFM**: Recency, Frequency, Monetary
- Chuẩn hóa dữ liệu với `StandardScaler`
- Phân cụm bằng **K-Means** (4 nhóm)
- Gợi ý đặt tên nhóm: VIP, Trung thành, Tiềm năng, Rời bỏ
- **Output:** `customer_clusters_result.csv`

---

### 4. `customer_classification.py` — Phân lớp khách hàng
- Dự đoán **Segment** khách hàng: `Consumer / Corporate / Home Office`
- Feature engineering: mã hóa Region, Category, Ship Mode
- Mô hình: **Decision Tree** (`max_depth=5`)
- Đánh giá bằng Accuracy & Classification Report

---

### 5. `sales_forecast.py` — Dự báo doanh số
- Tổng hợp doanh số theo tháng
- Dự báo **6 tháng tới** bằng **Linear Regression**
- Nhận xét xu hướng tăng/giảm doanh thu

---

### 6. `olap.py` — Phân tích OLAP
| Thao tác | Mô tả |
|----------|-------|
| **Original Cube** | Doanh số theo Năm × Vùng |
| **Drill-down** | Phân tích sâu xuống cấp Bang (State) |
| **Roll-up** | Tổng hợp chỉ theo Năm |
| **Slice** | Cắt lát dữ liệu theo năm cụ thể |

- **Output:** `olap_drilldown.csv`

---

## 📊 Dữ liệu đầu vào

File `train.csv` cần có các cột sau:

| Cột | Mô tả |
|-----|-------|
| `Order ID` | Mã đơn hàng |
| `Order Date` | Ngày đặt hàng |
| `Customer ID` | Mã khách hàng |
| `Segment` | Phân khúc khách hàng |
| `Region` | Vùng địa lý |
| `State` | Bang |
| `Category` | Danh mục sản phẩm |
| `Sub-Category` | Danh mục con |
| `Sales` | Doanh số |
| `Quantity` | Số lượng |
| `Ship Mode` | Phương thức vận chuyển |
| `Profit` | Lợi nhuận *(tự động tạo nếu thiếu)* |
