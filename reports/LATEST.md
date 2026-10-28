# VẤN KIẾM — Báo cáo tiến độ mới nhất

Cập nhật **29/10/2026, UTC+07:00**. Nhiệm vụ: **build và kiểm thử OpenStory trên Windows MSI**, thực hiện ngày 28–29/10.

**OpenStory đã build Release x64 và chạy kiểm tra khởi động. Client kết nối được ở mức TCP/handshake với Cosmic, rồi dừng ở thiếu Base.nx. Chưa hiển thị giao diện đăng nhập, chưa có gameplay hoặc nhân vật mới.**

## 1. Trạng thái theo mục tiêu

| Hạng mục | Kết quả đã xác minh |
| --- | --- |
| Clone source OpenStory riêng | Đạt, pin commit db2c983b859f6c7f9d09c1680cd279af3bd51a22 |
| Build client từ source | **Đạt — Release x64, exit code 0** |
| Dependency phục vụ build | Đủ để build; có một số thư viện upstream prebuilt |
| Runtime FreeType/BASS | Đã chuẩn bị: FreeType tự build, BASS có chữ ký nhà phát hành |
| NX phù hợp và được phép dùng | **Chưa có: 0/16 file bắt buộc** |
| TCP/handshake localhost | **Đạt qua mã Winsock thật và lần khởi động client tự build** |
| Parser cấu hình localhost | Đạt kiểm tra đọc/lưu/tải lại Settings |
| Login/PIN/PIC/world/channel UI | **Chưa kiểm thử qua giao diện** |
| Tạo nhân vật/vào Maple World | **Chưa** |
| Di chuyển/nhảy/leo/portal | **Chưa** |
| Combat/EXP/loot | **Chưa** |
| Thoát/mở lại và lưu nhân vật | **Chưa kiểm thử qua game** |
| Sẵn sàng triển khai Kiếm Tu | **Chưa: cần hoàn thành gameplay baseline trước** |

Hướng dự án vẫn là nhân vật tu tiên dùng kiếm trong **MapleStory cổ điển**, giữ map/quái/NPC quen thuộc. Không quay lại Phaser, không thay Maple World và chưa thêm cảnh giới, phi kiếm hoặc kỹ năng tu tiên.

## 2. Source, giấy phép và môi trường build

- Source: [rdiol12/OpenStory](https://github.com/rdiol12/OpenStory), nhánh upstream master, commit **db2c983b859f6c7f9d09c1680cd279af3bd51a22** ngày 23/10/2026. Dùng commit để nhận diện phiên bản; không gắn nhãn phát hành tự tạo.
- Clone riêng tại **cosmic-local/openstory/source**, nhánh local van-kiem-local. Source OpenStory upstream không có thay đổi sau nhiệm vụ; script build/test và tài liệu nằm ngoài source.
- Đã đọc LICENSE AGPL-3.0 và các notice trong source. Một số source header cho phép v3-or-later; giữ nguyên toàn bộ notices. Giấy phép source không tự cấp quyền cho mọi font/icon/game asset hoặc thư viện sở hữu riêng.
- Cấu trúc: src/ chứa nhân vật/gameplay/UI/network/config, vendor/ chứa dependency, platform/ và resource/ hỗ trợ nền tảng/đồ họa, tests/ chứa một số test upstream. Client C++17 dùng OpenGL, GLFW, GLEW, NoLifeNx/LZ4, FreeType và BASS.
- Windows mặc định dùng **Winsock**; Asio không bắt buộc cho backend Windows này. Launcher .NET là dự án riêng, không được build/đánh dấu đạt trong nhiệm vụ client C++.
- **Visual Studio Build Tools 2022 17.14.37824.6**, **MSVC 19.44.35229**, **Windows SDK 10.0.26100.0** đã cài thành công. Bootstrapper tải từ Microsoft và có chữ ký Valid; người dùng xác nhận UAC. Không reboot.
- **CMake portable 4.4.5** từ Kitware: ZIP khớp SHA256 công bố và executable có chữ ký Kitware hợp lệ. Không đổi PATH hệ thống.
- Dùng alias ASCII V: hiện có của môi trường Cosmic để build. Compiler path đưa vào CMake qua alias với dấu slash; không sửa endpoint trong executable.

### Artifact build

OpenStory.exe tại **cosmic-local/openstory/source/wz/OpenStory.exe**:

- Kiến trúc: **x64**, bản **Release**, **8.031.744 bytes**.
- SHA256: **307E092761DC67F08C5C28716940678EC5F6D39A5A12EF205FB7886A4ADC0C11**.
- Build thực sự hoàn tất ngày 29/10 lúc 00:04 giờ Việt Nam. **0 lỗi build cuối**, vẫn có cảnh báo được ghi ở mục 6.
- Executable và source không được upload lên repository báo cáo.

## 3. Thư viện bên thứ ba

| Thành phần | Kết quả kiểm tra / giới hạn |
| --- | --- |
| NoLifeNx | Source header AGPL-3.0-or-later; được compile từ source kèm OpenStory |
| LZ4 | BSD-2-Clause; compile C source thay vì chạy tool EXE đóng gói |
| GLFW 3.3.2 | Notice zlib-style; build client dùng static library upstream kèm source |
| GLEW 2.1.0 | Có nhiều notice permissive trong LICENSE.txt; dùng static library upstream |
| FreeType 2.13.3 | Đã đọc lựa chọn FTL/GPLv2+; tự build DLL đúng phiên bản header từ source chính thức |
| stb_image | MIT hoặc Unlicense theo header |
| BASS 2.4 | Thư viện sở hữu riêng; điều khoản phi thương mại, không được tự coi là AGPL |
| Asio | Chưa dùng ở build Windows mặc định; cần audit riêng nếu chuyển backend |
| Font/icon | Repository có tài nguyên và font nhúng; audit nguồn gốc/notices chưa hoàn tất, chưa phê duyệt phân phối binary/asset |

**FreeType:** build từ [source tag VER-2-13-3](https://github.com/freetype/freetype/tree/VER-2-13-3), commit **42608f77f20749dd6ddc9e0536788eaad70ea4b5**. DLL đã được tự build, không chạy DLL FreeType prebuilt trong vendor. Các tính năng tùy chọn zlib/bzip2/PNG/HarfBuzz/Brotli chưa bật; chưa kiểm thử glyph/UI thực tế. Giữ notice FreeType trong source; chưa phát hành gói binary.

**BASS:** tải từ [nhà phát hành Un4seen](https://www.un4seen.com/bass.html). DLL x64 có chữ ký **Valid — Un4seen Developments Ltd**; không chạy các EXE demo. Giấy phép của BASS cần xem riêng khi phân phối hoặc thương mại hóa. Chỉ chuẩn bị để thử cục bộ trong nhiệm vụ này.

Không tuyên bố toàn bộ chuỗi dependency đã tự rebuild hoặc được phê duyệt phân phối. Các thư viện static upstream và font/icon còn cần rà soát trước một bản phát hành.

## 4. Kiểm thử thực sự đã chạy

### Kiểm tra độc lập không cần tài nguyên thương mại

**3/3 CTest targets PASS, 0 failed/skipped:**

1. **Synthetic NX reader:** dùng NoLifeNx thật đọc file PKG4 do test tự tạo. Đọc node/string/integer/path, kiểm tra missing node, metadata bitmap, giải nén LZ4 đúng byte và 100 lần open/close. Fixture không chứa tài nguyên MapleStory và không dùng giả làm game assets.
2. **Supported localhost Settings:** dùng Configuration parser thật của OpenStory, xác minh đọc/lưu/tải lại 127.0.0.1:8484 và SaveLogin=false.
3. **Live Cosmic handshake:** dùng SocketWinsock.cpp thật của OpenStory kết nối 127.0.0.1:8484, xác minh handshake v83, subversion 1, locale 8. Không gửi credentials hoặc packet đăng nhập.

**10/10 trường hợp Session/Cryptography upstream PASS:** frame đầy đủ, từng byte, header/body phân mảnh, frame tiếp theo chưa đủ, reconnect giữa header/body và reconnect từ handler. Chỉ đổi lệnh compiler test từ c++ sang MSVC, giữ test body. Suite này dùng socket in-memory; kết quả không thay cho login/combat thật.

**25/25 opcode chọn lọc khớp khi đối chiếu tĩnh:** login, server/character list, chọn/tạo nhân vật, PIC, ping, map/channel, movement, melee và pickup. Chưa kiểm thử toàn bộ packet body/parser và không suy tương thích hoàn toàn từ số phiên bản v83.

### Khởi động executable tự build

- Đã kiểm tra DLL/import và chuẩn bị BASS từ nhà phát hành cùng FreeType tự build.
- Quét runtime bằng Defender; có event bắt đầu/kết thúc cùng Scan ID và **0 threat event liên quan**.
- Chạy executable tự build ở thư mục runtime riêng, endpoint localhost, **không có NX**, không nhập tài khoản/mật khẩu.
- Process đã chạy, nhận handshake qua Session trước bước kiểm tra asset, rồi ghi đúng thông báo: **Missing a game file: Base.nx**.
- Process thoát trong timeout, exit code 0. Đây là kết quả chẩn đoán thiếu asset, **không phải gameplay PASS**.
- Chưa tới bước tạo cửa sổ đồ họa/login. Không có ảnh gameplay; không tạo ảnh minh họa thay bằng chứng.

### Bảo toàn môi trường

Kiểm tra cuối lúc **00:10 ngày 29/10**: Cosmic/MySQL vẫn cùng tiến trình/listeners, JAR không đổi, **75 bảng / 37 Liquibase changeset / lock=0**. Vẫn 1 account, 1 character và 5 inventory item seed upstream; chưa tạo/xóa tài khoản hoặc nhân vật trong nhiệm vụ này.

Audit lại **34.654 file mỗi cây** của MapleRoyals: original không đổi, backup khớp original; working chỉ có label probe đã biết từ giai đoạn trước. Không dùng MapleRoyals làm nguồn asset OpenStory.

**1.886 server tests PASS** là kết quả đã chạy trong nhiệm vụ thiết lập trước. Không build lại server hoặc chạy lại suite này trong nhiệm vụ OpenStory.

## 5. NX và tích hợp Cosmic

Source NxFiles::init yêu cầu đúng 16 file:

**Base.nx, Character.nx, Effect.nx, Etc.nx, Item.nx, Map.nx, Mob.nx, Morph.nx, Npc.nx, Quest.nx, Reactor.nx, Skill.nx, Sound.nx, String.nx, TamingMob.nx, UI.nx.**

Hiện **0/16** được cung cấp. Chưa tìm được nguồn bộ NX v83 có quyền sử dụng được xác minh trong môi trường này. Không tải WZ/NX thương mại, không chuyển đổi hoặc trộn file MapleRoyals.

MapPretty.nx, MapLatest.nx và Map001.nx có đường đọc tùy chọn trong NoLifeNx; một số UI còn tham chiếu node phụ. Chưa kiểm chứng bộ v83 thực tế đủ node cho mọi màn hình. Không tự lấy asset bản mới để lấp thiếu. NoLifeNx có nhánh Data.nx, nhưng NxFiles::init hiện vẫn đòi 16 file; một Data.nx riêng chưa đáp ứng preflight hiện tại.

Đã đọc [NoLifeWzToNx](https://github.com/ryantpayton/NoLifeWzToNx): công cụ chuyển WZ/IMG sang NX có source mô tả AGPL. Chưa clone/build/chạy converter hoặc chuyển đổi game assets. Cần kiểm tra lại source/license và chế độ xuất bitmap/audio trước khi dùng với dữ liệu được phép sử dụng. Giấy phép converter không tự cấp quyền cho tài nguyên đầu vào.

Cấu hình được hỗ trợ đã chuẩn bị ở source/wz/Settings:

~~~ini
ServerIP = 127.0.0.1
ServerPort = 8484
SaveLogin = false
~~~

Cấu hình server vẫn giữ localhost, US-ASCII, PIN/PIC bật. Không đổi charset/auth hoặc patch EXE. Client có code PIC; các tên packet PIN như AFTER_LOGIN/REGISTER_PIN/CHECK_PIN chưa thấy triển khai tương ứng trong Net source đã kiểm tra. Cần xác minh hoặc bổ sung **luồng PIN hợp lệ** khi có điều kiện thử UI; chưa kết luận đây là lỗi đã tái hiện. Không tắt PIN/PIC hoặc bỏ xác thực để vượt bước này.

## 6. Lỗi đã xử lý và vấn đề còn tồn tại

**Đã xử lý:**

- Bootstrapper Microsoft lỗi giải nén 42 qua đường dẫn dự án; tiến trình quản trị không nhìn thấy alias V:. Dùng bản sao signed installer trong thư mục tạm ASCII thật và UAC do người dùng xác nhận; cài thành công, không reboot.
- CMake nhận diện được MSVC nhưng output đường dẫn tiếng Việt biến thành dấu hỏi: cung cấp compiler qua V:.
- Đường dẫn compiler chứa backslash gây lỗi escape trong generated CMake: chuyển sang slash, cấu hình/build đạt.
- Thiếu freetype.dll cạnh executable: build FreeType từ source chính thức đúng phiên bản header; BASS lấy bản signed từ nhà phát hành.
- Include path GLEW cho test Configuration độc lập đã bổ sung; 3 CTest targets cuối đều đạt.

**Còn tồn tại:**

1. Thiếu bộ NX v83 được phép dùng; đây là blocker trực tiếp trước giao diện và gameplay.
2. Login/PIN/PIC/character/world/channel chưa kiểm thử; static opcode match chưa đủ.
3. Cảnh báo upstream còn: deprecated std::iterator, narrowing/conversion, **LNK4099 thiếu PDB của GLEW** và **LNK4098 xung đột CRT mặc định** giữa thư viện đóng gói. Build và startup đã đạt; cần chuẩn hóa dependency/CRT và kiểm thử runtime dài trước phát hành, không chỉ ẩn warning.
4. Chưa kiểm thử render/font/audio thực tế; FreeType optional feature configuration cần đánh giá khi có UI assets.
5. Audit quyền dùng font/icon và điều kiện phân phối tổng thể chưa hoàn tất. Không phát hành client binary hoặc asset.
6. Chưa có bằng chứng di chuyển, combat, EXP, loot, portal hoặc persistence của nhân vật do người chơi tạo.

## 7. Bảo mật

Defender service và AntivirusEnabled true, nhưng **RealTimeProtectionEnabled đã false trước nhiệm vụ và vẫn false sau**. Không thay đổi thiết lập Defender hoặc exclusions. Quét theo yêu cầu không bảo đảm tuyệt đối an toàn.

Không chạy executable game không rõ nguồn gốc; lần khởi động mới dùng client tự build, DLL BASS có chữ ký nhà phát hành và FreeType tự build. EXE ứng viên Cosmic-client từ khảo sát cũ vẫn không được chạy.

Không restart/reset Cosmic hoặc database, không mở port ra Internet, không xóa save/nhân vật, không vượt DRM/anti-cheat/auth. Không upload source Cosmic/OpenStory, client, WZ/NX, DB, mật khẩu hoặc log thô lên GitHub. Repository source upstream có font/icon kèm theo; chưa dùng chúng để tuyên bố quyền phân phối game assets.

## 8. Bằng chứng và cách tiếp tục

Bằng chứng giữ **cục bộ**, không có trong repository báo cáo:

- cosmic-local/openstory/reports/build-result.json, source-provenance.json, vs-instances.json, cmake-download.json.
- dependency-license-audit.json, bass-official.json, freetype-build.json.
- independent-tests.xml, upstream-framing.log, assets-opcodes.json, startup-result.json.
- final-defender-events.json, final-defender-scan.json, defender-before.json, defender-after.json.
- backend-before.json, backend-after-startup.json, mapleroyals-preservation.log, task-result.json.

Hướng dẫn build/test đầy đủ: **cosmic-local/openstory/README.md** trên máy dự án. Scripts: tools/Build-OpenStory.ps1, Build-FreeType.ps1, Run-Checks.ps1 và Run-UpstreamFraming.cmd trong nhánh OpenStory. Log có thể chứa đường dẫn hoặc thông tin máy, được giữ riêng.

**Công việc tiếp theo:**

1. Xác lập nguồn tài nguyên v83 được phép sử dụng; tự build/audit converter nếu cần, giữ manifest/hash/quyền dùng. Không lấy NX/WZ ngẫu nhiên từ diễn đàn hoặc mặc định MapleRoyals.
2. Đối chiếu node và dữ liệu map/mob/skill với Cosmic XML; xác minh UI.nx v83 cùng các node phụ thật sự cần.
3. Chuẩn hóa dependency/CRT; kiểm tra font/audio và luồng PIN/PIC hợp lệ.
4. Thử tài khoản thường: login → tạo nhân vật → chọn world/channel → vào Maple World → chạy/nhảy/leo → portal → combat/EXP/loot → logout/reopen/persist. Lưu log đã lọc và ảnh gameplay thật.
5. Chỉ khi baseline trên đạt mới triển khai Kiếm Tu và Nhất Tuyến Kiếm Khí trong map/quái MapleStory cổ điển.

## 9. Báo cáo GitHub

Đường dẫn cố định: [reports/LATEST.md](https://github.com/r0bjn10-maker/van-kiem/blob/main/reports/LATEST.md). [Markdown trực tiếp](https://raw.githubusercontent.com/r0bjn10-maker/van-kiem/main/reports/LATEST.md).

Mỗi nhiệm vụ cập nhật báo cáo bằng commit riêng chỉ chứa Markdown đã duyệt trong reports/. Phiên bản trước có trong Git history. Kết quả đồng bộ được xác minh riêng bằng việc đọc lại nội dung/commit từ GitHub; không dùng chính câu trong báo cáo làm bằng chứng upload thành công.
