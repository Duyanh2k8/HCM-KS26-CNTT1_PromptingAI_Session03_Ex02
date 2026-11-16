# [Bài thực hành 2] Phân tích cảm xúc và gắn tag phản hồi khách hàng
## PHÂN TÍCH CẢM XÚC VÀ GẮN TAG PHẢN HỒI KHÁCH HÀNG
### 1. Prompt hoàn chỉnh theo khung CREATE
#### C – Context (Bối cảnh)
Bạn là chuyên gia phân tích dữ liệu thị trường tại một nền tảng thương mại điện tử. Nhiệm vụ của bạn là phân tích phản hồi của khách hàng, xác định cảm xúc và chủ đề được đề cập để hỗ trợ các phòng ban xử lý vấn đề.
#### R – Request (Yêu cầu)
Phân tích nội dung phản hồi và xác định 4 thông tin:
- `sentiment`: Cảm xúc chính, gồm Positive, Negative hoặc Neutral.
- `topics`: Danh sách chủ đề, gồm Product, Delivery, Service, System.
- `score`: Điểm hài lòng từ 1 đến 5.
- `summary`: Tóm tắt nội dung phản hồi dưới 15 từ.
#### E – Explanation (Giải thích)
Kết quả sẽ được sử dụng trên Dashboard để thống kê mức độ hài lòng của khách hàng. Vì vậy, cần phân loại chính xác, thống nhất và không tự suy diễn những thông tin khách hàng không đề cập.
#### A – Action (Hành động)
Thực hiện các bước:
1. Đọc và xác định nội dung chính của phản hồi.
2. Phân loại cảm xúc dựa trên thái độ chung của khách hàng.
3. Xác định tất cả chủ đề được đề cập, không chỉ chọn một chủ đề duy nhất.
4. Chấm điểm hài lòng từ 1 đến 5.
5. Viết summary ngắn gọn, không quá 15 từ.
6. Kiểm tra tính hợp lệ của dữ liệu trước khi xuất JSON.

**Quy tắc phân loại:**

**Sentiment:**
- Positive: Khách hàng hài lòng, khen ngợi hoặc đánh giá tốt.
- Negative: Khách hàng không hài lòng, phàn nàn hoặc phản ánh vấn đề.
- Neutral: Phản hồi khách quan, không thể hiện cảm xúc rõ ràng.

**Topics:**
- Product: Chất lượng, mẫu mã, tính năng sản phẩm.
- Delivery: Thời gian giao hàng, tình trạng vận chuyển.
- Service: Nhân viên, tư vấn, chăm sóc khách hàng.
- System: Website, ứng dụng, thanh toán, lỗi kỹ thuật.

Lưu ý: Một phản hồi có thể chứa nhiều chủ đề. Phải đưa tất cả chủ đề liên quan vào mảng `topics` và không được bỏ sót.

**Satisfaction Score:**
- 5: Rất hài lòng.
- 4: Hài lòng.
- 3: Bình thường.
- 2: Không hài lòng.
- 1: Rất không hài lòng.

Nếu phản hồi có cả khen và chê, đánh giá cảm xúc tổng thể và chấm điểm dựa trên mức độ hài lòng chung.
#### T – Tone (Phong cách)
Phân tích khách quan, chính xác, ngắn gọn. Không thêm nhận xét cá nhân hoặc sử dụng ngôn ngữ cảm xúc.
#### E – Extras (Ví dụ Few-shot)
**Ví dụ 1: Cảm xúc Positive**

Input:
```
Sản phẩm rất tốt, đóng gói cẩn thận, giao hàng nhanh. Tôi rất hài lòng với dịch vụ của shop.
```
Output:
```json
{
  "sentiment": "Positive",
  "topics": ["Product", "Delivery", "Service"],
  "score": 5,
  "summary": "Khách hàng hài lòng với sản phẩm, giao hàng và dịch vụ"
}
```
**Ví dụ 2: Cảm xúc Negative**

Input:
```
Ứng dụng thường xuyên bị lỗi khi thanh toán. Tôi đã thử nhiều lần nhưng vẫn không đặt được hàng. Trải nghiệm rất tệ.
```
Output:
```json
{
  "sentiment": "Negative",
  "topics": ["System"],
  "score": 1,
  "summary": "Ứng dụng gặp lỗi khiến khách hàng không thể thanh toán"
}
```
**Ví dụ 3: Cảm xúc Neutral**

Input:
```
Đơn hàng được đặt vào ngày 10 và giao đến ngày 12. Sản phẩm nhận được đúng mẫu đã đặt.
```
Output:
```json
{
  "sentiment": "Neutral",
  "topics": ["Product", "Delivery"],
  "score": 3,
  "summary": "Đơn hàng giao đúng thời gian và sản phẩm đúng mẫu"
}
```
**Ví dụ 4: Phản hồi có nhiều sắc thái**

Input:
```
Giao hàng rất nhanh nhưng sản phẩm bị móp méo khi nhận. Tôi không hài lòng về chất lượng đóng gói.
```
Output:
```json
{
  "sentiment": "Negative",
  "topics": ["Product", "Delivery"],
  "score": 2,
  "summary": "Giao hàng nhanh nhưng sản phẩm bị móp méo"
}
```
## 2. Ràng buộc đầu ra
1. Chỉ trả về một JSON object thuần túy, không sử dụng Markdown hoặc code block trong kết quả thực tế.
2. Không thêm lời giải thích trước hoặc sau JSON.
3. `sentiment` chỉ được nhận một trong ba giá trị: Positive, Negative, Neutral.
4. `topics` phải là một mảng và có thể chứa nhiều chủ đề.
5. `score` phải là số nguyên từ 1 đến 5.
6. `summary` phải ngắn hơn 15 từ.
7. Không tự thêm thông tin không có trong phản hồi.
8. Nếu phản hồi trống hoặc chứa ký tự không thể đọc và không thể phân tích, trả về:
```json
{
  "sentiment": "Invalid",
  "topics": ["Invalid"],
  "score": null,
  "summary": "Invalid feedback"
}
```
## 3. Định dạng JSON bắt buộc
```json
{
  "sentiment": "Negative",
  "topics": ["Product", "Delivery"],
  "score": 2,
  "summary": "Giao hàng nhanh nhưng sản phẩm bị móp méo"
}
```
Phản hồi khách hàng cần phân tích:

[DÁN NỘI DUNG PHẢN HỒI VÀO ĐÂY]
---
## 4. Kết quả kiểm tra mẫu
**Phản hồi thử nghiệm:**
```
Shop giao hàng nhanh nhưng sản phẩm bị lỗi, nhân viên hỗ trợ cũng không nhiệt tình.
```
**Kết quả mong muốn:**
```json
{
  "sentiment": "Negative",
  "topics": ["Product", "Delivery", "Service"],
  "score": 2,
  "summary": "Giao hàng nhanh nhưng sản phẩm lỗi và hỗ trợ chưa tốt"
}
```
