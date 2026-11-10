# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` (ID 0) | Xe con, sedan, SUV, taxi và xe bán tải. | Xe bán tải thuộc `car`, không thuộc `truck`. Không gán xe buýt, xe tải hoặc mô tô vào lớp này. |
| `truck` (ID 1) | Xe tải chở hàng và xe đầu kéo. | Phân biệt với xe bán tải hoặc xe cabin nhỏ mà schema quy định thuộc `car`. |
| `bus` (ID 2) | Xe buýt và minibus chở khách. | Phân biệt với van/minivan chở hàng hoặc chở người thuộc `van`; không chỉ dựa vào việc xe chở nhiều người để chọn lớp. |
| `van` (ID 3) | Xe van và minivan dùng để chở hàng hoặc chở người. | Không bao gồm xe buýt lớn thuộc `bus` hoặc xe con thông thường thuộc `car`. Nếu khó phân biệt với `bus` hoặc `car`, ghi lại tên ảnh, vị trí xe và hỏi giảng viên. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: mỗi xe hợp lệ có một box riêng, ôm sát phần thân xe nhìn thấy được; không lấy thêm mặt đường, trời hoặc xe bên cạnh. Không gộp nhiều xe vào một box.
- Vật thể bị che khuất: vẫn gán nhãn nếu phần nhìn thấy đủ để nhận diện lớp. Vẽ box quanh phần nhìn thấy, không suy đoán phần bị che. Hai xe che nhau được xử lý thành hai box riêng nếu nhận diện được từng xe.
- Vật thể bị cắt bởi rìa ảnh: vẫn gán nhãn nếu nhận diện được lớp; cạnh box chạm đúng rìa ảnh tại phía bị cắt. Không đoán phần xe nằm ngoài ảnh và không để box vượt biên ảnh.
- Vật thể rất nhỏ/mơ hồ: nếu chỉ còn mảnh nhỏ hoặc không xác định chắc lớp, không tự đoán. Ghi tên ảnh, vị trí vật thể và điểm chưa rõ vào bảng ca mơ hồ để hỏi giảng viên, sau đó cập nhật quyết định theo hướng dẫn; không tự thay đổi schema.
- Trạng thái của xe: lớp mô tả loại xe, không mô tả trạng thái. Xe đang đỗ vẫn được gán nhãn như xe đang chạy nếu thuộc một trong bốn lớp.
- Phạm vi gán nhãn: chỉ gán `car`, `truck`, `bus`, `van` với ID cố định 0, 1, 2, 3. Không gán người đi bộ, xe đạp, mô tô/xe máy, biển báo, đèn giao thông hoặc vật thể ngoài schema.
- Hình ảnh của xe: không gán hình xe trên bảng quảng cáo hoặc xe chỉ xuất hiện qua phản chiếu trong kính/gương.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001.jpg` | Cột đèn che một phần nhỏ ở giữa xe; các phần xe còn lại vẫn nhìn thấy. | Gán một box cho xe, bao các phần thân xe nhìn thấy ở hai bên cột đèn; không tách thành hai box. | Xe bị che một phần vẫn được gán nhãn nếu nhận diện được lớp. Mỗi xe có một box riêng; box chữ nhật bao các phần nhìn thấy có thể chứa cả đoạn cột đèn ở giữa, không cần suy đoán phần xe bị che. |
| `drive_042.jpg` | Xe tải chở hàng cồng kềnh, cần xác định phạm vi box giữa phần xe và hàng chở. | Gán lớp `truck` (ID 1), vẽ box sát phần xe nhìn thấy, gồm cabin và thân/thùng xe; không mở rộng box để bao phần hàng cồng kềnh nhô ra ngoài xe. | Schema xác định đối tượng là xe tải chở hàng. Theo quyết định cho ca này, box bám sát phần xe, không bao thêm phần hàng nhô ra ngoài. |
| `drive_002.jpg` | Ô tô 7 chỗ dễ gây phân vân giữa `car`, `van` và `bus`. | Gán lớp `car` (ID 0) cho chiếc ô tô 7 chỗ đã xác định là xe con/SUV. | Xe con/SUV thuộc `car`; số chỗ ngồi không tự làm xe chuyển thành `van` hoặc `bus`. Quyết định dựa trên loại xe, không chỉ dựa vào số chỗ. |
