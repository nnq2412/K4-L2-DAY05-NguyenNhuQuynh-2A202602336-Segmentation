# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602336
- Ngày / CVAT local: 17/09/2026
- Công cụ đã dùng: CVAT local

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh 1, chiếc xe ô tô nằm ở góc dưới bên phải.
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Quy tắc chọn biên: Vẽ bám sát viền ngoài của khung xe (bao gồm cả bánh xe và gương chiếu hậu), nhưng bắt buộc loại trừ phần bóng râm (shadow) dưới mặt đường vì bóng không thuộc thực thể xe.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Công cụ Auto-segmentation (gợi ý tự động) bị bắt nhầm/lem biên vào phần bóng râm dưới gầm xe. Tôi đã dùng chức năng sửa (subtract polygon) để gọt bớt phần bóng râm thừa đó đi, đảm bảo ranh giới chỉ nằm ở mép gầm xe thật.
- Nếu không dùng gợi ý: (Đã có dùng gợi ý như trên).

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task easy_semantic / vùng tán cây ở phía xa
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: biên và gộp-tách (vẽ quá chi tiết từng chiếc lá nhỏ)
- Bằng chứng tôi nhìn thấy: Mask ban đầu bị lởm chởm, có nhiều khoảng trống (holes) do cố gắng bám theo từng chiếc lá nhỏ xíu thay vì bao quanh cả tán cây lớn.
- Quy tắc và hành động sửa: Quy tắc cho semantic segmentation (lớp vegetation) là không cần vẽ từng chiếc lá nhỏ lẻ. Ta vẽ một polygon (đa giác) bao quanh toàn bộ khu vực tán lá chính (canopy), bỏ qua các khe sáng hoặc cành cây nhỏ bên trong để tránh nát mask. Đã xóa các mask vụn và bao lại thành 1 mask lớn.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại thành easy_semantic.zip.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): … / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| Ảnh cảnh đường rộng có tòa nhà đang xây / Phần cần cẩu tháp (crane) phía trên tòa nhà bên phải | Nên gộp cần cẩu này vào class `building` hay bỏ qua không gán nhãn? | Cần cẩu là máy móc thi công tạm thời, không phải là cấu trúc xây dựng kiên cố cố định của tòa nhà. | Quyết định: Bỏ qua (không gán nhãn `building` cho cần cẩu). Mask bầu trời (`sky`) sẽ len lỏi đằng sau cần cẩu. |
| Ảnh đường phố Nhật (có trạm ENEOS) / Các chiếc ô tô con đang nằm trên xe tải chở xe (car carrier) ở giữa đường | Những xe con này là hàng hóa nên gộp chung vào 1 mask `truck`, hay tách riêng thành các mask `car`? | Mặc dù đóng vai trò là hàng hóa, nhưng chúng vẫn mang hình thái rõ ràng của xe ô tô (class độc lập). | Quyết định: Tách riêng từng chiếc xe con trên thùng xe tải thành các object `car` riêng biệt, phần khung xe tải gán là `truck`. |
| Ảnh đường phố / Các biển báo hiệu đường bộ vẽ trên mặt đường (chữ, vạch kẻ ngang) | Gán là `road` (mặt đường) hay có class riêng cho vạch kẻ đường? | Trong `classes.json` không có class riêng cho road markings/vạch kẻ. Vạch kẻ này sơn trực tiếp và là một phần bề mặt của đường. | Quyết định: Gộp toàn bộ vạch kẻ và chữ sơn trên mặt đường vào chung lớp `road`. |
