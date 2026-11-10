# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Trần Anh Văn
- MSSV: 2A202603058
- Email: 26ai.vanta3@vinuni.edu.vn
- Lớp: L2A
- Link repo bài làm: https://github.com/vanahtran/K5-L2A-DAY02-TranAnhVan-2A202603058-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/github/vanahtran/K5-L2A-DAY02-TranAnhVan-2A202603058-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50 ảnh
- Kết quả checker và lỗi đã xử lý: Checker đạt cấu trúc 50/50 file nhãn hợp lệ. Trong quá trình kiểm tra đã phát hiện lỗi bounding box vượt ngoài biên ảnh tại drive_006.txt và drive_008.txt, sau đó đã chỉnh sửa và chạy lại checker thành công.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001.jpg, drive_002.jpg, drive_003.jpg, drive_004.jpg, drive_005.jpg, drive_006.jpg, drive_007.jpg, drive_008.jpg, drive_009.jpg, drive_010.jpg.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Phát hiện lỗi bounding box vượt ngoài biên ảnh và tiến hành điều chỉnh tọa độ. Với các trường hợp mơ hồ như drive_014, drive_023, drive_028, áp dụng GUIDELINE_MINI.md: chỉ chọn class khi đủ bằng chứng, chỉ đánh dấu phần đối tượng nhìn thấy và không tự suy đoán khi đối tượng quá nhỏ hoặc bị che khuất.
- Commit khóa nhãn ban đầu: afcebeb – commit chứa 50 file nhãn YOLO và GUIDELINE_MINI.md, đã push thành công lên GitHub.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 456 | |
| FP | 620 | |
| FN | 197 | |
| Precision | 0.4238 | |
| Recall | 0.6983 | |
| F1 | 0.5275 | |
| mean_iou_of_matches | 0.8589 | |
| passed | false | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

Tình trạng rework:

Chưa thực hiện rework nhãn dựa trên kết quả đánh giá này. Kết quả FP/FN chỉ được sử dụng để ghi nhận mức độ sai khác giữa nhãn sinh viên và Gold labels, không tự suy luận nguyên nhân cụ thể khi chưa kiểm tra trực tiếp từng ảnh.

Các lỗi cấu trúc bounding box vượt ngoài biên ảnh tại drive_006.txt và drive_008.txt đã được xử lý trước đó trong bước kiểm tra bằng checker.

Quyết định: Giữ nguyên bộ 50 nhãn hiện tại để tiếp tục quy trình train YOLO theo hướng dẫn của giảng viên. Không tự ý sửa nhãn chỉ dựa trên các chỉ số FP/FN tổng hợp.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
 Runtime: Google Colab.
 GPU/CPU: NVIDIA Tesla T4.
- Các tham số thay đổi so với notebook nếu có và lý do: Chưa ghi nhận thay đổi
- Đường dẫn best checkpoint trong phiên chạy: 
 Checkpoint: outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4105 |
| Recall | 0.2303 |
| mAP50 | 0.2723 |
| mAP50–95 | 0.2017 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
 Lớp có recall thấp nhất: class van có reacll thấp nhất.

 Ảnh eval minh họa: BoxR_curve.png.

 Nhận xét: model nhận diện chưa chính xác class.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
 Cần kiểm tra độ chính xác về vị trí và kích thước bounding box. Box có thể đã xác định đúng vật thể ở ngưỡng IoU 0.5 nhưng chưa đủ sát với vật thể thực tế khi áp dụng các ngưỡng IoU cao hơn. Cần quan sát các trường hợp box quá rộng, quá hẹp, lệch tâm, không bao hết vật thể hoặc lấn nhiều vào nền.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?\
 Các lớp cần chú ý là car, truck, bus và van, đặc biệt với phương tiện có hình dáng tương đồng hoặc bị che khuất, ở xa.

Mini guideline đã ghi nhận các trường hợp cần thận trọng:

drive_014.jpg: phương tiện màu đỏ có hình dáng tương tự van và minibus; không tự suy đoán class khi chưa đủ đặc điểm phân biệt.

drive_023.jpg: xe bus bị che khuất một phần; chỉ gán nhãn phần vật thể nhìn thấy khi có đủ căn cứ nhận diện.

drive_028.jpg: phương tiện nhỏ ở xa; chỉ gán class khi có đủ thông tin để xác định.

Đây là các trường hợp có nguy cơ gây nhầm lẫn khi gán nhãn hoặc dự đoán. Cần đối chiếu ảnh dự đoán với nhãn ground truth để xác định có thực sự xảy ra lỗi class hay không, không kết luận chỉ dựa trên việc các lớp có hình dáng tương tự.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
 Không dùng 10 ảnh eval để train nhằm giữ nguyên tập dữ liệu đánh giá, tránh để model học trực tiếp từ các ảnh dùng để kiểm tra kết quả. Nếu đưa các ảnh này vào train, kết quả đánh giá sẽ không còn phản ánh đúng khả năng dự đoán trên dữ liệu chưa được dùng để huấn luyện.

Trong notebook, 10 ảnh eval được khai báo ở phần val, được dùng làm validation trong quá trình train và để đánh giá, lựa chọn checkpoint best.pt. Vì vậy, đây không phải test set độc lập hoàn toàn.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Kiểm tra nhãn YOLO | `drive_006.txt` và `drive_008.txt` có bounding box vượt ngoài biên ảnh | Kiểm tra và chỉnh sửa tọa độ bounding box | Đã sửa; chạy lại `check_yolo_labels.py` đạt 50/50 file hợp lệ |
| Đánh giá chất lượng annotation | Kết quả Gold-set QC chưa đạt ngưỡng F1 yêu cầu | Chạy `evaluate_annotations.py` để kiểm tra Precision, Recall, F1 và mean IoU | F1 = 0.5275, mean IoU = 0.8589; chưa thực hiện vòng rework sau kết quả này |
| Huấn luyện và đánh giá YOLO | Model có Recall và mAP còn thấp | Huấn luyện trên Google Colab với GPU NVIDIA Tesla T4; đánh giá trên 10 ảnh eval | Đã có metrics và checkpoint; cần phân tích thêm lỗi theo từng class và ảnh dự đoán |
| Hoàn thiện báo cáo | Chưa xác định class có recall thấp nhất và ảnh minh họa cụ thể | Đối chiếu kết quả validation theo class và thư mục ảnh dự đoán | Còn cần bổ sung số liệu recall từng class và ảnh minh họa |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
 Đối chiếu `RUBRIC.md`, kết quả thực hiện hiện tại như sau:

| Tiêu chí | Kết quả tự đánh giá | Trạng thái |
|---|---|---|
| Hoàn thành và định dạng YOLO | Có 50 ảnh và 50 file nhãn; checker xác nhận 50/50 file hợp lệ | Đã kiểm tra |
| Đầy đủ vật thể | Đã gán nhãn 50 ảnh; cần tiếp tục đối chiếu các trường hợp khó và kết quả QC | Đã thực hiện, chất lượng còn cần cải thiện |
| Độ bám sát và hình học box | Đã sửa lỗi box vượt biên; mean IoU của các cặp khớp đạt 0.8589 | Có kết quả QC |
| Phân lớp nhất quán | Có mini guideline cho các lớp `car`, `truck`, `bus`, `van`; vẫn cần đối chiếu lỗi class thực tế | Đã có guideline |
| Mini guideline và quyết định mơ hồ | Có ít nhất ba trường hợp mơ hồ được ghi lại: `drive_014`, `drive_023`, `drive_028` | Đã thực hiện |
| Gold-set annotation QC | F1 = 0.5275, thấp hơn 0.80; mean IoU = 0.8589, cao hơn 0.70; chưa có vòng rework sau đánh giá | Chưa đạt đầy đủ tiêu chí |
| Pipeline YOLO và phân tích | Đã chạy huấn luyện, có metrics và checkpoint; cần bổ sung phân tích recall từng class và ảnh minh họa | Đã chạy, phân tích còn thiếu |

**Tự nhận xét:** Đã hoàn thành cấu trúc nhãn YOLO, kiểm tra định dạng, xây dựng mini guideline và chạy pipeline huấn luyện YOLO. Tuy nhiên, kết quả Gold-set QC có F1 thấp hơn ngưỡng 0.80 và chưa thực hiện vòng rework sau đánh giá. Phần phân tích YOLO cũng cần bổ sung bằng chứng theo từng class và ảnh dự đoán. Không tự nhận các tiêu chí này đã hoàn thành đầy đủ.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
 | Công cụ | Mục đích sử dụng | Phần đã tự kiểm tra |
|---|---|---|
| CVAT | Gán nhãn bounding box cho 50 ảnh | Kiểm tra nhãn và các trường hợp khó |
| Google Colab | Chạy notebook huấn luyện và đánh giá YOLO26 | Kiểm tra runtime, GPU, metrics và checkpoint |
| Ultralytics YOLO | Huấn luyện và dự đoán vật thể | Đối chiếu metrics và ảnh dự đoán |
| Python | Chạy script kiểm tra nhãn và đánh giá annotation | Kiểm tra kết quả checker và JSON |
| Git và GitHub | Quản lý phiên bản, lưu trữ mã nguồn và báo cáo | Kiểm tra trạng thái commit và push |
| ChatGPT | Hỗ trợ giải thích quy trình, kiểm tra lỗi và trình bày báo cáo | Đối chiếu nội dung với notebook, kết quả thực tế và rubric |
