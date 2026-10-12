# Bài làm: Chọn chiến lược phân rã cho Epic Hủy chuyến ghép

## Phần 1: Đề xuất 2 chiến lược phân rã Epic

### Chiến lược A: Phân rã theo Luồng nghiệp vụ / Trạng thái chuyến đi (Workflow / State-based)
- Feature A1: Hủy chuyến trước khi tài xế nhận cuốc (Trạng thái tìm xe).
- Feature A2: Hủy chuyến sau khi tài xế đã nhận cuốc (Trạng thái đang đón/chờ đón).
- Feature A3: Tính lại giá cước và gửi thông báo cho các hành khách còn lại trong chuyến ghép.

### Chiến lược B: Phân rã theo Vai trò người dùng (Actor / Persona-based)
- Feature B1: Chức năng hủy chuyến và trừ phí dành cho khách muốn hủy.
- Feature B2: Cập nhật lộ trình và chính sách hỗ trợ dành cho tài xế.
- Feature B3: Cập nhật giá cước mới dành cho các khách còn lại trên xe.

---

## Phần 2: So sánh 2 chiến lược

| Tiêu chí | Chiến lược A (Theo trạng thái / luồng nghiệp vụ) | Chiến lược B (Theo vai trò người dùng) |
| :--- | :--- | :--- |
| Độc lập (Independent) | Ưu điểm: Hai trạng thái "chưa có tài xế" và "đã có tài xế" có quy tắc tính phí riêng biệt, không bị ràng buộc phụ thuộc kỹ thuật. | Nhược điểm: Các vai trò liên quan chặt chẽ với nhau; khách bấm hủy thì tài xế và khách đi cùng mới phát sinh xử lý tiếp theo. |
| Vừa vặn (Small) | Ưu điểm: Mỗi Feature tập trung vào một trạng thái cụ thể, dễ tách thành các Story nhỏ làm gọn trong một Sprint. | Ưu điểm: Khối lượng công việc chia theo giao diện từng vai trò khá vừa sức. |
| Giá trị (Valuable) | Ưu điểm: Có thể làm và đưa ngay tính năng hủy miễn phí (khi chưa có tài xế) vào sử dụng trước cho người dùng. | Nhược điểm: Nếu mới chỉ làm xong màn hình của một bên thì luồng hủy chuyến vẫn chưa thể chạy thông suốt thực tế. |

---

## Phần 3: Lựa chọn và Đặc tả chi tiết

### 1. Lựa chọn chiến lược
- Quyết định: Chọn Chiến lược A (Phân rã theo trạng thái chuyến đi).
- Lý do: Chiến lược này giúp phân định rõ quy tắc tính phí (miễn phí so với trừ 10.000 đồng), hạn chế phụ thuộc chéo giữa các Sprint và sớm có luồng chạy hoàn chỉnh để kiểm thử end-to-end.

### 2. User Story đặc tả
As a hành khách đã đặt chuyến xe ghép,  
I want hủy chuyến sau khi tài xế đã nhận cuốc với mức phí 10.000 đồng,  
so that tôi có thể hủy khi bận việc đột xuất và bù đắp một phần công di chuyển cho tài xế.

### 3. Tiêu chí nghiệm thu (Acceptance Criteria) cho tình huống tranh chấp thời điểm
Áp dụng cấu trúc Given / When / Then:
- Given: Khách bấm nút "Hủy chuyến" gần như cùng lúc với thời điểm tài xế bấm "Nhận chuyến".
- When: Cả hai yêu cầu được gửi lên server và hệ thống ghi nhận mốc thời gian (timestamp) nhận chuyến trước thời điểm hủy chuyến.
- Then: Hệ thống xác định đây là trường hợp hủy sau khi có tài xế nhận, tự động trừ phí hủy 10.000 đồng từ ví của khách và hiển thị thông báo chi tiết căn cứ theo giờ của server.
