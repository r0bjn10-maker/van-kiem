# Client/server integration — Phase 4
Ngày 29/10/2026. **TCP/handshake đạt; login và payload/UI chưa được xác minh.**

Cosmic v1.1.3, commit fec53bc7714dc0f1ae3f50b2986cdf2727e0912a. JAR hiện có giữ nguyên; 1.886 server tests PASS là kết quả nhiệm vụ trước, không chạy lại hàng nghìn tests trong Phase 4.

OpenStory giữ ServerIP=127.0.0.1, ServerPort=8484, SaveLogin=false qua Settings được hỗ trợ. Không patch endpoint executable hoặc đổi charset/PIN/PIC để vượt xác thực.

## Resume môi trường
Khi bắt đầu Phase 4, baseline server/database hoạt động. Khi tiếp tục sau gián đoạn, cả hai process đã dừng; MySQL trả lỗi 2003. Đã kiểm tra data/config/bootstrap marker có sẵn, cổng rảnh, rồi chỉ khởi động MySQL cũ và Cosmic cũ. Không chạy initialize, tạo lại schema hoặc reset nhân vật.

Sau resume và test: **75 bảng, 37 changeset, lock=0, 1 account/1 character/5 inventory item seed**, JAR không đổi. Login/channel/MySQL vẫn chỉ 127.0.0.1. Không tạo nhân vật thử nghiệm; process ID thay đổi do resume từ trạng thái dừng, không được báo là cùng PID xuyên suốt.

## Phạm vi đã kiểm
Winsock thật đọc handshake v83/subversion 1/locale 8. Test Session/Cryptography in-memory xử lý fragmentation/reconnect. Có kết quả lịch sử 25 opcode chọn lọc khớp giữa client/server; CMake/linker thay đổi không sửa authentication/game packets.

compare_cosmic.py đã được tạo cho so sánh container ID sau preflight + evidence quyền dùng. Hiện output **blocked**, differences=null vì 0/16 asset đủ điều kiện. Script không giả tạo diff “không có khác biệt”. Item/skill/quest ID bên trong container, portal fields, animation và toàn bộ UI/resource semantics vẫn cần đối chiếu chuyên biệt khi có dữ liệu.

Cosmic giữ PIN/PIC bật. Client có PIC code; packet PIN chưa được xác minh đầy đủ. Sau khi login UI hiển thị, cần thử/bổ sung luồng PIN hợp lệ từ source client/server. Không bỏ qua authentication.

[Audit](NX_ASSET_AUDIT.md) · [Gameplay checklist](GAMEPLAY_BASELINE.md).
