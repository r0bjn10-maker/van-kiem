# Blockers — Phase 5

**29/10/2026, Asia/Saigon.** Chưa có baseline gameplay.

## B1 — Thiếu tài nguyên: đang chặn

**0/16 NX**; người dùng xác nhận chưa có bộ riêng. Workspace chỉ có MapleRoyals chưa xác minh quyền/tương thích và fixture. Không convert MapleRoyals hoặc tải WZ/NX từ mirror chưa rõ quyền. Cần NX v83 hoặc WZ/IMG tương ứng kèm nguồn và quyền dùng/chuyển đổi. [Đầu vào](ASSET_INTAKE.md).

Compiler/converter/server/database mới không giải quyết thiếu hình ảnh/UI/âm thanh. XML Cosmic không thay thế bộ client NX.

## B2 — Tương thích dữ liệu: chưa thử

Profile validator chỉ bao phủ một phần paths; dynamic references, UI, codec/layout và fields map/portal/skill cần thử trên bộ thật. Chưa đối chiếu asset thật với Cosmic. Đúng tên .nx/PKG4 không chứng minh game chạy.

## B3 — Authentication/gameplay: chưa có bằng chứng

Chưa login UI; PIN/PIC/world/channel/nhân vật/map/combat/EXP/loot/save-reload chưa chạy. PIN/PIC vẫn bật. Luồng PIN phía client cần kiểm chứng khi có UI; chưa gọi là lỗi đã tái hiện và không tắt xác thực.

## Sự cố riêng

**MapleRoyals full integrity FAIL** do Defender cách ly 6 binary ở Phase 4. Không restore/exclusion. Không cần khôi phục MapleRoyals hoặc tắt bảo vệ để dùng OpenStory. [Báo cáo bảo mật](SECURITY_INCIDENT.md).

## Đã có và bước tiếp theo

Backend hiện chạy localhost; health Phase 5 PASS, 75 bảng/37 changeset/lock=0, JAR/client hash không đổi. Không restart/reset ở Phase 5. Tests Phase 4 là bằng chứng lịch sử có phạm vi; không chạy lại khi chưa có thay đổi cần kiểm.

Đầu vào hợp lệ → review quyền/hash → staging/conversion → preflight/NoLifeNx/đối chiếu XML → runtime riêng → gameplay/persistence thật → mới làm Kiếm Tu. [Latest](LATEST.md).
