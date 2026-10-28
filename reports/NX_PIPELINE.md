# NX pipeline — Phase 4
Ngày 29/10/2026 (Asia/Saigon). **Pipeline fixture hoạt động; chưa có Maple World.**

## Luồng đã triển khai
Input WZ/IMG được ghi nhận quyền dùng và SHA256 → copy vào staging riêng → converter tự build ở client mode → kiểm tra exit/log → NX preflight → công bố file mới không ghi đè. Script không tự đưa output vào runtime.

Code cục bộ: cosmic-local/openstory/phase4/tools/. Các lệnh chạy từ thư mục dự án, PowerShell 7:
~~~powershell
.\cosmic-local\openstory\phase4\tools\Build-Converter.ps1
.\cosmic-local\openstory\phase4\tools\Run-Phase4-QA.ps1
~~~

run_conversion.py nhận --input, --output, --converter, --manifest, --report, --build-report; --hc bật LZ4 HC. Converter phải khớp hash trong báo cáo build. Input phải có SHA256 tương ứng trong manifest đã review, approved_for_local_use=true, rights_evidence và origin là synthetic-fixture hoặc authorized-game-assets. Manifest chỉ ghi nhận bằng chứng được review, không tự chứng minh quyền pháp lý.

Input được giữ nguyên và hash lại; output phải là file NX mới. Windows dùng rename không thay destination đã tồn tại; POSIX dùng hard-link không ghi đè. Timeout conversion 120 giây. Lỗi converter, unresolved link hoặc validation lỗi sẽ chặn công bố output. Log chi tiết giữ cục bộ.

## Validator
nx_preflight.py kiểm tra 52-byte PKG4 header, giới hạn/count/bounds bảng, UTF-8, node type/index, child range, thứ tự/tên trùng, cycle/reachability, string value, bitmap size/LZ4 và audio range. Bitmap được decode có giới hạn 64 MiB; node budget 2 triệu, edge budget 8 triệu. File quá giới hạn cần review, không tự tăng giới hạn.

Kiểm tra node profile được khai báo cho Base/UI/Map/Sound/Etc, root không rỗng cho package game, explicit _inlink/_outlink và target file nằm trong allowlist. Các tham chiếu động/source mơ hồ được ghi để review; coverage không đầy đủ cho mọi đường dẫn dựng lúc runtime.

Output JSON và Markdown tách structural_valid, profile_valid, permission_evidence_matches_hash, qualified_game_asset. Tên đúng không đủ để hợp lệ. Fixture không được tính là game asset kể cả đổi tên thành 16 file bắt buộc.

## Giới hạn
Đã thử primitive/vector/UOL, RGBA8888, animation metadata và PCM tự tạo. Chưa kiểm chứng tất cả WZ layout/locale, DXT/palette/Convex2D, toàn bộ v83 package hoặc asset thương mại. Mẫu legacy83.wz là archive nhỏ một IMG tuần tự, không đại diện mọi archive thật.

[QA converter](NX_CONVERTER_QA.md) · [Audit tài nguyên](NX_ASSET_AUDIT.md) · [Trạng thái mới nhất](LATEST.md).
