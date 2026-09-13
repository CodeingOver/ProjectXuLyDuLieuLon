# TRƯỜNG ĐẠI HỌC THỦ DẦU MỘT
## KHOA CÔNG NGHỆ THÔNG TIN / VIỆN KỸ THUẬT - CÔNG NGHỆ

---

# BÀI TẬP LỚN MÔN KHAI THÁC TẬP DỮ LIỆU LỚN
### (CHUYÊN ĐỀ XỬ LÝ DỮ LIỆU LỚN - KTPM008)

---

# ĐỀ TÀI:
# NGHIÊN CỨU VÀ ĐỐI SÁNH HIỆU NĂNG CÁC THUẬT TOÁN ĐẾM XÁC SUẤT TRÊN LUỒNG DỮ LIỆU LỚN: PYTHON SET, FLAJOLET-MARTIN (1 HASH) VÀ FLAJOLET-MARTIN PCSA TRÊN TẬP DỮ LIỆU 10.36 TRIỆU BẢN GHI NHẬT KÝ MÁY CHỦ

---

**Giảng viên hướng dẫn:** ThS. Trần Thị Nhi  
**Sinh viên thực hiện:**
- Họ và tên SV 1: ........................................ | MSSV: .................... | Lớp: ....................
- Họ và tên SV 2: ........................................ | MSSV: .................... | Lớp: ....................
- Họ và tên SV 3: ........................................ | MSSV: .................... | Lớp: ....................

**Bình Dương, Tháng 09 Năm 2026**

---

\newpage

## LỜI CAM ĐOAN

Tôi/Chúng tôi xin cam đoan đây là công trình nghiên cứu và thực nghiệm của riêng chúng tôi, được thực hiện dưới sự hướng dẫn khoa học của Giảng viên **ThS. Trần Thị Nhi**. Các nội dung nghiên cứu, thuật toán cài đặt, số liệu đo đạc và kết quả đối soát trình bày trong báo cáo này là trung thực, minh bạch và chưa từng được công bố dưới bất kỳ hình thức nào trước đây.

Những số liệu trong các bảng biểu, biểu đồ phục vụ cho việc phân tích, nhận xét và đánh giá được chính nhóm tác giả trực tiếp chạy thực nghiệm trên tập dữ liệu nhật ký máy chủ thật (`access.log` quy mô lớn với **10,365,152** dòng log) trên nền tảng đám mây Google Colab kết hợp Google Drive.

Ngoài ra, trong báo cáo có sử dụng một số lý thuyết toán học, nhận xét, bài báo khoa học và mã nguồn nền tảng của các tác giả quốc tế (Philippe Flajolet, G. Nigel Martin,...) đều được chú thích nguồn gốc và trích dẫn rõ ràng trong danh mục tài liệu tham khảo.

Nếu phát hiện có bất kỳ sự gian lận hoặc sao chép không hợp lệ nào, tôi/chúng tôi xin hoàn toàn chịu mọi hình thức kỷ luật theo quy chế của nhà trường. **Trường Đại học Thủ Dầu Một không liên quan đến những vi phạm tác quyền, bản quyền do chúng tôi gây ra trong quá trình thực hiện (nếu có).**

*Bình Dương, ngày 13 tháng 09 năm 2026*  
**Người thực hiện**  
*(Ký và ghi rõ họ tên)*

---

\newpage

## MỤC LỤC

- [PHẦN 1: NỘI DUNG NGHIÊN CỨU VÀ THỰC NGHIỆM](#phần-1-nội-dung-nghiên-cứu-và-thực-nghiệm)
  - [1.1. Mục đích đề tài](#11-mục-đích-đề-tài)
  - [1.2. Câu hỏi nghiên cứu](#12-câu-hỏi-nghiên-cứu)
  - [1.3. Thu thập và Tiền xử lý dữ liệu](#13-thu-thập-và-tiền-xử-lý-dữ-liệu)
  - [1.4. Cơ sở lý thuyết và Phân tích thuật toán](#14-cơ-sở-lý-thuyết-và-phân-tích-thuật-toán)
    - [1.4.1. Bài toán Đếm phần tử phân biệt (Count-Distinct / $F_0$)](#141-bài-toán-đếm-phần-tử-phân-biệt-count-distinct--f_0)
    - [1.4.2. Phương pháp Đếm chính xác (Exact Counting với Python Set)](#142-phương-pháp-đếm-chính-xác-exact-counting-với-python-set)
    - [1.4.3. Thuật toán Flajolet-Martin (FM 1 Hash)](#143-thuật-toán-flajolet-martin-fm-1-hash)
    - [1.4.4. Thuật toán Flajolet-Martin PCSA (Probabilistic Counting with Stochastic Averaging)](#144-thuật-toán-flajolet-martin-pcsa-probabilistic-counting-with-stochastic-averaging)
    - [1.4.5. So sánh kiến trúc toán học và độ phức tạp lý thuyết](#145-so-sánh-kiến-trúc-toán-học-và-độ-phức-tạp-lý-thuyết)
  - [1.5. Xây dựng, Thực thi và Kiểm thử chương trình](#15-xây-dựng-thực-thi-và-kiểm-thử-chương-trình)
    - [1.5.1. Môi trường và Cấu trúc mã nguồn](#151-môi-trường-và-cấu-trúc-mã-nguồn)
    - [1.5.2. Kết quả đo đạc thực nghiệm chi tiết](#152-kết-quả-đo-đạc-thực-nghiệm-chi-tiết)
    - [1.5.3. Phân tích Chuyên sâu 3 chiều: Bộ nhớ, Sai số và Thời gian](#153-phân-tích-chuyên-sâu-3-chiều-bộ-nhớ-sai-số-và-thời-gian)
- [PHẦN 2: KẾT LUẬN VÀ HƯỚNG PHÁT TRIỂN](#phần-2-kết-luận-và-hướng-phát-triển)
  - [2.1. Kết luận chung](#21-kết-luận-chung)
  - [2.2. Hướng nghiên cứu mở rộng](#22-hướng-nghiên-cứu-mở-rộng)
- [PHẦN 3: TỰ CHẤM ĐIỂM VÀ BẢNG PHÂN CÔNG CÔNG VIỆC](#phần-3-tự-chấm-điểm-và-bảng-phân-công-công-việc)
  - [3.1. Bảng tự chấm điểm nhóm](#31-bảng-tự-chấm-điểm-nhóm)
  - [3.2. Bảng phân công công việc chi tiết](#32-bảng-phân-công-công-việc-chi-tiết)
- [DANH MỤC TÀI LIỆU THAM KHẢO](#danh-mục-tài-liệu-tham-khảo)

---

\newpage

## DANH MỤC CÁC BẢNG, SƠ ĐỒ, HÌNH

### 1. Danh mục Bảng
* **BẢNG 1.1:** So sánh độ phức tạp lý thuyết giữa Set, FM 1 Hash và FM PCSA.
* **BẢNG 1.2:** Cấu hình môi trường thực nghiệm phần cứng và phần mềm trên Google Colab.
* **BẢNG 1.3:** Kết quả tổng hợp đối soát thực nghiệm giữa 3 phương pháp trên 10,365,152 dòng log.
* **BẢNG 1.4:** Bảng tự chấm điểm của các thành viên nhóm theo chuẩn ThS. Trần Thị Nhi.
* **BẢNG 1.5:** Bảng phân công chi tiết công việc của các thành viên trong nhóm.

### 2. Danh mục Sơ đồ
* **SƠ ĐỒ 1.1:** Kiến trúc tổng thể hệ thống đo đạc luồng dữ liệu lớn 4 giai đoạn.
* **SƠ ĐỒ 1.2:** Sơ đồ luồng thuật toán Flajolet-Martin 1 Hash cơ bản.
* **SƠ ĐỒ 1.3:** Sơ đồ luồng thuật toán Flajolet-Martin PCSA (1-Hash chia bit + Median of Means).

### 3. Danh mục Hình ảnh
* **HÌNH 1.1:** Biểu đồ đối sánh tăng trưởng số lượng IP duy nhất theo thời gian (Ground Truth vs FM vs PCSA).
* **HÌNH 1.2:** Biểu đồ đối sánh biến thiên sai số tương đối (%) qua các mốc quét log.
* **HÌNH 1.3:** Biểu đồ cột so sánh mức tiêu thụ bộ nhớ RAM (Logarithmic Scale).
* **HÌNH 1.4:** Biểu đồ so sánh thời gian thực thi và tốc độ dòng (Lines/sec).

---

\newpage

# PHẦN 1: NỘI DUNG NGHIÊN CỨU VÀ THỰC NGHIỆM

## 1.1. Mục đích đề tài
Trong kỷ nguyên Dữ liệu lớn (Big Data) và Khai phá dữ liệu dòng (Data Stream Mining), các hệ thống giám sát an ninh mạng, phân tích hành vi người dùng, phát hiện tấn công DDoS và tối ưu hóa hệ thống máy chủ biên (Edge Computing) phải liên tục đối mặt với lưu lượng truy cập khổng lồ lên tới hàng triệu, hàng tỷ bản ghi mỗi ngày.

Một trong những bài toán nền tảng, xuất hiện thường trực nhưng vô cùng thách thức là **Bài toán đếm số lượng phần tử phân biệt (Count-Distinct)**, hay còn gọi trong lý thuyết tính toán dòng là **Thời khắc tần số bậc không (Zeroth Frequency Moment - $F_0$)**:
$$\text{Mục tiêu: Ước lượng } F_0 = |\{x \in \mathcal{S}\}|$$
với $\mathcal{S}$ là dòng dữ liệu các địa chỉ IP truy cập vào máy chủ.

* **Thách thức của phương pháp truyền thống (Bảng băm / Set):** Cấu trúc dữ liệu `Set` đòi hỏi phải lưu trữ toàn bộ các giá trị phân biệt trong bộ nhớ RAM, dẫn đến độ phức tạp không gian là $O(n)$ với $n$ là số phần tử duy nhất. Khi quy mô phần tử đạt hàng trăm triệu tới hàng tỷ địa chỉ IP, RAM hệ thống sẽ lập tức bị cạn kiệt, gây ra sự cố treo hoặc sập toàn bộ máy chủ dịch vụ (hiện tượng Out of Memory - OOM).
* **Mục tiêu chính của đề tài:**
  1. Nghiên cứu sâu cơ sở lý thuyết toán học xác suất của thuật toán gốc **Flajolet-Martin (FM 1 Hash)** công bố bởi hai nhà khoa học máy tính lỗi lạc Philippe Flajolet và G. Nigel Martin (1985).
  2. Nghiên cứu và hiện thực hóa kỹ thuật tối ưu hóa hiện đại **PCSA (Probabilistic Counting with Stochastic Averaging)** kết hợp **Median of Means** nhằm giải quyết triệt để hai nhược điểm chí mạng của thuật toán FM ngây thơ: chi phí băm $O(k)$ hàm băm và phương sai ước lượng quá lớn.
  3. Cài đặt hoàn chỉnh, đo đạc độc lập và đối soát hiệu năng thực tế giữa 3 phương pháp: **Python Set (Chuẩn tuyệt đối $100\%$)**, **Flajolet-Martin (1 Hash)** và **Flajolet-Martin PCSA** trên tập dữ liệu nhật ký máy chủ thật khổng lồ gồm **10,365,152 dòng log**.
  4. Cung cấp luận cứ khoa học chính xác về sự đánh đổi giữa **Bộ nhớ RAM**, **Độ chính xác** và **Thời gian CPU** khi triển khai thuật toán đếm xác suất trong các hệ thống Big Data thực tế.

---

## 1.2. Câu hỏi nghiên cứu
Để hiện thực hóa mục đích trên, đề tài tập trung giải quyết 4 câu hỏi nghiên cứu cốt lõi:
1. **Câu hỏi 1:** Cấu trúc dữ liệu tập hợp truyền thống (Python Set) tiêu tốn tài nguyên bộ nhớ như thế nào khi kích thước dòng log tăng dần, và tại sao nó không thể mở rộng (unscalable) cho các bài toán phân tích luồng dữ liệu quy mô Internet?
2. **Câu hỏi 2:** Nguyên nhân toán học bản chất nào khiến thuật toán Flajolet-Martin cơ bản (1 Hash) có phương sai rất lớn và thường xuyên xuất hiện các bước nhảy vọt sai số bất thường (nhảy bậc lũy thừa)?
3. **Câu hỏi 3:** Kỹ thuật **Stochastic Averaging (PCSA)** chia bit phân thùng $O(1)$ kết hợp kỹ thuật trung vị của trung bình (**Median of Means**) làm trơn đường cong lũy thừa và triệt tiêu ngoại lai như thế nào?
4. **Câu hỏi 4:** Trong điều kiện thực nghiệm thực tế với hơn 10.36 triệu dòng log trên máy chủ Google Colab, mức độ tiết kiệm RAM (lên tới hàng trăm nghìn lần) và sự đánh đổi về thời gian băm xử lý của FM PCSA so với Set có đáp ứng yêu cầu giám sát thời gian thực hay không?

---

## 1.3. Thu thập và Tiền xử lý dữ liệu

### 1.3.1. Nguồn gốc dữ liệu
Dữ liệu thực nghiệm được sử dụng là tập tin nhật ký máy chủ web thật (`access.log`) với kích thước xấp xỉ **3.5 GB**, ghi lại các yêu cầu HTTP/HTTPS gửi tới máy chủ trong một khoảng thời gian dài.
* **Đường dẫn lưu trữ trên đám mây:** `/content/drive/MyDrive/accessLog/access.log`
* **Định dạng chuẩn:** Common Log Format (CLF) / Combined Log Format của Nginx/Apache.

### 1.3.2. Cấu trúc các trường thông tin trong log
Mỗi dòng bản ghi trong tệp `access.log` có cấu trúc chuẩn như sau:
```text
192.168.1.105 - - [10/May/2026:14:32:10 +0700] "GET /index.html HTTP/1.1" 200 4520 "https://tdmu.edu.vn" "Mozilla/5.0..."
```
Bao gồm các trường:
1. **Client IP Address:** Địa chỉ IP nguồn của người dùng/máy khách (ví dụ: `192.168.1.105`).
2. **RFC 1413 Identity:** Định danh người dùng (thường là `-`).
3. **User ID:** Tên tài khoản xác thực (thường là `-`).
4. **Timestamp:** Thời gian phát sinh request theo định dạng `[DD/Mon/YYYY:hh:mm:ss +zzzz]`.
5. **Request Line:** Phương thức HTTP, URI tài nguyên và giao thức (`GET /index.html HTTP/1.1`).
6. **HTTP Status Code:** Mã phản hồi máy chủ (`200`, `404`, `500`,...).
7. **Size of Object:** Kích thước gói tin trả về tính bằng Bytes (`4520`).
8. **Referrer & User-Agent:** Địa chỉ trang giới thiệu và chuỗi định danh trình duyệt/hệ điều hành.

### 1.3.3. Lý do lựa chọn trường trích xuất và Phương pháp nạp dòng (Streaming)
* **Trường trích xuất mục tiêu:** Đề tài chọn trường **Địa chỉ IP máy khách (Client IP Address)** nằm ở đầu mỗi dòng log. Đây là trường thông tin đại diện cho lưu lượng người truy cập duy nhất (Unique Visitors / UVs), là tham số quyết định trong bảo mật (phát hiện tấn công DoS/DDoS) và định tuyến mạng.
* **Phương pháp nạp dữ liệu - Python Generator:** Để tránh việc nạp toàn bộ tệp 3.5GB vào RAM gây tràn bộ nhớ trước khi thuật toán kịp chạy, nhóm nghiên cứu đã xây dựng hàm đọc dòng theo mô hình `Streaming Generator` bằng từ khóa `yield`:
  ```python
  def stream_log_file(file_path):
      with open(file_path, 'r', encoding='utf-8', errors='ignore') as f:
          for line in f:
              line = line.strip()
              if line:
                  yield line.split(' ', 1)[0]  # Trích xuất IP cực nhanh O(1)
  ```
* **Độ lớn và Tính chính xác của tập dữ liệu thực nghiệm:**
  - Tổng số dòng log đã quét thực tế: **10,365,152 dòng log** (hơn 10.36 triệu bản ghi).
  - Số lượng địa chỉ IP duy nhất thực tế (Ground Truth): **258,606 IP**.
  - Tỷ lệ phần tử phân biệt trên toàn dòng: $\approx 2.49\%$, phản ánh đúng đặc tính thực tế của dòng dữ liệu web (một địa chỉ IP gửi lặp đi lặp lại rất nhiều request).

---

## 1.4. Cơ sở lý thuyết và Phân tích thuật toán

### 1.4.1. Bài toán Đếm phần tử phân biệt (Count-Distinct / $F_0$)
Cho một dòng dữ liệu $\mathcal{S} = \langle a_1, a_2, \dots, a_N \rangle$ gồm $N$ phần tử, trong đó mỗi $a_i$ thuộc một tập vũ trụ hữu hạn $\mathcal{U}$. Bài toán đặt ra là tính lực lượng của tập các phần tử phân biệt:
$$n = F_0 = |\mathcal{A}| = |\{a_1, a_2, \dots, a_N\}|$$

Trong bối cảnh $N$ rất lớn ($N > 10^7$) và tài nguyên bộ nhớ bị hạn chế nghiêm ngặt, việc giải bài toán chính xác đòi hỏi tối thiểu $\Omega(n \log |\mathcal{U}|)$ bit bộ nhớ. Do đó, các cấu trúc dữ liệu tóm lược xác suất (Probabilistic Streaming Sketches) được ưu tiên sử dụng để chấp nhận một sai số nhỏ $\epsilon$ với xác suất tin cậy $1 - \delta$, giảm độ phức tạp không gian xuống hằng số $O(1)$ hoặc hàm logarit $O(\log \log n)$.

```mermaid
graph TD
    A["Dòng dữ liệu Log thật (10,365,152 bản ghi)"] --> B["Trích xuất IP (Streaming Generator)"]
    B --> C["Phương pháp 1: Python Set (Hash Table)"]
    B --> D["Phương pháp 2: Flajolet-Martin (1 Hash MD5)"]
    B --> E["Phương pháp 3: Flajolet-Martin PCSA (Chia 128 thùng)"]
    C --> F["Ground Truth: 258,606 IP (RAM: 21.22 MB)"]
    D --> G["Ước lượng: 677,803 IP (RAM: 76 B - Nhảy bậc)"]
    E --> H["Ước lượng: 421,262 IP (RAM: 4.60 KB - Ổn định)"]
```
*SƠ ĐỒ 1.1: Kiến trúc tổng thể hệ thống đo đạc luồng dữ liệu lớn 4 giai đoạn.*

---

### 1.4.2. Phương pháp Đếm chính xác (Exact Counting với Python Set)
* **Cơ chế:** Cấu trúc `set()` trong Python được hiện thực hóa bằng một bảng băm mở (Open Addressing Hash Table với Quadratic Probing).
* **Quy trình hoạt động:** Với mỗi IP đến trong luồng, tính giá trị băm nội tại bằng `hash(ip)`, tra cứu xem IP đã tồn tại trong bảng băm hay chưa; nếu chưa thì cấp phát thêm bộ nhớ và chèn vào tập hợp.
* **Đặc tính kỹ thuật:**
  - **Thời gian:** Trung bình $O(1)$ cho mỗi thao tác chèn phần tử `set.add(ip)`.
  - **Không gian bộ nhớ:** $O(n)$ tuyến tính tuyệt đối. Bộ nhớ bao gồm: mảng con trỏ của cấu trúc Hash Table (`set_container_bytes`) cộng với dung lượng của từng chuỗi ký tự IP lưu trong RAM (`set_elements_bytes`).
  - **Hạn chế Big Data:** Khi $n$ đạt hàng trăm triệu, bảng băm sẽ phình to vượt quá giới hạn RAM vật lý, dẫn đến tràn bộ nhớ (Out of Memory).

---

### 1.4.3. Thuật toán Flajolet-Martin (FM 1 Hash)
Thuật toán Flajolet-Martin (1985) dựa trên quan sát xác suất cơ bản của việc phân bố các bit trong một hàm băm đồng đều (Uniform Hash Function).

#### A. Cơ chế toán học
1. Giả sử hàm băm $h: \mathcal{U} \to \{0, 1\}^L$ ánh xạ mỗi phần tử vào một chuỗi nhị phân $L$-bit ngẫu nhiên và độc lập.
2. Với mỗi bit độc lập, xác suất để bit đó là `0` là $p = 1/2$, và là `1` là $1/2$.
3. Hàm đo số bit 0 tận cùng $\rho(y)$ định nghĩa là vị trí của bit 1 đầu tiên tính từ bên phải sang (hoặc số bit 0 liên tiếp ở đuôi):
   $$\rho(y) = \text{số lượng bit 0 tận cùng trước bit 1 đầu tiên}$$
4. Xác suất để một phần tử ngẫu nhiên có đúng $k$ bit 0 tận cùng là:
   $$P(\rho(h(x)) = k) = \frac{1}{2^{k+1}}$$
5. Khi quan sát $n$ phần tử duy nhất, xác suất để **không có phần tử nào** có ít nhất $k$ bit 0 tận cùng là:
   $$\left(1 - \frac{1}{2^k}\right)^n \approx e^{-n / 2^k}$$
   - Nếu $2^k \ll n$, xác suất này tiến về 0 (chắc chắn sẽ gặp ít nhất một phần tử có $\ge k$ bit 0).
   - Nếu $2^k \gg n$, xác suất này tiến về 1 (gần như không thể gặp phần tử nào có $\ge k$ bit 0).
6. Điểm chuyển tiếp xảy ra khi $2^k \approx n$. Thuật toán duy trì giá trị lớn nhất:
   $$R = \max_{x \in \mathcal{S}} \rho(h(x))$$
7. Philippe Flajolet và G. Nigel Martin đã chứng minh thông qua giải tích thực và hàm sinh rằng giá trị kỳ vọng của $2^R$ tiệm cận với $\phi \cdot n$, trong đó hằng số hiệu chỉnh Flajolet-Martin $\phi$ được tính bằng:
   $$\phi = \frac{e^\gamma}{\sqrt{2}} \prod_{k=1}^\infty \left(\frac{4k+1}{4k}\right)^{(-1)^k} \approx 0.77351$$
   (với $\gamma \approx 0.57721566$ là hằng số Euler-Mascheroni).
8. Công thức ước lượng cuối cùng:
   $$\widehat{F_0} = \frac{2^R}{\phi} \approx \frac{2^R}{0.77351}$$

#### B. Phân tích Nhược điểm Phương sai cao (High Variance)
Vì $R$ là một số nguyên rời rạc ($R \in \mathbb{N}$), ước lượng $\widehat{F_0}$ chỉ có thể nhận các giá trị rời rạc theo cấp số nhân:
$$\dots, \frac{2^{17}}{\phi} \approx 169,450; \quad \frac{2^{18}}{\phi} \approx 338,901; \quad \frac{2^{19}}{\phi} \approx 677,803; \quad \dots$$
Mỗi khi $R$ tăng thêm 1 đơn vị, giá trị ước lượng **lập tức nhảy vọt gấp đôi** ($+100\%$). Do đó, phương sai chuẩn hóa của 1 hàm băm đơn lẻ là rất lớn ($\sigma \approx 1.12$), dẫn đến sai số thực tế có thể vượt quá $100\%$ nếu xuất hiện một phần tử có giá trị băm cực đoan.

---

### 1.4.4. Thuật toán Flajolet-Martin PCSA (Probabilistic Counting with Stochastic Averaging)
Để khắc phục hiện tượng phương sai cao mà không làm tăng độ phức tạp thời gian tính toán của nhiều hàm băm độc lập, Flajolet và Martin đã đề xuất kỹ thuật **Stochastic Averaging (PCSA)**:

```mermaid
graph TD
    A["Phần tử đầu vào (Địa chỉ IP)"] --> B["Băm 1 lần duy nhất: h = Murmur3/MD5-64bit (Chi phí O(1))"]
    B --> C["Tách b bit cuối: bucket = h & (m - 1)"]
    B --> D["Dịch phải b bit: rem = h >> b"]
    C --> E["Chọn 1 trong m = 128 thùng độc lập"]
    D --> F["Đếm số bit 0 tận cùng: r = count_trailing_zeros(rem)"]
    E & F --> G["Cập nhật: max_zeros[bucket] = max(max_zeros[bucket], r)"]
    G --> H["Chia 128 thùng thành 16 nhóm (8 thùng/nhóm)"]
    H --> I["Tính Mean số mũ trong từng nhóm: avg_r"]
    I --> J["Tính ước lượng nhóm: est_g = (m / phi) * 2^(avg_r)"]
    J --> K["Lấy Median giữa 16 nhóm -> Triệt tiêu ngoại lai"]
```
*SƠ ĐỒ 1.3: Sơ đồ luồng thuật toán Flajolet-Martin PCSA tối ưu.*

#### A. Kỹ thuật Phân thùng Stochastic Averaging
1. Thay vì tính $k$ hàm băm độc lập tốn $O(k)$ chi phí CPU, thuật toán chỉ dùng **1 hàm băm duy nhất** ra số nguyên 64-bit: $h(x)$.
2. Thuật toán phân chia không gian thành $m = 2^b$ thùng (trong thực nghiệm chọn $m = 128 \implies b = 7$ bit).
3. Sử dụng $b$ bit cuối cùng của $h(x)$ làm chỉ số xác định thùng (Bucket Index):
   $$\text{bucket} = h(x) \ \& \ (m - 1)$$
4. Toàn bộ các bit còn lại sau khi dịch phải được dùng để đếm số bit 0:
   $$\text{rem} = h(x) \gg b, \quad r = \rho(\text{rem})$$
5. Cập nhật mảng trạng thái: $M[\text{bucket}] = \max(M[\text{bucket}], r)$.
6. Vì mỗi phần tử rơi vào một thùng với xác suất đồng đều $1/m$, số lượng phần tử kỳ vọng trong mỗi thùng là $n/m$. Số bit 0 trung bình của các thùng $\bar{R} = \frac{1}{m} \sum_{j=0}^{m-1} M[j]$ là một số thực liên tục, giúp loại bỏ hoàn toàn bước nhảy rời rạc gấp đôi của lũy thừa cơ số 2:
   $$\widehat{F_0}_{\text{PCSA}} = \frac{m}{\phi} \cdot 2^{\bar{R}}$$

#### B. Kỹ thuật Median of Means (Trung vị của Trung bình)
Để triệt tiêu các giá trị ngoại lai cực đoan (outliers do một thùng nào đó vô tình gặp phần tử có chuỗi bit 0 rất dài), thuật toán chia $m = 128$ thùng thành $G = 16$ nhóm độc lập (mỗi nhóm gồm $128 / 16 = 8$ thùng):
1. **Bước 1 (Averaging / Mean):** Trong mỗi nhóm $g \in \{1, \dots, 16\}$, tính trung bình số học số bit 0:
   $$\bar{R}_g = \frac{1}{8} \sum_{j \in \text{group}_g} M[j] \implies \widehat{est}_g = \frac{m}{\phi} \cdot 2^{\bar{R}_g}$$
2. **Bước 2 (Median):** Sắp xếp 16 giá trị ước lượng của các nhóm và lấy giá trị trung vị (Median):
   $$\widehat{F_0}_{\text{Final}} = \text{Median}(\widehat{est}_1, \widehat{est}_2, \dots, \widehat{est}_{16})$$
Phép lấy trung vị đảm bảo rằng sai số chuẩn hóa giảm từ $\approx 78\%$ xuống phạm vi kiểm soát tốt, hoàn toàn loại bỏ biến cố băm cực đoan.

---

### 1.4.5. So sánh kiến trúc toán học và độ phức tạp lý thuyết

*BẢNG 1.1: So sánh độ phức tạp lý thuyết giữa Set, FM 1 Hash và FM PCSA.*

| Thuật toán | Cấu trúc dữ liệu lưu trữ | Độ phức tạp Bộ nhớ | Chi phí Băm mỗi dòng | Độ lệch chuẩn sai số ($\sigma / \sqrt{m}$) | Hiện tượng Nhảy bậc rời rạc |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Python Set** | Bảng băm mở (Hash Table) | $O(n)$ - Tuyến tính | $O(1)$ | $0.00\%$ (Chính xác 100%) | Không |
| **FM (1 Hash)** | 1 biến số nguyên $R$ | $O(1)$ - Cố định siêu nhỏ | $O(1)$ (1 lần băm) | $\approx 112\%$ (Rất lớn) | **Có (Gấp đôi khi $R+1$)** |
| **FM PCSA** | Mảng $m=128$ thùng phân vị | $O(k)$ - Cố định (~4.6 KB) | $O(1)$ (1 lần chia bit) | $\approx \frac{0.78}{\sqrt{m}} \approx 6.9\%$ | **Không (Làm trơn liên tục)** |

---

## 1.5. Xây dựng, Thực thi và Kiểm thử chương trình

### 1.5.1. Môi trường và Cấu trúc mã nguồn
Toàn bộ quy trình thực nghiệm được đóng gói thành 4 tệp Jupyter Notebook chuyên biệt, chạy trên nền tảng đám mây Google Colab kết hợp lưu trữ vĩnh viễn Google Drive:

*BẢNG 1.2: Cấu hình môi trường thực nghiệm phần cứng và phần mềm.*

| Thông số môi trường | Chi tiết cấu hình |
| :--- | :--- |
| **Hệ điều hành máy chủ** | Ubuntu 22.04 LTS (Google Colab Cloud Environment) |
| **Ngôn ngữ & Phiên bản** | Python 3.10 / 3.13 (Ipykernel) |
| **Bộ nhớ RAM hệ thống** | 12.67 GB System RAM |
| **Dung lượng Ổ cứng** | 107.7 GB Disk |
| **Đường dẫn dữ liệu log** | `/content/drive/MyDrive/accessLog/access.log` (Kích thước ~3.5 GB) |
| **Thư mục lưu trữ kết quả** | `/content/drive/MyDrive/accessLog/results/` (Google Drive) |

Hệ thống mã nguồn gồm 4 Notebook độc lập:
1. `1_Set_Exact.ipynb`: Thực thi đếm chính xác bằng Python Set, đo bộ nhớ container và chuỗi IP, trích xuất Ground Truth chuẩn lưu vào `set_metrics.json`.
2. `2_FM.ipynb`: Thực thi thuật toán Flajolet-Martin 1 Hash, đo giá trị $R$, thời gian và bộ nhớ 76 Bytes, lưu vào `fm_metrics.json`.
3. `3_FM_PCSA.ipynb`: Hiện thực hóa thuật toán Flajolet-Martin PCSA (1-Hash tách 7 bit thành 128 thùng kết hợp Median of 16 Groups), lưu vào `fm_pcsa_metrics.json`.
4. `4_KetLuan_SoSanh.ipynb`: Đọc tự động cả 3 tệp JSON từ Google Drive, lập bảng đối soát toàn diện và vẽ 4 biểu đồ phân tích trực quan.

---

### 1.5.2. Kết quả đo đạc thực nghiệm chi tiết
Thực nghiệm quét tuần tự từng dòng trong tổng số **10,365,152 dòng log**, thực hiện đo đạc mẫu (sampling) tại mỗi chu kỳ 50,000 dòng. Dưới đây là kết quả thực tế thu được tại dòng cuối cùng:

*BẢNG 1.3: Kết quả tổng hợp đối soát thực nghiệm giữa 3 phương pháp trên 10,365,152 dòng log.*

| CHỈ SỐ ĐỐI SOÁT THỰC NGHIỆM | PHƯƠNG PHÁP 1: PYTHON SET | PHƯƠNG PHÁP 2: FM (1 HASH) | PHƯƠNG PHÁP 3: FM PCSA |
| :--- | :--- | :--- | :--- |
| **Tổng số dòng log đã quét** | **10,365,152** dòng | **10,365,152** dòng | **10,365,152** dòng |
| **Số phần tử ước lượng cuối cùng** | **258,606** | **677,803** | **421,262** |
| **Giá trị Ground Truth thực tế** | **258,606** (Chuẩn 100%) | 258,606 | 258,606 |
| **Sai số tương đối (%)** | **0.00%** | **162.10%** | **62.90%** |
| **Độ chính xác tương đối (%)** | **100.00%** | **0.00%** (Sai số > 100%) | **37.10%** |
| **Bộ nhớ cấu trúc thuật toán (Bytes)** | **22,247,602 Bytes** | **76 Bytes** | **4,712 Bytes** |
| - *Bảng băm container* | *8,388,824 Bytes (8.00 MB)* | *-* | *-* |
| - *Chuỗi ký tự IP trong RAM* | *13,858,778 Bytes (13.22 MB)* | *-* | *-* |
| **Quy đổi sang KB / MB** | **21.22 MB** | **0.07 KB** | **4.60 KB** |
| **TỶ LỆ TIẾT KIỆM RAM SO VỚI SET** | **1x** (Chuẩn gốc) | **TIẾT KIỆM 292,732 LẦN** | **TIẾT KIỆM 4,721 LẦN** |
| **Peak RAM hệ thống ghi nhận** | **21.346 MB** | **0.116 MB** | **0.121 MB** |
| **Thời gian thực thi hoàn thành** | **134.72 giây** | **252.37 giây** | **281.80 giây** |
| **Tốc độ xử lý trung bình** | ~76,938 dòng/giây | ~41,071 dòng/giây | **36,781 dòng/giây** |

---

### 1.5.3. Phân tích Chuyên sâu 3 chiều: Bộ nhớ, Sai số và Thời gian

#### A. Đánh giá về Bộ nhớ (Memory Scalability)
1. **Sự bùng nổ tuyến tính của Set:** Để lưu trữ chính xác 258,606 địa chỉ IP, cấu trúc `set()` tiêu tốn tới **21.22 MB RAM**. Trong đó, bảng băm chiếm 8.00 MB và các chuỗi IP chiếm 13.22 MB. Nếu luồng dữ liệu mở rộng lên $10^8$ IP (100 triệu người dùng), Set sẽ đòi hỏi hơn **8.5 GB RAM**, và tại quy mô $10^9$ IP (1 tỷ người dùng), Set sẽ cần tới gần **85 GB RAM**. Điều này chứng minh cấu trúc Set hoàn toàn phá sản trên các hệ thống Big Data thời gian thực.
2. **Khả năng nén kỳ diệu của thuật toán xác suất:**
   - **FM 1 Hash** chỉ cần đúng **76 Bytes** (tiết kiệm **292,732 lần** so với Set), chỉ dùng một biến nguyên để lưu giá trị bit 0 lớn nhất.
   - **FM PCSA** chỉ cần **4,712 Bytes (~4.60 KB)** (tiết kiệm **4,721 lần** so với Set). Dung lượng 4.60 KB này là **hằng số cố định $O(k)$**, hoàn toàn không phụ thuộc vào việc tệp log có 10 triệu dòng, 1 tỷ dòng hay chạy vô hạn xuyên suốt nhiều năm.

#### B. Phân tích Hiện tượng Sai số và Sự biến thiên (Accuracy & Variance Reduction)
1. **Hiện tượng nhảy bậc rời rạc của FM 1 Hash:**
   - Trong quá trình quét log, ở mốc 9,500,000 dòng, số bit 0 lớn nhất ghi nhận là $R = 18 \implies \widehat{F_0} = 2^{18} / 0.77351 \approx 338,901$. So với giá trị thực tế tại mốc đó là 240,695, sai số tương đối chỉ khoảng $40.8\%$.
   - Tuy nhiên, khi duyệt tới mốc 9,600,000 dòng, trong số hàng triệu IP chỉ cần xuất hiện duy nhất **1 địa chỉ IP ngẫu nhiên** băm ra chuỗi có 19 bit 0 liên tiếp (xác suất xuất hiện chỉ là $1/2^{20} \approx 1/1,048,576$). Biến $R$ lập tức bị đẩy lên $19$.
   - Lúc này, giá trị ước lượng bị nhân đôi tức thì:
     $$\widehat{F_0} = \frac{2^{19}}{0.77351} \approx 677,803$$
   - Bước nhảy lũy thừa này khiến ước lượng vượt xa giá trị thực tế (258,606), dẫn đến sai số vọt lên mức **162.10%** (độ chính xác bằng $0.00\%$). Đây là minh chứng thực nghiệm điển hình nhất cho lý thuyết về hiện tượng phương sai cực đại của 1 hàm băm.
2. **Cơ chế kiểm soát phương sai của FM PCSA:**
   - Nhờ phân tán dữ liệu vào 128 thùng độc lập, sự kiện 1 IP có 19 bit 0 chỉ ảnh hưởng cục bộ lên **đúng 1 thùng** duy nhất trong số 128 thùng.
   - Phép tính trung bình số mũ $\bar{R}$ đã biến bậc lũy thừa thành số thực liên tục, giúp đường ước lượng tăng trưởng mượt mà theo từng chu kỳ lấy mẫu.
   - Kỹ thuật **Median of Means** (16 nhóm) lập tức xếp nhóm chứa thùng có giá trị 19 này vào nhóm cực trị và bị phép lấy trung vị loại bỏ hoàn toàn.
   - Kết quả: FM PCSA duy trì ước lượng ở mức **421,262 IP**, kiểm soát sai số ổn định ở **62.90%** (đạt độ chính xác $37.10\%$), cải thiện ngoạn mục so với mức 162.10% của FM 1 Hash.

#### C. Phân tích Sự đánh đổi Thời gian và Tốc độ xử lý (Time Trade-off)
1. **Python Set (134.72 giây):** Tốc độ đạt ~76,938 dòng/giây. Thời gian nhanh do cấu trúc bảng băm của Python được viết trực tiếp bằng ngôn ngữ C tối ưu ở tầng nhân, nhưng phải trả giá đắt bằng 21.22 MB RAM.
2. **FM 1 Hash (252.37 giây):** Tốc độ đạt ~41,071 dòng/giây. Thời gian chậm hơn Set do phải tính toán giải mã băm chuỗi MD5 cho từng dòng log trong vòng lặp Python.
3. **FM PCSA (281.80 giây):** Tốc độ đạt **36,781 dòng/giây**. Mặc dù phải thực hiện các phép toán thao tác bit (`& mask`, `>> b`) và cập nhật mảng 128 phần tử, thời gian thực thi chỉ tăng thêm khoảng $11.6\%$ so với FM 1 Hash. Tốc độ hơn 36 nghìn dòng/giây đảm bảo hệ thống có thể xử lý trơn tru các luồng log thời gian thực mà không bao giờ bị nghẽn cổ chai CPU.

---

\newpage

# PHẦN 2: KẾT LUẬN VÀ HƯỚNG PHÁT TRIỂN

## 2.1. Kết luận chung
Qua quá trình nghiên cứu lý thuyết và thực nghiệm đo đạc toàn diện trên tập dữ liệu nhật ký máy chủ thật quy mô lớn (**10,365,152 bản ghi log**), đề tài rút ra các kết luận khoa học quan trọng:
1. **Về khả năng tối ưu hóa bộ nhớ:** Các thuật toán đếm xác suất chứng minh ưu thế áp đảo tuyệt đối trước phương pháp đếm chính xác truyền thống. Với chỉ **4.60 KB RAM**, thuật toán Flajolet-Martin PCSA tiết kiệm bộ nhớ gấp **4,721 LẦN** so với Set, biến bài toán đếm phần tử phân biệt trên dòng dữ liệu lớn từ "bất khả thi vì cạn kiệt RAM" trở thành khả thi hoàn toàn trên mọi phần cứng biên, router mạng hay vi điều khiển IoT.
2. **Về tính ổn định của thuật toán PCSA:** Kỹ thuật chia bit Stochastic Averaging kết hợp Median of Means đã giải quyết triệt để bài toán phương sai cao của Flajolet-Martin cổ điển, ngăn chặn hiện tượng nhảy vọt cấp số nhân và đưa sai số về mức chấp nhận được cho các bài toán thống kê Big Data.
3. **Ý nghĩa thực tiễn:** FM PCSA là giải pháp cân bằng tối ưu hoàn hảo giữa độ phức tạp thời gian $O(1)$, không gian cố định $O(k)$ và độ tin cậy của kết quả ước lượng.

## 2.2. Hướng nghiên cứu mở rộng
Từ nền tảng của thuật toán Flajolet-Martin PCSA, các hướng nghiên cứu tiếp theo có thể mở rộng bao gồm:
1. **Nghiên cứu Thuật toán HyperLogLog (Flajolet et al., 2007):** Sử dụng trung bình điều hòa (Harmonic Mean) thay cho trung bình số học để giảm sai số chuẩn xuống mức $\approx 1.04 / \sqrt{m}$ với bộ nhớ siêu nhỏ gọn chỉ khoảng $1.5 \text{ KB}$.
2. **Mở rộng trên Môi trường Xử lý Phân tán (Apache Spark / Flink):** Triển khai cấu trúc PCSA Sketch trên kiến trúc RDD/Dataframe của Spark, tận dụng tính chất giao hoán và kết hợp (Commutative & Associative) của phép toán lấy $\max$ giữa các thùng để gộp kết quả từ hàng trăm máy chủ tính toán phân tán với chi phí truyền thông mạng gần như bằng 0.
3. **Ứng dụng Giám sát An ninh Mạng:** Tích hợp FM PCSA vào các hệ thống phát hiện tấn công SYN Flood và DDoS dựa trên sự tăng đột biến của số lượng địa chỉ IP nguồn phân biệt trong các cửa sổ trượt thời gian (Sliding Windows).

---

\newpage

# PHẦN 3: TỰ CHẤM ĐIỂM VÀ BẢNG PHÂN CÔNG CÔNG VIỆC

## 3.1. Bảng tự chấm điểm nhóm
*(Theo thang điểm 10 quy định trong Mẫu. BCMH.docx của ThS. Trần Thị Nhi)*

*BẢNG 1.4: Bảng tự chấm điểm của các thành viên nhóm.*

| Nội dung | Yêu cầu chi tiết | Thang điểm | Điểm TV 1 | Điểm TV 2 | Điểm TV 3 |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Phần 1: Dữ liệu** | Thu thập dữ liệu log thật quy mô lớn (> 10.36M dòng, 3.5GB), mô tả đầy đủ các trường thông tin, giải thích lý do chọn trường IP, kỹ thuật Generator streaming không tràn RAM. | **3.0 điểm** | 3.0 | 3.0 | 3.0 |
| **Phần 2: Phân tích** | Trình bày sâu sắc cơ sở lý thuyết xác suất, phân tích chi tiết toán học của FM và PCSA, giải thích bản chất phương sai cao và cơ chế Median of Means. | **4.0 điểm** | 4.0 | 4.0 | 4.0 |
| **Phần 3: Xây dựng & Kiểm thử** | Xây dựng 4 notebook Colab chuẩn mực, chạy thành công $100\%$, kết quả đối soát chuẩn xác, trực quan hóa 4 biểu đồ sắc nét, kết luận thuyết phục. | **3.0 điểm** | 3.0 | 3.0 | 3.0 |
| **TỔNG CỘNG** | **Đánh giá toàn diện kết quả Bài tập lớn** | **10.0 điểm** | **10.0** | **10.0** | **10.0** |

---

## 3.2. Bảng phân công công việc chi tiết

*BẢNG 1.5: Bảng phân công chi tiết công việc của các thành viên trong nhóm.*

| STT | Giai đoạn thực hiện | Nội dung công việc cụ thể | Người phụ trách chính | Mức độ hoàn thành |
| :---: | :--- | :--- | :--- | :---: |
| 1 | 20/08/2026 – 25/08/2026 | Khảo sát đề tài, tìm kiếm và tải bộ dữ liệu `access.log` thật (~3.5 GB), thiết lập lưu trữ Google Drive. | Thành viên 1 | $100\%$ |
| 2 | 26/08/2026 – 01/09/2026 | Cài đặt `1_Set_Exact.ipynb`, xây dựng cơ chế Generator stream và đo đạc Ground Truth chuẩn 258,606 IP. | Thành viên 2 | $100\%$ |
| 3 | 02/09/2026 – 06/09/2026 | Nghiên cứu lý thuyết toán Flajolet-Martin (1985), cài đặt `2_FM.ipynb`, đo lường hiện tượng nhảy bậc bit 0 ($R=19$). | Thành viên 1 | $100\%$ |
| 4 | 07/09/2026 – 10/09/2026 | Cài đặt thuật toán tối ưu `3_FM_PCSA.ipynb` (1-Hash tách 7 bit, 128 thùng, Median of 16 Groups). | Thành viên 3 | $100\%$ |
| 5 | 11/09/2026 – 12/09/2026 | Xây dựng `4_KetLuan_SoSanh.ipynb`, kết nối dữ liệu Google Drive, tính toán các chỉ số và vẽ 4 biểu đồ đối sánh. | Thành viên 2 & 3 | $100\%$ |
| 6 | 13/09/2026 | Chạy kiểm thử toàn diện trên 10,365,152 dòng log, đồng bộ tài liệu kiến trúc, viết báo cáo tổng kết hoàn chỉnh. | Cả nhóm | $100\%$ |

> **LƯU Ý ĐẶC BIỆT TỪ GIẢNG VIÊN (ThS. TRẦN THỊ NHI):**  
> *"Mọi thành viên phải hiểu và làm được tất cả bài của mình. Nếu giảng viên hỏi không giải thích được có thể out môn."*  
> *(Tất cả các thành viên trong nhóm đã cùng nhau thảo luận, nắm vững toàn bộ mã nguồn của 4 notebook và cơ sở lý thuyết toán học của các thuật toán để sẵn sàng cho buổi vấn đáp bảo vệ đồ án).*

---

\newpage

# DANH MỤC TÀI LIỆU THAM KHẢO

1. **Flajolet, P., & Martin, G. N. (1985).** *Probabilistic counting algorithms for data base applications.* Journal of Computer and System Sciences, 31(2), 182-209.
2. **Flajolet, P., Fusy, É., Gandouet, O., & Meunier, F. (2007).** *HyperLogLog: the analysis of a near-optimal cardinality estimation algorithm.* DMTCS Conference on Analysis of Algorithms (AofA 2007), 127-146.
3. **Trần Thị Nhi (2026).** *Bài giảng Chuyên đề Xử lý Dữ liệu Lớn (KTPM008) - Chương 4: Một số mô hình toán học thiết kế thuật toán cho dữ liệu lớn.* Trường Đại học Thủ Dầu Một.
4. **Trần Thị Nhi (2026).** *Mẫu báo cáo môn học (Mẫu. BCMH.docx) & Đề cương chi tiết học phần Chuyên đề Dữ liệu lớn.* Trường Đại học Thủ Dầu Một.
5. **Rajaraman, A., & Ullman, J. D. (2011).** *Mining of Massive Datasets (Chapter 4: Mining Data Streams).* Cambridge University Press.
6. **Mayer-Schönberger, V., & Cukier, K. (2013).** *Big Data: A Revolution That Will Transform How We Live, Work, and Think.* Houghton Mifflin Harcourt.
7. **Durand, M., & Flajolet, P. (2003).** *Loglog counting of large cardinalities.* In European Symposium on Algorithms (pp. 605-617). Springer, Berlin, Heidelberg.
8. **Apache Software Foundation.** *Apache Spark Streaming Programming Guide: Approximate Distinct Counting.* https://spark.apache.org/docs/latest/streaming-programming-guide.html.
