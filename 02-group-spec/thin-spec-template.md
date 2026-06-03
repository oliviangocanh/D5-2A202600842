# Template — Thin SPEC Cuối Day 05

Thin SPEC không phải PRD đầy đủ. Đây là bản cam kết đủ rõ để sáng Day 06 nhóm build ngay.

## 1. Track, product/app và user

**Track:**  
**Product/app thật:**  
**User cụ thể:**  
**Nhóm có phải user thật không? Nếu không, khác ở đâu?**  

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |

## 3. Pain statement

```text
User muốn đặt phòng khách sạn / combo vé máy bay + phòng qua app
đang gặp khó ở bước tìm kiếm và xem kết quả,
vì hệ thống không hiển thị phòng trống theo ngày,
không đồng bộ inventory giữa app và website,
và không có cơ chế fallback khi tìm kiếm thất bại —
chỉ báo "Không có kết quả" mà không gợi ý thay thế,
dẫn tới user bị dead-end, phải quay lại tìm từ đầu hoặc bỏ app hoàn toàn.
Bằng chứng chính là:
- Review App Store "App chán" (★1): "muốn đặt phải xem từng ngày một rất mất thời gian"
- Review App Store "Quá tệ" (★1): "chủ biệt thự không thể đặt nổi phòng... trong khi đặt bình thường lại báo có phòng"
- Review App Store "App và website có vấn đề" (★1): "muốn đặt 2 đêm ở Phú Quốc, cả website lẫn app đều không đặt được"
- Screenshot thực tế: tìm khách sạn Hà Nội → HCM 05–08/06, kết quả trả về "Không có kết quả tìm kiếm phù hợp" không kèm gợi ý thay thế
```

**Bằng chứng hình ảnh:**

![bangchung1](file:///d:/VIN_UNI/day05/Batch02-Day05-AI-Product-Labs-main/Batch02-Day05-AI-Product-Labs-main/bangchungpaintstament/z7896546701893_9f991da8c83a408351b16071a6c164bf.jpg)

![bangchung2](file:///d:/VIN_UNI/day05/Batch02-Day05-AI-Product-Labs-main/Batch02-Day05-AI-Product-Labs-main/bangchungpaintstament/z7896546706097_d27639e193247d4f7bdaf139bd628c86.jpg)

![bangchung3](file:///d:/VIN_UNI/day05/Batch02-Day05-AI-Product-Labs-main/Batch02-Day05-AI-Product-Labs-main/bangchungpaintstament/z7896547559598_4d089e047163d4598a1a89974b632a49.jpg)

## 4. Build slice

```text
Cho [user] đang [task/workflow],
prototype sẽ dùng AI để [augment/automate hành động hẹp],
tạo ra [output],
và xử lý [failure mode] bằng [mitigation].
```

## 5. Auto/Aug decision

Chọn một:

- [ ] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [x] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:** Khi user cung cấp đủ thông tin (điểm đi, điểm đến, ngày, số người), AI tự parse và render kết quả ngay mà không cần hỏi thêm. Khi thiếu thông tin hoặc câu nhập mơ hồ (ngày tương đối, budget không rõ), AI hỏi lại từng field thay vì tự đoán. Đặt phòng/vé liên quan đến tiền nên user luôn là người confirm cuối trước khi thanh toán.

**Human role:** decider (user confirm kết quả và bấm đặt), rescuer (khi AI không tìm được kết quả → show fallback + gợi ý thay thế)

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| Happy |  |
| Low-confidence |  |
| Failure |  |
| Correction |  |

## 7. Failure mode nguy hiểm nhất

```text
Nếu user [trigger],
AI có thể [failure],
hậu quả là [impact].
Prototype sẽ xử lý bằng [ask again / show source / human review / undo / fallback].
Owner kiểm thử path này là [tên thành viên].
```

## 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|
|  | Research / evidence |  |
|  | SPEC |  |
|  | Prototype |  |
|  | Test / failure path |  |
|  | Demo script / repo |  |
