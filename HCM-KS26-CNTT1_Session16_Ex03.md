# Bài 3: Xây dựng Không gian Cộng tác Doanh nghiệp

## 1. Mục tiêu

Thiết lập một không gian cộng tác thống nhất trên Slack hoặc Lark cho dự án **bãi đỗ xe thông minh**. Không gian này giúp:

- Tách thông báo quan trọng khỏi trao đổi kỹ thuật và trò chuyện giải trí.
- Giảm tình trạng tin nhắn bị trôi trong Messenger.
- Ghi lại quyết định, người phụ trách và thời hạn để mọi thành viên có thể đọc lại khi cần.
- Khuyến khích làm việc bất đồng bộ, hạn chế việc phải trả lời ngay lập tức.

Quy ước dưới đây có thể áp dụng cho cả Slack và Lark. Tên kênh dùng tiền tố `smartpark-` để dễ tìm kiếm.

## 2. Sơ đồ kiến trúc kênh

```text
smartpark (Workspace)
│
├── 00-thong-bao           Thông báo chính thức, deadline, thay đổi phạm vi
├── 01-dieu-phoi           Điều phối công việc, phân công, blocker
├── 02-ky-thuat-chung      Kiến trúc hệ thống và các vấn đề liên module
├── 03-cam-bien-iot        Cảm biến, gateway, giao tiếp phần cứng
├── 04-backend-api         API, cơ sở dữ liệu, xác thực, xử lý nghiệp vụ
├── 05-frontend-app        Web/app quản trị và giao diện người dùng
├── 06-kiem-thu            Kịch bản test, lỗi, kết quả kiểm thử
├── 07-tai-lieu             Tài liệu, biên bản họp, quyết định kỹ thuật
├── 08-van-hanh             Triển khai, giám sát, sự cố môi trường chạy thật
└── 99-phong-trao           Meme, chuyện vui, hoạt động gắn kết nhóm
```

### Danh sách kênh và quy định nội dung

| Kênh | Dùng cho | Không dùng cho | Ai nên theo dõi |
|---|---|---|---|
| `#smartpark-00-thong-bao` | Thông báo chính thức, mốc deadline, thay đổi yêu cầu, quyết định đã chốt | Tranh luận dài, meme, cập nhật cá nhân | Tất cả thành viên |
| `#smartpark-01-dieu-phoi` | Phân công, cập nhật trạng thái, nêu blocker, xin hỗ trợ | Trao đổi chi tiết về code hoặc phần cứng | Tất cả thành viên; trưởng nhóm theo dõi sát |
| `#smartpark-02-ky-thuat-chung` | Kiến trúc tổng thể, luồng dữ liệu, tiêu chuẩn tích hợp, câu hỏi liên module | Lỗi riêng của một module | Thành viên kỹ thuật |
| `#smartpark-03-cam-bien-iot` | Cấu hình cảm biến, MQTT, gateway, lỗi đọc dữ liệu, sơ đồ đấu nối | Nội dung backend/frontend không liên quan | Nhóm IoT và người tích hợp |
| `#smartpark-04-backend-api` | Thiết kế API, database, auth, xử lý trạng thái chỗ đỗ | Báo lỗi giao diện hoặc đấu nối cảm biến | Nhóm backend và người tích hợp |
| `#smartpark-05-frontend-app` | Màn hình bản đồ bãi xe, dashboard, trải nghiệm người dùng | Thay đổi schema API chưa được thống nhất | Nhóm frontend và QA |
| `#smartpark-06-kiem-thu` | Test case, bug report, log kiểm thử, kết quả regression | Tán gẫu hoặc báo lỗi thiếu thông tin | QA, dev và người phụ trách tính năng |
| `#smartpark-07-tai-lieu` | Link tài liệu, biên bản họp, ADR, hướng dẫn cài đặt | Hỏi đáp ngắn đã có kênh chuyên môn | Tất cả thành viên |
| `#smartpark-08-van-hanh` | Deploy, monitoring, cảnh báo, sự cố production hoặc staging | Thảo luận tính năng chưa triển khai | Người vận hành, backend, trưởng nhóm |
| `#smartpark-99-phong-trao` | Meme, chúc mừng, hoạt động nhóm, chuyện ngoài lề | Deadline, blocker, yêu cầu hỗ trợ khẩn | Ai muốn tham gia |

### Topic và thread trong từng kênh

Mỗi kênh có thể chia thêm **Topic** hoặc dùng **thread** theo cùng một nguyên tắc:

- Một chủ đề = một tin nhắn gốc.
- Các câu hỏi, phản biện, log và cập nhật của chủ đề đó trả lời trong thread.
- Khi chủ đề đã chốt, người tạo thread đăng một dòng kết luận bắt đầu bằng `KẾT LUẬN:`.
- Nếu quyết định ảnh hưởng cả dự án, tóm tắt lại trong `#smartpark-00-thong-bao` và dẫn link về thread gốc.

Ví dụ:

```text
#smartpark-03-cam-bien-iot
[CẦN QUYẾT ĐỊNH] Chọn chu kỳ gửi dữ liệu của cảm biến chỗ đỗ
└─ Thread: ưu/nhược điểm 5 giây và 10 giây
   └─ KẾT LUẬN: dùng 5 giây ở khu vực vào/ra, 10 giây ở khu vực ổn định.
```

## 3. Cách dùng Reply in Thread để giữ kênh chính sạch

Tin nhắn gốc phải đủ thông tin để người đọc quyết định có cần mở thread hay không. Có thể dùng mẫu sau:

```text
[LOẠI] Tóm tắt ngắn
Mục tiêu: ...
Bối cảnh hoặc link liên quan: ...
Cần ai phản hồi: @tên hoặc nhóm
Hạn phản hồi: YYYY-MM-DD HH:mm (GMT+7)
```

Các loại tin nhắn nên dùng:

- `[THÔNG BÁO]`: thông tin một chiều, đã được xác nhận.
- `[CẦN QUYẾT ĐỊNH]`: cần chọn một phương án; tranh luận trong thread.
- `[CẦN HỖ TRỢ]`: đang bị chặn; nêu rõ đã thử gì và cần hỗ trợ nào.
- `[CẬP NHẬT]`: báo tiến độ, không yêu cầu thảo luận dài.
- `[BUG]`: lỗi có bước tái hiện, mức độ ảnh hưởng và link log.

Không tạo tin nhắn mới cho từng câu trả lời của cùng một vấn đề. Nếu có thêm thông tin, hãy trả lời vào thread cũ để lịch sử không bị chia nhỏ.

## 4. Quy ước giao tiếp: 3 quy tắc vàng

### Quy tắc 1: Vào thẳng nội dung, không mắc “No-Hello Trap”

Không gửi riêng một tin nhắn chỉ có “Hi”, “Mọi người ơi” hoặc “Cho hỏi chút”. Hãy viết luôn câu hỏi và bối cảnh để người nhận có thể trả lời khi rảnh.

**Không nên:**

```text
Hi anh Minh ơi...
```

**Nên:**

```text
@Minh Mình gặp lỗi timeout khi gateway gửi MQTT ở chu kỳ 5 giây.
Đã kiểm tra: Wi-Fi và broker đều hoạt động. Bạn xem giúp phần retry trong hôm nay được không?
```

### Quy tắc 2: Chỉ dùng @all khi cả nhóm phải hành động

`@all` chỉ dành cho thông báo ảnh hưởng đến toàn bộ dự án, chẳng hạn đổi deadline, dừng môi trường test hoặc sự cố nghiêm trọng. Với việc thường ngày, tag đúng người hoặc nhóm phụ trách. Nếu cần nhiều người nhưng không phải tất cả, dùng nhóm như `@backend`, `@iot`, `@qa` nếu workspace đã thiết lập.

Mỗi thông báo có `@all` phải nói rõ: **việc gì**, **ai cần làm**, **hạn nào**. Không dùng `@all` để thúc tiến độ hoặc thay cho việc phân công.

### Quy tắc 3: Viết để người khác đọc lại được

Mỗi yêu cầu cần có bối cảnh, mức ưu tiên và thời hạn. Đính kèm link tài liệu, log hoặc issue thay vì gửi nhiều mẩu tin rời rạc. Khi đã có kết quả, cập nhật lại thread bằng `ĐÃ XỬ LÝ`, `ĐANG CHỜ` hoặc `KẾT LUẬN`.

## 5. Giải quyết “ép rep ngay” và bảo vệ thời gian tập trung

Mặc định, dự án làm việc theo cơ chế bất đồng bộ:

- Tin nhắn thông thường không yêu cầu phản hồi ngay. Thành viên kiểm tra Slack/Lark vào các khung giờ đã thống nhất, ví dụ đầu ngày, sau giờ nghỉ trưa và cuối ngày.
- Người gửi phải đặt hạn phản hồi cụ thể thay vì viết “rep gấp”. Nếu chưa có hạn, người nhận xử lý theo mức ưu tiên của mình.
- Khi đang lập trình, thành viên có thể bật trạng thái `Đang tập trung` hoặc tắt thông báo trong khoảng thời gian đã đăng ký. Trạng thái này không bị xem là thiếu hợp tác.
- Chỉ đánh dấu **khẩn cấp** khi có nguy cơ mất dữ liệu, hệ thống đang ngừng hoạt động, lỗi chặn việc phát hành hoặc vấn đề an toàn. Khi đó dùng kênh `#smartpark-08-van-hanh` và quy trình gọi trực tiếp theo danh sách on-call.
- Những việc không khẩn cấp vẫn được ghi thành issue/task để không phụ thuộc vào việc ai đang online.

### Ma trận phản hồi đề xuất

| Mức | Ví dụ | Kênh/cách báo | Thời gian phản hồi kỳ vọng |
|---|---|---|---|
| P0 - Khẩn cấp | Mất dữ liệu, hệ thống production dừng | `#smartpark-08-van-hanh` + gọi on-call | Bắt đầu xử lý trong 15 phút |
| P1 - Chặn tiến độ | Không thể tích hợp hoặc test một hạng mục | Kênh chuyên môn + `[CẦN HỖ TRỢ]` | Trong cùng ngày làm việc |
| P2 - Bình thường | Câu hỏi kỹ thuật, góp ý cải tiến | Thread trong kênh phù hợp | Trong 1 ngày làm việc |
| P3 - Tham khảo | Ý tưởng, tài liệu đọc thêm | Thread hoặc `#smartpark-99-phong-trao` | Khi có thời gian |

## 6. Nhịp vận hành tối thiểu

1. Mỗi đầu tuần, trưởng nhóm đăng mục tiêu và deadline trong `#smartpark-00-thong-bao`.
2. Mỗi thành viên cập nhật tiến độ hoặc blocker trong `#smartpark-01-dieu-phoi`, ưu tiên dùng thread theo task.
3. Mỗi quyết định kỹ thuật quan trọng được ghi lại trong `#smartpark-07-tai-lieu` dưới dạng ADR hoặc biên bản ngắn.
4. Cuối tuần, trưởng nhóm rà lại các thread có trạng thái `ĐANG CHỜ` để nhắc đúng người, không nhắc cả nhóm bằng `@all`.

## 7. Kết quả mong đợi

Sau khi áp dụng cấu trúc này, thành viên có thể biết ngay:

- Thông báo chính thức nằm ở đâu.
- Vấn đề thuộc module nào và ai phụ trách.
- Một câu hỏi đang chờ ai trả lời, chờ đến khi nào.
- Nội dung nào cần xử lý ngay và nội dung nào có thể đọc sau.

Nhờ vậy, nhóm vẫn phản hồi đủ nhanh cho sự cố thật sự nhưng giữ được thời gian tập trung cho việc lập trình và kiểm thử.
