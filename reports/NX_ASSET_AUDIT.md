# NX asset audit — Phase 5

**29/10/2026, Asia/Saigon.** Runtime/source/wz: **0/16 có file, 0 hợp lệ cấu trúc, 0 đủ điều kiện dùng**. Người dùng xác nhận chưa có bộ tài nguyên riêng.

## Kiểm tra từng package

| File | Có | Nguồn | Quyền xử lý | SHA256 | Cấu trúc/nodes | OpenStory | Cosmic | Runtime |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Base.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Character.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Effect.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Etc.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Item.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Map.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Mob.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Morph.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Npc.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Quest.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Reactor.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Skill.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| Sound.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| String.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| TamingMob.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |
| UI.nx | Không | Chưa cung cấp | Chưa xác minh | — | Không thể kiểm tra | Chưa kiểm chứng | Chưa đối chiếu | Bị chặn |

Preflight Phase 4 được chạy lại trên thư mục source/wz hiện tại, ghi trạng thái thiếu file. Không tạo NX giả hoặc copy fixture vào runtime. Không mở client lại chỉ để lặp lỗi thiếu Base.nx. Hash OpenStory/Cosmic khớp bản Phase 4.

## Phân loại dữ liệu

- **Maplestory, maple-mod/backups/original-client, maple-mod/working-client:** mỗi cây có **34.628 WZ/IMG**, gồm 1 WZ và 34.627 IMG. Nguồn MapleRoyals, quyền xử lý và tương thích v83/Cosmic chưa được xác minh. Không convert.
- **Fixture tự tạo:** metadata inventory thấy 165 WZ/IMG/NX trong các thư mục fixture/build-checks. Gồm cả mẫu cố ý lỗi và mẫu đổi tên để test âm tính; không được tính là game asset. Số lượng mẫu không phải số package hợp lệ.
- **Bộ game v83 riêng đủ nguồn/quyền:** chưa tìm thấy. Người dùng xác nhận chưa cung cấp.
- **Không tương thích/hư hỏng:** chưa đánh giá trên dữ liệu game thật; không gọi MapleRoyals chắc chắn tương thích hoặc không tương thích chỉ dựa tên. Mẫu cố ý lỗi là test data.

## Bảo toàn

Đã hoàn tất kiểm tra mới: **34.628/34.628 WZ/IMG khớp ở mỗi cây**, tổng **103.884 lượt đối chiếu SHA256**; không file thiếu, hash sai hoặc asset thêm ngoài manifest. Bản working được so với hash label probe cũ đã biết ở Data/String/Skill.img, không tính probe đó là thay đổi mới. Original và backup so với manifest gốc.

Evidence cục bộ: phase5/reports/asset-preservation.json. Script chỉ mở WZ/IMG; không đọc nội dung EXE/DLL MapleRoyals hoặc sửa đầu vào. Kiểm tra tồn tại bằng metadata xác nhận 6 đường dẫn binary bị cách ly vẫn không còn ở vị trí cũ. **PASS cho subset asset**, không phải toàn bộ client hoặc phê duyệt quyền dùng.

**Toàn bộ client MapleRoyals vẫn FAIL** vì Defender đã cách ly MapleRoyals.exe và mr.dll trong cả ba cây ở Phase 4. Không restore/exclusion, không kết luận false positive. [Sự cố bảo mật](SECURITY_INCIDENT.md).

## Cần để tiếp tục

NX v83 hoặc WZ/IMG tương ứng được phép dùng/chuyển đổi, cùng thông tin nguồn/phiên bản và bằng chứng quyền. Agent sẽ tính hash và review manifest trước pipeline. Không yêu cầu cung cấp executable MapleRoyals bị cách ly. [Hướng dẫn đầu vào](ASSET_INTAKE.md).

Sau review: staging/conversion → cấu trúc/nodes/media/NoLifeNx → đối chiếu XML Cosmic → runtime riêng → gameplay thực. Profile/dynamic paths và một số codec/layout chưa được chứng minh; không coi 16 file đúng tên là đủ.

Evidence metadata/preflight/hash chi tiết chỉ giữ cục bộ. Không upload NX/WZ/IMG, manifest riêng tư hoặc raw logs. [Latest](LATEST.md).
