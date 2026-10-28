# VẤN KIẾM — Báo cáo tiến độ mới nhất

**29/10/2026 — Phase 4: NX assets & first playable Maple World.**

Các điều kiện kỹ thuật **Minimum Success** của pipeline/OpenStory/Cosmic đã đạt. **Toàn nhiệm vụ chưa hoàn tất:** vẫn thiếu 16 NX game và có sự cố bảo toàn binary MapleRoyals do Defender cách ly trong lúc đọc hash. Target/Ideal Success chưa đạt; chưa có login UI hoặc gameplay.

## 1. Kết quả theo milestone

| Milestone | Trạng thái thực tế |
| --- | --- |
| A — NX pipeline | Đạt trên fixture hợp lệ tự tạo; chưa chứng minh toàn bộ WZ thương mại |
| B — Asset integration | Bị chặn: 0/16 file game đủ điều kiện trong runtime |
| C — Login UI | Chưa; process báo Missing a game file: Base.nx trước giao diện |
| D — Gameplay | Chưa login/tạo nhân vật/vào map/combat/EXP/loot/save-reload |

Dự án vẫn giữ map, quái và NPC MapleStory cổ điển. Không quay lại Phaser hoặc thêm cảnh giới/phi kiếm/kỹ năng tu tiên.

## 2. Đã code và build

- Clone riêng NoLifeWzToNx, pin commit **73bddde1214445795d0a5cd4c3d431fe15eb49eb**. Giữ AGPL notices trong source; không dùng converter EXE đóng gói sẵn.
- Build bằng MSVC/SDK/CMake đã có; không cài lại toolchain. Compile zlib 1.3.2, LZ4 1.10.0 và libsquish từ source.
- Sửa filesystem C++17, đường dẫn Unicode Windows, bounds/count/size và exit code của converter; từ chối bitmap inflate lỗi thay vì xuất ảnh trắng.
- Tạo runner copy input vào staging, bắt buộc quyền dùng/SHA256 đã review, hash converter đúng bản build, timeout, kiểm tra log/validator và công bố file mới không overwrite. Không chỉnh WZ/IMG gốc hoặc tự đẩy output vào runtime.
- Tạo NX validator JSON/Markdown, kiểm tra cấu trúc/nodes/media, explicit links, source profile và bằng chứng quyền dùng tách biệt. Fixture không được tính là game asset.
- Tạo scripts inventory, so sánh container ID có gate, runtime riêng và thu status/hash log. Full dynamic path coverage chưa hoàn tất.
- Rebuild **GLEW 2.1.0 từ source cùng /MD** và relink OpenStory qua local CMake option. Kết quả cuối **0 lỗi build, 0 LNK4098/LNK4099**; không ẩn cảnh báo.

**Converter:** 455.168 bytes; SHA256 **C22D9B31F1EB66459F3403FAD994307A6D5A2ABA112C504DDA473609F7F69B48**.

**OpenStory Release x64:** 8.109.056 bytes; SHA256 **F4762A8EBB1879F94E853E53EECD4A05730F2198D21009816974E928866CAC5E**. Upstream db2c983b859f6c7f9d09c1680cd279af3bd51a22, local patch CMake chọn GLEW; bản EXE trước được backup để rollback. Các file source/binary/patch chỉ giữ cục bộ.

## 3. Đã kiểm thử

| Kiểm tra thực thi Phase 4 | Kết quả |
| --- | --- |
| Pipeline/converter/validator | **29/29 PASS**, 0 failure/error/skip |
| OpenStory CTest loader/config/handshake | **3/3 PASS** |
| Upstream Session/Cryptography framing/reconnect | **10/10 PASS** |
| GLEW CRT/PDB linker warnings | **LNK4098/LNK4099 không còn** trong build cuối |
| Defender scan runtime riêng | Có event bắt đầu/kết thúc cùng Scan ID, 0 threat event liên quan |
| Khởi động native runtime riêng | Đạt chẩn đoán thiếu Base.nx, exit 0 trong timeout |
| NX thực tế trong runtime | **0/16 có file; 0 valid; 0 qualified game asset** |
| Asset comparison với Cosmic | **Blocked**, differences=null, không giả báo không có khác biệt |
| Bảo toàn toàn bộ client MapleRoyals | **FAIL — Defender cách ly 6 binary trong original/backup/working** |
| Login/character/map/combat/persistence qua game | **NOT RUN** |

29 test bao gồm IMG và archive legacy v83 nhỏ tự tạo, LZ4 HC, output lặp lại giống nhau, input giữ nguyên, Unicode paths/values, primitive/vector/UOL, bitmap/animation/audio, corruption/bounds/cycle/string/LZ4/reference, chống overwrite và hash sai, không chấp nhận 16 fixture đổi tên là game data. Ba nhánh conversion được đọc lại qua source NoLifeNx và decoder LZ4 1.8.2 của OpenStory, mỗi nhánh có 15 kiểm tra dữ liệu. BASS có chữ ký nhà phát hành decode được PCM tự tạo, không playback.

Mẫu WZ là archive một IMG tuần tự với header legacy v83; chưa thử mọi layout/locale/codec thương mại. Audio fixture có prefix tổng hợp và WAV/PCM tự tạo; chưa chứng minh mọi Sound_DX8 thật. Các format DXT/palette/Convex2D còn ngoài bằng chứng hiện có.

Runtime chẩn đoán chỉ chứa EXE/DLL đã kiểm hash và Settings localhost/SaveLogin=false; **không copy fixture vào runtime**. Client nhận handshake rồi dừng ở thiếu Base.nx trước window/login. Không có ảnh gameplay; không tạo ảnh giả.

**1.886 server tests PASS** là kết quả nhiệm vụ trước, không chạy lại trong Phase 4. Kết quả lịch sử 25 selected opcode values khớp không thay cho xác minh payload/login.

## 4. Bảo toàn và bảo mật

Khi Phase 4 bắt đầu, backend hoạt động. Khi tiếp tục sau gián đoạn, Cosmic/MySQL đều đã dừng; kết nối MySQL lỗi 2003. Đã xác minh marker/data/config có sẵn và cổng rảnh, rồi resume đúng instance cũ. **Không initialize/reset/reinstall database, không tạo lại schema, không xóa nhân vật.**

Kiểm tra sau resume và chẩn đoán lúc 06:20: **75 bảng, 37 changeset, lock=0, 1 account/1 character/5 inventory item seed**, JAR Cosmic không đổi. Năm listener vẫn chỉ 127.0.0.1. Process ID thay đổi vì resume từ trạng thái dừng; không báo là cùng PID xuyên suốt.

**Sự cố bảo toàn MapleRoyals:** full hash audit không đạt. Trong lúc đọc hash khoảng 06:21–06:24, Defender ghi event 1116/1117 và cách ly MapleRoyals.exe cùng mr.dll ở original, backup và working (6 file), nhãn Trojan:Win32/Malgent!MSR. Metadata sau đó xác nhận các file không còn ở vị trí cũ. Không gọi đây là false positive hoặc báo client original/backup còn nguyên.

Agent không sửa/xóa/chạy các binary, không thay Defender, không tự restore/exclusion; tuy nhiên kết quả bảo toàn binary vẫn FAIL vì quarantine. WZ/IMG không có thay đổi hash mới; working vẫn có label probe cũ. Kiểm kê mỗi nhóm có 34.628 WZ/IMG, không đồng nghĩa quyền dùng đã được xác minh. Log/detection metadata giữ cục bộ, không upload file bị cách ly. Cần xử lý riêng nguồn binary đáng tin cậy, không tự chép lại từ nguồn chưa xác minh để né antivirus.

**Defender real-time hiện bật.** Đầu Phase 4 ghi false, ở lần tiếp tục/kiểm tra cuối ghi true; agent không thay cài đặt/exclusion và không xác định nguyên nhân thay đổi bên ngoài phiên thao tác. Quét theo yêu cầu không bảo đảm tuyệt đối an toàn.

Không public port, không bypass DRM/anti-cheat/auth, không dùng executable game không rõ nguồn. Runtime dùng client tự build, FreeType tự build, BASS có chữ ký Un4seen. Không upload client/source/game asset, DB, credentials hoặc raw logs.

## 5. Thiếu gì và việc tiếp theo

**Bộ tài nguyên v83 được phép dùng vẫn thiếu toàn bộ:** Base, Character, Effect, Etc, Item, Map, Mob, Morph, Npc, Quest, Reactor, Skill, Sound, String, TamingMob, UI (đuôi .nx). Hoặc cần WZ/IMG phù hợp có quyền chuyển đổi để chạy pipeline trên bản copy. Chưa tìm thấy bộ này trong workspace; không mặc định MapleRoyals là nguồn.

Sau khi có dữ liệu:
1. Review nguồn/quyền dùng/hash → convert nếu cần → validator cấu trúc và node profile.
2. Đối chiếu IDs/fields/portals/animation/UI với Cosmic XML; không chỉ so container ID.
3. Runtime riêng → login UI thật → PIN/PIC hợp lệ → world/channel → tạo/chọn nhân vật.
4. Vào map cổ điển, movement/portal/combat/EXP/loot → logout/reopen và đối chiếu save bằng bằng chứng thật.

PIN/PIC vẫn bật; luồng PIN trong OpenStory chưa được xác minh đầy đủ. Node động và một số node UI phụ cần thử trên dữ liệu thật. Giấy phép font/icon và phân phối binary tổng thể vẫn cần review. Không thay hoặc xóa dữ liệu gốc chỉ để né lỗi.

**Chưa sẵn sàng bắt đầu Kiếm Tu/Nhất Tuyến Kiếm Khí.**

## 6. Tài liệu Phase 4

- [NX_PIPELINE.md](NX_PIPELINE.md)
- [NX_ASSET_AUDIT.md](NX_ASSET_AUDIT.md)
- [NX_CONVERTER_QA.md](NX_CONVERTER_QA.md)
- [OPENSTORY_STARTUP_QA.md](OPENSTORY_STARTUP_QA.md)
- [CLIENT_SERVER_INTEGRATION.md](CLIENT_SERVER_INTEGRATION.md)
- [GAMEPLAY_BASELINE.md](GAMEPLAY_BASELINE.md)
- [BLOCKERS.md](BLOCKERS.md)

Code/tools/evidence cục bộ ở **cosmic-local/openstory/phase4/**. Reports gồm converter-build.json, pipeline-tests.json/log, runtime-nx.json/md, workspace-assets.json, cosmic-asset-compatibility.json, startup-result.json, backend-after.json, defender-scan.json và evidence-manifest.json. Chỉ Markdown đã review trong reports/ được đưa lên GitHub.

Đường dẫn đọc cố định: [báo cáo mới nhất](https://github.com/r0bjn10-maker/van-kiem/blob/main/reports/LATEST.md), [Markdown trực tiếp](https://raw.githubusercontent.com/r0bjn10-maker/van-kiem/main/reports/LATEST.md). Commit báo cáo riêng được kiểm tra bằng cách đọc lại trên GitHub; không lấy nội dung báo cáo làm bằng chứng tự xác nhận đã đồng bộ.
