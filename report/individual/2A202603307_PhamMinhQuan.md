# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Phạm Minh Quân
- MSSV: 2A202603307
- Email: phamminhquan11012003@gmail.com
- Lớp: L2B
- Link repo bài làm: https://github.com/phamminhquan1101/K5-L2B-DAY02-PhamMinhQuan-2A202603307-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/1pQ8DidAORl8rL13wg-2xGYOuJMfmR75L?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50 file YOLO trong `dataset/labels/train/`, cùng basename với ảnh train.
- Kết quả checker: đã kiểm tra bằng `python -X utf8 tools/check_yolo_labels.py`, kết quả **ĐẠT cấu trúc: 50/50 file nhãn hợp lệ**. Kết quả này xác nhận định dạng, ID lớp và hình học tọa độ hợp lệ, chưa chứng minh nhãn đầy đủ hoặc đúng nội dung.
- Danh sách 10 ảnh train đã rà bằng mắt: `drive_001.jpg`, `drive_002.jpg`, `drive_003.jpg`, `drive_004.jpg`, `drive_005.jpg`, `drive_006.jpg`, `drive_007.jpg`, `drive_008.jpg`, `drive_009.jpg`, `drive_010.jpg`.
- Các tình huống đã xem xét và ghi trong [mini guideline](../../GUIDELINE_MINI.md): `drive_001.jpg` có xe bị cột đèn che một phần nhỏ; `drive_042.jpg` có xe tải chở hàng cồng kềnh; `drive_002.jpg` có ô tô 7 chỗ. Quyết định tương ứng là mỗi xe một box bao các phần xe nhìn thấy, chỉ bao phần xe tải thay vì hàng nhô ra, và gán `car` cho xe con/SUV 7 chỗ.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: **[Cần bổ sung theo quá trình tự rà thực tế; ba tình huống trên là quyết định annotation, chưa phải bằng chứng đã sửa lỗi.]**
- Commit bản nhãn ban đầu: `740f1285ba3c7c0a4629243ed6a14399396b896a` — `Complete initial train annotations and mini guideline`. Cần đối chiếu thời điểm commit với mốc khóa nhãn của lớp.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 454 | Không rework |
| FP | 493 | Không rework |
| FN | 199 | Không rework |
| Precision | 0.4794 | Không rework |
| Recall | 0.6953 | Không rework |
| F1 | 0.5675 | Không rework |
| mean_iou_of_matches | 0.8132 | Không rework |
| passed | false (ngưỡng mặc định của script) | Không rework |

Kết quả gốc được lưu tại [outputs/annotation_evaluation.json](../../outputs/annotation_evaluation.json). Evaluator ghép box cùng lớp tại IoU ≥ 0.50. Mean IoU của các box khớp đạt 0.8132, nhưng F1 0.5675 thấp hơn ngưỡng 0.80 mặc định nên trường `passed` vẫn là `false`.

Không thực hiện rework. Theo thông tin tôi nhận từ lớp, do có vấn đề với gold labels, ngưỡng được điều chỉnh thành F1 > 0.50 và tôi tiếp tục train với kết quả 0.5675. **[Cần bổ sung dẫn chứng thông báo/xác nhận của giảng viên nếu có.]** Giữ nguyên JSON đánh giá và không sửa gold labels. Các giá trị FP/FN chỉ phản ánh box không được ghép theo điều kiện của evaluator; chưa đủ để kết luận lỗi cụ thể thuộc về nhãn học viên hay gold.

## 5. Kết quả YOLO

- Runtime: Google Colab; cấu hình lưu trong [args.yaml](assets/yolo26_runs/vehicle_annotation/args.yaml) ghi `device: cpu`. Phiên bản Ultralytics và Torch: **[Cần bổ sung từ output cell cài đặt/runtime của Colab; các file kết quả hiện có chưa ghi phiên bản.]**
- Mô hình và tham số chính: `yolo26n.pt`, 50 epochs, `imgsz=640`, `batch=-1`, `seed=20260909`, optimizer `auto`. Các tham số này khớp cấu hình train trong notebook gốc; chưa ghi nhận thay đổi tham số chính.
- Dữ liệu: 50 ảnh train; 10 ảnh eval dùng làm validation và đánh giá. Giữ cấu hình split trong `data.yaml`, không đưa eval vào tập train.
- Đường dẫn best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`. Không đưa checkpoint `.pt` vào bài nộp.
- Log ghi đủ 50 epochs; thời gian tích lũy ở epoch cuối khoảng 2.284 giây (38 phút).

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.2389 |
| Recall | 0.3502 |
| mAP50 | 0.2796 |
| mAP50–95 | 0.1867 |

Số liệu được lấy từ [model_eval_metrics.json](assets/model_eval_metrics.json) của best checkpoint, không lấy chỉ số epoch cuối thay cho kết quả đánh giá best checkpoint. Đã lưu đủ 10 ảnh dự đoán trong `assets/eval_predictions/visuals/`, cùng log và biểu đồ trong `assets/yolo26_runs/`.

Minh họa tại [drive_051.jpg](assets/eval_predictions/visuals/drive_051.jpg): xe con lớn ở gần máy ảnh, phía dưới bên trái, được dự đoán là `car` với confidence 0.89; các xe nhỏ phía trên có confidence thấp hơn, chẳng hạn 0.56 và 0.63. Confidence không thay thế việc đối chiếu box và lớp với nhãn eval. Trong ảnh này, nhãn eval có một box `bus` ở phía trên bên phải nhưng ảnh prediction tại `conf=0.25` không có dự đoán lớp `bus` tương ứng.

Trả lời bốn câu hỏi cuối notebook:

1. **Lớp nào recall thấp nhất?** Theo [đường Recall–Confidence](assets/yolo26_runs/vehicle_eval/BoxR_curve.png), `truck`, `bus` và `van` đều có recall thấp hơn nhiều so với `car`; tại confidence 0.25, cả ba đường này đã về 0. Không xác định một lớp thấp nhất duy nhất tại ngưỡng này. `drive_051.jpg` minh họa trường hợp không phát hiện đúng lớp `bus` nêu trên. Cần output đánh giá theo từng lớp trên Colab để ghi recall chính xác tại điểm hoạt động dùng tính metrics tổng hợp.
2. **mAP50 cao hơn mAP50–95 nói lên điều gì?** Kết quả thực tế là 0.2796 so với 0.1867; mAP50 hiện cũng còn thấp. Cần kiểm tra các cạnh box, phần xe bị che/cắt khung và mức lấy thừa nền: box có thể khớp ở IoU 0.50 nhưng không đạt các ngưỡng chặt hơn. Chênh lệch hai chỉ số chưa đủ để khẳng định nguyên nhân duy nhất là box lỏng.
3. **Lỗi class nào có thể liên quan guideline?** [Confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix.png) ghi nhận 5 trường hợp gold `truck` và 1 trường hợp gold `bus` được dự đoán thành `car` ở cấu hình đánh giá tạo biểu đồ. Cần rà lại việc phân biệt xe bán tải với xe tải và xe con với bus/van trong nhãn train. Đây là giả thuyết cần kiểm tra; chưa kết luận guideline là nguyên nhân. Sự mất cân bằng lớp và số ảnh train ít cũng có thể góp phần.
4. **Vì sao không dùng eval để train?** Đưa 10 ảnh eval vào tối ưu trọng số sẽ làm rò rỉ dữ liệu và khiến kết quả không còn phản ánh đúng đánh giá trên dữ liệu ngoài tập train. Trong notebook, eval được dùng làm validation để chọn `best.pt` và đánh giá cuối, nên cũng không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Xây dựng guideline | Xe bị cột đèn che, xe tải chở hàng cồng kềnh và ô tô 7 chỗ cần quyết định nhất quán. | Đối chiếu schema và ghi ba quyết định trong `GUIDELINE_MINI.md`. | Đã có tên ảnh, quyết định và lý do; cần giảng viên làm rõ nếu còn bất đồng về phạm vi box. |
| Gold QC | F1 = 0.5675, thấp hơn ngưỡng mặc định 0.80; script trả `passed: false`. | Giữ nguyên kết quả và tiếp tục theo thông tin ngưỡng F1 > 0.50 được điều chỉnh trong lớp. | Không rework; cần bổ sung dẫn chứng thông báo của giảng viên. Không sửa gold hoặc tự thay số liệu. |
| Thiết lập Colab | Chưa rõ vị trí điền link repo và cách đưa nhãn lên Colab. | Điền `REPO_URL` trong notebook; commit/push nhãn để notebook tự clone repo. | Đã chạy train/eval và có bộ kết quả xuất từ Colab. |
| Lấy kết quả Colab | Khó tìm thư mục kết quả sau khi chuyển sang xem thư mục gốc; không tải trực tiếp được cả folder. | Mở lại `/content/Day2-Lab/outputs/`, tải ZIP kết quả rồi giải nén trên máy. | Đã đưa ảnh dự đoán, log, biểu đồ và metrics vào `report/individual/assets/`. |
| Kiểm tra cấu trúc trên Windows khi hoàn thiện báo cáo | Lệnh Python mặc định gặp `UnicodeEncodeError` khi in thông báo tiếng Việt với encoding cp1252. | Chạy `python -X utf8 tools/check_yolo_labels.py`. | Checker in được thông báo và đạt 50/50 file; không phải lỗi nội dung nhãn. |
| Phân tích model | Precision, recall và mAP còn thấp; recall các lớp ngoài `car` thấp trên biểu đồ. | Xem ảnh prediction, Recall–Confidence và confusion matrix để mô tả kết quả. | Chưa thực hiện thí nghiệm bổ sung để tách ảnh hưởng của mất cân bằng lớp, annotation và số lượng dữ liệu. |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

- Công cụ AI: ChatGPT/Codex, dùng để đọc tài liệu repo, giải thích schema và quy trình; soạn phần định nghĩa lớp/quy tắc chung; diễn đạt ba ca mơ hồ từ quan sát tôi cung cấp; hướng dẫn thao tác Colab, đọc kết quả đánh giá và hỗ trợ soạn báo cáo.
- Phạm vi hỗ trợ trên file: Codex chép kết quả đã giải nén vào thư mục assets, chạy checker, đọc JSON/log và đối chiếu một số biểu đồ/ảnh prediction để viết bản nháp các mục 3–6 và 8. AI không gán nhãn thay bộ 50 ảnh train.
- Phần tôi thực hiện: gán nhãn 50 ảnh train; cung cấp ba ca mơ hồ; chạy evaluator với gold; chạy notebook trên Colab và tải bộ kết quả. **[Cần bổ sung những ảnh và nhận xét trong bản nháp báo cáo đã tự đối chiếu; không tự nhận đã kiểm tra phần chưa rà.]**
- Nguồn số liệu: `outputs/annotation_evaluation.json`, `assets/model_eval_metrics.json`, `assets/yolo26_runs/vehicle_annotation/args.yaml`, `results.csv`, cùng ảnh/biểu đồ xuất từ phiên Colab của bài làm.
- Tài liệu tham khảo trong repo: [README](../../README.md), [RULES](../../RULES.md), [GUIDE](../../GUIDE.md), [CHECKPOINTS](../../CHECKPOINTS.md), [RUBRIC](../../RUBRIC.md), [SUBMISSION](../../SUBMISSION.md) và [notebook của bài](../../notebooks/day2_annotation_to_yolo.ipynb). Không sử dụng nguồn ngoài repo để truy tìm nhãn hoặc đáp án.
