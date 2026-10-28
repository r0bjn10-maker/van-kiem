# VẤN KIẾM — Báo cáo tiến độ mới nhất

**29/10/2026 — Phase 5: Real Asset Integration & First Playable.**

**Chưa chơi được game.** OpenStory/Cosmic và pipeline đã có, nhưng vẫn **0/16 NX game thực tế**. Người dùng xác nhận chưa bổ sung bộ tài nguyên riêng. Không convert MapleRoyals, không tải asset thương mại chưa rõ quyền và không triển khai Kiếm Tu.

## Đã thực hiện trong Phase 5

- Đọc yêu cầu, các báo cáo Phase 4 và tài liệu vận hành. Dùng lại converter/validator; không rebuild hoặc chạy lại toàn bộ suite đã PASS.
- Kiểm kê metadata workspace bằng công cụ Phase 4: chỉ thấy MapleRoyals/quyền chưa xác minh và fixture tự tạo, không thấy bộ game v83 riêng. Kiểm kê không tự chứng minh quyền hoặc tương thích.
- Preflight source/wz: **present=0, structurally_valid=0, qualified_game_assets=0 trên 16 yêu cầu**. Đây là trạng thái bị chặn vì thiếu file, không phải test thành công.
- Host/backend đang chạy: Defender real-time bật, 5 listener đúng 127.0.0.1, handshake protocol 83; **75 bảng, 37 changeset, lock=0**, health PASS.
- Cosmic.jar/OpenStory.exe khớp hash Phase 4. Không restart/reinstall/reset server/MySQL, không tạo/xóa tài khoản/nhân vật hoặc sửa authentication.
- Tách [báo cáo Defender](SECURITY_INCIDENT.md) và [hướng dẫn cung cấp asset](ASSET_INTAKE.md). Kiểm tra WZ/IMG theo manifest: **34.628/34.628 khớp mỗi cây original/backup/working**, tổng 103.884 đối chiếu; working giữ probe cũ đã biết, không có thay đổi mới. Không đọc nội dung EXE/DLL MapleRoyals. [Chi tiết](NX_ASSET_AUDIT.md).

## First Playable

| Mốc | Trạng thái Phase 5 |
| --- | --- |
| NX game đủ điều kiện | **0/16** |
| Bộ v83 riêng có nguồn/quyền | Chưa có; người dùng xác nhận |
| Converter trên dữ liệu game thật | Chưa; chỉ có fixture lịch sử |
| Login UI | Chưa hiển thị |
| Login/PIN/PIC/world/channel | Chưa kiểm thử qua game |
| Tạo nhân vật / vào Maple World | Chưa |
| Movement/portal/combat/EXP/loot | Chưa |
| Lưu/mở lại nhân vật đã chơi | Chưa |
| Ảnh gameplay | Chưa có |

Không mở lại client chỉ để lặp lỗi thiếu Base.nx. Lần chạy thật Phase 4 nhận handshake rồi báo **Missing a game file: Base.nx** trước window/login. Handshake, seed và unit test không chứng minh gameplay.

## Bảo toàn và kết quả lịch sử

**MapleRoyals full integrity vẫn FAIL**: Defender cách ly MapleRoyals.exe/mr.dll trong original/backup/working (6 đường dẫn) ở Phase 4. Không restore/exclusion/kết luận false positive. Host Phase 5 lúc **06:44** ghi RealTimeProtectionEnabled=true; agent không đổi bảo vệ. WZ/IMG được báo riêng, không dùng kết quả subset để xóa sự cố binary.

Kết quả **các nhiệm vụ trước, không chạy lại ở Phase 5**:

- Cosmic v1.1.3 build: **1.886/1.886 tests PASS**.
- Converter/pipeline: **29/29 PASS** trên fixture; chưa kiểm chứng mọi WZ/codec thương mại.
- OpenStory: **3/3 CTest, 10/10 framing PASS**; LNK4098/LNK4099 đã sửa bằng GLEW source build cùng CRT.
- OpenStory x64 SHA256: F4762A8EBB1879F94E853E53EECD4A05730F2198D21009816974E928866CAC5E.
- Cosmic.jar SHA256: 11454982AD649B136AC7973DA07B9D50D102C14A0E0957A53AF6F0964CB6301A.

Code/evidence mới giữ cục bộ tại **cosmic-local/openstory/phase5/**. Binary, asset, database, manifest chi tiết, secrets, raw logs và source không thuộc commit báo cáo.

## Tiếp theo

**Cần đầu vào từ người dùng:** thư mục NX hoặc WZ/IMG v83 với nguồn và bằng chứng quyền dùng/chuyển đổi. Agent sẽ băm và tạo manifest sau review; người dùng không cần tự băm. [Cách cung cấp](ASSET_INTAKE.md).

Sau đó: staging/conversion → validator/NoLifeNx → đối chiếu Cosmic → runtime riêng → login/PIN/PIC → nhân vật/map cổ điển/combat → lưu/mở lại. Giữ nguyên map/quái MapleStory, chưa thêm nội dung tu tiên.

**Phase 5 bị chặn ở asset; First Playable chưa đạt.** [Audit](NX_ASSET_AUDIT.md) · [Blockers](BLOCKERS.md) · [Gameplay](GAMEPLAY_BASELINE.md). Tài liệu kỹ thuật Phase 4: [pipeline](NX_PIPELINE.md), [converter QA](NX_CONVERTER_QA.md), [startup QA](OPENSTORY_STARTUP_QA.md), [client/server](CLIENT_SERVER_INTEGRATION.md).

Đường dẫn ổn định: [GitHub](https://github.com/r0bjn10-maker/van-kiem/blob/main/reports/LATEST.md) · [Markdown trực tiếp](https://raw.githubusercontent.com/r0bjn10-maker/van-kiem/main/reports/LATEST.md). Chỉ gọi đồng bộ thành công sau khi đọc lại file và kiểm tra diff commit.
