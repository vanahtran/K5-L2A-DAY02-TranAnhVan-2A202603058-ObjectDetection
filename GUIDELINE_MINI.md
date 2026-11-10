# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Ô tô chở người thông thường, bao gồm sedan, SUV, taxi và pickup | Không bao gồm xe buýt, xe tải và xe máy |
| `truck` | Xe tải chở hàng và xe đầu kéo | Phân biệt với car, van dựa trên cấu trúc và mục đích vận chuyển hàng hóa |
| `bus` | Xe bus và mini bus dùng để chở khách | Không nhầm với van/mini vì có kích thước nhỏ hơn |
| `van` | Xe van/mini van dùng để chở người hoặc chở hàng hoá | Phân biệt với bus và car dựa trên kiểu dáng thân xe |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bounding box bao quanh phần thân xe nhìn thấy được, hạn chế tối đa nền đường và vùng không thuộc phương tiện
- Vật thể bị che khuất: Vẫn gán nhãn nếu phần phương tiện còn nhìn thấy đủ để xác định lớp
- Vật thể bị cắt bởi rìa ảnh: Bounding box kết thúc tại rìa ảnh, không suy đoán hoặc vẽ phần nằm ngoài ảnh
- Vật thể rất nhỏ/mơ hồ: Không gán nhãn khi không đủ thông tin để xác định phương tiện hoặc lớp, ghi nhận trường hợp để xem xét

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_023.jpg` | Một phần thân xe bus bị phương tiện khác che khuất, chỉ nhìn thấy một phần phương tiện. | Vẫn gán nhãn bus vì phần còn nhìn thấy đủ để xác định phương tiện và phân loại class. Bounding box chỉ bao quanh phần thân xe nhìn thấy được | Đảm bảo tính nhất quán khi gán nhãn, không suy đoán phần bị che khuất và tránh tạo bounding box bao gồm vùng không nhìn thấy. |
| `drive_014.jpg` | Phương tiện màu đỏ có kiểu dáng tương tự van và mini bus, khó xác định chính xác class | Gán nhãn theo đặc điểm hình dáng và cấu trúc phương tiện. Chỉ chọn class khi có đủ thông tin để phân biệt; nếu không đủ thông tin thì không tự suy đoán | Tránh gán sai class và đảm bảo tính nhất quán giữa các ảnh |
| `drive_028.jpg` | Phương tiện xuất hiện ở khoảng cách xa, kích thước rất nhỏ và khó quan sát đặc điểm nhận dạng | Chỉ gán nhãn khi vẫn xác định được phương tiện và class. Nếu không đủ thông tin để xác định thì không gán nhãn | Tránh tạo bounding box thiếu chính xác và hạn chế nhãn sai |
