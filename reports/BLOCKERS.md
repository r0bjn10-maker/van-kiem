# Blockers — Phase 4
Ngày 29/10/2026.

## Đang chặn
**B0 — Bảo toàn binary MapleRoyals không đạt.** Defender đã cách ly MapleRoyals.exe và mr.dll ở original/backup/working trong lúc đọc hash. 6 file hiện không còn ở vị trí cũ. Không kết luận false positive, không tự restore/exclusion. WZ/IMG không có thay đổi hash mới. Đây là sự cố phải được ghi nhận riêng, không được gọi toàn bộ môi trường là đã bảo toàn. Xem [audit](NX_ASSET_AUDIT.md).

**B1 — Bộ NX v83 được phép dùng: 0/16.** Thiếu Base, Character, Effect, Etc, Item, Map, Mob, Morph, Npc, Quest, Reactor, Skill, Sound, String, TamingMob, UI (đuôi .nx). Cần nguồn và bằng chứng quyền xử lý/sử dụng, kèm hash. Nếu cung cấp WZ/IMG phù hợp thay NX, có thể chạy converter trên bản copy sau review. Không mặc định MapleRoyals là nguồn hợp lệ. [Chi tiết](NX_ASSET_AUDIT.md).

**B2 — UI/node/version compatibility.** Profile validator mới bao phủ một phần các đường dẫn source-derived; dynamic paths, một số node MapLatest/MapPretty và semantic fields cần kiểm trên dữ liệu thật. 16 tên file/PKG4 hợp lệ chưa đủ.

**B3 — Login/PIN/PIC và gameplay.** Chưa có login UI nên chưa thử authentication, character, map/combat/save. Cần xử lý protocol hợp lệ khi có bằng chứng lỗi, giữ PIN/PIC.

**B4 — Phạm vi converter/validator.** Đã kiểm primitive/vector/UOL/RGBA8888/animation metadata/PCM trên fixture; chưa thử mọi codec, layout/locale WZ hoặc asset thương mại. Validator có safety limits và full_dynamic_reference_coverage=false, không phải audit parser hoàn chỉnh.

## Đã xử lý
Converter build trên MSVC hiện có; Unicode path/exit code/bounds/bitmap lỗi; runner giữ input và không overwrite. LNK4098/LNK4099 được xử lý bằng GLEW source build cùng CRT. Cosmic/MySQL đã resume từ dữ liệu cũ sau khi phát hiện process dừng, không initialize/reset.

Lượt build trước từng bị chặn bởi bộ duyệt tự động chạm hạn mức. Lần tiếp tục đã được duyệt và build/test thực thi thành công; đây không còn là blocker hiện tại. Không dùng cách bỏ qua bước duyệt.

## Tiếp theo
1. Có bộ asset v83 đủ quyền và manifest được review.
2. Convert trên bản copy → preflight → so sánh IDs/fields/node → runtime riêng.
3. Login/UI/PIN/PIC → nhân vật → map → combat/EXP/loot → logout/reload với bằng chứng thật.
4. Chỉ sau đó bắt đầu Kiếm Tu trong Maple World cổ điển.

Các điều kiện kỹ thuật Minimum Success của pipeline/OpenStory/Cosmic đạt. **Toàn nhiệm vụ chưa hoàn tất**, vì thiếu NX game và có sự cố bảo toàn binary MapleRoyals. Target/Ideal Success chưa đạt. [Báo cáo mới nhất](LATEST.md).
