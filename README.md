

Phần 1. Phân tích bằng Inspect Element
Em nhấn F12, hoặc Ctrl + Shift + C để chọn trực tiếp khung “Thông tin tài xế”. Trong tab Elements, DevTools tô sáng phần tử HTML tương ứng; tiếp theo chọn Computed để xem Box Model. Em làm tương tự với khung “Danh sách trạm”.
Box Model gồm Content (phần nội dung), Padding (khoảng đệm bên trong), Border (viền) và Margin (khoảng cách bên ngoài). Tổng chiều rộng thực tế trong chế độ content-box là Content + Padding trái/phải + Border trái/phải; Margin chiếm thêm khoảng trống xung quanh.
Vì đề không cung cấp số đo khi mở F12, em lấy ví dụ minh họa: khung tài xế có Content 360 px, Padding trái/phải mỗi bên 24 px và Border mỗi bên 2 px. Tổng chiều ngang phần hộp là 360 + 48 + 4 = 412 px, chưa tính Margin. Nếu hai khung đặt cạnh nhau mà tổng bề rộng vượt khung cha, chúng dễ bị tràn hoặc chồng lên nhau khi có position, width hay layout chưa hợp lý.

Phần 2. Triển khai thay đổi trong tab Styles
Em chỉ thử các thuộc tính ở Styles của DevTools, không sửa file CSS gốc. Giả sử đã xác định được tên lớp của khung cha và hai khung con, em thử các giá trị dưới đây. Khi có hiệu quả mới ghi lại để người phụ trách giao diện áp dụng vào mã nguồn sau.
.route-layout {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
.driver-info, .station-list {
  box-sizing: border-box;
  position: static;
  flex: 1 1 320px;
  min-width: 0;
  margin: 0;
  padding: 16px;
  border: 1px solid #999;
}

gap: 16px tách hai khung, flex-wrap giúp xuống dòng khi màn hình hẹp, còn box-sizing: border-box tính cả Padding và Border trong kích thước đã đặt. Tên lớp trên là ví dụ, cần đổi theo phần tử thực tế trong DOM. Nếu trang vốn đang dùng CSS Grid thì có thể giữ Grid và chỉnh gap, minmax thay vì thay toàn bộ bằng Flexbox.
Phần 3. Kiểm thử khi Margin không có tác dụng
Nếu đổi Margin mà vị trí vẫn không như mong muốn, em xem lại Computed và Styles xem có position: absolute/fixed, top/left, transform: translate(...), margin bị gạch đi do quy tắc ưu tiên cao hơn, hoặc !important hay style inline đang ghi đè không. Cũng cần kiểm tra width, overflow và bố cục Flex/Grid của khung cha. Đây là kết quả áp dụng CSS và tính toán layout của trình duyệt, không phải một “lỗi Browser Engine” riêng. Các thay đổi trong DevTools chỉ mang tính thử nghiệm và sẽ mất khi tải lại trang.
