# Hướng dẫn chạy & Sử dụng (Usage/Run Instructions)

1. **Chuẩn bị dữ liệu trên Google Drive:** Đặt tệp tin `access.log` vào thư mục `accessLog/` trên Google Drive cá nhân của bạn (đường dẫn: `/MyDrive/accessLog/access.log`).
2. **Khởi chạy trên Google Colab:**
   - Mở lần lượt các notebook trên Google Colab.
   - Kết nối GPU / High-RAM runtime (nếu cần).
3. **Thực thi theo quy trình 4 bước:**
   - **Bước 1:** Chạy [1_Set_Exact.ipynb](file:///d:/CodePython/XuLyDuLieuLon/1_Set_Exact.ipynb) để thu thập dữ liệu đếm chính xác (Ground Truth).
   - **Bước 2:** Chạy [2_FM.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM.ipynb) để đo đạc thuật toán Flajolet-Martin (1 hash).
   - **Bước 3:** Chạy [3_FM_PCSA.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_PCSA.ipynb) để đo đạc thuật toán Flajolet-Martin PCSA (1 Hash Chia Bit & Median of Means).
   - **Bước 4:** Chạy [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb) để tự động tổng hợp kết quả từ Google Drive, kết xuất bảng đối đầu và vẽ 4 biểu đồ phân tích.
     *(Lưu ý: Hệ thống chạy thuần $100\%$ trên Google Colab kết hợp Google Drive. Mọi tệp kết quả JSON và biểu đồ được tự động lưu vĩnh viễn trên Drive của bạn).*
