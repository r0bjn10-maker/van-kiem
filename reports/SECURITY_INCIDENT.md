# Sự cố Defender — MapleRoyals

**29/10/2026, Asia/Saigon — Phase 5.**

## Sự cố đã ghi nhận

Khoảng **06:21–06:24 ngày 29/10/2026**, trong lúc đọc hash ở Phase 4, Defender ghi sự kiện 1116/1117, nhãn **Trojan:Win32/Malgent!MSR**, hành động **Quarantine**. MapleRoyals.exe và mr.dll bị ảnh hưởng trong original, backup và working: **6 đường dẫn**.

Không kết luận false positive. Agent không thực thi, sửa hoặc chủ động xóa các binary, nhưng bảo toàn toàn bộ client vẫn **FAIL**. Băm lại WZ/IMG không đổi kết luận đó thành PASS.

## Kiểm tra Phase 5

- Đọc bằng chứng sự kiện/báo cáo đã có, không mở nội dung binary nghi vấn hoặc kho quarantine.
- Host lúc **06:44 ngày 29/10**: AMServiceEnabled=true, AntivirusEnabled=true, **RealTimeProtectionEnabled=true**, signature **1.463.10.0**.
- Không restore, exclusion, tắt bảo vệ hoặc tải binary thay thế.
- Kiểm tra SHA256 mới chỉ mở WZ/IMG trong manifest: 34.628/34.628 khớp mỗi cây; working tính probe cũ đúng hash đã ghi. Không có thay đổi asset mới. Metadata xác nhận 6 đường dẫn binary vẫn vắng mặt; không đọc nội dung binary. Không chuyển đổi/phê duyệt quyền dùng. [Chi tiết](NX_ASSET_AUDIT.md).
- Cosmic/MySQL vẫn 127.0.0.1; không restart/reset/thay dữ liệu ở Phase 5. JAR và client tự build khớp hash Phase 4.

## Xử lý tiếp và giới hạn

Không cần khôi phục MapleRoyals.exe/mr.dll để dùng OpenStory tự build với NX riêng. Giữ nguyên cách ly; không dùng bản sao/nguồn chưa xác minh để né Defender.

Log sự kiện, metadata và hash chi tiết giữ cục bộ trong cosmic-local/openstory/phase4/reports và phase5/reports. Không upload file bị phát hiện, asset hoặc log thô.

Bảo vệ hiện bật không bảo đảm mọi file an toàn. Hash không đổi không chứng minh quyền dùng, phiên bản tương thích hoặc an toàn tuyệt đối.
