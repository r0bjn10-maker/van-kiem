# OpenStory startup QA — Phase 4
Ngày 29/10/2026. **Release x64 build đạt; chạy chẩn đoán đạt; chưa hiển thị login.**

## Build/runtime
OpenStory source db2c983b859f6c7f9d09c1680cd279af3bd51a22, có local CMake patch nhỏ để chọn thư viện GLEW. GLEW 2.1.0 được build từ source release chính thức, cùng /MD với client và GLFW, debug info nhúng.

Trước đó glew32s.lib dùng LIBCMT, còn client/GLFW dùng MSVCRT. Sau relink, build log có **0 lỗi, 0 LNK4098/LNK4099**; không dùng /NODEFAULTLIB hay tùy chọn ẩn warning để đạt kết quả. Các cảnh báo C++ upstream khác chưa được dọn toàn bộ.

OpenStory.exe hiện 8.109.056 bytes, SHA256 F4762A8EBB1879F94E853E53EECD4A05730F2198D21009816974E928866CAC5E. Bản trước được backup riêng; local diff được giữ để rollback.

Runtime mới được tạo riêng trong phase4/runtimes/diagnostic-...: EXE tự build, FreeType 2.13.3 tự build, BASS có chữ ký Un4seen và Settings localhost/SaveLogin=false. Không copy NX giả. Prepare-Runtime.ps1 có nhánh nhận asset/rights manifest nhưng từ chối khi không đủ 16 package đạt kiểm tra.

## Kiểm thử trực tiếp
- 3/3 CTest loader/config/handshake đạt.
- 10/10 Session/Cryptography framing/reconnect đạt.
- Runtime đã quét Defender với event bắt đầu/kết thúc cùng Scan ID, 0 threat event liên quan. Dùng đường dẫn vật lý vì Defender không quét được qua alias V:.
- Process client đã chạy từ runtime riêng, kết nối handshake trước bước asset, báo **Missing a game file: Base.nx**, thoát exit 0 trong timeout.
- Đây là kết quả lỗi thiếu asset được quan sát, không phải login/UI/gameplay PASS.
- Chưa khởi tạo đồ họa/font/render/audio game do thiếu NX; không có ảnh gameplay.

## Defender
Đầu Phase 4 ghi nhận real-time=false. Khi tiếp tục và kiểm tra cuối, RealTimeProtectionEnabled=true, service/AntivirusEnabled=true. Agent không chạy lệnh đổi cài đặt/exclusion; không xác định nguyên nhân thay đổi ngoài phiên thao tác. Trạng thái hiện tại là bật, không lấy trạng thái cũ làm kết luận mới. Quét không bảo đảm tuyệt đối an toàn.

[Blockers](BLOCKERS.md) · [Gameplay](GAMEPLAY_BASELINE.md).
