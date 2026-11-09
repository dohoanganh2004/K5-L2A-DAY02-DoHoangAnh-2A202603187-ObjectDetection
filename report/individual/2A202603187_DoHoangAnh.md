# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- **Họ tên:** Đỗ Hoàng Anh
- **MSSV:** 2A202603187
- **Email:** dohoanhanh92004@gmail.com
- **Lớp:** L2A
- **Link repo bài làm:** https://github.com/dohoanganh2004/K5-L2A-DAY02-DoHoangAnh-2A202603187-ObjectDetection
- **Link Colab đã chạy:** https://colab.research.google.com/drive/1N8oN9vQEUBT84UrvsWB4KDwlDNW2ZjBK

## 2. Schema và mini guideline

Tài liệu Mini Guideline: `GUIDELINE_MINI.md`.

Dataset sử dụng bốn class:

- `car`: Xe chở người.
- `truck`: Xe vận chuyển hàng hóa.
- `bus`: Xe vận chuyển nhiều hành khách.
- `van`: Xe có thân kín, khoang lái và khoang phía sau thường được thiết kế liền nhau.

Ba quyết định annotation tiêu biểu và các trường hợp bổ sung:

1. **Image 1 – Occlusion:** Phương tiện bị che khuất. Chỉ gán nhãn nếu phần còn nhìn thấy đủ để nhận diện class. Bounding Box bao quanh phần nhìn thấy.
2. **Image 3 – Motion Blur:** Hình ảnh bị mờ do chuyển động, không đủ thông tin nhận diện phương tiện. Không tự suy đoán class.
3. **Image 7 – Ambiguous Class:** Khó phân biệt `van` và `truck`. Không gán class nếu không đủ đặc điểm nhận diện.
4. **Image 6 – Background Confusion:** Phương tiện bị lẫn vào khu vực công trình, khó xác định ranh giới.
5. **Image 11 – Unclear Object:** Phương tiện chỉ xuất hiện một phần, hình ảnh không đủ rõ để nhận dạng.

**Cách áp dụng schema:** Chỉ sử dụng bốn class được quy định. Không tự tạo class mới, không đoán nhãn khi thiếu thông tin và ưu tiên tính chính xác của Bounding Box.

**Nội dung cần giảng viên làm rõ:**
- Ngưỡng tối thiểu về độ nhìn thấy để quyết định annotation đối với vật thể bị che khuất.
- Quy định xử lý vật thể quá nhỏ hoặc bị motion blur.
- Có cần bỏ hoàn toàn những trường hợp không thể xác định class hay có cơ chế đánh dấu riêng không?

## 3. Annotation và self-QC

- **Số file nhãn train:** [CẦN BỔ SUNG – kiểm tra số file `.txt` trong thư mục labels/train].
- **Kết quả checker và lỗi đã xử lý:** [CẦN BỔ SUNG – lấy kết quả chạy checker thực tế].
- **Danh sách ít nhất 10 ảnh đã rà bằng mắt:**
  - Image 1
  - Image 3
  - Image 6
  - Image 7
  - Image 11
  - [BỔ SUNG 5 ẢNH ĐÃ RÀ THỰC TẾ]
- **Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:**
  Đã chú ý kiểm tra các trường hợp vật thể bị che khuất, ảnh mờ, phương tiện khó phân biệt với nền và các trường hợp khó xác định class. Các quyết định annotation được đối chiếu với Mini Guideline. [BỔ SUNG lỗi thực tế đã sửa nếu có].
- **Commit khóa nhãn ban đầu:** [CẦN BỔ SUNG – Git commit hash].

## 4. Gold QC và rework

Kết quả đánh giá được lưu tại:

`outputs/annotation_evaluation.json`

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | [CẦN BỔ SUNG] | [CẦN BỔ SUNG] |
| FP | [CẦN BỔ SUNG] | [CẦN BỔ SUNG] |
| FN | [CẦN BỔ SUNG] | [CẦN BỔ SUNG] |
| Precision | [CẦN BỔ SUNG] | [CẦN BỔ SUNG] |
| Recall | [CẦN BỔ SUNG] | [CẦN BỔ SUNG] |
| F1 | [CẦN BỔ SUNG] | [CẦN BỔ SUNG] |
| mean_iou_of_matches | [CẦN BỔ SUNG] | [CẦN BỔ SUNG] |
| passed | [CẦN BỔ SUNG] | [CẦN BỔ SUNG] |

**Rework:** [Ghi rõ có hoặc không có rework dựa trên lịch sử chỉnh sửa thực tế].

Nếu có rework, ghi rõ ảnh được kiểm tra, lỗi phát hiện, quyết định sửa hoặc giữ lại và lý do. Không suy luận nguyên nhân chỉ từ TP, FP hoặc FN.

## 5. Kết quả YOLO

- **Runtime:** [CẦN BỔ SUNG – Google Colab].
- **GPU/CPU:** [CẦN BỔ SUNG].
- **Ultralytics version:** [CẦN BỔ SUNG – lấy từ phiên chạy Colab].
- **Torch version:** [CẦN BỔ SUNG].
- **Các tham số thay đổi so với notebook:** [Ghi rõ nếu có, nếu không ghi Không thay đổi].
- **Best checkpoint:** [CẦN BỔ SUNG – đường dẫn best checkpoint trong phiên chạy].

### Kết quả trên 10 ảnh eval

| Chỉ số | Giá trị |
|---|---:|
| Precision | [CẦN BỔ SUNG] |
| Recall | [CẦN BỔ SUNG] |
| mAP50 | [CẦN BỔ SUNG] |
| mAP50–95 | [CẦN BỔ SUNG] |

### Kết quả dự đoán

- Thư mục dự đoán: `assets/eval_predictions/`
- Thư mục YOLO runs: `assets/yolo26_runs/`

**Ví dụ ảnh và vật thể cụ thể:** [CẦN BỔ SUNG – chọn một ảnh trong 10 ảnh eval, ghi class dự đoán, vị trí Bounding Box và kết quả quan sát được].

### Trả lời bốn câu hỏi cuối notebook

**Câu 1. Lớp nào recall thấp nhất? Ảnh nào minh họa?**

[CẦN BỔ SUNG theo bảng kết quả evaluation thực tế]. Recall thấp cho thấy mô hình bỏ sót nhiều đối tượng thuộc class tương ứng.

**Câu 2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?**

Cần kiểm tra độ chính xác của Bounding Box. Có thể mô hình xác định đúng class và phát hiện được vật thể nhưng khung bao chưa sát với vật thể thực tế. Cần kiểm tra các lỗi box quá rộng, quá hẹp, lệch vị trí hoặc không bao phủ đầy đủ phần vật thể.

**Câu 3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**

Cần kiểm tra sự nhầm lẫn giữa `van` và `truck`, vì hai class có một số đặc điểm ngoại hình tương đối giống nhau. Trong quá trình rà dataset, Image 7 là trường hợp khó phân biệt hai class này. Đây là dữ kiện để xem xét guideline có đủ rõ ràng hay không. Tuy nhiên, cần đối chiếu thêm kết quả dự đoán và nhãn thực tế trước khi kết luận lỗi mô hình có liên quan đến guideline.

**Câu 4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**

Không sử dụng 10 ảnh eval để train vì cần giữ dữ liệu đánh giá độc lập với dữ liệu huấn luyện. Nếu đưa các ảnh này vào training, kết quả đánh giá có thể không phản ánh chính xác khả năng tổng quát hóa của mô hình.

10 ảnh eval được sử dụng để kiểm tra khả năng dự đoán của mô hình trên dữ liệu chưa được sử dụng trong quá trình huấn luyện và đánh giá các chỉ số như Precision, Recall, mAP50 và mAP50–95.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Annotation trên CVAT | Khó xác định class của một số phương tiện bị che khuất. | Quan sát phần phương tiện còn nhìn thấy và đối chiếu Mini Guideline. | Không đoán class khi thiếu thông tin. |
| Rà soát dataset | Hình ảnh bị mờ hoặc phương tiện khó phân biệt với nền. | Kiểm tra trực tiếp từng ảnh. | Ghi nhận các trường hợp không đủ điều kiện nhận diện. |
| Phân loại phương tiện | Khó phân biệt `van` và `truck`. | So sánh cấu trúc thân xe và đặc điểm thiết kế. | Không gán nhãn nếu không đủ thông tin. |
| Chạy checker | [CẦN BỔ SUNG lỗi thực tế nếu có]. | [CẦN BỔ SUNG]. | [CẦN BỔ SUNG]. |
| Chạy YOLO trên Colab | [CẦN BỔ SUNG lỗi thực tế nếu có]. | [CẦN BỔ SUNG]. | [CẦN BỔ SUNG]. |

## 7. Tự đánh giá theo rubric

| Hạng mục | Trạng thái |
|---|---|
| Schema 4 class | Đã xây dựng |
| Mini Guideline | Đã soạn |
| Annotation train | Cần xác nhận số lượng file và chất lượng |
| Self-QC | Đã rà một số trường hợp khó; cần xác nhận đủ ít nhất 10 ảnh |
| Gold QC | Cần bổ sung kết quả JSON |
| Rework | Cần xác nhận |
| YOLO training | Cần đối chiếu kết quả Colab |
| Evaluation 10 ảnh | Cần bổ sung metrics và ví dụ ảnh |
| Báo cáo cá nhân | Đang hoàn thiện |

**Tự đánh giá:** Đã thực hiện các bước xây dựng schema, Mini Guideline và rà soát những trường hợp annotation khó. Các kết quả định lượng và một số minh chứng kỹ thuật cần được kiểm tra lại từ dữ liệu thực tế trước khi xác nhận hoàn thành.

## 8. Công cụ AI và tài liệu tham khảo

### Công cụ đã sử dụng

- **ChatGPT:** Sử dụng để tìm hiểu khái niệm Object Detection, Bounding Box, IoU, mAP, Precision, Recall; hỗ trợ giải thích lỗi kỹ thuật, hướng dẫn sử dụng CVAT và góp ý cách trình bày Mini Guideline, báo cáo.
- **CVAT:** Sử dụng để thực hiện và kiểm tra annotation.
- **Google Colab:** Sử dụng để chạy notebook và thực hiện các bước huấn luyện, đánh giá YOLO.
- **GitHub:** Sử dụng để quản lý mã nguồn, lưu trữ bài làm và commit kết quả.

**Phần tự kiểm tra:** Các quyết định gán nhãn, phân loại phương tiện, rà soát hình ảnh và kết quả thực nghiệm được đối chiếu với dữ liệu thực tế của bài làm. Nội dung do AI hỗ trợ được người học kiểm tra lại trước khi sử dụng.

### Tài liệu tham khảo

- Notebook bài tập Ngày 2 Object Detection.
- Tài liệu hướng dẫn và rubric của khóa học.
- Tài liệu CVAT: https://docs.cvat.ai/
- Tài liệu Ultralytics: https://docs.ultralytics.com/
- Tài liệu GitHub: https://docs.github.com/

