# Mini Guideline — Bài làm cá nhân

## 1. Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
|---|---|---|
| `car` | Xe được thiết kế chủ yếu để chở người. | Sedan, SUV, hatchback. Thường có kích thước nhỏ hơn bus và truck. |
| `truck` | Xe được thiết kế chủ yếu để vận chuyển hàng hóa. | Xe tải thùng, xe tải ben. Thường có cabin và thùng chở hàng riêng biệt. |
| `bus` | Xe được thiết kế để vận chuyển nhiều hành khách. | Xe buýt công cộng, xe buýt trường học. Thường có thân xe lớn, dài và nhiều cửa sổ. |
| `van` | Xe có thân kín, khoang lái và khoang phía sau thường được thiết kế liền nhau. | Cargo van, passenger van. Thường có thân xe dạng hộp, có thể có cửa trượt bên hông. |

## 2. Quy tắc chung

- **Box bám sát vật thể:** Bounding Box phải bao quanh sát phần nhìn thấy của phương tiện, không lấy quá nhiều phần nền xung quanh và không cắt mất phần xe đang nhìn thấy.
- **Vật thể bị che khuất:** Vẫn gán nhãn nếu có thể nhận diện được loại xe. Box chỉ bao quanh phần vật thể nhìn thấy, không tự đoán phần bị che.
- **Vật thể bị cắt bởi rìa ảnh:** Vẫn gán nhãn nếu có thể xác định được đó là một trong bốn class. Box bao quanh phần xe nằm trong ảnh.
- **Vật thể rất nhỏ hoặc mơ hồ:** Nếu không đủ thông tin để xác định class thì không tự đoán nhãn. Cần xem xét theo quy định dataset và ghi lại trường hợp chưa chắc chắn để kiểm tra.
- **Ảnh bị mờ do chuyển động:** Chỉ gán nhãn khi vẫn có thể nhận diện phương tiện và xác định class một cách đáng tin cậy.

## 3. Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
|---|---|---|---|
| drive-01 | Một phương tiện bị che khuất bởi vật thể khác. | Chỉ gán nhãn nếu phần phương tiện còn nhìn thấy đủ để xác định class. | Việc bị che khuất không làm thay đổi class, nhưng không được tự suy đoán phần bị che. |
| drive-03 | Hình ảnh phương tiện bị mờ do chuyển động hoặc được chụp quá nhanh, không thể nhận diện rõ. | Không gán nhãn nếu không xác định được class. | Không đủ thông tin hình ảnh để phân biệt chính xác giữa `car`, `truck`, `bus` và `van`. |
| driver-06 | Phương tiện bị lẫn vào khu vực công trình xây dựng, khó phân biệt với các vật thể xung quanh. | Không gán nhãn nếu không xác định được ranh giới và class của phương tiện. | Không thể tạo Bounding Box đáng tin cậy khi đối tượng không được phân biệt rõ với nền. |
| driver-07 | Phương tiện xuất hiện không rõ ràng, khó xác định là `van` hay `truck`. | Không gán nhãn khi không đủ đặc điểm để phân biệt hai class. | Theo schema, `van` và `truck` có định nghĩa khác nhau. Không được lựa chọn class chỉ dựa trên phỏng đoán. |
| drive-08 | Phương tiện chỉ xuất hiện một phần trong ảnh, hình ảnh không đủ rõ để nhận diện loại xe. | Không gán nhãn nếu phần nhìn thấy không đủ thông tin để xác định class. | Vật thể bị cắt bởi rìa ảnh vẫn có thể được gán nhãn, nhưng phải có đủ đặc điểm nhận diện. |


## 4. Nguyên tắc thực hiện

1. Chỉ gán nhãn bốn class được quy định trong schema: `car`, `truck`, `bus`, `van`.
2. Mỗi phương tiện hợp lệ tương ứng với một Bounding Box.
3. Giữ nguyên ID class theo schema, không tự ý thay đổi.
4. Không tự suy đoán loại xe khi hình ảnh không cung cấp đủ thông tin.
5. Ghi lại những trường hợp khó phân loại để duy trì sự nhất quán trong quá trình annotation và self-QC.
6. Ưu tiên độ chính xác của nhãn và Bounding Box hơn số lượng đối tượng được gán nhãn.

---

**Annotation Tool:** CVAT  
**Annotation Type:** Bounding Box  
**Annotation Approach:** Individual Annotation and Self-Quality Control