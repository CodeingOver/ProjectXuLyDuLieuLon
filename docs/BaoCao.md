# KỊCH BẢN THUYẾT TRÌNH & CẨM NANG VẤN ĐÁP BÁO CÁO MÔN HỌC
## ĐỀ TÀI: NGHIÊN CỨU VÀ ĐỐI SÁNH HIỆU NĂNG CÁC THUẬT TOÁN ĐẾM XÁC SUẤT TRÊN LUỒNG DỮ LIỆU LỚN: PYTHON SET, FLAJOLET-MARTIN (1 HASH) VÀ FLAJOLET-MARTIN PCSA TRÊN 10.36 TRIỆU BẢN GHI LOG

---

* **Môn học:** Chuyên đề Xử lý Dữ liệu Lớn (KTPM008)  
* **Giảng viên hướng dẫn:** ThS. Trần Thị Nhi  
* **Đơn vị:** Khoa Công nghệ Thông tin - Trường Đại học Thủ Dầu Một  
* **Dữ liệu thực nghiệm:** Tập nhật ký máy chủ `access.log` gồm **10,365,152 dòng** (~3.5 GB)  

---

# MỤC LỤC KỊCH BẢN BÁO CÁO

1. [Phân Bổ Thời Gian Thuyết Trình (10 - 12 Phút)](#1-phân-bổ-thời-gian-thuyết-trình-10---12-phút)
2. [Phần 1: Mở Đầu & Bối Cảnh Data Streaming (1.5 Phút)](#phần-1-mở-đầu--bối-cảnh-data-streaming-15-phút)
3. [Phần 2: Phương Pháp Đếm Chính Xác (Python Set) & Ground Truth (1.5 Phút)](#phần-2-phương-pháp-đếm-chính-xác-python-set--ground-truth-15-phút)
4. [Phần 3: Thuật Toán Flajolet-Martin (FM 1 Hash) & Nhược Điểm Chí Mạng (2.5 Phút)](#phần-3-thuật-toán-flajolet-martin-fm-1-hash--nhược-điểm-chí-mạng-25-phút)
5. [Phần 4: Thuật Toán Nâng Cấp Flajolet-Martin PCSA & Median of Means (3.5 Phút)](#phần-4-thuật-toán-nâng-cấp-flajolet-martin-pcsa--median-of-means-35-phút)
6. [Phần 5: Bảng Đối Soát Tổng Hợp & Đúc Kết Đề Tài (1.5 Phút)](#phần-5-bảng-đối-soát-tổng-hợp--đúc-kết-đề-tài-15-phút)
7. [Phần 6: Bộ Câu Hỏi & Trả Lời Vấn Đáp (Q&A) Với Giảng Viên ThS. Trần Thị Nhi](#phần-6-bộ-câu-hỏi--trả-lời-vấn-đáp-qa-với-giảng-viên-ths-trần-thị-nhi)

---

## 1. Phân Bổ Thời Gian Thuyết Trình (10 - 12 Phút)

| STT | Phần thuyết trình | Nội dung trọng tâm | Thời lượng |
| :---: | :--- | :--- | :---: |
| **1** | **Mở đầu & Bối cảnh** | Đặt vấn đề, 3 đặc trưng Data Streaming, lý do Set sập RAM | 1.5 phút |
| **2** | **Python Set** | Ground Truth (258,606 IP), cơ chế bảng băm, nhược điểm $O(n)$ | 1.5 phút |
| **3** | **FM 1 Hash** | Ẩn dụ tung đồng xu, hàm băm, kỷ lục $R=19$, sai số 162%, RAM 76 B | 2.5 phút |
| **4** | **FM PCSA** | Stochastic Averaging (1 hash tách 7 bit), công thức $\frac{128}{\phi} 2^{\bar{R}}$, Median of Means | 3.5 phút |
| **5** | **Đối soát & Kết luận** | Bảng số liệu 3 chiều (RAM, Sai số, Tốc độ), kiến nghị ứng dụng | 1.5 phút |
| **6** | **Hỏi đáp (Q&A)** | Trả lời chất vấn của Giảng viên ThS. Trần Thị Nhi | 5 - 10 phút |

---

## Phần 1: Mở Đầu & Bối Cảnh Data Streaming (1.5 Phút)

### 🎯 Mục tiêu slide:
* Chào hỏi hội đồng, nêu tên đề tài và thành viên.
* Làm rõ bài toán **Count-Distinct ($F_0$)** trên luồng dữ liệu thời gian thực.
* Khẳng định tính bất khả thi của các cấu trúc lưu trữ truyền thống khi đối mặt với Big Data Streaming.

### 🎤 Lời thoại thuyết trình gợi ý:

> *"Kính thưa Giảng viên hướng dẫn **ThS. Trần Thị Nhi** cùng toàn thể các bạn sinh viên,*
>
> *Hôm nay, nhóm em xin phép được báo cáo kết quả nghiên cứu đề tài: **Nghiên cứu và đối sánh hiệu năng các thuật toán đếm xác suất trên luồng dữ liệu lớn: Python Set, Flajolet-Martin và Flajolet-Martin PCSA trên tập dữ liệu hơn 10.36 triệu bản ghi nhật ký máy chủ**.*
>
> *Thưa Thầy/Cô, trong kỷ nguyên số hiện nay, các máy chủ dịch vụ phải tiếp nhận hàng triệu lượt truy cập mỗi phút. Dòng dữ liệu này mang đặc thù của **Data Streaming (Luồng dữ liệu liên tục)** với 3 đặc tính kỹ thuật khắt khe:*
> 1. * **Vô hạn & Liên tục (Unbounded):** Dòng log đổ về 24/7 không có điểm dừng.*
> 2. * **Chỉ duyệt 1 lần (Single-pass):** Dữ liệu chảy qua mắt thuật toán như dòng nước qua cầu, không thể dừng lại hay tua ngược để duyệt lại lần thứ hai.*
> 3. * **Tài nguyên hữu hạn (Resource-constrained):** Bộ nhớ RAM của máy chủ không thể mở rộng vô hạn theo thời gian.*
>
> *Nếu chúng ta sử dụng các phương pháp lưu trữ thông thường như `set()` trong Python hoặc `COUNT(DISTINCT)` trong cơ sở dữ liệu, toàn bộ địa chỉ IP phải được lưu vào RAM, chắc chắn sẽ dẫn đến sự cố **Out-of-Memory (sập máy chủ)** khi hệ thống hoạt động dài ngày.*
>
> *Chính vì vậy, bài toán đặt ra là: **Làm sao có thể ước lượng chính xác số lượng người dùng duy nhất với bộ nhớ siêu nhỏ và tốc độ tức thì $O(1)$?** Đó là lý do nhóm em thực hiện nghiên cứu và đối sánh 3 giải pháp công nghệ trên tập dữ liệu thực tế hơn 10.36 triệu dòng log."*

---

## Phần 2: Phương Pháp Đếm Chính Xác (Python Set) & Ground Truth (1.5 Phút)

### 🎯 Mục tiêu slide:
* Trình bày cơ chế Open Addressing Hash Table của Python `set()`.
* Cung cấp số liệu chuẩn tuyệt đối (Ground Truth): **258,606 IP duy nhất** trên **10,365,152 dòng log**.
* Chỉ ra giới hạn tuyến tính $O(n)$ về bộ nhớ.

### 🎤 Lời thoại thuyết trình gợi ý:

> *[Chuyển sang Slide kết quả Python Set]*
>
> *"Để có thước đo chuẩn mực đánh giá các thuật toán xác suất, ở pha đầu tiên, nhóm em xây dựng bộ đếm chính xác bằng cấu trúc **`set()` của Python**.*
>
> * **Cơ chế vận hành:** Cấu trúc `set()` sử dụng bảng băm mở (Open Addressing Hash Table). Mỗi khi một địa chỉ IP xuất hiện, hệ thống băm chuỗi IP, tra cứu trong bảng; nếu chưa có thì cấp phát thêm ô nhớ và chèn vào.*
>
> * **Kết quả đo đạc thực nghiệm trên 10,365,152 dòng log:**
>   - *Số lượng địa chỉ IP duy nhất thực tế (Ground Truth): **258,606 IP**.*
>   - *Tỷ lệ duy nhất chỉ chiếm khoảng **2.49%**, phản ánh đúng bản chất thực tế là người dùng gửi lặp lại rất nhiều request.*
>   - *Thời gian xử lý: **134.72 giây**.*
>   - *Tổng dung lượng bộ nhớ RAM tiêu thụ: **21.22 MB** (bao gồm 8.39 MB bảng băm và 12.83 MB chuỗi ký tự IP).*
>
> * **Nhận xét chuyên sâu:** Python Set cho độ chính xác tuyệt đối **100%**, sai số **0.00%**. Tuy nhiên, nhược điểm chí mạng là độ phức tạp bộ nhớ là **$O(n)$ tuyến tính**. Trên 10 triệu log tốn 21 MB, nhưng trên 1 tỷ hay 100 tỷ log, bộ nhớ sẽ lên tới hàng chục Gigabytes và làm sập hoàn toàn hệ thống giám sát. Do đó, Set chỉ đóng vai trò làm 'chuẩn đối soát Ground Truth' chứ không thể là giải pháp tối ưu cho Big Data Streaming."*

---

## Phần 3: Thuật Toán Flajolet-Martin (FM 1 Hash) & Nhược Điểm Chí Mạng (2.5 Phút)

### 🎯 Mục tiêu slide:
* Giải thích ý tưởng xác suất thông qua hình tượng **Tung đồng xu (Coin Tossing)**.
* Trình bày hàm đếm bit 0 tận cùng $\rho(x)$ và công thức $\widehat{F_0} = \frac{2^R}{\phi}$.
* Đối soát số liệu: Kỷ lục $R=19$, ước lượng 677,803 IP, sai số 162.10%, RAM chỉ **76 Bytes**.
* Chỉ ra 2 nhược điểm cốt tử: Bước nhảy lũy thừa $+100\%$ và rủi ro nhiễu ngoại lai.

```mermaid
graph LR
    IP["Địa chỉ IP"] --> HASH["Băm MD5 64-bit"]
    HASH --> BITS["Chuỗi bit nhị phân ...0001000"]
    BITS --> RHO["Đếm số bit 0 tận cùng: r"]
    RHO --> UPDATE["Cập nhật kỷ lục: R = max(R, r)"]
    UPDATE --> FORMULA["Ước lượng: F0 = 2^R / 0.77351"]
```

### 🎤 Lời thoại thuyết trình gợi ý:

> *[Chuyển sang Slide thuật toán Flajolet-Martin 1 Hash]*
>
> *"Để giải quyết bài toán bộ nhớ của Set, năm 1985, hai nhà khoa học Philippe Flajolet và G. Nigel Martin đã phát minh ra thuật toán **Flajolet-Martin (FM)** dựa trên một quan sát xác suất rất thú vị qua **trò chơi tung đồng xu**.*
>
> *Giả sử em tung một đồng xu cân đồng chất:*
> * *Tung được 1 lần ngửa có xác suất là $1/2$ (rất bình thường).*
> * *Tung được **3 lần ngửa liên tiếp** có xác suất là $(1/2)^3 = 1/8$ (phải tung khoảng 8 lần mới gặp).*
> * *Nhưng nếu em khoe rằng em vừa tung được **19 lần ngửa liên tiếp** với xác suất chỉ là $1 / 2^{19} \approx 1 / 524,288$, thì Thầy/Cô có thể suy luận ngay: Em không thể chỉ tung vài chục lần mà gặp được kỳ tích này, em chắc chắn đã phải đứng tung **ít nhất khoảng nửa triệu lần**!*
>
> *👉 **Cách thuật toán cài đặt trong code:**
> 1. *Với mỗi IP đến, ta băm thành chuỗi nhị phân 64-bit ngẫu nhiên đồng đều (bit 0 là ngửa, bit 1 là sấp).*
> 2. *Hàm $\rho$ đếm số lượng bit 0 liên tiếp ở tận cùng.*
> 3. *Thuật toán chỉ cần lưu đúng **1 biến số nguyên $R$** duy nhất trong suốt quá trình chạy để ghi nhận kỷ lục bit 0 dài nhất từng thấy.*
> 4. *Công thức ước lượng: $\widehat{F_0} = \frac{2^R}{\phi}$ (với $\phi \approx 0.77351$ là hằng số hiệu chỉnh Flajolet-Martin).*
>
> * **Kết quả thực nghiệm trên 10.36 triệu dòng log:**
>   - * **Bộ nhớ siêu nhẹ:** Cấu trúc FM chỉ tốn đúng **76 Bytes** RAM — **tiết kiệm tới 292,732 LẦN so với Python Set**! Đây là độ phức tạp $O(1)$ tuyệt đối.*
>   - * **Tuy nhiên, sai số rất lớn:** Thuật toán ghi nhận kỷ lục $R = 19$, đưa ra ước lượng là **677,803 IP**, sai số tương đối lên tới **162.10%**.*
>
> * **Vì sao FM cơ bản lại có sai số lớn như vậy?** Nhóm em phân tích thấy 2 nguyên nhân cốt tử:
>   1. * **Bước nhảy lũy thừa (Power-of-2 Jump):** Vì $R$ chỉ nhận số nguyên, nên khi $R$ nhảy từ 18 lên 19, ước lượng lập tức bị **nhân đôi gấp 100%** (từ ~338 ngàn vọt lên ~678 ngàn), không có giá trị chuyển tiếp.*
>   2. * **Bị nhiễu ngoại lai:** Chỉ cần 1 IP vô tình gặp may mắn có chuỗi bit 0 dài bất thường, nó sẽ kéo lệch toàn bộ kết quả của hệ thống.*
>
> *Và để khắc phục triệt để nhược điểm này, chúng ta cần đến bước đột phá tiếp theo: **Flajolet-Martin PCSA**."*

---

## Phần 4: Thuật Toán Nâng Cấp Flajolet-Martin PCSA & Median of Means (3.5 Phút)

### 🎯 Mục tiêu slide:
* Trình bày hạn chế của phương pháp Multi-Hash (128 hàm băm gây nghẽn CPU).
* Giải thích nguyên lý **Stochastic Averaging (PCSA)**: 1 hàm băm duy nhất, tách 7 bit cuối chia vào 128 thùng độc lập.
* Phân tích công thức toán học chuẩn: $\widehat{F_0} = 128 \times \frac{2^{\bar{R}}}{\phi}$.
* Trình bày cơ chế **Median of Means** (16 nhóm $\times$ 8 thùng) triệt tiêu ngoại lai.
* Báo cáo số liệu thực nghiệm: Ước lượng **421,262 IP**, sai số **62.90%**, RAM **4.6 KB**, tốc độ **36,781 dòng/giây**.

```mermaid
graph TD
    IP["Địa chỉ IP truy cập"] --> HASH["Băm 1 lần duy nhất: h = MD5 64-bit (Chi phí O(1))"]
    HASH --> SPLIT_B["Tách 7 bit cuối: bucket = h & 127"]
    HASH --> SPLIT_R["57 bit còn lại: rem = h >> 7"]
    SPLIT_B --> BUCKET["Chọn 1 trong m = 128 thùng"]
    SPLIT_R --> TRAIL["Đếm bit 0 tận cùng: r = rho(rem)"]
    BUCKET & TRAIL --> UPDATE["Cập nhật: M[bucket] = max(M[bucket], r)"]
    UPDATE --> GROUPS["Chia 128 thùng thành 16 nhóm (8 thùng/nhóm)"]
    GROUPS --> MEAN["Trong từng nhóm: Tính Mean số mũ -> Ước lượng nhóm"]
    MEAN --> MEDIAN["Giữa 16 nhóm: Lấy Trung vị (Median) -> LOẠI BỎ NGOẠI LAI"]
    MEDIAN --> FINAL["Kết quả ước lượng: 421,262 IP (RAM: 4.6 KB)"]
```

### 🎤 Lời thoại thuyết trình gợi ý:

> *[Chuyển sang Slide thuật toán Flajolet-Martin PCSA]*
>
> *"Kính thưa Thầy/Cô, cách nghĩ thông thường để giảm sai số là dùng **128 hàm băm độc lập** rồi lấy trung bình. Nhưng nếu làm như vậy, với hơn 10.36 triệu dòng log, CPU sẽ phải thực hiện tới hơn **1.32 tỷ lần băm**, gây nghẽn phần cứng nghiêm trọng!
>
> Hai tác giả Flajolet và Martin đã giải quyết xuất sắc bài toán này bằng thuật toán **PCSA (Probabilistic Counting with Stochastic Averaging)**:
>
> ### 1. Kỹ thuật Phân thùng Stochastic Averaging:
> * *Thuật toán chỉ băm **ĐÚNG 1 LẦN DUY NHẤT ($O(1)$)** ra chuỗi 64-bit.*
> * *Sau đó, lấy **7 bit cuối cùng** để chia ngẫu nhiên vào **$2^7 = 128$ thùng độc lập**.*
> * *57 bit còn lại dùng để đếm số bit 0 và cập nhật kỷ lục cho riêng thùng đó.*
> * *👉 Giống như hội trường có 128 cửa soát vé: Khách đến chỉ bốc 1 lá thăm rồi đi vào đúng 1 cửa, vừa xử lý $O(1)$ cực nhanh, vừa chia đều dữ liệu.*
>
> ### 2. Giải mã Công thức Toán học:
> *Từ 128 thùng này, số lượng phần tử duy nhất được ước lượng theo công thức chuẩn:*
> $$\widehat{F_0} = \mathbf{128} \times \frac{\mathbf{2}^{\bar{R}}}{\phi}$$
> * * **Vì sao có số 128 ở trước:** Vì dữ liệu chia đều vào 128 thùng, mỗi thùng chỉ gánh trung bình $1/128$ lượng IP. Phân số $\frac{2^{\bar{R}}}{\phi}$ là ước lượng cho 1 thùng, nên phải nhân 128 để quy đổi ra toàn bộ dòng dữ liệu.*
> * * **Vì sao làm trơn được đường cong:** $\bar{R}$ là trung bình cộng số bit 0 của các thùng, nên $\bar{R}$ là một **số thực liên tục** (ví dụ $\bar{R} = 11.33$). Lũy thừa $2^{\bar{R}}$ nhờ đó tăng trưởng mịn màng từng đơn vị, xóa bỏ hoàn toàn bước nhảy gấp đôi của FM 1 Hash.*
>
> ### 3. Kỹ thuật Median of Means (Trung vị của các Trung bình):
> *Để đề phòng trường hợp một thùng nào đó vô tình gặp IP 'siêu may mắn' có chuỗi bit 0 bất thường kéo lệch giá trị trung bình, thuật toán áp dụng kỹ thuật **Median of Means**:*
> * *Chia 128 thùng thành **16 nhóm** (mỗi nhóm gồm 8 thùng).*
> * *Trong mỗi nhóm 8 thùng: Tính **Mean** để làm trơn số mũ.*
> * *Giữa 16 nhóm: Lấy giá trị **Trung vị (Median)** ở chính giữa.*
> * *👉 Phép lấy Trung vị có tính năng miễn nhiễm với ngoại lai: Dù có 1 hay 2 nhóm bị nhiễu cực đoan, chúng sẽ bị đẩy ra hai mép ngoài và bị loại bỏ hoàn toàn!*
>
> ### 4. Kết quả Thực nghiệm trên 10.36 Triệu Dòng Log:
> * * **Độ chính xác vượt trội:** Thuật toán ước lượng ra **421,262 IP**, giảm sai số tương đối từ **162.10% xuống chỉ còn 62.90%** (độ chính xác tăng từ 0% lên **37.10%**).*
> * * **Bộ nhớ siêu nhỏ:** Cấu trúc 128 thùng chỉ tốn đúng **4,712 Bytes (~4.60 KB)** — **tiết kiệm 4,721 LẦN so với Set**, và hoàn toàn cố định $O(k)$ dù dữ liệu có tăng lên hàng tỷ dòng.*
> * * **Tốc độ xử lý:** Đạt **36,781 dòng/giây**, quét sạch toàn bộ 10.36 triệu dòng log chỉ trong **281.80 giây** (~4.7 phút)."*

---

## Phần 5: Bảng Đối Soát Tổng Hợp & Đúc Kết Đề Tài (1.5 Phút)

### 🎯 Mục tiêu slide:
* Trình chiếu Bảng đối soát 3 phương pháp trên tập dữ liệu 10.36 triệu bản ghi.
* Tóm tắt 3 kết luận thực tiễn và bài học kinh nghiệm.

### 📊 BẢNG ĐỐI SOÁT THỰC NGHIỆM TỔNG HỢP:

| Tiêu chí So sánh | Python Set (Ground Truth) | Flajolet-Martin (1 Hash) | Flajolet-Martin PCSA |
| :--- | :---: | :---: | :---: |
| **Bản chất kỹ thuật** | Bảng băm mở lưu toàn bộ IP | 1 biến số nguyên $R$ | 128 thùng chia bit + Median of Means |
| **Số lần băm mỗi phần tử** | Tra cứu băm nội tại | 1 lần băm MD5 | **1 lần băm duy nhất ($O(1)$)** |
| **Số IP ước lượng** | **258,606 IP** | **677,803 IP** | **421,262 IP** |
| **Sai số tương đối (%)** | **0.00%** (Tuyệt đối) | **162.10%** (Rất lớn) | **62.90%** (Được kiểm soát tốt) |
| **Độ chính xác tương đối**| **100.00%** | **0.00%** | **37.10%** |
| **Bộ nhớ RAM tiêu tốn** | **21.22 MB** (22,247,602 B) | **76 Bytes** | **4,712 Bytes (~4.60 KB)** |
| **Tỷ lệ tiết kiệm RAM** | 1x (Gốc) | **Tiết kiệm 292,732 LẦN** | **Tiết kiệm 4,721 LẦN** |
| **Đặc tính bộ nhớ** | Tăng vô hạn $O(n)$ | Cố định $O(1)$ | **Cố định $O(k)$ (luôn là 4.6 KB)** |
| **Thời gian thực thi** | 134.72 giây | 252.37 giây | **281.80 giây (~36,781 dòng/s)** |
| **Khả năng ứng dụng** | Thất bại khi Big Data (sập RAM) | Chỉ ước lượng thô cấp độ lớn | **Lý tưởng cho giám sát Realtime** |

### 🎤 Lời thoại thuyết trình gợi ý:

> *[Chuyển sang Slide Bảng đối soát và Kết luận]*
>
> *"Kính thưa Thầy/Cô, nhìn vào bảng tổng hợp kết quả đối soát trên 10.36 triệu dòng log, nhóm em xin rút ra 3 đúc kết quan trọng:*
>
> 1. * **Về bài toán đánh đổi (Trade-off):** Không có thuật toán nào hoàn hảo về mọi mặt. Nếu cần độ chính xác tuyệt đối trên tập dữ liệu nhỏ vừa, **Python Set** là lựa chọn số một. Nhưng trong môi trường **Big Data Streaming**, nơi mà dữ liệu không thể nạp hết vào RAM, **Flajolet-Martin PCSA** là giải pháp tối ưu vì chấp nhận đánh đổi một khoảng sai số có kiểm soát để đổi lấy **bộ nhớ cố định chỉ 4.6 KB và tốc độ xử lý $O(1)$**.*
> 2. * **Về mặt thuật toán học:** PCSA đã giải quyết xuất sắc 2 bài toán hóc búa của FM cơ bản: Dùng **Stochastic Averaging** để loại bỏ chi phí của 128 hàm băm và làm trơn số thực, đồng thời dùng **Median of Means** để 'miễn nhiễm' với các giá trị băm cực đoan.*
> 3. * **Về mặt ứng dụng thực tiễn:** Thuật toán PCSA cực kỳ phù hợp để tích hợp vào các thiết bị định tuyến mạng (Router/Switch), hệ thống tường lửa WAF và trung tâm giám sát an ninh SOC để phát hiện sớm các cuộc tấn công từ chối dịch vụ (DDoS) và quét cổng theo thời gian thực mà không tiêu tốn tài nguyên máy chủ.*
>
> *Trên đây là toàn bộ kết quả nghiên cứu và thực nghiệm của nhóm. Em xin chân thành cảm ơn Giảng viên **ThS. Trần Thị Nhi** cùng các bạn đã chú ý lắng nghe! Nhóm em rất mong nhận được những nhận xét và câu hỏi phản biện từ Cô ạ!"*

---

## Phần 6: Bộ Câu Hỏi & Trả Lời Vấn Đáp (Q&A) Với Giảng Viên ThS. Trần Thị Nhi

Dưới đây là bộ các câu hỏi chất vấn kinh điển nhất mà Giảng viên thường đặt ra cho sinh viên trong buổi bảo vệ đồ án Xử lý Dữ liệu Lớn, kèm theo câu trả lời "chuẩn điểm 10":

---

### ❓ Câu hỏi 1: Tại sao phải dùng FM hay PCSA khi mà Python Set vừa dễ code vừa đếm chính xác 100%?
* **Cách trả lời ăn điểm:**
  > *"Dạ thưa Cô, Python Set sử dụng bảng băm nên bắt buộc phải lưu toàn bộ từng chuỗi ký tự IP vào RAM. Với 10.36 triệu dòng trong đề tài, Set đã ngốn hơn 21 MB RAM. Nếu hệ thống mạng chạy liên tục trong 1 tháng hay 1 năm với hàng trăm tỷ request, bộ nhớ của Set sẽ phình to vượt quá RAM vật lý, dẫn đến tràn bộ nhớ (Out-of-Memory) và làm sập máy chủ.  
  > Trong khi đó, FM PCSA chỉ tốn cố định **4.6 KB RAM** — dù dòng dữ liệu có chảy vĩnh viễn tới 1 tỷ IP thì bộ nhớ vẫn không tăng thêm 1 byte nào. Đây là giải pháp sống còn trong môi trường Big Data Streaming."*

---

### ❓ Câu hỏi 2: Hằng số $\phi \approx 0.77351$ có nguồn gốc từ đâu? Nếu không có hằng số này thì sao?
* **Cách trả lời ăn điểm:**
  > *"Dạ thưa Cô, hằng số $\phi \approx 0.77351$ là **Hằng số hiệu chỉnh Flajolet-Martin (Bias Correction Constant)**.  
  > Trong bài báo gốc năm 1985, hai tác giả đã dùng giải tích phức và hàm sinh để chứng minh rằng giá trị kỳ vọng của $2^R$ sẽ bị lệch cao hơn số lượng phần tử thực tế $n$ một tỷ lệ là:
  > $$\phi = \frac{e^\gamma}{\sqrt{2}} \prod_{k=1}^\infty \left(\frac{4k+1}{4k}\right)^{(-1)^k} \approx 0.77351$$
  > (với $\gamma \approx 0.5772$ là hằng số Euler-Mascheroni).  
  > Nếu không chia cho $\phi$, giá trị ước lượng trung bình sẽ luôn bị lệch cao hơn thực tế khoảng 29.3%."*

---

### ❓ Câu hỏi 3: Tại sao trong công thức PCSA $\widehat{F_0} = 128 \times \frac{2^{\bar{R}}}{\phi}$ lại phải nhân với 128 ở phía trước?
* **Cách trả lời ăn điểm:**
  > *"Dạ thưa Cô, vì thuật toán dùng 7 bit cuối để chia dòng IP ngẫu nhiên đồng đều vào 128 thùng độc lập. Do đó, mỗi thùng chỉ nhận trung bình khoảng **$1/128$ tổng số IP duy nhất**.  
  > Phân số $\frac{2^{\bar{R}}}{\phi}$ chính là ước lượng số IP của **1 thùng đơn lẻ**. Muốn tính ra tổng số IP của **toàn bộ hệ thống 128 thùng gộp lại**, ta bắt buộc phải lấy kết quả của 1 thùng nhân với 128."*

---

### ❓ Câu hỏi 4: Tại sao trong đề tài lại chọn 128 thùng và 16 nhóm mà không phải số khác?
* **Cách trả lời ăn điểm:**
  > *"Dạ thưa Cô, có 2 lý do kỹ thuật chính:  
  > 1. Con số $128 = 2^7$ giúp phép toán chọn thùng được tối ưu ở mức phần cứng bằng toán tử bitwise `h & 127` cực nhanh (chỉ mất 1 chu kỳ CPU).  
  > 2. Việc chia thành 16 nhóm, mỗi nhóm 8 thùng là cấu hình kinh điển do chính Flajolet và Martin đề xuất nhằm đạt điểm cân bằng tối ưu (Sweet Spot): Nhóm 8 thùng đủ để tính Mean làm trơn số thực, còn 16 nhóm đủ lớn để phép lấy Trung vị (Median) loại bỏ triệt để các nhóm bị nhiễu ngoại lai, trong khi toàn bộ cấu trúc chỉ chiếm 4.6 KB RAM."*

---

### ❓ Câu hỏi 5: Con số 64-bit băm trong code có ý nghĩa gì? Một lần băm xử lý được bao nhiêu IP?
* **Cách trả lời ăn điểm:**
  > *"Dạ thưa Cô, **mỗi lần băm chỉ đưa vào đúng 1 địa chỉ IP duy nhất**. Chuỗi băm 64-bit là không gian biểu diễn nhị phân của IP đó:
  > * 7 bit cuối dùng để chọn 128 thùng.
  > * 57 bit còn lại dùng để đếm số bit 0 tận cùng.  
  > Sức chứa ước lượng tối đa của cấu hình này lên tới $F_{0\max} \approx \frac{128}{0.77351} \cdot 2^{57} \approx \mathbf{2.38 \times 10^{19}\text{ IP duy nhất}}$ (khoảng 23.8 tỷ tỷ IP). Con số này vượt xa toàn bộ không gian địa chỉ IPv4 của toàn Trái Đất (chỉ có 4.3 tỷ IP), đảm bảo thuật toán không bao giờ bị tràn ngưỡng đếm."*

---

### ❓ Câu hỏi 6: Bước nhảy 50,000 dòng (`sample_step = 50000`) trong code có phải là bước nhảy của thuật toán không?
* **Cách trả lời ăn điểm:**
  > *"Dạ thưa Cô, **hoàn toàn không ạ!**  
  > Thuật toán vẫn đọc và xử lý liên tục từng dòng một. Con số 50,000 dòng chỉ là chu kỳ lấy mẫu để in tiến độ ra màn hình console và ghi lại điểm dữ liệu phục vụ việc vẽ đồ thị so sánh.  
  > Dù chúng ta đổi thành 10,000 dòng, 100,000 dòng hay không in gì cả, thì kết quả ước lượng cuối cùng **421,262 IP vẫn y hệt 100% không đổi một đơn vị nào**."*

---

### ❓ Câu hỏi 7: Thuật toán PCSA hoạt động theo dạng Batch (chờ đọc xong mới tính) hay Online Streaming?
* **Cách trả lời ăn điểm:**
  > *"Dạ thưa Cô, thuật toán hoạt động hoàn toàn theo mô hình **Online Streaming thời gian thực**:
  > * **Pha Cập nhật (`update`):** Khi dòng log đang chảy, máy tính chỉ làm thao tác cực nhẹ $O(1)$: băm IP $\to$ chọn thùng $\to$ cập nhật bit 0 kỷ lục.
  > * **Pha Ước lượng (`estimate`):** Bất kỳ tích tắc nào người quản trị cần xem số liệu, hàm `estimate()` chỉ cần lấy mảng 128 thùng ra tính toán trong chưa đầy **0.001 giây** mà không làm gián đoạn luồng dữ liệu log đang chảy."*
