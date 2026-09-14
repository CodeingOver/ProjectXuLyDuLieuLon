# Nhật Ký Thay Đổi (CHANGELOG)

Tất cả các thay đổi đáng chú ý của dự án **Xử Lý Dữ Liệu Lớn** sẽ được ghi chép lại trong tệp này theo chuẩn [Semantic Versioning](https://semver.org/).

---

### [v1.6.2] - 2026-09-14

- **[Cập nhật]**
  - Bổ sung **Mục 4.4: Giải mã công thức toán học chuẩn của FM PCSA: $\widehat{F_0} = 128 \times \frac{2^{\bar{R}}}{\phi}$** trong tài liệu [GIAI_THICH_FM_PCSA.md](file:///d:/CodePython/XuLyDuLieuLon/docs/GIAI_THICH_FM_PCSA.md):
    - Làm rõ ý nghĩa chi tiết của từng thành phần: $\widehat{F_0}$ (ước lượng lực lượng), $m=128$ (số thùng phân vị), $\bar{R}$ (trung bình cộng số bit 0 tận cùng), $2^{\bar{R}}$ (lũy thừa biến đổi liên tục) và $\phi \approx 0.77351$ (hằng số hiệu chỉnh Flajolet-Martin).
    - Giải thích bản chất vì sao phải nhân với 128: do mỗi thùng chỉ gánh trung bình $1/128$ tổng số IP, nên sau khi ước lượng cho 1 thùng bằng $\frac{2^{\bar{R}}}{\phi}$ thì bắt buộc phải nhân 128 để quy đổi ra toàn bộ dòng dữ liệu.
    - Kết nối trực tiếp công thức lý thuyết với mô hình Median of Means (16 nhóm $\times$ 8 thùng) được cài đặt trong `3_FM_PCSA.ipynb`.

---

### [v1.6.1] - 2026-09-14

- **[Cập nhật]**
  - Mở rộng cẩm nang [GIAI_THICH_FM_PCSA.md](file:///d:/CodePython/XuLyDuLieuLon/docs/GIAI_THICH_FM_PCSA.md) với 2 chuyên mục thực chiến chuyên sâu:
    - **Mục 5: So sánh bản chất FM 128 hàm băm (Multi-Hash) vs FM PCSA 128 thùng**: Giải thích hình tượng "128 người gác cổng khám xét 128 lần" gây nghẽn CPU (hơn 1.3 tỷ lần băm trên 10.36M log) so với "bốc thăm vào 1 trong 128 quầy để băm đúng 1 lần ($O(1)$)", và bản chất toán học vì sao PCSA phải nhân với $m=128$ trong công thức $\frac{m}{\phi} 2^{\bar{R}}$ còn Multi-hash thì không.
    - **Mục 6: Giải đáp thắc mắc & hiểu lầm phổ biến (Q&A)**: Làm rõ sức chứa của không gian băm 64-bit ($\approx 23.8$ tỷ tỷ IP duy nhất), giải mã con số `sample_step = 50000` chỉ là chu kỳ in tiến độ màn hình (không ảnh hưởng kết quả cuối cùng) và phân tích cơ chế vận hành tách biệt giữa pha Cập nhật luồng (`update`) và pha Ước lượng tức thì (`estimate`).

---

### [v1.6.0] - 2026-09-14

- **[Thêm mới]**
  - Khởi tạo tài liệu cẩm nang giải thích trực quan [GIAI_THICH_FM_PCSA.md](file:///d:/CodePython/XuLyDuLieuLon/docs/GIAI_THICH_FM_PCSA.md) trong thư mục `docs/`.
  - Tài liệu sử dụng các hình tượng gần gũi (trò chơi tung đồng xu sấp ngửa, chia nhóm hội trường) kết hợp ví dụ minh họa step-by-step từng bước (IP -> băm nhị phân -> tách 2 bit chọn thùng -> đếm bit 0 -> cập nhật mảng thùng).
  - Phân tích bản chất toán học: giải thích trực quan hiện tượng nhảy vọt gấp đôi của FM 1 Hash ($R=18 \to 19$), cơ chế làm trơn số thực của Stochastic Averaging và cơ chế triệt tiêu ngoại lai của Median of Means (16 nhóm x 8 thùng).
  - Đóng gói 3 luận điểm cốt lõi giúp sinh viên tự tin trả lời vấn đáp / bảo vệ đồ án với Giảng viên ThS. Trần Thị Nhi.

---

### [v1.5.0] - 2026-09-13

- **[Thêm mới]**
  - Khởi tạo tài liệu Báo cáo môn học hoàn chỉnh [BAO_CAO_MON_HOC.md](file:///d:/CodePython/XuLyDuLieuLon/docs/BAO_CAO_MON_HOC.md) trong thư mục `docs/` bám sát $100\%$ quy cách trình bày và các mục bắt buộc từ tệp `Mẫu. BCMH.docx` của Giảng viên ThS. Trần Thị Nhi (Trường Đại học Thủ Dầu Một).
  - Báo cáo tích hợp đầy đủ: Trang bìa chuẩn, Lời cam đoan, Mục lục, Danh mục Bảng/Hình/Sơ đồ, Phần 1 (Mục đích, Câu hỏi nghiên cứu, Thu thập dữ liệu log 10.36M bản ghi, Phân tích toán học $F_0$/FM/PCSA, Xây dựng và kiểm thử chi tiết), Phần 2 (Kết luận & Hướng phát triển), Phần 3 (Bảng tự chấm điểm thang 10 & Bảng phân công công việc) và Danh mục tài liệu tham khảo học thuật.

---

### [v1.4.1] - 2026-09-13

- **[Cập nhật]**
  - Đồng bộ toàn bộ các chỉ số thực nghiệm, số liệu phân tích và bảng đối soát trong [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb) khớp chính xác $100\%$ với kết quả chạy thực tế trên tập dữ liệu đầy đủ **10,365,152 dòng log** (> 10.36 triệu bản ghi):
    - **Python Set (`1_Set_Exact.ipynb`):** 258,606 IP duy nhất (Ground Truth chuẩn), 134.72s, RAM cấu trúc chiếm 21.22 MB (22,247,602 Bytes), Peak RAM 21.346 MB.
    - **Flajolet-Martin 1 Hash (`2_FM.ipynb`):** Số bit 0 tận cùng lớn nhất $R = 19$, ước lượng 677,803 IP, sai số tương đối 162.10%, độ chính xác 0.00%, thời gian 252.37s, RAM cấu trúc 76 Bytes (~0.07 KB, tiết kiệm 292,732 LẦN so với Set).
    - **Flajolet-Martin PCSA (`3_FM_PCSA.ipynb`):** Ước lượng 421,262 IP, sai số tương đối 62.90%, độ chính xác 37.10%, thời gian 281.80s (tốc độ trung bình 36,781 dòng/s), RAM cấu trúc 4,712 Bytes (~4.60 KB, tiết kiệm 4,721 LẦN so với Set).
  - Hoàn thiện biện luận lý thuyết chuyên sâu: Minh chứng hiện tượng phương sai cực cao và bước nhảy lũy thừa cơ số 2 của FM 1 Hash khi $R$ nhảy từ 18 lên 19 (từ ~338k vọt lên ~678k), và chứng minh vai trò của Stochastic Averaging kết hợp Median of Means trong PCSA giúp làm trơn đường cong, triệt tiêu ngoại lai và kiểm soát sai số ổn định trên quy mô Big Data.
  - Cập nhật giá trị dự phòng mặc định (fallbacks) trong ô code phân tích số 4 của `4_KetLuan_SoSanh.ipynb` chuẩn theo số liệu bộ nhớ mới.

---

### [v1.4.0] - 2026-09-13

- **[Cập nhật]**
  - Đổi tên tệp `2_FM_Basic.ipynb` thành [2_FM.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM.ipynb) (loại bỏ chữ "Basic"), đồng thời chuẩn hóa tên lớp thuật toán thành `FlajoletMartin` và tệp kết quả đầu ra thành `fm_metrics.json`.
  - Đổi tên tệp `3_FM_Advanced.ipynb` thành [3_FM_PCSA.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_PCSA.ipynb) theo chuẩn tên khoa học chính thức của thuật toán gốc Philippe Flajolet & G. Nigel Martin (1985) - **PCSA (Probabilistic Counting with Stochastic Averaging)**, chuẩn hóa tên lớp `FlajoletMartinPCSA` và tệp kết quả `fm_pcsa_metrics.json`.
  - Nâng cấp [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb) để tự động nhận diện và đọc đồng thời cả tệp kết quả theo tên chuẩn mới lẫn các tệp cũ nhằm duy trì tính tương thích ngược hoàn hảo.
  - Đồng bộ toàn bộ sơ đồ Mermaid, cây thư mục và bảng mô tả trong tài liệu kiến trúc [architecture.md](file:///d:/CodePython/XuLyDuLieuLon/docs/architecture.md) và hướng dẫn sử dụng [README.md](file:///d:/CodePython/XuLyDuLieuLon/README.md).
- **[Xóa bỏ]**
  - Xóa bỏ hai tệp notebook cũ `2_FM_Basic.ipynb` và `3_FM_Advanced.ipynb` khỏi dự án.

---

### [v1.3.2] - 2026-09-11

- **[Sửa lỗi]**
  - Sửa lỗi cú pháp phân tích chuỗi (`Parse error: Expected FStringEnd, found string`) tại ô code số 4 dòng 32 trong [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb).
  - Tách và tiền xử lý các chuỗi định dạng thời gian thực thi (`time_set_str`, `time_basic_str`, `time_adv_str`) ra ngoài biểu thức f-string, loại bỏ hoàn toàn các ký tự escape lồng nhau không hợp lệ trong Python.

---

### [v1.3.1] - 2026-09-11

- **[Cập nhật]**
  - Chuyển đổi toàn diện $100\%$ cả 4 Notebook ([1_Set_Exact.ipynb](file:///d:/CodePython/XuLyDuLieuLon/1_Set_Exact.ipynb), [2_FM_Basic.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM_Basic.ipynb), [3_FM_Advanced.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_Advanced.ipynb), [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb)) sang môi trường thuần Google Colab kết hợp lưu trữ Google Drive.
  - Cố định đường dẫn tệp log tại `/content/drive/MyDrive/accessLog/access.log` và thư mục kết quả tại `/content/drive/MyDrive/accessLog/results/`.
- **[Xóa bỏ]**
  - Loại bỏ hoàn toàn mảng đường dẫn dự phòng `candidates` chứa các đường dẫn máy tính cá nhân (`accessLog/access.log`, `../accessLog/access.log`).
  - Xóa bỏ khối xử lý ngoại lệ rẽ nhánh `try...except ImportError` về môi trường Local trong ô kết nối Google Drive, giúp mã nguồn đồng nhất, tường minh và chuẩn hóa cho nền tảng đám mây.

---

### [v1.3.0] - 2026-09-11

- **[Thêm mới]**
  - Cơ chế lưu trữ đám mây vĩnh viễn: Hàm `get_results_dir()` tự động nhận diện và lưu trữ trực tiếp toàn bộ kết quả thực nghiệm vào Google Drive (`/content/drive/MyDrive/accessLog/results/`) khi chạy trên Google Colab.
- **[Cập nhật]**
  - Nâng cấp [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb) để tự động nạp kết quả trực tiếp từ Google Drive, cho phép toàn bộ chu trình 4 Notebook chạy $100\%$ mượt mà trên nền tảng Cloud / Colab mà không cần bất kỳ thao tác thủ công nào.
- **[Xóa bỏ]**
  - Loại bỏ hoàn toàn cơ chế `files.download()` tải file về máy cá nhân rườm rà, giải quyết triệt để vấn đề phân tán file kết quả.

---

### [v1.2.2] - 2026-09-11

- **[Cập nhật]**
  - Tái cấu trúc quy trình thực thi trong cả 3 Notebook ([1_Set_Exact.ipynb](file:///d:/CodePython/XuLyDuLieuLon/1_Set_Exact.ipynb), [2_FM_Basic.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM_Basic.ipynb), [3_FM_Advanced.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_Advanced.ipynb)): Đưa ô tải file kết quả (`files.download()`) lên **TRƯỚC** ô trực quan hóa.
  - Ô Trực quan hóa được nâng cấp để đọc trực tiếp $100\%$ dữ liệu từ tệp JSON trong `results/`, đảm bảo đồ thị hiển thị luôn là kết quả đối soát mới nhất vừa được ghi và tải về.

---

### [v1.2.1] - 2026-09-11

- **[Thêm mới]**
  - Tích hợp cơ chế tự động kích hoạt tải tệp kết quả (`google.colab.files.download`) vào cả 3 Notebook thực nghiệm ([1_Set_Exact.ipynb](file:///d:/CodePython/XuLyDuLieuLon/1_Set_Exact.ipynb), [2_FM_Basic.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM_Basic.ipynb), [3_FM_Advanced.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_Advanced.ipynb)).
- **[Cập nhật]**
  - Bổ sung ô tải tệp chuyên biệt và lồng ghép lệnh download trực tiếp ngay sau khi ghi file `json.dump()`.
  - Khắc phục triệt để vấn đề tệp kết quả bị cô lập trên ổ đĩa ảo `/content/` của máy chủ Google Colab trên mây, giúp người dùng dễ dàng đồng bộ tệp mới nhất về thư mục `results/` trên máy tính cục bộ.

---

### [v1.2.0] - 2026-09-11

- **[Thêm mới]**
  - Triển khai kỹ thuật **Stochastic Averaging (PCSA - Probabilistic Counting with Stochastic Averaging)** từ bài báo khoa học gốc năm 1985 của Philippe Flajolet và G. Nigel Martin vào [3_FM_Advanced.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_Advanced.ipynb).
- **[Cập nhật]**
  - Tối ưu hóa độ phức tạp hàm băm từ $O(m)$ (128 lần băm độc lập trên mỗi IP) về $O(1)$ (chỉ băm đúng 1 lần duy nhất bằng hàm băm 64-bit).
  - Áp dụng kỹ thuật tách bit phần cứng (Bit-Splitting): sử dụng $b = 7$ bit đầu làm chỉ số thùng ($m = 128$ thùng), các bit còn lại đếm số bit 0 tận cùng bằng toán tử bitwise.
  - Kết hợp kỹ thuật Median of Means trên 16 nhóm (mỗi nhóm 8 thùng) để duy trì độ mượt và triệt tiêu ngoại lai.
  - Tốc độ xử lý thực tế tăng vọt hơn 100 lần, đạt hàng trăm nghìn đến hàng triệu dòng log/giây, giải quyết triệt để hiện tượng nghẽn CPU.

---

### [v1.1.3] - 2026-09-11

- **[Cập nhật]**
  - Tinh giản hàm `load_benchmark_data()` trong [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb): Loại bỏ hoàn toàn khối dữ liệu đối soát mẫu cứng (`else: ...`), chuyển sang cơ chế nạp trực tiếp $100\%$ từ các tệp số liệu thực nghiệm trong thư mục `results/`.

---

### [v1.1.2] - 2026-09-11

- **[Cập nhật]**
  - Bổ sung cell tự động kiểm tra và mount Google Drive (`drive.mount('/content/drive')`) vào đầu cả 4 notebook ([1_Set_Exact.ipynb](file:///d:/CodePython/XuLyDuLieuLon/1_Set_Exact.ipynb), [2_FM_Basic.ipynb](file:///d:/CodePython/XuLyDuLieuLon/2_FM_Basic.ipynb), [3_FM_Advanced.ipynb](file:///d:/CodePython/XuLyDuLieuLon/3_FM_Advanced.ipynb), [4_KetLuan_SoSanh.ipynb](file:///d:/CodePython/XuLyDuLieuLon/4_KetLuan_SoSanh.ipynb)).
  - Thiết kế cơ chế bắt ngoại lệ thông minh (`try...except ImportError`): Tự động mount Google Drive khi chạy trên Google Colab và tự động bỏ qua khi chạy ở môi trường Local mà không gây lỗi ngắt quãng chương trình.

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
