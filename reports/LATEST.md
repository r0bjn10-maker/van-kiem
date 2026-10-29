# VẤN KIẾM — Dự án đã dừng

**29/10/2026, Asia/Saigon.** Người dùng yêu cầu dừng dự án và xóa toàn bộ file liên quan trên máy; xác nhận **giữ repository GitHub**.

## Đã thực hiện

- Dừng Cosmic và MySQL bằng công cụ shutdown của môi trường; không tiếp tục phát triển hoặc chạy thử gameplay.
- Gỡ đúng instance Microsoft Visual Studio Build Tools 2022 được cài riêng dưới thư mục dự án. Trình gỡ Microsoft có chữ ký hợp lệ trả **exit code 0**; kiểm tra vswhere sau đó không còn instance.
- Xóa toàn bộ nội dung **D:\Project\Vấn Kiếm**, gồm source Phaser/Cosmic/OpenStory, client, MapleRoyals original/backup/working, database, secrets, cache, dependency, công cụ và báo cáo cục bộ.
- Kiểm tra đường dẫn tuyệt đối trước khi xóa; duyệt metadata không đi theo reparse point, bỏ liên kết trước thao tác recursive để tránh đi ra ngoài thư mục được chỉ định.
- Gỡ alias V: của riêng dự án. Không xóa repository GitHub hoặc upload dữ liệu dự án.

## Kiểm tra thực tế và phần còn lại

| Kiểm tra sau dọn dẹp | Kết quả |
| --- | --- |
| Nội dung trong thư mục dự án, kể cả file ẩn | **0 entry** |
| Cosmic/MySQL và executable chạy từ thư mục/alias dự án | Không còn |
| Listener 3308, 8484, 7575–7577 | Không còn |
| Alias V: | Không còn |
| Instance Build Tools riêng của dự án | Đã gỡ |
| Thư mục gốc D:\Project\Vấn Kiếm | **Còn rỗng; Windows báo đang được tiến trình sử dụng** |
| Repository r0bjn10-maker/van-kiem | Giữ theo yêu cầu |

Đã thử xóa thư mục gốc lần nữa sau khi tiến trình xóa kết thúc; Windows vẫn báo đang được sử dụng. **File đã xóa hết, nhưng chưa xóa được chính thư mục rỗng.** Việc còn lại là xóa thư mục rỗng sau khi tiến trình giữ khóa giải phóng nó; chưa xác định tiến trình giữ khóa, không tự đóng ứng dụng người dùng.

Phạm vi dọn dẹp là dữ liệu dự án trên máy. Không tác động kho cách ly do Windows Defender quản lý hoặc phần mềm/hạ tầng dùng chung. Không tạo lại thư mục reports/ cục bộ sau khi xóa; báo cáo đóng dự án được cập nhật trực tiếp trên GitHub theo quy trình đã được người dùng cho phép.

## Trạng thái phát triển cuối cùng

Trước khi dừng, client chưa có bộ NX v83 đủ điều kiện; **chưa xác minh login UI, tạo nhân vật, vào map, đánh quái hoặc lưu/mở lại qua gameplay**. Không triển khai Kiếm Tu hoặc Nhất Tuyến Kiếm Khí.

Các báo cáo khác là lịch sử, không chứng minh server/database còn đang chạy sau lần dọn dẹp này. [Báo cáo Phase 5 trước khi xóa](https://github.com/r0bjn10-maker/van-kiem/blob/5802c51d9d506a50f2d00dbf9fb84e277d8c4825/reports/LATEST.md) được giữ trong lịch sử Git.

**Không có công việc phát triển tiếp theo được lên lịch.** Chỉ còn việc xóa thư mục gốc rỗng khi hết khóa. Commit này chỉ cập nhật Markdown trong reports/; không chứa binary, asset, database, secrets hoặc log thô.
