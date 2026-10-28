# NX asset audit — Phase 4
Ngày 29/10/2026. **Runtime: 0/16 NX tồn tại; 0 hợp lệ cấu trúc; 0 game asset đủ điều kiện.**

| File bắt buộc | Có trong runtime | Đủ điều kiện game |
| --- | --- | --- |
| Base.nx | Không | Không |
| Character.nx | Không | Không |
| Effect.nx | Không | Không |
| Etc.nx | Không | Không |
| Item.nx | Không | Không |
| Map.nx | Không | Không |
| Mob.nx | Không | Không |
| Morph.nx | Không | Không |
| Npc.nx | Không | Không |
| Quest.nx | Không | Không |
| Reactor.nx | Không | Không |
| Skill.nx | Không | Không |
| Sound.nx | Không | Không |
| String.nx | Không | Không |
| TamingMob.nx | Không | Không |
| UI.nx | Không | Không |

Đã kiểm tra đường dẫn thật cosmic-local/openstory/source/wz và runtime chẩn đoán riêng trong phase4/runtimes/. Không copy fixture vào hai vị trí này.

## Phân loại workspace
- Source mở: Cosmic/OpenStory AGPL, converter NoLifeWzToNx có header AGPL-3.0-or-later; dependency source có notices riêng.
- Công cụ: converter, validator, native interop reader, scripts audit/compare/runtime/evidence.
- Thương mại, quyền chưa xác minh: Maplestory, maple-mod/backups, maple-mod/working-client. Mỗi nhóm kiểm kê có 34.628 WZ/IMG: 1 WZ và 34.627 IMG. Đây là subset asset, khác tổng 34.654 file/cây của audit client đầy đủ.
- Fixture tự tạo: số, chuỗi Unicode, vector, ảnh 2×2, animation metadata, PCM và archive legacy nhỏ; nằm ngoài runtime. Có cả fixture lỗi và bản đổi tên để test âm tính, không phải nội dung game.
- Bộ v83 thực tế có bằng chứng quyền dùng: chưa tìm thấy. Không suy quyền từ vị trí file hoặc việc file được đăng công khai.

Không tải WZ/NX thương mại hoặc chuyển đổi MapleRoyals. Kiểm kê metadata và kiểm tra hash bảo toàn không được coi là phê duyệt quyền dùng. Runtime không dùng các IMG MapleRoyals đã có.

## Sự cố bảo toàn binary — FAIL
Lần full hash check ngày 29/10 không đạt. Defender real-time đang bật đã phát hiện và cách ly MapleRoyals.exe và mr.dll trong original, backup và working (6 file), khoảng 06:21–06:24, trùng thời gian đọc hash. Event 1116/1117 ghi nhãn Trojan:Win32/Malgent!MSR và action Quarantine; kiểm tra metadata sau đó thấy cả 6 file không còn ở vị trí cũ. Không kết luận false positive.

Không có lệnh agent sửa/xóa/chạy các binary này, nhưng kết quả bảo toàn nội dung client đầy đủ vẫn **FAIL** vì quarantine. Không tự restore hoặc tạo exclusion. WZ/IMG không có thay đổi hash mới; working vẫn có label probe cũ. Giữ log/detection metadata cục bộ; không upload file bị cách ly. Cần xử lý riêng nguồn binary đáng tin cậy, không dùng nguồn chưa xác minh để tự khôi phục các bản lưu.

## Nguồn công cụ và profile
Đã đọc [NoLifeWzToNx](https://github.com/ryantpayton/NoLifeWzToNx), [NX format](https://nxformat.github.io/) và loader trong source OpenStory. Giấy phép công cụ không cấp quyền cho input.

NxFiles::init bắt buộc 16 tên trên. NoLifeNx có đọc tùy chọn Map001/MapLatest/MapPretty và nhánh Data.nx; preflight OpenStory hiện vẫn đòi đủ 16 file. Không tự tải tài nguyên bản mới để bù node UI cũ.

Cần cung cấp NX v83 hoặc WZ/IMG được phép xử lý, kèm nguồn/quyền dùng và manifest SHA256 đã review. Nên đặt riêng ở phase4/user-assets; không chép lên GitHub. Node profile khớp chỉ cho phép bước kiểm tra tiếp, chưa chứng minh tương thích toàn game.

[Pipeline](NX_PIPELINE.md) · [Blockers](BLOCKERS.md).
