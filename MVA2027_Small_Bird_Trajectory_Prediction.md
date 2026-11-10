# MVA2027 — Small Multi-Object Trajectory Prediction for Spotting Birds Challenge

> **Cuộc thi dự đoán quỹ đạo chuyển động của nhiều đối tượng nhỏ (chim) từ video UAV — MVA2027.**  
> Tài liệu được biên soạn lại bằng tiếng Việt từ nội dung do người dùng cung cấp. Tên riêng, thuật ngữ, dữ liệu, mức thưởng và thời hạn được giữ theo nguồn.

## 1. Tổng quan (Overview)

Cuộc thi **Small Multi-Object Trajectory Prediction for Spotting Birds Challenge** được tổ chức trong khuôn khổ **MVA2027**. Cuộc thi kế thừa hai thử thách:

- **MVA2023 — SOD4SB:** Small Object Detection for Spotting Birds (phát hiện chim có kích thước nhỏ trong ảnh).
- **MVA2025 — SMOT4SB:** Small Multi-Object Tracking for Spotting Birds (theo dõi nhiều chim nhỏ trong video).

MVA2027 chuyển trọng tâm từ việc xác định **chim đang ở đâu** sang dự đoán **chim sẽ di chuyển đến đâu trong tương lai gần**.

Người tham gia được cung cấp một khoảng thời gian quan sát gồm **video từ thiết bị bay không người lái (UAV)** và **vị trí các chim đã được theo dõi với ID nhất quán**. Nhiệm vụ là dự đoán **quỹ đạo 2D tương lai** của từng đối tượng mục tiêu trong một khoảng thời gian dự báo cố định, không sử dụng khung hình hoặc nhãn của tương lai.

### 1.1. Ứng dụng thực tế

- **UAV tự hành:** dự đoán chuyển động của chim để chủ động tránh va chạm.
- **Nông nghiệp:** dự báo hướng bay gần cánh đồng, ruộng lúa để hỗ trợ giám sát và xua đuổi chim.
- **Quan trắc sinh thái:** phân tích hành vi di chuyển của chim phục vụ nghiên cứu và bảo tồn môi trường.

Mục tiêu của cuộc thi là thúc đẩy nghiên cứu thị giác máy tính dự đoán (*anticipatory perception*) và các hệ thống có khả năng nhận biết sự kiện trước khi chúng xảy ra.

## 2. Thông báo (Announcements)

- **15/11/2026:** Trang web cuộc thi được công bố (theo thông báo trong nội dung nguồn).

## 3. Bài toán (Task)

Với mỗi mẫu đánh giá, người tham gia phải:

1. Sử dụng **quỹ đạo chim trong khoảng quan sát** và **các khung hình video UAV tương ứng** làm đầu vào.
2. Dự đoán **quỹ đạo 2D tương lai** của mỗi chim được chỉ định là mục tiêu.
3. Dự báo trên toàn bộ khoảng thời gian yêu cầu mà **không truy cập video hoặc nhãn tương lai**.

### 3.1. Các thách thức kỹ thuật

| Thách thức | Mô tả |
|---|---|
| **Kích thước đối tượng nhỏ** | Chim có thể chỉ chiếm vài pixel, khó trích xuất tín hiệu chuyển động ổn định. |
| **Quỹ đạo phi tuyến mạnh** | Chim có thể tăng tốc, đổi hướng và rẽ đột ngột; ngoại suy vận tốc không đổi thường không đủ. |
| **Bất định của dự đoán** | Cùng một quỹ đạo quan sát có thể dẫn đến nhiều quỹ đạo tương lai hợp lý. |
| **Điều kiện UAV thực tế** | Camera chuyển động, ánh sáng thay đổi và nền ảnh phức tạp. |
| **Tương tác giữa chuyển động camera và đối tượng** | Cần phân biệt chuyển động do UAV/camera với chuyển động thực của chim. |

### 3.2. Các hướng tiếp cận được khuyến khích

- **Trajectory-based motion models:** học động lực học theo thời gian từ các tọa độ/quỹ đạo đã quan sát.
- **Video/optical-flow-based prediction:** tận dụng thông tin chuyển động thị giác, optical flow và bối cảnh cảnh quay.
- **Transformer, graph và mô hình xác suất:** mô hình hóa phụ thuộc thời gian dài, tương tác giữa đối tượng và mức độ bất định.

Mẫu dự đoán được hình thành bằng cách tách các quỹ đạo hợp lệ thành **đoạn quan sát** và **đoạn tương lai cần dự đoán**.

## 4. Bộ dữ liệu (Dataset)

Bộ dữ liệu kế thừa từ SOD4SB (MVA2023) và dữ liệu video UAV của SMOT4SB (MVA2025), được tổ chức thành hai phần.

### 4.1. Auxiliary Object Detection Data — dữ liệu ảnh bổ trợ

Gồm các ảnh tĩnh có gắn nhãn vị trí chim, dùng để tiền huấn luyện mô hình trích xuất đặc trưng thị giác hoặc bộ phát hiện chim.

| Tập dữ liệu | Số ảnh | Số cá thể chim được gắn nhãn |
|---|---:|---:|
| **Train1** (điều chỉnh từ tài liệu [2]) | 47.260 | 60.971 |
| **Train2** (tập con của SOD4SB [1]) | 9.759 | 29.037 |

### 4.2. Trajectory Prediction Dataset — dữ liệu video dự đoán quỹ đạo

- Bao gồm các chuỗi video UAV, bounding box của chim và **tracking ID nhất quán**.
- Mỗi mẫu cho biết quỹ đạo trong khoảng quan sát, còn phần tiếp theo là quỹ đạo mục tiêu cần dự báo.

| Phân tập | Trạng thái/kịch bản sử dụng |
|---|---|
| **Train** | Số lượng chưa công bố (**TBA**); có nhãn quỹ đạo tương lai để huấn luyện và kiểm định. |
| **Public Test** | Số lượng chưa công bố (**TBA**); ẩn nhãn tương lai và chấm điểm trên máy chủ công khai. |
| **Private Test** | Số lượng chưa công bố (**TBA**); phục vụ xếp hạng cuối cùng. |

### 4.3. Định dạng dữ liệu

**Đầu vào:** chuỗi khung hình video UAV và quỹ đạo chim trong đoạn quan sát, đi kèm bounding box và tracking ID.

**Đầu ra cần dự đoán:** chuỗi tọa độ tâm bounding box **(centroid) trong mặt phẳng ảnh 2D** tại các thời điểm tương lai.

Người tham gia có thể khai thác dữ liệu phát hiện ảnh bổ trợ và các khung hình quan sát để xây dựng đặc trưng thị giác. Tuy nhiên, **mục tiêu đánh giá chính** vẫn là dự đoán quỹ đạo tương lai trên *Trajectory Prediction Dataset*.

> **Liên kết bộ dữ liệu:** Nội dung nguồn chỉ hiển thị tên “MVA2027 trajectory prediction dataset”, chưa kèm URL trực tiếp.

## 5. Đánh giá (Evaluation)

Hai chỉ số đánh giá là:

- **ADE — Average Displacement Error:** sai số khoảng cách Euclid trung bình giữa vị trí dự đoán và vị trí thực tế qua toàn bộ khoảng thời gian dự báo.
- **FDE — Final Displacement Error:** sai số khoảng cách Euclid ở **bước thời gian cuối cùng** của dự báo.

Do đối tượng có kích thước rất nhỏ, phép đo sử dụng **tọa độ tâm bounding box 2D** thay vì độ chồng lấp bounding box (IoU). Điều này làm giảm mức nhạy quá cao của IoU với những chênh lệch vị trí nhỏ.

**Chỉ số quyết định thứ hạng cuối cùng là ADE; ADE càng thấp càng tốt.** FDE được công bố như chỉ số bổ sung, không quyết định thứ hạng.

Sau cuộc thi, báo cáo tổng kết sẽ phân tích mở rộng các yếu tố như ADE, FDE, sai số theo từng khoảng dự báo, tốc độ suy luận, độ phức tạp mô hình và chi phí tính toán.

> **Chi tiết công thức:** Nguồn có mục mở rộng “Trajectory Prediction Metrics Details”, nhưng phần công thức không xuất hiện trong văn bản được cung cấp.

## 6. Baseline

Ban tổ chức cho biết có mã nguồn baseline trên **GitHub Repository**.

> Văn bản nguồn không chứa URL trực tiếp của repository.

## 7. Các mốc thời gian (Important Dates)

**Múi giờ do ban tổ chức ghi: 23:59 PST.**

| Sự kiện | Thời hạn |
|---|---|
| Trang cuộc thi trực tuyến | **15/11/2026** |
| Công bố bộ dữ liệu và mã baseline | **20/11/2026** |
| Mở máy chủ Public Test | **20/11/2026** |
| Đóng máy chủ Public Test | **10/02/2027** |
| Hạn nộp mã nguồn/mô hình (*Code Submission Deadline*) | **01/03/2027** |
| Công bố kết quả Private Test | **29/03/2027** |
| Hạn nộp bài báo | **07/04/2027** |
| Thông báo kết quả phản biện | **10/04/2027** |
| Hạn nộp bản camera-ready | **20/04/2027** |

Lịch trình có thể thay đổi.

### 7.1. Lưu ý về mốc thời gian

- Ở phần ghi chú, nguồn lại ghi **24/03/2027** là ngày công bố kết quả Private Test, **không khớp** với ngày **29/03/2027** trong bảng lịch. Cần xác nhận với ban tổ chức trước khi lập kế hoạch nộp bài.
- Sau hạn nộp mã nguồn **01/03/2027**, ban tổ chức dự kiến chia sẻ thống kê số lượng bài dự thi hợp lệ.
- Người dự định viết paper được khuyến nghị **bắt đầu chuẩn bị bản thảo ngay sau hạn nộp mã**, không đợi kết quả cuối cùng, vì khoảng cách từ lúc có kết quả đến hạn nộp paper rất ngắn.

## 8. Giải thưởng (Prizes & Awards)

| Xếp hạng | Tiền thưởng (JPY) | Danh hiệu |
|---|---:|---|
| **Hạng 1** | **200.000** | Best Solution Award |
| **Hạng 2** | **150.000** | Runner-Up Solution Award |
| **Hạng 3** | **100.000** | Honorable Mention Solution Award |
| **Hạng 4** | **50.000** | — |
| **Hạng 5** | **50.000** | — |

Người nhận giải còn được **miễn phí tham dự MVA2027** theo chính sách của cuộc thi.

### 8.1. Best Booster Award

Giải dành cho cá nhân đóng góp tích cực nhất trong thảo luận trên Discord, dựa trên:

- Chất lượng nội dung thảo luận.
- Số lượt phản hồi/tương tác tích cực nhận được.

Người nhận giải này cũng được **miễn phí tham dự MVA2027**.

### 8.2. Cơ hội công bố nghiên cứu

- Các đội/cá nhân **xếp hạng 1–5** sẽ được mời đồng tác giả **báo cáo tổng kết challenge**; việc tham gia báo cáo này là **bắt buộc** đối với nhóm dẫn đầu theo thể lệ nguồn.
- Mỗi đội/cá nhân trong nhóm **1–5** có cơ hội nộp **bài báo kỹ thuật riêng** mô tả phương pháp đề xuất, theo định dạng và hướng dẫn của hội nghị MVA.
- Các bài báo trải qua **quy trình phản biện độc lập**. Nếu được chấp nhận, tác giả có thể trình bày tại phiên chuyên đề của challenge; nguồn cũng cho biết tác giả được lựa chọn trình bày dưới hình thức poster.
- Các đội dẫn đầu không mặc nhiên có bài báo được chấp nhận chỉ nhờ thứ hạng.

## 9. Đăng ký (Registration)

1. **Đăng ký tham gia challenge trên Codabench** để nhận thông báo qua email.
2. Theo hướng dẫn được gửi qua email, **tham gia kênh Discord** dành riêng cho người dự thi.
3. Theo dõi thông báo cập nhật dữ liệu, baseline, cách nộp bài và các thời hạn.

> **Codabench page:** Nguồn chỉ cung cấp tên liên kết, không có URL cụ thể.

## 10. Thảo luận (Discussion)

Ban tổ chức cung cấp một **kênh Discord riêng** để người tham gia thảo luận về cuộc thi. Cơ hội nhận **Best Booster Award** gắn với mức độ đóng góp hữu ích trên kênh này.

## 11. Nộp bài (Submission)

### 11.1. Giai đoạn Public Test

- Định dạng nộp: **tệp ZIP**.
- Nội dung: các kết quả **quỹ đạo tương lai dự đoán**, được lưu bằng **định dạng JSON**.
- Kết quả được đánh giá trên máy chủ Public Test.

### 11.2. Giai đoạn Private Test

- Nộp **mô hình đã huấn luyện** cùng **test scripts**.
- Kênh nộp: **Google Form** do ban tổ chức cung cấp.
- Hạn nộp mã/mô hình: theo lịch mục 7, **01/03/2027**.

### 11.3. Sau khi có kết quả

- Người đứng **hạng 1–5** được mời tham gia viết báo cáo chung.
- Nhóm này có thể nộp bài báo kỹ thuật riêng, qua phản biện của MVA.
- Hướng dẫn nộp cụ thể sẽ được ban tổ chức gửi trực tiếp cho người đạt giải.

> Nguồn không kèm đường dẫn Google Form hoặc schema JSON chi tiết. Cần sử dụng hướng dẫn chính thức khi ban tổ chức công bố.

## 12. Ban tổ chức (Challenge Organizers)

| Vai trò | Cá nhân/đơn vị | Tổ chức |
|---|---|---|
| Technical Event Chair | **Norimichi Ukita** | Toyota Technological Institute |
| Technical Event Chair | **Yuki Kondo** | TOYOTA Motor Corporation |
| Staff | **Nanonn KITBARAMEE** | Thai-Nichi Institute of Technology |
| Contributor | **Takayuki Yamaguchi** | Iwate Prefecture Coastal Regional Development Bureau |
| Adviser | **Masatsugu Kidode** | Nara Institute of Science and Technology |
| Technical Partner | **Prodrone Co., Ltd.** | Hỗ trợ thu thập dữ liệu UAV cho SMOT4SB, nền tảng của challenge dự đoán quỹ đạo |

## 13. Trích dẫn và tài liệu tham khảo (Citing & References)

Nếu sử dụng dữ liệu, thước đo đánh giá hoặc mã baseline của challenge trong nghiên cứu, người dùng cần trích dẫn các tài liệu liên quan bên dưới. Trích dẫn chính thức cho báo cáo MVA2027 sẽ được bổ sung sau khi báo cáo xuất bản.

**[1]** Y. Kondo, N. Ukita, T. Yamaguchi, H.-Y. Hou, M.-Y. Shen, C.-C. Hsu, E.-M. Huang, Y.-C. Huang, Y.-C. Xia, C.-Y. Wang, C.-Y. Lee, D. Huo, M. A. Kastner, T. Liu, Y. Kawanishi, T. Hirayama, T. Komamizu, I. Ide, Y. Shinya, X. Liu, G. Liang, and S. Yasui. “MVA2023 Small Object Detection Challenge for Spotting Birds: Dataset, Methods, and Results.” *Proceedings of the 18th International Conference on Machine Vision and Applications (MVA)*, 2023.  
https://www.mva-org.jp/mva2023/challenge

**[2]** S. Fujii, K. Akita, N. Ukita. “Distant Bird Detection for Safe Drone Flight and Its Dataset.” *Proceedings of the 17th International Conference on Machine Vision and Applications (MVA)*, 2021.

**[3]** A. Alahi, K. Goel, V. Ramanathan, A. Robicquet, L. Fei-Fei, and S. Savarese. “Social LSTM: Human Trajectory Prediction in Crowded Spaces.” *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR)*, 2016.

**[4]** C. Xu, J. Wang, W. Yang, and L. Yu. “Dot Distance for Tiny Object Detection in Aerial Images.” *Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshop (CVPRW)*, 2021.

**[5]** Y. Kondo, N. Ukita, R. Kanayama, Y. Yoshida, T. Yamaguchi, X. Yu, G. Liang, X. Liu, G.-Z. Wang, W.-T. Chu, B.-C. Chuang, J.-H. Lee, P.-T. Kuo, I.-H. Chu, Y.-S. Hsiao, C.-H. Wu, P.-Y. Wu, J.-C. Tsou, H.-C. Liu, C.-Y. Lee, Y.-F. Yang, K. Shigematsu, A. Shin, and B. Tran. “MVA 2025 Small Multi-Object Tracking for Spotting Birds Challenge: Dataset, Methods, and Results.” *Proceedings of the 19th International Conference on Machine Vision and Applications (MVA)*, 2025.  
https://www.mva-org.jp/mva2025/challenge

## 14. Liên hệ (Contact)

Theo nội dung nguồn, ban tổ chức cung cấp một **Google Form** liên hệ, nhưng văn bản được gửi **không chứa URL**.

---

### Ghi chú về chất lượng dữ liệu nguồn

- Nguồn ghi hai ngày khác nhau cho **Private Test results: 24/03/2027 và 29/03/2027**.
- Cuối văn bản có dòng **“Published on July 31st, 2027”**, không nhất quán về thời gian với thông báo mở trang ngày **15/11/2026** và bản quyền **© 2026**. Tài liệu này giữ nguyên thông tin mang tính lịch sử của nguồn và **không dùng dòng đó làm ngày xác nhận của cuộc thi**.
- Các tên liên kết **Codabench, GitHub Repository, Dataset và Google Form** không có URL trong văn bản được cung cấp, vì vậy không bổ sung đường dẫn suy đoán.
