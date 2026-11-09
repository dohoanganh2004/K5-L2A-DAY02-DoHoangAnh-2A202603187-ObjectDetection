# Mini Guideline — Bài làm cá nhân

## 1. Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe  được thiết kế chủ yếu để chở người | Sedan, SUV, hatchback. Thường có kích thước nhỏ hơn bus và truck. |
| `truck` | Xe được thiết kế chủ yếu để vận chuyển hàng hóa. | Xe tải thùng, xe tải ben. Thường có cabin và thùng chở hàng riêng biệt. |
| `bus` | Xe được thiết kế để vận chuyển nhiều hành khách. | Xe buýt công cộng, xe buýt trường học. Thường có thân xe lớn, dài và nhiều cửa sổ. |
| `van` | Xe có thân kín, khoang lái và khoang phía sau thường được thiết kế liền nhau. | Cargo van, passenger van. Thường có thân xe dạng hộp, có thể có cửa trượt bên hông. |

## 2. Quy tắc chung

- **Box bám sát vật thể nghĩa là:** Bounding Box phải bao quanh sát phần nhìn thấy của phương tiện, không lấy quá nhiều phần nền xung quanh và không cắt mất phần xe đang nhìn thấy.
- **Vật thể bị che khuất:** Vẫn gán nhãn nếu có thể nhận diện được loại xe. Box chỉ bao quanh phần vật thể nhìn thấy, không tự đoán phần bị che.
- **Vật thể bị cắt bởi rìa ảnh:** Vẫn gán nhãn nếu có thể xác định được đó là một trong bốn class. Box bao quanh phần xe nằm trong ảnh.
- **Vật thể rất nhỏ/mơ hồ:** Nếu không đủ thông tin để xác định class thì không tự đoán nhãn. Cần xem xét theo quy định dataset và ghi lại trường hợp chưa chắc chắn để kiểm tra.

## 3. Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001.jpg`  | Một chiếc xe  nhỏ bị che khuất bỏi vật thể khác | Chọn `truck` nếu quan sát được cấu trúc cabin và thùng hàng riêng biệt. | Phân loại theo đặc điểm thiết kế chở hàng của truck, không chỉ dựa vào kích thước. |
| `drive_002.jpg` (ví dụ) | Một chiếc xe khách lớn bị che khuất một phần bởi phương tiện khác. | Chọn `bus` nếu vẫn nhận diện được đặc điểm xe buýt. | Việc bị che khuất không làm thay đổi class của phương tiện. |
| `drive_003.jpg`  | Một chiếc van bị cắt mất phần đầu xe do nằm sát rìa ảnh. | Chọn `van` nếu phần còn lại đủ đặc điểm nhận diện. | Phân loại theo cấu trúc thân xe dạng van, không loại bỏ chỉ vì xe bị cắt bởi rìa ảnh. |

## 4. Nguyên tắc thực hiện

1. Chỉ gán nhãn bốn class được quy định trong schema: `car`, `truck`, `bus`, `van`.
2. Mỗi phương tiện hợp lệ tương ứng với một Bounding Box.
3. Giữ nguyên ID class theo schema, không tự ý thay đổi.
4. Không tự suy đoán loại xe khi hình ảnh không cung cấp đủ thông tin.
5. Ghi lại những trường hợp khó phân loại để duy trì sự nhất quán trong quá trình annotation và self-QC.