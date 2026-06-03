# Template — Evidence Pack

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

**Tên nhóm:** Nhóm 02 (AI Travel Planner)  
**Track:** AI Travel Assistant / Booking Assistant  
**Product/app đã chọn:** Trợ lý Du lịch Thông minh V-Travel AI  
**Build slice đang nghĩ:** Trích xuất thông tin tìm kiếm từ câu lệnh tự nhiên để hiển thị danh sách vé máy bay và phòng khách sạn trực quan (Rich UI Cards) thay vì chỉ trả về text thô hoặc báo lỗi không tìm thấy.

## 2. Self-use evidence

Nhóm tự dùng app/workflow và ghi lại điểm gãy.

| Observation | Screenshot/link | Path liên quan | Điều học được |
|---|---|---|---|
| User nhập câu lệnh: "Tìm vé máy bay khứ hồi từ Hà Nội đi Phú Quốc cuối tuần này và khách sạn 3 sao." AI trả lời chung chung khuyên user tự lên web hãng bay hoặc báo lỗi hệ thống không tìm thấy dữ liệu. | [z7896518379617_a2c7d5345cb40fa6a0b7328b65729187.jpg](file:///d:/VIN_UNI/day05/Batch02-Day05-AI-Product-Labs-main/Batch02-Day05-AI-Product-Labs-main/maybay/z7896518379617_a2c7d5345cb40fa6a0b7328b65729187.jpg) | Failure Path | AI thiếu khả năng tích hợp dữ liệu thời gian thực (API flights/hotels) và không có cơ chế fallback hiển thị kết quả thay thế khi không tìm được dữ liệu chính xác. |
| User hỏi cụ thể: "Vinpearl Nha Trang còn phòng trống ngày mai không?" AI không nhận diện được ngày mai là ngày cụ thể nào, phản hồi "Không có dữ liệu phòng" và yêu cầu gọi hotline. | [z7896518380436_18b623765d4db15f0b9c0cc81dac7b86.jpg](file:///d:/VIN_UNI/day05/Batch02-Day05-AI-Product-Labs-main/Batch02-Day05-AI-Product-Labs-main/maybay/z7896518380436_18b623765d4db15f0b9c0cc81dac7b86.jpg) | Failure / Low-confidence Path | Hệ thống chưa có bộ phân tích thực thể (Entity Extractor) tốt để chuyển đổi các cụm từ thời gian tương đối ("ngày mai", "cuối tuần này") thành ngày tháng cụ thể để truy vấn cơ sở dữ liệu. |

## 3. User / review / social evidence

Nguồn có thể là review App Store/Play, group, comment, phỏng vấn nhanh, hoặc nguồn public khác.

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Tôi muốn đặt combo vé máy bay và phòng trực tiếp qua trợ lý AI nhưng nó cứ báo lỗi tìm kiếm hoặc bắt tôi tự nhập tay từng bước trên website, rất mất thời gian và phiền phức." | Đánh giá trên App Store của ứng dụng du lịch | Khách du lịch tự túc bận rộn | Core workflow failure: Trợ lý AI không thực hiện được nghiệp vụ cốt lõi mà chỉ hoạt động như một chatbot hỏi đáp thông tin cơ bản. |
| "Hỏi AI tìm phòng giá dưới 1 triệu ở Đà Nẵng thì nó list ra danh sách toàn chữ, không có hình ảnh phòng, không có giá cụ thể từng loại phòng để so sánh." | Group cộng đồng Du lịch bụi | Người trẻ thích đi phượt, so sánh giá | UX/Presentation Failure: Thiếu Rich UI (hình ảnh, giá cả so sánh trực quan) khiến user khó ra quyết định. |

Nếu chưa có nguồn ngoài nhóm, ghi rõ:

```text
Đây là giả định. Nhóm sẽ kiểm bằng phỏng vấn nhanh 3 người dùng tiềm năng trước checkpoint M1 Day 06.
```

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| **Traveloka AI Chatbot / Kayak Chat** | Khi nhận câu hỏi tự nhiên, chatbot tự động trích xuất: Nơi đi, nơi đến, ngày đi, số khách. Sau đó render trực tiếp một danh sách các chuyến bay/khách sạn dưới dạng thẻ (Cards) trực quan ngay trong khung chat để user nhấn chọn và thanh toán. | **Hybrid UI / Card List Component**: Không hiển thị text thuần mà dùng các khối giao diện (UI Cards) sinh động có ảnh, giá, rating. | **Có**: Tạo component UI động hiển thị danh sách phòng/vé dạng thẻ dựa trên các tham số AI trích xuất được từ câu chat của user. |

## 5. Evidence -> Insight

```text
Evidence nổi bật nhất:
User tìm kiếm vé máy bay và phòng khách sạn qua AI nhưng không xem được kết quả trực quan mà bị báo lỗi tìm kiếm hoặc chỉ nhận được hướng dẫn tự tìm trên website.

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
Lý do: Điểm gãy lớn nhất của user là không xem và tìm kiếm được vé/phòng trực quan, khiến chatbot trở nên vô dụng đối với nhu cầu đặt dịch vụ.
```
