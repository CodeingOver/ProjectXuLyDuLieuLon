# Hệ Thống Ước Lượng Dữ Liệu Dòng Lớn - Flajolet-Martin Algorithm

## 1. Tên dự án & Giới thiệu (Project Title & Introduction)
Dự án **Xử Lý Dữ Liệu Lớn (Big Data Stream Processing)** tập trung vào giải quyết bài toán đếm phần tử duy nhất (Cardinality Estimation / Count-Distinct Problem) trên luồng dữ liệu truy cập máy chủ (Server Access Logs) có kích thước lớn (hàng triệu bản ghi, nhiều Gigabyte) theo thời gian thực. Bằng cách áp dụng thuật toán xác suất **Flajolet-Martin** (cả biến thể Cơ bản và Cải tiến kết hợp kỹ thuật Median of Means), hệ thống giúp tiết kiệm hàng trăm lần bộ nhớ RAM so với cấu trúc bảng băm truyền thống (`set`) mà vẫn duy trì độ chính xác cao.

## 2. Tính năng chính (Key Features)
- **Đọc luồng dữ liệu (Data Streaming):** Đọc tuần tự từng dòng từ file log dung lượng lớn (3.5 GB+) bằng bộ tạo (generator), không nạp toàn bộ file vào RAM, tránh lỗi tràn bộ nhớ (Out of Memory).
- **Hệ thống 4 Notebook thực nghiệm chuyên biệt:**
  - `1_Set_Exact.ipynb`: Đếm chính xác $100\%$ bằng cấu trúc Python Set làm Ground Truth đối chứng.
  - `2_FM_Basic.ipynb`: Thuật toán Flajolet-Martin cơ bản (1 Hash) với hằng số $\phi \approx 0.77351$.
  - `3_FM_Advanced.ipynb`: Thuật toán Flajolet-Martin cải tiến (Kỹ thuật PCSA 1-Hash Chia Bit & Median-of-Means).
  - `4_KetLuan_SoSanh.ipynb`: Báo cáo đối chuẩn hiệu năng toàn diện, vẽ 4 biểu đồ trực quan hóa và rút ra kết luận lý thuyết chuyên sâu.
- **Giám sát tài nguyên chi tiết (Resource & Performance Benchmarking):** Đo lường và đối sánh dung lượng RAM chiếm dụng thực tế (`tracemalloc`, `sys.getsizeof`) giữa giải pháp chính xác (Python `set`) và cấu trúc Flajolet-Martin, cùng thời gian xử lý.
- **Cơ chế lưu trữ trung gian linh hoạt (`results/`):** Tự động xuất và nạp các tệp `set_metrics.json`, `fm_basic_metrics.json`, `fm_advanced_metrics.json` để vẽ biểu đồ so sánh tức thì mà không cần chạy lại toàn bộ luồng log từ đầu.

## 3. Yêu cầu hệ thống (Prerequisites)
- **Hệ điều hành:** Windows, Linux, hoặc macOS.
- **Python:** Phiên bản 3.9 trở lên (Khuyến nghị Python 3.10+).
- **Môi trường thực thi:** Jupyter Notebook, Google Colab, hoặc VS Code / Antigravity IDE.
- **Thư viện phụ thuộc:**
  - `matplotlib` (cho vẽ biểu đồ trực quan hóa)
  - Thư viện chuẩn Python tích hợp sẵn: `hashlib`, `math`, `re`, `sys`, `time`, `tracemalloc`, `random`.

## 4. Hướng dẫn cài đặt (Installation)
1. **Clone hoặc tải mã nguồn dự án:**
   ```bash
   git clone <repository_url>
   cd XuLyDuLieuLon
   ```
2. **Khởi tạo môi trường ảo (khuyến nghị):**
   ```bash
   python -m venv venv
   # Trên Windows:
   .\venv\Scripts\activate
   # Trên Linux/macOS:
   source venv/bin/activate
   ```
3. **Cài đặt các gói thư viện cần thiết:**
   ```bash
   pip install matplotlib notebook ipykernel
   ```

## 5. Biến môi trường (Environment Variables)
Dự án hiện tại hoạt động độc lập và không bắt buộc phải cấu hình file `.env`. Tuy nhiên, các đường dẫn dữ liệu có thể điều chỉnh linh hoạt trong code:

| Tên biến / Tham số | Kiểu dữ liệu | Mặc định | Mô tả |
| :--- | :--- | :--- | :--- |
| `LOG_FILE` | Chuỗi (`str`) | `/content/drive/MyDrive/accessLog/access.log` | Đường dẫn tuyệt đối tới tệp tin access log lưu trữ trên Google Drive |
| `RESULTS_DIR` | Chuỗi (`str`) | `/content/drive/MyDrive/accessLog/results` | Thư mục lưu trữ các tệp JSON kết quả đo đạc trên Google Drive |
| `sample_step` | Số nguyên (`int`) | `50000` | Chu kỳ số dòng log được xử lý để ghi nhận thống kê và vẽ biểu đồ |

## 6. Hướng dẫn chạy & Sử dụng (Usage/Run Instructions)
1. **Chuẩn bị dữ liệu trên Google Drive:** Đặt tệp tin `access.log` vào thư mục `accessLog/` trên Google Drive cá nhân của bạn (đường dẫn: `/MyDrive/accessLog/access.log`).
2. **Khởi chạy trên Google Colab:**
   - Mở lần lượt các notebook trên Google Colab.
   - Kết nối GPU / High-RAM runtime (nếu cần).
3. **Thực thi theo quy trình 4 bước:**
   - **Bước 1:** Chạy [1_Set_Exact.ipynb](file:///d:/CodePython/XuLyDuLieuLon/1_Set_Exact.ipynb) để thu thập dữ liệu đếm chính xác (Ground Truth).
   - **Bước 2:** Chạy [2_FM_Basic.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM_Basic.ipynb) để đo đạc thuật toán FM 1 hash cơ bản.
   - **Bước 3:** Chạy [3_FM_Advanced.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_Advanced.ipynb) để đo đạc thuật toán FM PCSA (1 Hash Chia Bit & Median of Means).
   - **Bước 4:** Chạy [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb) để tự động tổng hợp kết quả từ Google Drive, kết xuất bảng đối đầu và vẽ 4 biểu đồ phân tích.
   *(Lưu ý: Hệ thống chạy thuần $100\%$ trên Google Colab kết hợp Google Drive. Mọi tệp kết quả JSON và biểu đồ được tự động lưu vĩnh viễn trên Drive của bạn).*
