# Template — Evidence Pack

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

**Tên nhóm:** Nhóm 007  
**Track:** AI Travel Assistant / Booking Assistant  
**Product/app đã chọn:** Trợ lý Du lịch Thông minh V-Travel AI  
**Build slice đang nghĩ:** Trích xuất thông tin tìm kiếm từ câu lệnh tự nhiên để hiển thị danh sách vé máy bay và phòng khách sạn trực quan (Rich UI Cards) thay vì chỉ trả về text thô hoặc báo lỗi không tìm thấy.

## 2. Self-use evidence

Nhóm tự dùng app/workflow và ghi lại điểm gãy.

| Observation | Screenshot/link | Path liên quan | Điều học được |
|---|---|---|---|
| User tìm khách sạn Hà Nội → HCM ngày 05/06–08/06, 1 phòng 1 khách. App hiển thị màn hình "Chọn khách sạn" nhưng trả về "Không có kết quả tìm kiếm phù hợp" kèm nút "Quay lại tìm kiếm" — không có gợi ý thay thế, không giải thích lý do, không có fallback. | [z7896518379617_a2c7d5345cb40fa6a0b7328b65729187.jpg](file:///d:/VIN_UNI/day05/Batch02-Day05-AI-Product-Labs-main/Batch02-Day05-AI-Product-Labs-main/maybay/z7896518379617_a2c7d5345cb40fa6a0b7328b65729187.jpg) | Failure Path | Khi không có kết quả, hệ thống chỉ báo lỗi mà không gợi ý thay thế (nới lỏng bộ lọc, thay đổi ngày, đề xuất khách sạn gần nhất). User bị dead-end hoàn toàn. |
| Để tìm kiếm combo vé máy bay + khách sạn (Hà Nội → HCM, 05–08 tháng 6/2026), user phải nhập thủ công từng trường riêng lẻ: điểm đi, điểm đến, ngày nhận phòng, ngày trả phòng, số phòng, số người lớn, trẻ em, em bé, hạng vé. Không có AI hỗ trợ trích xuất thông tin từ câu tự nhiên. | [z7896518380436_18b623765d4db15f0b9c0cc81dac7b86.jpg](file:///d:/VIN_UNI/day05/Batch02-Day05-AI-Product-Labs-main/Batch02-Day05-AI-Product-Labs-main/maybay/z7896518380436_18b623765d4db15f0b9c0cc81dac7b86.jpg) | Low-confidence / Happy Path (manual) | Toàn bộ việc trích xuất intent (địa điểm, ngày, số người) đang đổ lên vai user. Nếu AI tự parse câu "Tìm combo vé + phòng Hà Nội đi HCM 5 đến 8 tháng 6, 1 người" và tự điền form, bước này sẽ biến mất. |

## 3. User / review / social evidence

Nguồn: Đánh giá thực tế trên App Store — màn hình "Xếp hạng & nhận xét" (screenshot 15:49–15:51).

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Tôi sd ipad nhưng đăng nhập rồi ko chọn đc gì khác ko vào đc chọn ks" — review "Ko dung dc" ★★★★★ | App Store · Vatc sleeppod · 11 tháng 10 | User dùng iPad, đăng nhập thành công nhưng không điều hướng được | Device compatibility failure: App không hoạt động đúng trên iPad — sau đăng nhập user bị kẹt, không vào được màn hình chọn khách sạn. |
| "App lỗi liên tục" ★☆☆☆☆ | App Store · chs2026 · 10 tháng 10 | User phổ thông | Stability failure: App crash/lỗi liên tục, không dùng được. |
| "Đặt phòng bằng điểm không thể hiện ngày nào có phòng và ngày nào ko có phòng, muốn đặt phải xem từng ngày một rất mất thời gian." — review "App chán" ★☆☆☆☆ | App Store · L.A.L · 1 năm trước | User đặt phòng bằng điểm thưởng | Availability UX failure: Không có calendar view hiển thị ngày có/hết phòng — user phải check thủ công từng ngày, cực kỳ tốn thời gian. |
| "Không thêm được khách hàng đi cùng, nhấn lưu nhưng không có tác dụng" — review "thêm khách hàng" ★★★☆☆ | App Store · nvk43423 · 1 năm trước | User đặt phòng nhóm/gia đình | Core feature failure: Tính năng thêm khách đi cùng bị lỗi — nút lưu không hoạt động. |
| "Không thể nào nhập được mật khẩu mình muốn lúc đăng kí. Thử đi thử lại không được. Trên ip13 nhé. Nên m xoá app r" — review "Lỗi đăng kí" ★☆☆☆☆ | App Store · phamhuyd · 1 năm trước | User iPhone 13 mới cài app | Onboarding failure: Không đăng ký được tài khoản — user bỏ app ngay từ bước đầu tiên. |
| "Chủ biệt thự không thể đặt nổi phòng, mò mẫm dò dẫm toàn hết phòng, chỉ thấy phòng ở mấy nơi rừng rú ko ai đi, trong khi đặt bình thường lại báo có phòng" — review "Quá tệ" ★☆☆☆☆ | App Store · nsicnejxndkx · 5 năm trước | Chủ biệt thự / user có membership | Inventory inconsistency: Kết quả tìm kiếm hiển thị hết phòng nhưng kênh khác vẫn báo có — data không đồng bộ giữa các luồng đặt phòng. |
| "Tôi muốn đặt 2 đêm nghỉ ở Phú Quốc nhưng không thể đặt được qua website vì thông tin không hiển thị. Nên cài app hi vọng có thể đặt được. Rút cuộc vẫn không thể." — review "App và website có vấn đề" ★☆☆☆☆ | App Store · UncleBay · 5 năm trước | Khách du lịch muốn đặt phòng Phú Quốc | Cross-channel failure: Cả website lẫn app đều không đặt được — thông tin phòng không hiển thị, user hoàn toàn bị chặn khỏi core workflow. |

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| **Traveloka AI Chatbot / Kayak Chat** | Khi nhận câu hỏi tự nhiên, chatbot tự động trích xuất: Nơi đi, nơi đến, ngày đi, số khách. Sau đó render trực tiếp một danh sách các chuyến bay/khách sạn dưới dạng thẻ (Cards) trực quan ngay trong khung chat để user nhấn chọn và thanh toán. | **Hybrid UI / Card List Component**: Không hiển thị text thuần mà dùng các khối giao diện (UI Cards) sinh động có ảnh, giá, rating. | **Có**: Tạo component UI động hiển thị danh sách phòng/vé dạng thẻ dựa trên các tham số AI trích xuất được từ câu chat của user. |

## 5. Evidence -> Insight

```text
Evidence nổi bật nhất:
User tìm kiếm vé máy bay và phòng khách sạn thủ công nhưng không xem được kết quả trực quan mà bị báo lỗi tìm kiếm hoặc chỉ nhận được hướng dẫn tự tìm trên website.

Insight:
User không chỉ gặp khó khăn ở bước nhập liệu tìm kiếm (surface problem).
Thật ra họ cần một công cụ so sánh nhanh chóng, trực quan (Rich UI) và đáng tin cậy về tình trạng phòng trống/vé trống ngay tại khung chat để đưa ra quyết định mà không phải chuyển tab hay tự thao tác thủ công (deeper need for seamless decision support).

Opportunity:
AI có thể giúp bằng cách tự động trích xuất các tham số chuyến đi (địa điểm, thời gian, ngân sách) từ câu hỏi tự nhiên và tự động hiển thị danh sách so sánh vé bay và phòng khách sạn dạng thẻ trực quan (Card Carousel) kèm giá cập nhật.
```

## 6. Evidence đổi SPEC như thế nào?

- [ ] Đổi user chính.
- [ ] Đổi pain statement.
- [x] Đổi build slice.
- [x] Đổi Auto/Aug decision.
- [x] Đổi 4 paths.
- [ ] Đổi failure mode.
- [ ] Đổi owner/test plan.

Ghi rõ 1-2 thay đổi quan trọng:

```text
Trước evidence, nhóm định xây dựng chatbot tư vấn thông tin du lịch chung chung (dạng Q&A text).
Sau evidence, nhóm đổi thành tập trung vào build slice: "Trích xuất thực thể tìm kiếm từ câu chat và hiển thị danh sách vé máy bay/phòng khách sạn dưới dạng Rich UI Cards để so sánh trực tiếp".
Lý do: Điểm gãy lớn nhất của user là không xem và tìm kiếm được vé/phòng trực quan, dẫn đến trải nghiệm kém không thu hút được khách hàng.
```
