# Cần cung cấp gì để mở được Maple World

**29/10/2026 — Phase 5.** Người dùng xác nhận: **chưa có bộ tài nguyên riêng**.

OpenStory/converter đã build và Cosmic/MySQL hoạt động. Thiếu nội dung client v83 được phép dùng, không phải compiler/database. XML server không thay thế đầy đủ bitmap, âm thanh và UI của client.

## Đầu vào cần có

Một trong hai dạng:

1. **16 NX game thật**: Base, Character, Effect, Etc, Item, Map, Mob, Morph, Npc, Quest, Reactor, Skill, Sound, String, TamingMob, UI, đuôi .nx.
2. **WZ/IMG v83 tương ứng có quyền chuyển đổi**. Một IMG, XML server hoặc EXE riêng không đủ. Layout/locale/codec phải thử trên bộ thật; converter chưa chứng minh mọi biến thể WZ.

Đặt riêng trong **cosmic-local/openstory/phase4/user-assets/** hoặc cho đường dẫn thư mục riêng ngoài MapleRoyals. Không ghi đè source/wz/runtime và không đưa tài nguyên lên GitHub.

Kèm thông tin:

- Nguồn gốc, phiên bản/locale nếu biết, các file thuộc bộ nào.
- Giấy phép, điều khoản hoặc bằng chứng cho phép dùng cục bộ; với WZ/IMG, cả quyền xử lý/chuyển đổi. Có file trên máy hoặc URL công khai chưa đủ để tự xác minh quyền.
- Bằng chứng riêng tư giữ cục bộ; báo cáo công khai chỉ ghi kết luận đã review.

Agent sẽ kiểm kê, tính SHA256 và tạo manifest gắn bằng chứng đã review với đúng file. **Người dùng không cần tự băm từng file.** Không tự điền approved=true khi chưa có căn cứ. AGPL của source không tự cấp quyền đối với asset MapleStory.

Không có nguồn tải game asset nào được phê duyệt trong nhiệm vụ này. Không tự lấy NX/WZ từ mirror/server lậu hoặc MapleRoyals để lấp thiếu. Quyền chuyển đổi và tương thích v83/Cosmic của MapleRoyals hiện chưa được xác minh.

## Công việc khi đầu vào đủ điều kiện

1. Review nguồn/quyền/hash; copy vào staging, giữ nguyên bản gốc.
2. Dùng converter Phase 4 nếu cần, rồi validator/NoLifeNx; không viết lại pipeline.
3. Kiểm nodes/UI/bitmap/animation/audio/links và IDs/fields/map/portal/skill với Cosmic XML. Đúng 16 tên file chưa đủ.
4. Runtime riêng, Settings 127.0.0.1:8484; không sửa EXE hoặc tắt PIN/PIC.
5. Login UI → auth/PIN/PIC → world/channel → nhân vật → map cổ điển → movement/portal/combat/EXP/loot → logout/mở lại và đối chiếu dữ liệu thật.

Nếu lỗi, tái hiện và sửa đúng source hoặc dừng đúng asset chưa đạt; không thay bằng fixture. Chỉ sau gameplay baseline mới làm Kiếm Tu/Nhất Tuyến Kiếm Khí.
