# Gameplay baseline — Phase 4
Ngày 29/10/2026. **NOT RUN — thiếu NX v83 được phép sử dụng.**

| Bước | Trạng thái |
| --- | --- |
| Build client và chạy process chẩn đoán | Đạt |
| TCP/handshake Cosmic | Đạt |
| Hiển thị login thật | Chưa |
| Account authentication / PIN / PIC | Chưa |
| World/channel selection | Chưa |
| Character list/create/select | Chưa |
| Vào Maple Island/map cổ điển | Chưa |
| Trái/phải, nhảy, leo dây/thang | Chưa |
| Portal | Chưa |
| Gây damage, giết quái | Chưa |
| Nhận EXP | Chưa |
| Loot / Inventory | Chưa |
| Logout/reopen; Level/EXP/map/items giữ đúng | Chưa |

Không dùng unit test, file fixture, đổi tên 16 package, nhân vật Admin seed, handshake hoặc ảnh minh họa để gọi là gameplay thành công. Chưa có ảnh gameplay thật.

## Quy trình khi đủ asset
1. Preflight dữ liệu có quyền dùng; đối chiếu XML và node UI/animation/portal.
2. Chuẩn bị runtime riêng từ EXE/DLL đã kiểm nguồn, Settings localhost.
3. Mở login thật, dùng tài khoản thường mới và PIN/PIC hợp lệ; giữ dữ liệu người dùng khác.
4. Chọn world/channel, tạo nhân vật, vào map phù hợp; thử movement, portal, mob damage/death, EXP, loot/inventory.
5. Ghi Level/EXP/map/items của chính nhân vật thử nghiệm; logout/mở lại và đối chiếu.
6. Chụp từ gameplay thật; giữ ảnh/log cục bộ và chỉ công bố dữ liệu đã review theo phạm vi cho phép.

collect_evidence.py hiện thu status và hash/kích thước log, không chép nội dung log nhạy cảm hoặc tự đánh dấu gameplay PASS. Khi chạy được game cần bổ sung bằng chứng quan sát thực tế; script không thay người kiểm thử.

**Kiếm Tu chưa sẵn sàng bắt đầu.** Không triển khai cảnh giới/phi kiếm/kỹ năng tu tiên trước baseline này. [Blockers](BLOCKERS.md).
