# Nhật Ký Thay Đổi (CHANGELOG)

Tất cả các thay đổi đáng chú ý của dự án **Xử Lý Dữ Liệu Lớn** sẽ được ghi chép lại trong tệp này theo chuẩn [Semantic Versioning](https://semver.org/).

---

### [v1.1.1] - 2026-09-11

- **[Xóa bỏ]**
  - Xóa tệp `XuLyDuLieuLon.ipynb` gốc theo yêu cầu sau khi toàn bộ mã nguồn, cấu trúc thuật toán và dữ liệu đo đạc đã được kế thừa và phân tách trọn vẹn sang 4 notebook chuyên biệt ([1_Set_Exact.ipynb](file:///d:/CodePython/XuLyDuLieuLon/1_Set_Exact.ipynb), [2_FM_Basic.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM_Basic.ipynb), [3_FM_Advanced.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_Advanced.ipynb), [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb)).
- **[Cập nhật]**
  - Cập nhật tài liệu kiến trúc [architecture.md](file:///d:/CodePython/XuLyDuLieuLon/docs/architecture.md) đồng bộ sơ đồ cây thư mục dự án.

---

### [v1.1.0] - 2026-09-11

- **[Thêm mới]**
  - Tách và xây dựng 4 Notebook độc lập phục vụ nghiên cứu và đối chuẩn hiệu năng:
    - [1_Set_Exact.ipynb](file:///d:/CodePython/XuLyDuLieuLon/1_Set_Exact.ipynb): Phương pháp đếm chính xác bằng cấu trúc Python Set làm Ground Truth chuẩn xác 100%.
    - [2_FM_Basic.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM_Basic.ipynb): Thuật toán Flajolet-Martin cơ bản (1 hàm băm MD5, hằng số hiệu chỉnh $\phi \approx 0.77351$).
    - [3_FM_Advanced.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_Advanced.ipynb): Thuật toán Flajolet-Martin cải tiến ($k = 128$ hàm băm chia thành 16 nhóm, kỹ thuật Mean in group và Median of Means).
    - [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb): Báo cáo tổng hợp đối sánh toàn diện, hiển thị bảng số liệu đối đầu, 4 đồ thị Matplotlib và bài phân tích kết luận chuyên sâu.
  - Khởi tạo thư mục `results/` chứa các tệp lưu trữ số liệu trung gian: `set_metrics.json`, `fm_basic_metrics.json`, `fm_advanced_metrics.json`.
- **[Cập nhật]**
  - Tái sử dụng và kế thừa các thuật toán từ `XuLyDuLieuLon.ipynb`, đồng thời bổ sung cơ chế lưu/đọc kết quả JSON linh hoạt để hỗ trợ phân tích tức thì.
  - Cập nhật tài liệu kiến trúc [architecture.md](file:///d:/CodePython/XuLyDuLieuLon/docs/architecture.md) và hướng dẫn sử dụng [README.md](file:///d:/CodePython/XuLyDuLieuLon/README.md).

---

### [v1.0.0] - 2026-09-11

- **[Thêm mới]**
  - Khởi tạo tài liệu [README.md](file:///d:/CodePython/XuLyDuLieuLon/README.md) giới thiệu dự án, các tính năng cốt lõi và hướng dẫn khởi chạy thực nghiệm xử lý luồng dữ liệu log lớn.
  - Xây dựng tài liệu kiến trúc hệ thống [architecture.md](file:///d:/CodePython/XuLyDuLieuLon/docs/architecture.md) bao gồm 8 phân mục tiêu chuẩn và các sơ đồ trực quan hóa Mermaid.js (Flowchart, Sequence Diagram, ER Diagram).
  - Hoàn thiện thuật toán Flajolet-Martin cơ bản và nâng cao (128 hàm băm với kỹ thuật Median of Means).
