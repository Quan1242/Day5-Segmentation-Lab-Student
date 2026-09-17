# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602053
- Ngày / CVAT local: 17/09/2026 / http://localhost:8080
- Công cụ đã dùng: Brush, Polygon, Gợi ý tự động (SegFormer, YOLOv8-seg)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, chiếc xe ô tô màu trắng đỗ phía lề đường bên phải.
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Tôi vẽ bao trùm toàn bộ phần thân vỏ, bánh xe, gương chiếu hậu và kính xe nhìn thấy được. Dừng biên tại mép tiếp xúc với mặt đường và vỉa hè; không kéo lẹm sang phần bóng đổ dưới gầm xe. Giữ nguyên kính xe trong mask theo quy tắc không khoét lỗ.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Gợi ý nhận diện đúng biên thân xe nhưng phần mép dưới bánh xe bị tràn lẹm vào bóng râm mặt đường; tôi đã dùng Polygon nắn lại phần biên dưới của lốp xe cho khớp sát phần tiếp xúc thực tế.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình: Đã dùng gợi ý kết hợp kiểm tra và nắn biên thủ công bằng Polygon.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `cp2_slice`, ảnh duy nhất, vùng hai chiếc xe ô tô đỗ sát nhau.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: Gộp-tách (hai vật cùng class đứng sát nhau bị gộp thành một mask).
- Bằng chứng tôi nhìn thấy: Hai xe đỗ nối tiếp sát nhau; gợi ý ban đầu gộp cả hai xe thành một polygon duy nhất, không phân biệt được ranh giới giữa cản sau xe trước và đầu xe sau.
- Quy tắc và hành động sửa: Theo quy tắc "Slice: adjacent same-class vehicles must be separate instances", tôi dùng công cụ cắt/tách polygon thành hai object `car` riêng biệt, phóng to để vẽ đường rãnh chia cắt chính xác ở khe hở giữa 2 xe.
- Sau sửa đã Save và export lại chưa? Đã Save trên CVAT và cập nhật lại vào file export ZIP `cp2_slice.zip`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Chạy `python scripts/inspect_submissions.py` xác nhận `cp2_slice.zip` có 17 polygon annotations phân tách hợp lệ, trạng thái [OK]. Chạy scorecard với ground truth chính thức cho kết quả Easy `17.0/20`, Medium `7.9/32`, Hard `14.4/30`, tổng `39.3/82`.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `easy_semantic` (ảnh `817bca71-00000000.jpg`), mép đường rẽ góc phải | Có nên gán phần bóng râm tối màu sát lề là `road` hay `sidewalk`? | Màu nhựa đường dưới bóng râm và đá vỉa hè bị sẫm màu giống nhau, nhưng nhìn kỹ thấy gờ bó vỉa (curb) nổi cao hơn mặt đường. | Quyết định phân chia theo gờ bó vỉa chức năng: phần mặt phẳng thấp xe chạy là `road`, phần gờ cao cho người đi bộ là `sidewalk`. |
| 2. `cp1_holes` (ảnh duy nhất) | Kính chắn gió và cửa sổ ô tô có nhìn thấy cảnh vật phía sau có cần khoét lỗ (hole) không? | Cửa sổ nhìn xuyên thấu nền trời và nhà, dễ nhầm là lỗ thủng cần khoét. | Áp dụng đúng quy tắc `cp1_holes`: "windows/gaps stay inside the mask — do NOT cut them out", giữ toàn bộ vùng kính nằm trong mask `car`. |
| 3. `cp5_occlusion` (ảnh duy nhất) | Xe ô tô bị cột biển báo che cắt ngang làm đôi | Tách thành 2 mask xe riêng biệt hay gộp thành 1 instance xe? | Áp dụng quy tắc `cp5_occlusion`: một vật thể bị vật khác che cắt đôi nhưng về mặt ngữ nghĩa vẫn là một vật duy nhất, do đó gán chung vào 1 instance xe. |
