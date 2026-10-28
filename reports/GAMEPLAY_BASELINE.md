# Gameplay baseline — Phase 5

**29/10/2026 — NOT RUN: thiếu NX v83 được phép dùng.**

| Bước | Kết quả thực tế |
| --- | --- |
| Backend localhost | Health Phase 5 PASS; 75 bảng, 37 changeset, lock=0 |
| Client tự build/chẩn đoán | Bằng chứng Phase 4; Missing a game file: Base.nx |
| NX game | Preflight Phase 5: 0/16 hiện có, 0 đủ điều kiện |
| Login UI / authentication / PIN / PIC | Chưa |
| World/channel / tạo/chọn nhân vật | Chưa |
| Map cổ điển / chạy / nhảy / leo | Chưa |
| Portal / damage / giết quái | Chưa |
| EXP / loot / inventory | Chưa |
| Logout / mở lại / đối chiếu nhân vật | Chưa |
| Ảnh gameplay thật | Chưa có |

Phase 5 không mở lại process thiếu asset, không tạo account/character bằng SQL. Seed, handshake và tests lịch sử không phải gameplay.

Khi đủ asset: validator/đối chiếu dữ liệu → runtime riêng → account thường thử nghiệm → PIN/PIC hợp lệ → world/channel → nhân vật/map → movement/portal/combat/EXP/loot → ghi Level/EXP/map/items → logout/mở lại và so sánh. Lưu log/ảnh thật cục bộ; chỉ công bố phần đã review trong phạm vi cho phép.

Không triển khai Kiếm Tu/Nhất Tuyến Kiếm Khí trước baseline thực tế. [Đầu vào](ASSET_INTAKE.md) · [Latest](LATEST.md).
