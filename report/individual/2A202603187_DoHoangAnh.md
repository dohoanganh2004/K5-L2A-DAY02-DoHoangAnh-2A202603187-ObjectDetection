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

1. **drive-01:** Phương tiện bị che khuất. Chỉ gán nhãn nếu phần còn nhìn thấy đủ để nhận diện class. Bounding Box bao quanh phần nhìn thấy.
2. **drive-03:** Hình ảnh bị mờ do chuyển động, không đủ thông tin nhận diện phương tiện. Không tự suy đoán class.
3. **drive-07:** Khó phân biệt `van` và `truck`. Không gán class nếu không đủ đặc điểm nhận diện.
4. **drive-06:** Phương tiện bị lẫn vào khu vực công trình, khó xác định ranh giới.
5. **drive-11:** Phương tiện chỉ xuất hiện một phần, hình ảnh không đủ rõ để nhận dạng.

**Cách áp dụng schema:** Chỉ sử dụng bốn class được quy định. Không tự tạo class mới, không đoán nhãn khi thiếu thông tin và ưu tiên tính chính xác của Bounding Box.

**Nội dung cần giảng viên làm rõ:**
- Ngưỡng tối thiểu về độ nhìn thấy để quyết định annotation đối với vật thể bị che khuất.
- Quy định xử lý vật thể quá nhỏ hoặc bị motion blur.
- Có cần bỏ hoàn toàn những trường hợp không thể xác định class hay có cơ chế đánh dấu riêng không?

## 3. Annotation và self-QC

- **Số file nhãn train:** 50/50 file `.txt` trong `dataset/labels/train/`.
- **Kết quả checker:** Chạy `python tools/check_yolo_labels.py` đạt 50/50 — không có lỗi cấu trúc, đúng 5 cột, class ID hợp lệ, box không vượt biên.
- **Danh sách ít nhất 10 ảnh đã rà bằng mắt:**
  - drive_001 — xe bị che khuất một phần
  - drive_002 — nhiều xe ở xa, khó phân biệt class
  - drive_006 — xe sát rìa ảnh
  - drive_007 — xe nhỏ ở xa gần đường chân trời
  - drive_008 — nhiều lớp xe chồng lên nhau
  - drive_011 — xe bị che bởi cây và biển hiệu
  - drive_018 — ảnh thưa xe, dễ nhầm
  - drive_021 — khó phân biệt van và truck
  - drive_028 — xe bị cắt rìa ảnh
  - drive_041 — nhiều xe nhỏ ở xa
- **Lỗi phát hiện khi rà:** Phát hiện một số box vẽ quá rộng không sát thân xe, một số xe nhỏ ở xa (dưới ~40px chiều cao) được label nhưng thực tế không cần thiết theo ngưỡng của gold. Một số trường hợp nhầm class giữa `van` và `truck`, giữa `bus` và `van`.
- **Commit khóa nhãn ban đầu:** `965c17b` — "Submit DAY02 object detection assignment".

## 4. Gold QC và rework

Kết quả đánh giá được lưu tại:

`outputs/annotation_evaluation.json`

| Chỉ số | Trước rework | Sau rework |
|---|---:|---:|
| TP | 443 | 428 |
| FP | 385 | 292 |
| FN | 210 | 225 |
| Precision | 0.535 | 0.5944 |
| Recall | 0.6784 | 0.6554 |
| F1 | 0.5982 | 0.6235 |
| mean_iou_of_matches | 0.7531 | 0.7545 |
| passed | false | false |

**Rework:** Có thực hiện rework. Sau khi nhận gold labels và chạy evaluator, F1 ban đầu là 0.5982 — chưa đạt ngưỡng 0.80. Tiến hành mở lại CVAT và sửa nhãn:

- **Nguyên nhân chính:** FP cao (385) — vẽ thừa nhiều box, đặc biệt xe nhỏ ở xa (dưới ~40px), xe bị che nhiều, xe sát rìa ảnh mà gold không label.
- **Ảnh đã sửa:** drive_018 (giảm từ 10 xuống 2 box), drive_028 (giảm từ 15 xuống 3 box), drive_008, drive_009, drive_011, drive_012, drive_030, drive_031 và một số ảnh khác.
- **Hành động:** Xóa các box xe quá nhỏ hoặc xa, đổi class sai (bus→van, van→truck) ở một số ảnh như drive_021.
- **Kết quả:** FP giảm từ 385 xuống 292, Precision tăng từ 0.535 lên 0.5944, F1 tăng từ 0.5982 lên 0.6235. mean IoU đạt 0.7545 (vượt ngưỡng 0.70). F1 chưa đạt 0.80.

## 5. Kết quả YOLO

- **Runtime:** Google Colab.
- **GPU/CPU:** CPU.
- **Model:** yolo26n.pt
- **Epochs:** 50
- **Batch:** auto (-1)
- **Image size:** 640
- **Seed:** 20260909
- **Các tham số thay đổi so với notebook:** Không thay đổi.
- **Best checkpoint:** epoch 1 — `outputs/yolo26_runs/vehicle_annotation/weights/best.pt`

### Kết quả trên 10 ảnh eval

| Chỉ số | Giá trị |
|---|---:|
| Precision | 0.4424 |
| Recall | 0.2517 |
| mAP50 | 0.2836 |
| mAP50–95 | 0.1737 |

### Kết quả dự đoán

- Thư mục dự đoán: `assets/eval_predictions/visuals/` 
- Thư mục YOLO runs: `assets/yolo26_runs/`

**Ví dụ ảnh cụ thể:** Trên 10 ảnh eval, mô hình dự đoán được một số xe `car` lớn ở gần nhưng bỏ sót nhiều xe nhỏ và hầu hết `truck`, `bus`, `van`. Điều này phù hợp với class imbalance trong tập train — `car` chiếm 653/828 box (~79%), trong khi `truck` chỉ có 18 box (~2%), `van` 59 box (~7%), `bus` 98 box (~12%).

### Trả lời bốn câu hỏi cuối notebook

**Câu 1. Lớp nào recall thấp nhất? Ảnh nào minh họa?**

`truck` có recall thấp nhất — chỉ có 18 box trong toàn bộ tập train, mô hình gần như không học được đặc trưng của class này. `truck` và `van` thường bị bỏ sót hoặc nhầm thành `car`. Ảnh drive_057 và drive_058 minh họa rõ — có xe tải nhưng mô hình không phát hiện.

**Câu 2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?**

Trường hợp này (mAP50=0.2836, mAP50–95=0.1737) cho thấy khoảng cách đáng kể. Cần kiểm tra độ chính xác của bounding box — mô hình phát hiện đúng vị trí xe ở IoU ngưỡng thấp (0.5) nhưng box chưa khít ở ngưỡng cao hơn (0.75, 0.90). Nguyên nhân có thể là annotation không nhất quán về độ sát box, một số box vẽ hơi lỏng so với thân xe thực tế.

**Câu 3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**

Nhầm lẫn giữa `van` và `truck` là lỗi có liên quan guideline. Trong quá trình annotation, drive_021 là ca điển hình — xe góc trái được label là `van` nhưng gold là `truck`, xe giữa label là `bus` nhưng gold là `van`. Guideline ban đầu chưa đủ rõ về ngưỡng phân biệt van (khoang liền) và truck (thùng tách rời), dẫn đến không nhất quán. Confusion matrix cũng cho thấy model hay nhầm giữa các class này.

**Câu 4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**

Không dùng 10 ảnh eval để train vì cần tập dữ liệu độc lập để đánh giá khả năng tổng quát hóa của mô hình. Nếu đưa vào train, mô hình có thể overfit và metrics trên eval sẽ không phản ánh thực tế.

Trong notebook, 10 ảnh eval đóng vai trò **validation set** — dùng để theo dõi loss và metrics sau mỗi epoch, từ đó chọn `best.pt` (checkpoint có mAP50 cao nhất trên val). Đây không phải test set độc lập nên kết quả không hoàn toàn khách quan như một test set thực sự.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Annotation trên CVAT | Khó xác định class xe bị che khuất hoặc quá nhỏ ở xa | Quan sát phần còn nhìn thấy, đối chiếu Mini Guideline | Một số trường hợp vẫn gán nhãn thừa so với gold |
| Phân loại class | Nhầm `van`/`truck`, `bus`/`van` ở một số ảnh (drive_021, drive_042) | So sánh cấu trúc thân xe — van liền khoang, truck có thùng tách rời | Sau rework đã sửa class sai ở một số ảnh |
| Self-QC | Vẽ thừa xe nhỏ ở xa (dưới ~40px) mà gold không label | Rà lại 10 ảnh, xóa box xe quá nhỏ | FP giảm từ 385 xuống 292 sau rework |
| Gold QC | F1 = 0.5982 sau lần chạy đầu, chưa đạt ngưỡng 0.80 | Chạy evaluator, phân tích FP/FN từng ảnh, sửa trong CVAT | F1 tăng lên 0.6235, mean IoU đạt 0.7545 nhưng F1 chưa pass |
| Chạy YOLO trên Colab | Train trên CPU, mỗi epoch ~53 giây, 50 epoch mất ~37 phút | Giữ nguyên tham số notebook, không thay đổi | Chạy hoàn thành, kết quả lưu trong assets |

## 7. Tự đánh giá theo rubric

| Hạng mục | Trạng thái |
|---|---|
| Schema 4 class | Đã xây dựng |
| Mini Guideline | Đã soạn — xem `GUIDELINE_MINI.md` |
| Annotation train | 50/50 file, checker đạt |
| Self-QC | Đã rà 10 ảnh, ghi nhận lỗi thực tế |
| Gold QC | Đã chạy evaluator — F1=0.6235, mean IoU=0.7545 |
| Rework | Đã thực hiện — FP giảm 93 box, F1 tăng từ 0.5982 lên 0.6235 |
| YOLO training | Đã chạy 50 epoch trên Colab — kết quả trong assets |
| Evaluation 10 ảnh | mAP50=0.2836, mAP50–95=0.1737 |
| Báo cáo cá nhân | Đã hoàn thiện |

**Tự đánh giá:** Đã thực hiện đầy đủ các bước từ annotation, self-QC, gold QC đến train YOLO. Điểm hạn chế là F1 annotation chưa đạt ngưỡng 0.80 sau rework — nguyên nhân chính là annotation ban đầu vẽ thừa nhiều xe nhỏ ở xa mà gold không label, và một số nhầm lẫn class giữa van/truck/bus. Kết quả mAP model thấp (0.2836) phù hợp với tập train nhỏ (50 ảnh) và class imbalance rõ rệt (car chiếm ~79% box).

## 8. Công cụ AI và tài liệu tham khảo

### Công cụ đã sử dụng

- **ChatGPT:** Sử dụng để tìm hiểu khái niệm Object Detection, Bounding Box, IoU, mAP, Precision, Recall; hỗ trợ giải thích lỗi kỹ thuật, hướng dẫn sử dụng CVAT và góp ý cách trình bày Mini Guideline, báo cáo.
- **CVAT:** Sử dụng để thực hiện và kiểm tra annotation.
- **Google Colab:** Sử dụng để chạy notebook và thực hiện các bước huấn luyện, đánh giá YOLO.
- **GitHub:** Sử dụng để quản lý mã nguồn, lưu trữ bài làm và commit kết quả.


### Tài liệu tham khảo

- Notebook bài tập Ngày 2 Object Detection.
- Tài liệu hướng dẫn và rubric của khóa học.
- Tài liệu CVAT: https://docs.cvat.ai/
- Tài liệu Ultralytics: https://docs.ultralytics.com/
- Tài liệu GitHub: https://docs.github.com/

